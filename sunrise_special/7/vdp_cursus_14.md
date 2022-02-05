                 M S X 2 / 2 +   V D P   C U R S U S   ( 1 4  )
                                                                
          
          
                                    L I N E 
          
          In BASIC  is een lijn trekken erg simpel en behoort het LINE 
          commando  tot de  eerste commando's die iedereen kent. In ML 
          is dat  wel anders,  want het  LINE commando is verreweg het 
          ingewikkeldste commando van de VDP.
          
          De  theorie is  reeds in deel 4 van de VDP cursus behandeld, 
          maar omdat  het toch  nog vrij ingewikkeld is om aan de hand 
          daarvan  een eigen  lijn routine  te schrijven,  behandel ik 
          deze keer een standaardroutine om lijnen te trekken.
          
          De routine  is wel  enigszins beperkt,  aangezien de co�rdi- 
          naten  maar  8  bits  zijn.  Hierdoor is  de routine  minder 
          geschikt  voor SCREEN  6 en  7 en de lijnen kunnen maar in 1 
          page worden getrokken.
          
          Laat ik maar meteen beginnen met de source.
          
          ; L I N E . A S M 
          ; Trek een lijn (H,L)-(D,E)
          ; Kleur=B; Log. op=A
          ; Door Stefan Boer
          ; VDP Cursus deel 15
          ; Sunrise Magazine #9
          ; (c) Stichting Sunrise 1993
          
          LINE:   AND   &H0F            ; logische operatie
                  OR    &H70            ; line
                  LD    (LINDAT+10),A
                  LD    A,B             ; kleur
                  LD    (LINDAT+8),A
                  LD    A,H             ; DX
                  LD    (LINDAT),A
                  LD    A,L             ; DY
                  LD    (LINDAT+2),A
          
          Het makkelijkst  zijn de  kleur, logische operatie en begin- 
          co�rdinaten.  Die  kunnen  zonder  verdere  problemen alvast 
          worden  ingevuld. &H70 is de opcode voor line. LINDAT is een 
          blokje met  de 11  bytes die  zodadelijk naar  de VDP worden 
          gestuurd (registers 36 t/m 46).
          
          Nu komen  de problemen.  Het LINE  commando van de VDP werkt 
          namelijk  niet zoals  alle andere  commando's met  NX en NY, 
          maar met  Maj en  Min. De  lijn wordt  gezien als de schuine 
          zijde  van een rechthoekige driehoek, waarbij Maj en Min dan 
          de twee  rechte zijden  zijn. Maj  is de  langste en  Min de 
          kortste.
          
          Met  MAJ  (bit  0  van  het argument  register) moet  worden 
          aangegeven of de langste zijde in de x richting is (0) of in 
          de  y  richting  (1).  Verder  moet  met  DIX  en  DIY zoals 
          gewoonlijk  worden aangegeven  of de lijn vanaf (DX,DY) naar 
          links/rechts resp.  boven/beneden gaat. Je snapt wel dat het 
          even wat logisch nadenken kost om dit alles te programmeren.
          
          We  gaan nu  eerst gewoon de NX berekenen, alsof er niks aan 
          de hand is. NX moet gelijk zijn aan ABS(x1-x2).
          
                  LD    A,H             ; x1
                  SUB   D               ; x1-x2
                  LD    D,&B00000100    ; DIX=links
                  JR    NC,LINE_1       ; x1>x2
                  LD    D,0             ; DIX=rechts
                  NEG                   ; positief maken
          LINE_1: LD    H,A             ; NX
          
          Behalve  NX hebben we nu ook meteen DIX, en wel in bit 2 van 
          D. NY gaat analoog:
          
                  LD    A,L             ; y1
                  SUB   E               ; y1-y2
                  LD    E,&B00001000    ; DIY=omhoog
                  JR    NC,LINE_2       ; y1>y2
                  LD    E,0             ; DIY=omlaag
                  NEG                   ; positief maken
          LINE_2: LD    L,A             ; NY
          
          Ook  hier meteen  weer DIY, nu in bit 3 van E. Nu gaan we NX 
          en NY  vergelijken om te kijken welke Min en welke Maj (nee, 
          dat  heeft niets  te maken met verkeer en waterstaat) wordt. 
          NY staat nog steeds in A.
          
                  CP    H               ; vergelijk NX met NY
                  LD    A,0             ; MAJ=X (XOR A kan niet!)
                  JR    C,LINE_3        ; NX>NY
                  LD    C,L
                  LD    L,H
                  LD    H,C             ; verwissel NX en NY
                  LD    A,&B00000001    ; MAJ=Y
          
          Het wisselen van L en H gebeurt via het C register, omdat er 
          geen EX L,H instructie bestaat. Normaal gesproken gebruik je 
          natuurlijk  altijd XOR A in plaats van LD A,0, maar hier kan 
          dat niet  omdat dan  de uitslag van de CP H wordt gewist. De 
          XOR  A voor  de CP  H zetten kan ook niet, want we hebben de 
          waarde van  A dan nog nodig voor de vergelijking! A bevat nu 
          MAJ in bit 0, H bevat Maj en L bevat Min.
          
          (Nvdr. Het  "verlies" van  XOR A kan wel goed worden gemaakt 
          met INC A in plaats van LD A,1.)
          
          
          LINE_3: OR    D
                  OR    E
                  LD    (LINDAT+9),A    ; Arg
                  LD    A,H
                  LD    (LINDAT+4),A    ; Maj
                  LD    A,L
                  LD    (LINDAT+6),A    ; Min
          
          In A  zat al  MAJ, en  daar worden  DIX en DIY nu bij geORd. 
          Samen  is dat  de juiste  waarde voor het argument register. 
          Tot slot worden Maj en Min nog in LINDAT gezet. We hoeven nu 
          alleen  nog  de  11  bytes in  LINDAT naar  de VDP  commando 
          registers  36  t/m 46  te schrijven,  waarbij we  natuurlijk 
          eerst moeten  checken of  de VDP  niet nog  bezig is met een 
          ander commando.
          
                  LD    BC,&H0B9B
                  LD    HL,LINDAT
                  CALL  VDPRDY
                  DI
                  LD    A,36
                  OUT   (&H99),A
                  LD    A,17+128
                  OUT   (&H99),A
                  EI
                  OTIR
                  RET
          
          VDPRDY: LD    A,2
                  CALL  RDSTAT
                  BIT   0,A
                  JP    NZ,VDPRDY
                  RET
          
          RDSTAT: DI
                  OUT   (&H99),A
                  LD    A,15+128
                  OUT   (&H99),A
                  NOP
                  NOP
                  IN    A,(&H99)
                  EX    AF,AF
                  XOR   A
                  OUT   (&H99),A
                  LD    A,128+15
                  OUT   (&H99),A
                  EI
                  EX    AF,AF
                  RET
          
          LINDAT: DB    0,0,0,0,0,0,0,0,0,0,0
          
          N.B. De routine zet de lijn nu automatisch in page 0. Wilt u 
          een andere page, zet die dan op (LINDAT+3).
          
          
                               T E N S L O T T E 
          
          De  source  staat  op disk  onder de  naam LINE.ASC.  Het is 
          natuurlijk ook mogelijk om een 16 bits routine te schrijven, 
          maar dat is nog een stuk moeilijker dan deze 8 bits routine. 
          Een  korte aflevering  dit keer,  maar naar ik hoop wel weer 
          een interessante. De volgende keer ga ik een aantal routines 
          voor sprites behandelen. Tot dan!
          
                                                          Stefan Boer
