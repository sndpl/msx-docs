                 M S X 2 / 2 +   V D P   C U R S U S   ( 1 2  )
                                                                
          
          Welkom bij  het twaalfde deel van de inmiddels legendarische 
          VDP  cursus.  Ook  deze  keer  gaan we  weer een  ML routine 
          bespreken.  Dit keer  is het  een routine  om tekst  met een 
          grafische karakterset  op het  scherm te zetten in SCREEN 5. 
          De  theorie die  hiervoor nodig  is is  allemaal al  aan bod 
          geweest, dus we beginnen meteen met de source.
          
          In dit  voorbeeld gebruiken we een 6x8 karakterset, maar het 
          is  heel  eenvoudig  om  de  routine  aan  te  passen  zodat 
          karakters  van andere  formaten worden  gebruikt. Ik geef de 
          routine hier in de BASIC uitvoering, maar hij kan natuurlijk 
          ook gewoon in een puur ML programma worden gebruikt.
          
          
          ; P R T E X T . A S M 
          ; Zet tekst op scherm in SCREEN 5 met COPY (font in page 3)
          ; Aanroepen door coordinaten in te POKEn:
          ; POKE &HD003,X : POKE &HD004,Y
          ; en daarna U$=USR("tekst")
          ; Door Stefan Boer, (c) Ectry 1993
          ; VDP Cursus Sunrise Magazine #7
          
                  ORG   &HD000
          
                  JP    PRTEXT
          XCOOR:  DB    0
          YCOOR:  DB    0
          
          
          Dit  ziet er  misschien een  beetje vreemd  uit, maar dit is 
          gedaan om het aanroepadres en de adressen van de co�rdinaten 
          die moeten worden ingePOKEt standaard te houden.
          
          
          PRTEXT: LD    A,(DE)
                  LD    B,A
                  INC   DE
                  LD    A,(DE)
                  LD    L,A
                  INC   DE
                  LD    A,(DE)
                  LD    D,A
                  LD    E,L             ; DE=startadres, B=lengte
          
          
          Als je een ML routine vanuit BASIC aanroept met
          
                  U$= USR(string)
          
          dan  staat in  het DE  register het  begin van de zogenaamde 
          "string desriptor".  Deze string descriptor bestaat uit drie 
          bytes.  De eerste  is de lengte van de twee strings, de twee 
          andere bytes  geven het  beginadres in  het geheugen  staan. 
          Hier zetten wij het beginadres in DE en de lengte in B.
          
          
                  LD    A,(XCOOR)
                  LD    (COPDAT+4),A    ; DX invullen
                  LD    A,(YCOOR)
                  LD    (COPDAT+6),A    ; DY invullen
          
          
          Hier  worden   de  ingePOKEte  co�rdinaten  in  de  copydata 
          ingevuld.
          
          
          PRINT:  PUSH  BC              ; bewaar aantal karakters
                  LD    A,(DE)          ; haal teken
                  INC   DE              ; DE wijst naar volgende teken
                  SUB   32              ; eerste karakter is spatie
                  ADD   A,A             ; A=A*2
                  LD    C,A
                  LD    B,0             ; LD BC,A
                  LD    HL,CORTAB       ; startadres coordinatentabel
                  ADD   HL,BC           ; bereken adres in tabel
          
          
          We maken  gebruik van  een co�rdinatentabel omdat de routine 
          dan  met elke  willekeurige grafische karakterset kan worden 
          gebruikt. We  trekken 32  van de ASCII code van het teken af 
          en  vermenigvuldigen dat  met twee. Vervolgens tellen we dit 
          bij het startadres van de tabel op en we hebben het adres in 
          de tabel  waar de  co�rdinaten van  het desbetreffende teken 
          staan.
          
          
                  LD    A,(HL)
                  LD    (COPDAT),A      ; SX invullen
                  INC   HL
                  LD    A,(HL)
                  LD    (COPDAT+2),A    ; SY invullen
          
          
          Die  co�rdinaten worden  hier uit  de tabel gehaald en in de 
          copydata gezet.  Zoals we gewend zijn werken we weer met een 
          blokje copydata van 15 bytes. De benodigde gegevens (source, 
          destination,  etc.) kunnen  dan zonodig  door het  programma 
          worden veranderd  en het  uitvoeren van het commando door de 
          VDP is een kwestie van een simpele OTIR.
          
          
                  LD    HL,COPDAT
                  LD    BC,&H0F9B
          WACHT:  LD    A,2
                  CALL  RDSTAT          ; lees statusregister
                  BIT   0,A
                  JR    NZ,WACHT
          
          
          Eerst wachten we netjes totdat de VDP klaar is door op bit 0 
          van S#2  te testen.  Doen we  dit niet,  dan kan dat vreemde 
          effecten  hebben. Zeker  als de routine op een turbo R onder 
          R800 stand  wordt gedraaid,  omdat de kans dan groter is dat 
          de  VDP nog  niet klaar  is met  het kopi�ren  van de vorige 
          letter  als  hij hier  is aangeland.  De data  voor de  OTIR 
          (HL=beginadres,  B=aantal  bytes,  C=I/O  poort)  zetten  we 
          alvast klaar.
          
          
                  DI
                  LD    A,32
                  OUT   (&H99),A
                  LD    A,17+128
                  OUT   (&H99),A
                  EI
                  OTIR                  ; voer copy uit
          
          
          Hier schrijven we de waarde 32 naar R#17, hiermee selecteren 
          we  het startregister  voor het indirect registers schrijven 
          van de  VDP. Met  de OTIR wordt het blokje met copydata naar 
          Port  #3 gestuurd, waardoor deze bytes netjes in de commando 
          registers (32 t/m 46) terecht komen en de VDP de letter naar 
          het scherm zal kopi�ren.
          
          
                  LD    A,(COPDAT+4)    ; oude DX
                  ADD   A,6             ; volgende positie
                  LD    (COPDAT+4),A    ; nieuwe DX invullen
          NXTKAR: POP   BC
                  DJNZ  PRINT
                  RET
          
          
          Hier  wordt  er 6  bij de  x co�rdinaat  opgeteld, zodat  de 
          volgende  letter  netjes naast  deze komt  te staan.  De lus 
          wordt afgesloten met een DJNZ.
          
          
          ; Statusregister lezen. In: A=register, Uit: A=data
          
          RDSTAT: DI
                  OUT   (&H99),A
                  LD    A,128+15
                  OUT   (&H99),A        ; selecteer statusregister
                  NOP                   ; geef VDP tijd om data
                  NOP                   ; klaar te zetten
                  IN    A,(&H99)
                  EX    AF,AF
                  XOR   A
                  OUT   (&H99),A
                  LD    A,128+15
                  OUT   (&H99),A        ; selecteer S#0
                  EI
                  EX    AF,AF
                  RET
          
          
          Dit  is  een  standaardroutine  voor  het  uitlezen  van een 
          statusregister,  dit zou  u inmiddels  zonder verdere uitleg 
          moeten kunnen begrijpen.
          
          
          COPDAT: DB    0,0,0,3         ; SX en SY (page 3)
                  DB    0,0,0,0         ; DX en DY (page 0)
                  DB    5,0,7,0         ; NX en NY
                  DB    0,0             ; CLR en ARG
                  DB    &B10010000      ; LMMM
          
          
          Dit  is het  blok met  copy data.  De pages,  grootte en het 
          commando zijn  al ingevuld,  door de routine wordt de source 
          (de letter op page 3) en de destination (waar komt de letter 
          op  page 0)  ingevuld. &B10010000  staat voor  LMMM, Logical 
          Move VRAM to VRAM.
          
          Tot  slot  de  co�rdinatentabel. Dit  ziet er  misschien een 
          beetje  lang uit, maar het is met een BASIC programmaatje in 
          een wip  aangemaakt. Deze  tabel kunt  u aanpassen voor elke 
          gewenste karakterset.
          
          
          ; Coordinaten van ASCII 32 t/m ASCII 93
          
          CORTAB: DB    0,197           ;
                  DB    6,197           ; !
                  DB    12,197          ; "
                  DB    18,197          ; #
                  DB    24,197          ; $
                  DB    30,197          ; %
                  DB    36,197          ; &
                  DB    42,197          ; '
                  DB    48,197          ; (
                  DB    54,197          ; )
                  DB    60,197          ; *
                  DB    66,197          ; +
                  DB    72,197          ; ,
                  DB    78,197          ; -
                  DB    84,197          ; .
                  DB    90,197          ; /
                  DB    96,197          ; 0
                  DB    102,197         ; 1
                  DB    108,197         ; 2
                  DB    114,197         ; 3
                  DB    120,197         ; 4
                  DB    126,197         ; 5
                  DB    132,197         ; 6
                  DB    138,197         ; 7
                  DB    144,197         ; 8
                  DB    150,197         ; 9
                  DB    156,197         ; :
                  DB    162,197         ; ;
                  DB    168,197         ; <
                  DB    174,197         ; =
                  DB    180,197         ; >
                  DB    186,197         ; ?
                  DB    192,197         ; @
                  DB    0,205           ; A
                  DB    6,205           ; B
                  DB    12,205          ; C
                  DB    18,205          ; D
                  DB    24,205          ; E
                  DB    30,205          ; F
                  DB    36,205          ; G
                  DB    42,205          ; H
                  DB    48,205          ; I
                  DB    54,205          ; J
                  DB    60,205          ; K
                  DB    66,205          ; L
                  DB    72,205          ; M
                  DB    78,205          ; N
                  DB    84,205          ; O
                  DB    90,205          ; P
                  DB    96,205          ; Q
                  DB    102,205         ; R
                  DB    108,205         ; S
                  DB    114,205         ; T
                  DB    120,205         ; U
                  DB    126,205         ; V
                  DB    132,205         ; W
                  DB    138,205         ; X
                  DB    144,205         ; Y
                  DB    150,205         ; Z
                  DB    156,205         ; [
                  DB    162,205         ; \
                  DB    168,205         ; ]
          
          
                               V O O R B E E L D 
          
          Zo, dat  was de  ML routine. Niet echt moeilijk denk ik. Hij 
          staat  op de  disk onder  de naam  PRTEXT.ASC. Om het uit te 
          testen gebruiken we het volgende BASIC programma:
          
          100 ' PRTEXT voorbeeld
          110 ' Door Stefan Boer, 20/03/93
          120 ' (c) Ectry 1993
          130 ' Sunrise Magazine #7 VDP cursus
          140 '
          150 SCREEN5:COLOR15,0,0:CLS
          160 CLEAR200,&HCFFF:DEFINTA-Z:BLOAD"PRTEXT.BIN":
              DEFUSR=&HD000
          170 OPEN"GRP:"AS1:SETPAGE,3:CLS:A=32:FORI=0TO192STEP6:
              PRESET(I,197):PRINT#1,CHR$(A):A=A+1:NEXT:
              FORI=0TO168STEP6:PRESET(I,205):PRINT#1,CHR$(A):A=A+1:
              NEXT:SETPAGE,0
          180 FORI=50TO150STEP20:READT$:POKE&HD003,128-3*LEN(T$):
              POKE&HD004,I:U$=USR(T$):NEXT
          190 IFSTRIG(0)=0THEN190ELSEEND
          200 DATA "PRTEXT"
          210 DATA " "
          220 DATA "(C) ECTRY 1993"
          230 DATA " "
          240 DATA "VDP CURSUS"
          250 DATA "SUNRISE MAGAZINE #7"
          
          Dit  staat  ook  op  de disk  onder de  naam PRTEXT.BAS.  De 
          geassembleerde ML  file staat  er ook  op: PRTEXT.BIN. Samen 
          staan deze files in PRTEXT.PMA.
          
          Dit  voorbeeld   is  niet  bepaald  spectaculair,  omdat  de 
          standaardkarakterset wordt gebruikt. Wel valt het op dat dit 
          een  stuk sneller  is dan  OPEN"GRP:"AS1/PRINT#1. Het  grote 
          voordeel  hiervan  is  dat  u elk  gewenste formaat  letters 
          (zelfs  proportioneel  als  u  de routine  een klein  beetje 
          aanpast!) kunt gebruiken, ook met meerdere kleuren!
          
          
                               T E N S L O T T E 
          
          Een  beetje  korte  tekst  dit  keer,  maar dat  hoop ik  de 
          volgende keer goed te maken met een extra lange routine.
          
          Dit  is een  leuk voorbeeld  om uit te breiden, ik noemde al 
          een andere  (grotere) karakterset  of proportioneel schrift. 
          Zet  daarvoor  achter  de  co�rdinaten  in de  tabel ook  de 
          breedte van de letter, en tel dat getal op bij de oude DX.
          
          Veel plezier en tot de volgende keer!
          
                                                          Stefan Boer
