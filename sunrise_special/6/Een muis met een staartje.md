               E E N   M U I S   M E T   E E N   S T A A R T J E


                                (I/O Interface 2)

          Inderdaad, dit  verhaal krijgt  een staartje.  Er is al eens
          een  tekst geschreven  over het aansturen (en vooral lezen!)
          van de mouse. Er zaten alleen wat nadelen aan de routine die
          toen werd  geschreven. Het werkte alleen in de Z80 stand, en
          was  nogal 'gevaarlijk'  geschreven, doordat er rechtstreeks
          in de  routines werd  ge'poked' voor  het uitlezen van zowel
          poort 1 als poort 2.

          In  deze  tekst  hoop ik  wat meer  duidelijkheid te  kunnen
          scheppen  in het  hele I/O gebeuren wat betreft de mouse, en
          een betere routine te leveren (die NIET gebruik maakt van de
          standaard MSX ROM BIOS!). Er wordt hier gebruikt gemaakt van
          de PSG  poorten, dus  raad ik  aan de tekst over het gebruik
          van  de joystick in ML (special #5) eerst (nog) eens door te
          lezen.


                              M E N   N E M E . .

          Om een  mouse te gebruiken moet er toch eerst worden gezocht
          of  er wel  een mouse aanwezig is. Deze kan aangesloten zijn
          in zowel  poort 1  als 2.  Om aan te geven in welke poort we
          willen  werken, moeten  we van PSG r#15 het 6e bit (lees ook
          artikel  over  joysticks)  aan  of uit  zetten. (0=poort  1,
          1=poort 2).

          Om aan  te geven  dat we  gebruik willen  gaan maken  van de
          mouse, moeten we (weer afhankelijk van welke poort) bit 4 of
          bit  5 van  r#15 zetten.  Deze bits zijn verbonden met de 8e
          pen  van   de  game-poorten  (deze  zitten,  zoals  bekend?,
          hardware matig aan de buitenkant van uw MSX).

          Als er aan uw MSX geen mouse hangt, maar een programma neemt
          aan van wel, en er wordt mouse data gelezen, bijvoorbeeld in
          en  tekenprogramma, dan  zal het  teken-pijltje langzaam met
          stappen  van  1  naar  de  rechter  onderhoek van  uw scherm
          glijden. Van dit proces kunnen we gebruik maken om te kijken
          of er wel of niet een mouse aanwezig is!

          Als we een aantal keren gaan kijken of de mouse offsets X en
          Y iedere  keer met  1 worden  verhoogt, dan zal er blijkbaar
          geen mouse aanwezig zijn. Als we dit 1 keer gaan testen, dan
          is  er het  gevaar dat  de gebruiker de mouse beweegt op het
          moment van  testen, en dat deze beweging precies overeenkomt
          met  het al eerder genoemde proces. Dus is het verstandig om
          meerdere malen  dit te  gaan testen.  Want het  is praktisch
          onmogelijk om precies de X en Y met 1 te laten verhogen voor
          meerdere malen achter elkaar!


                             Z 8 0   O F   R 8 0 0

          Om een mouse te lezen, moeten er vertragings routines worden
          gebruikt,  omdat de  mouse een  relatief traag apparaatje is
          ten opzichte van de CPU (Z80 of R800).
          Met andere  woorden: Er  moet gewacht worden op de mouse tot
          dat  deze klaar  is met het verzenden van data. Daar er geen
          'strobe' (klaar voor aktie signaal) wordt gegeven, moeten we
          gewoon de CPU een tijdje laten wachten.

          Hier stuiten  we op een probleem. We kunnen namelijk wachten
          door  middel van  interrupts of  gewoon een vertragings lus.
          Maar een interrupt duurt veel te lang voor het wachten op de
          muis  (1/50  of  1/60  seconde). Daarom  kiezen we  voor een
          vertagings lus.  Maar omdat  de Z80  op 3.5  MHz loopt en de
          R800  op 7,4  MHz, moeten we 2 verschillende routines maken.
          Tijdens het  installeren van  de mouse kunnen we hier gewoon
          even  kijken wat  voor CPU  er aan boord is en na aanleiding
          van dit een pointer 'poken'.

          Het feitelijk  uitlezen gaat via de PSG poorten. De data die
          gelezen  kan worden  zijn altijd  in afstanden (offsets) ten
          opzichte van  de laatst gelezen waarde. Dus de afstand dat u
          de  mouse beweegt. Door deze bij de vorige coordinaten op de
          tellen, hebt u de nieuwe coordinaten.
          De data  die wordt  gegeven zijn  slechts 4  bits, daar er 8
          nodig  zijn voor  het bereik  van 0-255  moet er  dus 2 keer
          gelezen worden om een 8 bits offset te krijgen. Dit kan door
          gewoon twee keer achter elkaar r#15 te lezen. De eerste keer
          worden de  4 hoogste  bits gegeven,  daarna de 4 laagste. De
          offsets worden in bits 0-3 geplaats. De mouse doet er echter
          langer  over om de 4 hoogste X-offset bits door te geven dan
          de 4  laagste, dus zal de eerste keer dat r#15 wordt gelezen
          langer  moeten worden  gewacht. De  Y offsets  worden altijd
          even  snel   doorgegeven.  Een   voorbeeld  van  twee  wacht
          routines:

          ; - Delay routs -
          ; Z80.
          ; In: B, delay length
          DL_Z80: DJNZ    DL_Z80
                  RET

          ; (R800)
          ; In: C, delay lenth
          DL_R80: IN      A,(&HE6)        ; turbo R timer
                  LD      B,A
          DR80.0: IN      A,(&HE6)
                  SUB     B
                  CP      C
                  JP      C,DR80.0
                  RET

          Bij  de MSX  tubo R wacht routine wordt gebruikt gemaakt van
          een  razend  snelle teller  die in  deze pracht  machine zit
          ingebouwd. Bij  de eerste  keer uitlezen van de mouse zullen
          de volgende vertragingen moeten worden gebruikt (minimaal):
                  Z80  -  B,40
                  R800 -  C,20

          De  volgende keren dat gelezen moet worden kan de vertraging
          korter zijn namelijk:
                  Z80  -  B,7
                  R800 -  C,6


                         P A D D L E   U I T L E Z E N

          De  volgende  data gebruiken  we bij  de mouse  acties. Deze
          staan origineel  ook in  het System  variabelen gebied,  dus
          waarom zouden we die niet gebruiken?

          ; Mouse vars.
          MOUSID: EQU     &HFAFD          ; ID code: 0=Niet aanwezig
          MSEOFS: EQU     &HFAFE          ; Mouse offsets X en Y
          MOUSXY: EQU     &HFB00          ; Mouse X en Y positie
          MSEPRT: EQU     &HFC82          ; Mouse Poort (0=niet,
          ;                                 &H10=poort 1, &H60=poort2)


          De  data die de padle geeft is ook nog een keer negatief dus
          als de mouse naar boven wordt bewogen, zal bijvoorbeeld de Y
          offset +20  zijn in  plaats van  -20. Zowel  de X  als de  Y
          offsets zijn negatief.
          Een  dergelijke routine  om de beide offsets te lezen kan er
          als volgt uit zien:

          ; Get paddle offset.
          ; In:  [MSEPRT], port nr. (see mouse vars.)
          ; Out: [MSEOFS]/HL, YYXX ofs. (A, Y ofs.)
          GETPAD: PUSH    BC
                  PUSH    DE

                  LD      DE,(MSEPRT)     ; E=Mouse poort.
                  LD      A,15            ; Lees PSG r#15
                  CALL    RD_PSG
                  AND     &B10001111      ; bewaar rest bits.
                  OR      E               ; Maak nr bit "1"
                  LD      E,A

          ; X offset
                  LD      B,40            ; Eerste vertraging Z80
                  LD      C,20            ; ,,     ,,         R800
                  CALL    GPAD.2          ; MSB
                  CALL    GPAD.0          ; LSB
                  LD      L,A             ; L=X offset

          ; Y offset
                  CALL    GPAD.1          ; MSB
                  CALL    GPAD.0          ; LSB
                  LD      H,A             ; H=Y offset
                  LD      (MSEOFS),HL     ; bewaar beide offsets

                  EI
                  POP     DE
                  POP     BC
                  RET

          ; Shift bits (LSB -> MSB), read LSB and make offset neg.
          GPAD.0: RLCA
                  RLCA
                  RLCA
                  RLCA
                  AND     &HF0
                  LD      D,A
                  CALL    GPAD.1          ; Lees LSB
                  OR      D               ; Voeg MSB hier aan toe
                  NEG                     ; Maak offset negatief.
                  RET

          ; Read paddle data
          ; In:  [MSEPRT], mouse port (see mouse vars.)
          ;      (B/C, delay first time -> only in GPAD.2)
          ; Out: A, offset in LSB
          GPAD.1: LD      BC,&H0706       ; Vertraging variables
          GPAD.2: LD      A,15            ; Schrijf naar r#15
                  CALL    WR_PSG
                  LD      A,(MSEPRT)
                  AND     &H30            ; Alleen mouse bits
                  XOR     E
                  LD      E,A
          GP_DL#: CALL    xxxx            ; Hier wordt de vertragings
                                            pointer gepoked tijdens
                                            de mouse installatie
                                            (Z80 of R800)
                  LD      A,14            ; Lees r#14 (offset data)
                  CALL    RD_PSG
                  AND     &H0F            ; alleen LSB
                  RET

          Routines  voor het  schrijven of  lezen van de PSG kunnen er
          als volgt uit zien:

          ; Read PSG.
          ; In:  A, port nr
          ; Out: A, data
          RD_PSG: DI
                  OUT     (&HA0),A
                  IN      A,(&HA2)
                  EI
                  RET

           ; Write to PSG.
           ; In:  A, port nr. E, port data
          WR_PSG: DI
                  OUT     (&HA0),A
                  LD      A,E
                  OUT     (&HA1),A
                  EI
                  RET

          Deze laatste twee routines zijn feitelijk het zelfde als die
          in het  MSX ROM  BIOS aanwezig  zijn, maar aangezien ik hier
          (liever)   geen  gebruik  van  maak  heb  ik  ze  hier  maar
          bijgevoegd.

          Nu hebben  we dus  een routine die de paddle data kan lezen.
          Om  het vertragins  pointer adres  te poken, moet er nog een
          mouse installatie  routine worden  geschreven. Deze moet dus
          kijken  of we  met een  Z80 of een R800 CPU te maken hebben.
          Door het  adres &h2D  uit te  lezen weten  we of  we met een
          turbo  R computer  of een MSX 1/2/2+ te maken hebben. Indien
          het om  een MSX  turbo R gaat moet er ook nog gekeken worden
          in  welke CPU  mode hij  (of is  het een zij?) op dit moment
          staat.

          ; Install Mouse
          MOUSIN: LD    A,255
                  LD    (MOUSID),A
                  LD    HL,DL_Z80         ; vertraging pointer Z80
                  LD    A,(&H2D)          ; turbo R ?
                  CP    3                 ; nee..
                  JP    C,MSIN.0
                  CALL  &H0183            ; R800 aan?
                  AND   A
                  JP    Z,MSIN.0          ; nee..
                  LD    HL,DL_R80         ; ja, turbo R wacht routine
          MSIN.0: LD    (GPW_#A+1),HL
                  RET

          Om te  kijken in  welke CPU  mode de turbo R staat heb ik nu
          wel  voor de ROM routine gekozen. Om de CPU te wisselen moet
          er namelijk  van alles  worden bewaard  zoals de register en
          stack  pointer etc.  Om de CPU te lezen hoeft dit niet, maar
          aangezien ik de CPU switch routine ook niet heb herschreven,
          leek mij het hier ook niet echt nootzakelijk.


                          M O U S E   D E T E C T I E

          Nu zijn  alle routines  klaar voor  het lezen  van de paddle
          waarde.   Deze  routines   kunnen  we   nu  gebruiken   voor
          bijvoorbeeld het  controleren of de mouse aanwezig is in een
          game-poort. De volgende routine lost dit voor ons op:

          ; Check mouse.
          ; Out: [MOUSEID]: 0=not found, 255=found and installed.
          ;      Cy: 0=found, 1=not found.
          CHMOUS: CALL  MOUSIN            ; instaleer wacht lus.
                  LD    A,&H10            ; probeer eerst poort 1...
                  LD    (MSEPRT),A
                  CALL  CHMS.0
                  JP    NC,MOUSIN         ; gevonden.

                  LD    A,&H60            ; niet, dan poort 2
                  LD    (MSEPRT),A
                  CALL  CHMS.0
                  JP    NC,MOUSIN         ; gevonden.

                  XOR   A                 ; niet gevonden.
                  LD    (MOUSID),A
                  LD    (MSEPRT),A
                  SCF                     ; Cy=1
                  RET

          CHMS.0: LD    B,40              ; kontroleer 40 x
          CHMS.1: PUSH  BC
                  CALL  G_PADL            ; Y-offset
                  CP    1                 ; stap+1?
                  JP    NZ,CHMS.2         ; nee, dus mouse aanwezig
                  LD    A,L               ; X-offset
                  CP    1                 ; stap+1?
                  JP    NZ,CHMS.2         ; nee, dus mouse aanwezig
                  POP   BC                ; nog een keer..
                  DJNZ  CHMS.1
                  SCF                     ; Cy:1 (niet gevonden)
                  RET
          CHMS.2: POP   BC                ; gevonden
                  AND   A                 ; Cy:0
                  RET

          Na  het aanroepen  van deze  routine kan er op drie manieren
          worden gecontroleerd  of de  mouse al  dan niet aanwezig is.
          Als  de carry  (Cy) hoog is, zal deze niet zijn gevonden. De
          variable MOUSID  zal "0"  zijn als de mouse niet aanwezig is
          (255  is wel  aanwezig). Ook  kan de  variable MSEPRT worden
          gelezen. Is  deze "0"  dan is de mouse niet gevonden, anders
          bevat deze de mouse poort nummer data.
          Meer   is  eigenlijk  niet  nodig,  om  de  X/Y  positie  te
          verkrijgen kunnen  gewoon de X en Y offsets bij de originele
          coordinaten  worden op  geteld. Een routine die dat ook voor
          ons regelt:

          ; Read any mouse, and set X,Y pos.
          ; Out:  [MOUSXY]/HL, XXYY position.
          ;       Cy: 1, mouse not found
          RDMOUS: LD      HL,0
                  LD      (MSEOFS),HL
                  LD      A,(MOUSID)      ; Controleer of er een mouse
                  AND     A               ; aanwezig is.
                  SCF                     ; Zet carry flag.
                  RET     Z

                  CALL    GETPAD          ; Lees paddle
                  LD      A,(MOUSXY)      ; Originele X positie.
                  ADD     A,L             ; + X offset
                  LD      L,A
                  LD      (MOUSXY),A
                  LD      A,(MOUSXY+1)    ; Originele Y positie.
                  ADD     A,H             ; + Y offset
                  LD      H,A
                  LD      (MOUSXY+1),A
                  AND     A               ; Wis carry flag
                  RET

          Met deze  routines moet het mogelijk zijn om 100% gebruik te
          kunnen  maken  van  de  mouse.  De  twee  vuurknoppen worden
          natuurlijk  op de zelfde manier gelezen als de joystick (zie
          ook I/O interface deel 1, RDTRG1 / RDTRG2).

          Ik stimuleer  het enorm  om in  software pakketten  de mouse
          zoveel  mogelijk te kunnen gebruiken. Zowel in utilities als
          in demos! (zoals bijvoorbeeld in de MB muzax reeks). Want de
          mouse is  toch een  prima stukje  hardware voor interactieve
          doeleinden.  In screen  0 kan ook uitstekend de mouse worden
          gebruikt. Deel  gewoon de  X en Y coordinaten door 8 voor de
          juiste posities.

                                                  R�man van der Meulen
