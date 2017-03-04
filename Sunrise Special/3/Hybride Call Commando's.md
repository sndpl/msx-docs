                        - HYBRIDE -
          
                                C A L L   - 1 - 
                                                 
          
          In de  serie Hybride  artikelen hebben we het deze keer over 
          CALL   commando's.  Het  CALL  commando  is  aan  MSX  BASIC 
          toegevoegd om  het mogelijk  te maken uw eigen commando's of 
          toekomstige uitbreidingen te gebruiken.
          
          De syntax luidt:
          
          CALL <naam uitbreidingsbevel> [ (parameter,parameter ...) ]
          
          In plaats van CALL mag ook het onderstrepingsteken _  worden
          gebruikt.
          
          Kennis van machinetaal is vereist voor het  zelf  maken  van
          een CALL  commando.  BASIC  programmeurs  kunnen  de  nieuwe
          commando's natuurlijk wel zonder problemen gebruiken in  hun
          eigen programma's.
          
          Voordat we ingaan op het maken van een CALL  commando  wordt
          er eerst enige achtergrondinformatie gegeven.
          
          
                      U I T B R E I D I N G S P A G I N A 
          
          Een pagina is een stuk geheugen (ROM of RAM) van 16  kB.  De
          pagina's zijn genummerd van 0 t/m 3.
          
          Page:    Adresbereik:
          0        &H0000-&H3FFF
          1        &H4000-&H7FFF
          2        &H8000-&HBFFF
          3        &HC000-&HFFFF
          
          De MSX  computer heeft  standaard pagina's met RAM, MAIN ROM 
          en  Disk ROM.  Daar kunnen  echter door de fabrikant (in ROM 
          vorm) of door de gebruiker (in het RAM) uitbreidingspagina's 
          aan worden toegevoegd.
          
          Een uitbreidingspagina  kan  een  machinetaalprogramma,  een
          apparaatuitbreiding, een  BASIC-uitbreiding  of  een  BASIC-
          programma bevatten. De informatie daarover is opgeslagen  in
          SLTATR. SLTATR is een gedeelte van  het  systeemgebied,  dat
          begint op adres &HFCC9. Er  is  ÇÇn  byte  gereserveerd  per
          mogelijke pagina. Er zijn 4 primaire sloten mogelijk die elk
          4 secundaire sloten kunnen hebben. Elk slot bestaat weer uit
          4 pages, zodat SLTATR 4*4*4=64 bytes lang is.  De  bits  van
          de bytes in SLTATR hebben de volgende betekenis:
          
          Bit 7    BASIC programma (1=ja)
          Bit 6    Apparaat uitbreiding (1=ja)
          Bit 5    Statement uitbreiding (1=ja) (CALL commando)
          Bit 4-0  Ongebruikt
          
          U kunt het adres dat bij een bepaalde pagina hoort berekenen
          met de volgende formule:
          
          ADRES = &HFCC9 + 16 * primaire gleufnummer + 4 *  secundaire
          gleufnummer + paginanummer
          
          Voorbeeld: in uw computer zit het RAM in slot 3-2 en u heeft
          een statement uitbreiding in page 1 gemaakt. Het adres wordt
          dan:
          
          &HFCC9 + 16 * 3 + 4 * 2 + 1 = &HFD02
          
          Bit 5 staat voor een statement uitbreiding, dus de waarde 32 
          (2^5) moet naar adres &HFD02 worden geschreven: POKE &HFD02, 
          32. De  computer zal nu naar page 1 in slot 3-2 springen als 
          er een CALL commando wordt gegeven.
          
          
                                  H E A D E R 
          
          De eerste  16 bytes  van een uitbreidingspagina worden samen 
          ook wel de header genoemd en zijn als volgt opgebouwd:
          
          Offset:  Naam:               Bevat:
          ------------------------------------------------------------
          +0       ID                  "AB"
          +2       INIT                beginadr initialisatie
          +4       STATEMENT           beginadr statement uitbreiding
          +6       DEVICE              beginadr apparaat uitbreiding
          +8       TEXT                beginadr BASIC programma
          +10      -                   6 bytes gereserveerd
          ------------------------------------------------------------
          
          Aan het  begin van  een pagina moeten de letters "AB" (ASCII 
          &H41   en  &H42)  staan.  De  computer  herkent  daaraan  de 
          uitbreidingspagina.  Bij  een  RESET  zal  de  computer zelf 
          kijken wat  er in de pagina's staat en SLTATR overeenkomstig 
          invullen.  Als u een CALL commando wilt gebruiken zonder een 
          RESET, dan  zult u zelf in SLTATR moeten POKEn, zoals we bij 
          de uitleg van SLTATR al hebben gezien.
          
          
                   S T A T E M E N T   U I T B R E I D I N G 
          
          Wij  interesseren  ons   nu   alleen   voor   de   statement
          uitbreiding. Als er geen statement uitbreiding in de  pagina
          aanwezig is, moet STATEMENT gelijk zijn aan  &H0000.  Alleen
          page 1 kan een statement  uitbreiding  bevatten.  Er  kunnen
          problemen  ontstaan  als  bij  andere  pagina's   het   veld
          STATEMENT is ingevuld.
          
          Het is overigens logisch dat  de  statement  uitbreiding  in
          page 1 moet staan. In page 0 staat de BIOS en in page 3  het
          systeemgebied, dus  die  moeten  ingeschakeld  blijven.  Het
          BASIC programma staat in page 2 (als het een groot programma
          is tot in page 3), dus page 1 is de enige  mogelijkheid  die
          overblijft.
          
          Als  de computer  bij het  uitvoeren van een BASIC programma 
          een CALL  commando tegenkomt, wordt de naam van het commando 
          (het  woord dat  achter CALL  staat) in PROCNM gezet. PROCNM 
          ligt  in  het  systeem  gebied,  vanaf  &HFD89. Alle  kleine 
          letters worden  omgezet in hoofdletters en de komma's worden 
          eruit  gehaald  (probeert  u  maar  eens:  _F,O,R,M,A,T  het 
          werkt!).  De naam  mag 15  tekens lang  zijn. Achter de naam 
          wordt een  ASCII-teken 0 gezet. Daarna gaat de computer alle 
          pagina's  langs die volgens SLTATR een statement uitbreiding 
          bevatten.
          
          Als de computer bij STATEMENT een waarde vindt die  ongelijk
          is aan &H0000, dan springt hij naar dat  adres  en  laat  de
          besturing verder aan  die  routine  over.  De  taak  van  de
          routine is nu om te controleren of het het juiste  statement
          is.
          
          HL wijst naar het eerste teken  na  de  naam  van  het  CALL
          statement in het tokenized  BASIC.  Spaties  worden  daarbij
          niet meegeteld. Bij bijvoorbeeld CALL FORMAT  wijst  HL  dus
          naar de positie NA de T van FORMAT.
          
          Als  de routine  ziet dat  het de  juiste naam  is, moet het 
          statement  worden  uitgevoerd  en  moet  de  routine  worden 
          verlaten met  de carry  gereset (gelijk  aan 0 dus). HL moet 
          naar  het eerste teken na het commando wijzen (het einde van 
          de regel of een dubbele punt). Bij een CALL statement zonder 
          parameters  is   dat  dus  de  oude  situatie  (bijvoorbeeld 
          _FORMAT),  bij _COLOR(15,0)  moet HL  wijzen naar de positie 
          achter het tweede haakje.
          
          Is het een ander commando,  dan  moet  de  routine  verlaten
          worden met de carry  geset  (hoog)  en  HL  onveranderd.  De
          andere registers mogen wel zijn veranderd.
          
          Samenvatting:
          
          - CALL statement wordt niet gevonden: verlaat routine met HL 
          onveranderd en carry hoog
          -  CALL statement  is gevonden:  voer CALL  statement uit en 
          verlaat de  routine met  HL op  de positie achter het gehele 
          CALL statement en de carry laag
          
          
                       S T A N D A A R D   R O U T I N E 
          
          Om  het  allemaal  wat  makkelijker  te  maken  heb  ik  een 
          standaardroutine geschreven, die als CALL.ASC op de diskette 
          staat.  U  kunt  deze  routine  gebruiken  voor  al  uw CALL 
          statements.  De  routine  kan  ÇÇn,  maar  ook twee  of meer 
          routines  aansturen.  Als al  het RAM  in ÇÇn  slot zit  (is 
          meestal  zo),  dan  kunt  u  maar  ÇÇn blok  CALL commando's 
          tegelijk  gebruiken.  Er  is  dan namelijk  maar ÇÇn  page 1 
          beschikbaar, en de statement uitbreiding moet altijd in page 
          1 staan.
          
          Achter de  standaardroutine staan  drie voorbeelden van CALL 
          commando's,  die  u  bij het  maken van  uw eigen  blok CALL 
          commando's  natuurlijk  kunt  weglaten.  Ik ga  deze routine 
          zometeen uitgebreid bespreken. Maar eerst even dit.
          
          
                   N A A R   D I S K   A S S E M B L E R E N 
          
          Het is  het makkelijkst als het blok CALL commando's met een 
          BLOAD,R kan worden geãnitialiseerd. De CALL code moet echter 
          op  &H4000 staan,  en daar kunnen we niet laden onder BASIC. 
          We  moeten   daarom  twee   ORGs  gebruiken,  ÇÇn  voor  het 
          intialisatieprogramma  en  ÇÇn  voor  de  CALL code.  Om het 
          allemaal  toch netjes  in ÇÇn .BIN file te krijgen moeten we 
          naar disk assembleren.
          
          Ik  ga  hier  uit van  WB-ASS2. GEN80  gebruikers zullen  de 
          source  dus enigszins  moeten aanpassen.  Je kunt  met GEN80 
          trouwens alleen  maar naar disk assembleren. Bij WB-ASS2 kan 
          dit  door de vragen die het ASM statement stelt als volgt te 
          beantwoorden:
          
          Listing afdrukken? Nee          (Ja mag natuurlijk ook)
          Ascii listing naar disk? Nee
          Mcode in geheugen zetten? Nee
          Mcode naar disk? Ja
          Relocatie-tabel maken? Nee
          
          Vervolgens   moet   nog   de   filenaam   worden   ingetypt, 
          bijvoorbeeld  CALL.BIN.  WB-ASS2  gaat  aan  de slag  en als 
          resultaat  staat de  file CALL.BIN  op de  diskette, die met 
          BLOAD"CALL.BIN",R kan worden gestart.
          
          (Nvdr. De  tekst was  langer dan  16 kB,  u kunt  het tweede 
          gedeelte in het submenu vinden.)
          
          
          
                                  - HYBRIDE -
          
                                C A L L   - 2 - 
                                                 
          
          
          Het is nu hoog tijd voor:
          
                               D E   S O U R C E 
          
          ; CALL.ASC
          ; Standaardroutine voor het maken van CALL commando's
          ; Door Stefan Boer
          ; (c) Ectry 1993
          ; Sunrise Special #3
          ; (c) Stichting Sunrise 1993
          
          ; (Alleen) naar disk assembleren!
          ; Resultaat starten met BLOAD"...",R
          
          ; Header voor .BIN file
          
                  DEFB  &HFE
                  DEFW  EXEC
                  DEFW  ENCALL-&H4001+STCALL
                  DEFW  EXEC
          
          
          Omdat  we  rechtstreeks een  file aanmaken,  moeten we  zelf 
          zorgen voor de juiste header. Een BLOAD,R file moet beginnen 
          met een  header van  7 bytes. De eerste byte is altijd &HFE, 
          hieraan  kan een  .BIN file  worden herkend.  Vervolgens 3*2 
          bytes  beginadres,  eindadres en  startadres. ENCALL  is het 
          eindadres+1  van  de CALL  code die  vanaf &H4000  staat. De 
          lengte van  dat blok  CALL code is dus ENCALL-&H4001. Om het 
          eindadres  te berekenen  wordt daar nog het eindadres van de 
          initialisatie routine bij opgeteld.
          
          ; Dit startadres moet eventueel omlaag als CALL code lang is
          
                  ORG   &HC000
          
          ; Initialisatieroutine
          ; Kopieer CALL code naar &H4000
          ; En zet bitje in SLTATR
          
          EXEC:   LD    A,(&HF342)      ; slot RAM page 1
                  LD    H,&H40          ; PAGE 1
                  CALL  &H24            ; BIOS routine ENASLT
                  LD    HL,STCALL
                  LD    DE,&H4000
                  LD    BC,ENCALL-&H4000
                  LDIR                  ; verplaats CALL code
                  LD    A,(&HFCC0)      ; slot MAIN ROM
                  LD    H,&H40
                  CALL  &H24
          
          
          Hier wordt er RAM geselecteerd op adres &H4000, de CALL code 
          wordt  gekopieerd en vervolgens wordt de MAIN ROM weer terug 
          gezet op adres &H4000.
          
          
                  LD    HL,&HFCCA       ; &HFCC9 + pagenummer (=1)
                  LD    BC,&H10
                  LD    A,(&HF342)      ; slot RAM page 1
                  AND   &B00000011      ; primair slotnummer
          CLUS1:  JR    Z,CNEXT
                  ADD   HL,BC           ; tel primair*16 erbij op
                  DEC   A
                  JR    CLUS1
          CNEXT:  LD    BC,4
                  LD    A,(&HF342)      ; slot RAM page 1
                  AND   A               ; zet vlaggen goed
                  JP    P,CEINDE        ; is er wel secundair slot
                  AND   &B00001100      ; secundaire gleuf * 4
                  RRCA                  ; secundaire gleuf * 2
                  RRCA                  ; secundaire gleuf
          CLUS2:  JR    Z,CEINDE
                  ADD   HL,BC           ; tel secundair*4 erbij op
                  DEC   A
                  JR    CLUS2
          CEINDE: LD    (HL),&H20       ; zet bit 5 aan
                  RET                   ; einde initialisatie
          
          
          Hier wordt het juiste adres in SLTATR uitgerekend, en op dat 
          adres wordt de waarde &H20 gezet. Hieraan kan de interpreter 
          zien dat er een statement uitbreiding in die page zit.
          
          
          STCALL:                       ; beginadres van code IN FILE
          
          
          Als de BLOAD,R file is ingeladen, staat de CALL code niet op 
          adres &H4000 maar direct achter de RET van CEINDE. We hebben 
          dat adres  nodig om de CALL code te kunnen verplaatsen en om 
          het  eindadres  uit  te kunnen  rekenen. Vandaar  dit "loze" 
          label.
          
          
          ; CALL code
          
                  ORG   &H4000          ; hier begint CALL code
          
          ; CALL header
          
                  DEFM  "AB"
                  DEFW  0               ; geen initialisatie
                  DEFW  CAL             ; startadres CALL code
                  DEFW  0               ; geen device
                  DEFW  0               ; geen BASIC
                  DEFW  0
                  DEFW  0
                  DEFW  0
          
          
          ; Hier springt interpreter naar toe bij CALL commando
          
          CAL:    PUSH  HL              ; HL wijst naar teken na naam
                                          van CALL statement
          
          ; Kijk of naam van CALL statement overeenkomt met
          ; een van de namen in de tabel
          
                  LD    DE,TABEL        ; begin van de tabel
          C_1:    LD    HL,&HFD89       ; PROCNM bevat naam
                  LD    A,(DE)          ; lengte van naam
                  AND   A               ; zet vlaggen goed
                  JR    Z,C_ERR         ; einde tabel als A=0
                  LD    B,A             ; lengte
                  INC   DE
          
          C_2:    LD    A,(DE)          ; vergelijk naam in tabel
                  CP    (HL)            ; met naam in PROCNM
                  JR    NZ,NEXT1        ; naar NEXT1 als onjuist
                  INC   DE              ; volgende letter
                  INC   HL
                  DJNZ  C_2
          
                  LD    A,(HL)          ; laatste teken moet nu
                  AND   A               ; gelijk zijn aan 0, anders
                  JR    NZ,NEXT2        ; is CALL naam te lang
          
          
          Dit laatste stukje code is nodig om ervoor te zorgen dat het 
          verschil wordt  gezien tussen  bijvoorbeeld CALL SUN en CALL 
          SUNRISE.
          
          
          ; alles Ok, spring naar CALL routine
          
                  EX    DE,HL           ; HL is plaats in tabel
                  LD    E,(HL)          ; low byte sprongadres
                  INC   HL
                  LD    D,(HL)          ; high byte sprongadres
                  EX    DE,HL
                  JP    (HL)            ; spring naar dat adres
          
          ; Verkeerde naam, ga naar volgende naam in tabel
          
          NEXT1:  LD    C,B
                  LD    B,0             ; BC=B
                  INC   BC
                  INC   BC
                  EX    DE,HL
                  ADD   HL,BC           ; sla rest naam + sprongadres
                  EX    DE,HL           ; over
                  JR    C_1             ; probeer het nog eens
          
          ; Naam te lang, ga naar volgende naam in tabel
          
          NEXT2:  INC   DE              ; sla het sprongadres
                  INC   DE              ; over
                  JR    C_1             ; probeer het nog eens
          
          ; Niet gevonden, einde routine met carry hoog
          ; Als in andere sloten ook geen kloppend CALL commando
          ; wordt gevonden, zal BASIC foutmelding geven
          
          C_ERR:  POP   HL
                  SCF                   ; Set Carry Flag
                  RET
          
          
          Tot  zover de  "CALL handler".  Het is  in de code voor CALL 
          commando's makkelijk  om een  aantal routines  uit de  BASIC 
          interpreter "bij de hand te hebben":
          
          ; Handige routines
          
          ILLEGA: LD    IX,&H475A       ; Illegal function call
                  JP    &H0159
          
          SYNTAX: LD    IX,&H4055       ; Syntax error
                  JP    &H0159
          
          R_BYTE: LD    IX,&H521C       ; Lees een byte van getokenized
                  JP    &H0159          ; BASIC, resultaat in A
          
          R_WORD: LD    IX,&H6F0B       ; Lees een word van getokenized
                  JP    &H0159          ; BASIC, resultaat in DE
          
          
          Hier  ontbreekt  nog  FRMEVL op  adres &H4C64,  waarmee elke 
          willekeurige   expressie   kan   worden  "geâvalueerd".   De 
          interpreter  zal VALTYP  (&HF663) en DAC (&HF7F6) vullen met 
          het resultaat.  De namen  van de routines die door R_BYTE en 
          R_WORD  worden  aangeroepen  zijn overigens  respectievelijk 
          GETBYT en FRMQNT.
          
          
          ; Tabel met CALL commando's
          
          TABEL:  DB    4               ; lengte CALL commando
                  DM    "INFO"          ; de naam
                  DW    INFO            ; het adres
          
                  DB    4
                  DM    "DSKI"
                  DW    DSKI
          
                  DB    6
                  DM    "KILBUF"
                  DW    KILBUF
          
                  DEFB  0               ; afsluiten
          
          
          In deze  tabel moeten  alle namen,  lengtes en startadressen 
          van de CALL statements in het blok CALL code worden gezet.
          
          Voor de  duidelijkheid heb  ik nog  een drietal  voorbeelden 
          toegevoegd. Op de disk staat de file CALLTEST.BIN, waarmee u 
          het zelf kunt uitproberen.
          
          
          ; Voorbeelden
          
          ; CALL INFO
          ; Zet melding op het scherm
          
          INFO:   LD    HL,TEXT         ; begin tekst
          INFO_1: LD    A,(HL)          ; ASCII teken
                  AND   A
                  JR    Z,INFO_2        ; klaar als 0
                  CALL  &HA2            ; teken naar scherm
                  INC   HL
                  JR    INFO_1          ; volgende teken
          
          INFO_2: POP   HL              ; pointer achter statement
                  XOR   A               ; wis carry: CALL commando Ok
                  RET
          
          TEXT:   DM    "Standaard CALL routine",13,10
                  DM    "(c) Ectry 1993",13,10,0
          
          ; CALL DSKI (drive,sector,aantal,adres)
          ; Lees sectoren in
          
          DSKI:   POP   HL              ; pointer achter naam
          
                  LD    A,(HL)
                  CP    "("             ; check haakje
                  JP    NZ,SYNTAX
                  INC   HL
          
                  CALL  R_BYTE          ; haal drivenummer
                  LD    (DRIVE),A
          
                  LD    A,(HL)
                  CP    ","             ; check komma
                  JP    NZ,SYNTAX
                  INC   HL
          
                  CALL  R_WORD          ; haal start sector
                  LD    (SECTOR),DE
          
                  LD    A,(HL)
                  CP    ","             ; check komma
                  JP    NZ,SYNTAX
                  INC   HL
          
                  CALL  R_BYTE          ; haal aantal sectoren
                  LD    (COUNT),A
                  AND   A               ; zet vlaggen goed
                  JP    Z,ILLEGA        ; Illegal function call als
                                        ; 0 sectoren
          
                  LD    A,(HL)
                  CP    ","
                  JP    NZ,SYNTAX
                  INC   HL
          
                  CALL  R_WORD          ; haal adres (komt in DE)
          
                  LD    A,(HL)
                  CP    ")"
                  JP    NZ,SYNTAX
                  INC   HL
          
                  PUSH  HL              ; bewaar pointer
          
                  LD    C,&H1A          ; BDOS call SETDMA
                  CALL  &HF37D          ; Zet DMA adres op DE
          
                  LD    DE,(SECTOR)     ; DE=startsector
                  LD    HL,(DRIVE)      ; H=#sectoren, L=drive
                  LD    C,&H2F          ; BDOS call Absolute Disk Read
                  CALL  &HF37D          ; Lees sectoren
          
                  POP   HL              ; pointer achter statement
                  XOR   A               ; wis carry: CALL commando Ok
                  RET
          
          ; data voor DSKI routine
          
          DRIVE:  DB    0
          COUNT:  DB    0
          SECTOR: DW    0
          
          ; CALL KILBUF
          ; Wis toetsenbordbuffer
          
          KILBUF: CALL  &H0156          ; BIOS routine
                  POP   HL              ; pointer achter statement
                  XOR   A               ; wis carry: CALL commando Ok
                  RET
          
          ENCALL: END
          
          
                        Z E L F   A A N   D E   S L A G 
          
          Het zelf  maken van  een CALL commando is nu heel eenvoudig. 
          Schrijf  een machinetaalroutine die u door een CALL commando 
          wilt  laten  aanroepen.  U  mag daarin  de routines  ILLEGA, 
          SYNTAX,  R_BYTE  en  R_WORD  gebruiken.  Zorg  dat  bij  het 
          verlaten van  de routine  HL wijst  naar de  positie na  het 
          commando  en dat  de C vlag gewist is. Denk er verder om dat 
          de stack niet verandert ten opzichte van de situatie voordat 
          uw routine werd aangeroepen.
          
          Zet  de  standaardroutine erbij  en zet  het commando  in de 
          tabel. De  standaardroutine zorgt  ervoor dat  er alleen bij 
          het   juiste  CALL  commando  naar  uw  routine  zal  worden 
          gesprongen.
          
          Let erop  dat het totale blok CALL commando's nooit meer kan 
          zijn  dan 16  kB. Verlaag indien nodig de ORG &HC000. &H9000 
          is in ieder geval altijd laag genoeg. U mag de BIOS routines 
          in  page  0  en  het  systeem  gebied in  page 3  natuurlijk 
          gebruiken.
          
          Veel succes met het maken van uw eigen CALL commando's!
          
                                                          Stefan Boer
          