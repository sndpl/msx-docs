                        K U N - B A S I C   C U R S U S 
                                                         
          
          Vorige keer  had ik  tegen Kasper Souren (hoofdredacteur van 
          jullie  meest geliefde diskmagazine) gezegd dat ik deze keer 
          misschien een programma om vector-graphics te maken in BASIC 
          zou plaatsen.  Helaas zag  dhr. Souren  dit als een belofte. 
          Dit  programma heb  ik wel  al, het werkt ook, maar nog niet 
          helemaal zoals ik het wil. Ook heb ik te maken gehad met een 
          "disk-crash"  waar  dat programma  op stond  en ik  had geen 
          kopie... Ik  heb nu een cursus geschreven (in ��n deel) voor 
          het  gebruik van de KUN-BASIC compiler, het programma dat ik 
          heb  gebruikt   voor  het  vector-graphics  programma,  voor 
          ALPHA.BAS  (zie SRS #3) en ook voor de fractal-makers die op 
          deze  disk  staan.  Voor  deze  cursus  heb ik  o.a. gebruik 
          gemaakt van de gegevens uit de MSX2+ handleiding van MK.
          
          De KUN-BASIC compiler (ik kort het vanaf nu af tot BC: Basic 
          Compiler)  is een in gebruik snelle en handige compiler voor 
          het compileren  (=naar machinetaal  omzetten) van MSX-BASIC. 
          Door  gebruik te maken van de BC worden uw BASIC programma's 
          vele  malen  sneller.  In  het  geval  van ALPHA.BAS  heb ik 
          gemerkt dat  de ronddraaiende  balletjes op  7 MHz even snel 
          draaien  als op  3.5 MHz. De snelheid werd dus enkel bepaald 
          door de VDP en niet meer door BASIC
          
          Er  zijn verschillende  versies in  omloop: eentje voor MSX2 
          computers  en   eentje  die  ook  de  MSX2+  commando's  kan 
          compileren  (waarempel uit  mijn computer "gesloopt") (Nvdr: 
          geen commentaar.).  Beide versies  kunnen op  disk of in ROM 
          staat.   Disk-versies   moeten   met  BLOAD"naam",R   worden 
          opgestart,  ROM versies  worden met  CALL BC  [enter] vanuit 
          BASIC geactiveerd.
          
          Deze compiler  heeft als voordeel dat je gewoon in MSX-BASIC 
          blijft  en  dat  je  zelf  kunt kiezen  welke delen  van het 
          programma   gecompileerd   worden,   zodat   je   toch   ook 
          niet-compileerbare  commando's in  het zelfde programma kunt 
          gebruiken.  (later  meer hierover.)  Bovendien hoef  je niet 
          allerlei  apparte  programma's  op  te  starten  om  het  te 
          compileren.
          
          
                        D R I E   I N S T R U C T I E S 
          
          Om  een  programma  te  compileren  zijn  er  3  instructies 
          toegevoegd:
          
                  CALL RUN
                  CALL TURBO ON
                  CALL TURBO OFF
          
          Noot: CALL kan ook als underscore, "_" ingetikt worden.
          
          CALL  RUN  compileert het  hele BASIC  programma dat  in het 
          geheugen staat, bijvoorbeeld:
          
                  10 SCREEN 8
                  20 FOR I=0 TO 255
                  30 LINE(I,0)-(I,211),I
                  40 NEXT I
          
          Nadat  u dit  heeft ingevoerd, kunt het programma eerst eens 
          gewoon  RUNnen,  en  daarna  CALL RUN  [enter] intikken.  De 
          uitvoer is  precies het  zelfde; van  het compileren merkt u 
          niets,  dat gaat  behoorlijk snel. Na CALL RUN is de uitvoer 
          wel  een  stuk  sneller.  CALL RUN"filenaam"  werkt (helaas) 
          niet.
          
          
                N I E T - W E R K E N D E   C O M M A N D O 'S 
          
          Er  zijn commando's die de compiler niet kan compileren. Dit 
          zijn  vooral  de  commando's  voor  disk-drive,  printer  en 
          geluids-chips. Hieronder is een reeks van niet-compileerbare 
          commando's weergegeven.
          
          AUTO, BASE  BLOAD, BSAVE,  CALL, CDBL,  CINT, CLEAR,  CLOAD, 
          CLOSE,  CONT,  CSAVE,  CSNG, CVD,  CVI, CVS,  DEFFN, DELETE, 
          DRAW,  DSKF, EOF, ERASE, ERL, ERR, ERROR, EQV, FIELD, FILES, 
          FPOS, FRE,  GET, IMP,  INPUT#, KEYLIST, LFILES, LINE INPUT#, 
          LIST,  LLIST, LOAD,  LOC, LOF, LPRINT USING, LSET, MAXFILES, 
          MERGE, MOTOR,  MKD$, MKI$,  MKS$, NAME,  NEW, ON ERROR GOTO, 
          OPEN,  PLAY, PRINT#,  PRINT #USING,  PRINT USING, PUT KANJI, 
          RENUM, RESUME, RSET, SAVE, SPC, TAB, TRON, TROFF, WIDTH
          
          Het lijkt  heel wat,  maar het  is in de praktijk best op te 
          vangen.  Nadeel vind  ik dat  PLAY en DRAW niet gecompileerd 
          kan worden. (SOUND echter weer wel.) Als u de versie voor 2+ 
          op een  MSX2 gebruikt  worden de  commando's SET SCROLL e.d. 
          toch  gecompileerd,  maar  heeft  geen  uitwerking. Je  kunt 
          echter  wel gewoon horizontaal scrollen met SET SCROLL. Maar 
          VDP(24) werkt dan even makkelijk.
          
          
          Om toch  b.v. een  andere breedte  (WIDTH) te  kiezen is het 
          volgende mogelijk:
          
                  10 WIDTH 40
                  20 CALL TURBO ON
                  30 PRINT "...."
                     ...
                     ...
                  100 CALL TURBO OFF
          
          Nu wordt  alleen het stuk tussen CALL TURBO ON en CALL TURBO 
          OFF gecompileerd. Een ander voorbeeld:
          
                  10 SCREEN 8:SET PAGE 0,1
          
          Regel  10 zou  ook gecompileerd  kunnen worden,  maar is  te 
          omslachtig.)
          
                  20 BLOAD"PICTURE.PIC",s
          
          BLOAD kan niet gecompileerd worden.
          
                  30 CALL TURBO ON
                  40 SET PAGE 0,0
                  50 FOR I=0 TO 211
                  60 COPY(0,I)-(255,I),1 TO (0,0),0
                  70 NEXT I
          
          De regels hierboven worden snel uitgevoerd.
          
                  80 CALL TURBO OFF
                  90 OPEN"GRP:" FOR OUTPUT AS#1
                  100 PRESET(10,200):COLOR 255
                  110 PRINT #1,"Mooi he?"
                  120 GOTO 120
          
          OPEN en PRINT# kunnen niet gecompileerd worden.
          
          
                     B E P E R K T E   C O M M A N D O 'S 
          
          Van sommige commando's kunnen niet alle opties of parameters 
          gecompileerd worden:
          
          CIRCLE   Start- en eindhoeken en mate van afplatting kunen 
                   niet gecompileerd worden.
          COPY     Alleen graphische copy.
          DEFDBL   Zelfde werking als DEFSNG.
          DIM      Moet als eerste na CALL TURBO ON staan of vooraan 
                   in een programma.
          END      Springt naar de regel NA CALL TURBO OFF, stopt het 
                   programma dus niet!
          INPUT    Maximaal INPUT van ��n variabele, dus niet "INPUT 
                   A,B".
          KEY      Alleen ON KEY GOSUB en KEY(x) ON/OFF toegestaan.
          LOCATE   X en Y coordinaat moeten allebei gegeven worden. De 
                   switch cursor aan/uit (LOCATE X,Y,s(0/1)) is niet 
                   toegestaan.
          NEXT     Variabele moet er ALTIJD achter staan, dus NIET 
                   alleen NEXT, maar b.v. NEXT I of NEXT X,Y.
          ON       Men zegt dat ON STOP GOSUB en ON INTERVAL=X GOSUB 
                   niet werken, wegens een bug, maar ik heb niets 
                   gemerkt. (Ik heb echter alleen de versie voor 
                   MSX2+!)
          PRINT    De komma-functie werkt anders dan normaal.
          PUT      Alleen PUT SPRITE toegestaan.
          SCREEN   Alleen scherm-modus en sprite-grootte kunnen 
                   opgegeven worden.
          SET      Alleen SET PAGE en SET SCROLL (2+ versie) 
                   toegestaan.
          STOP     Zelfde werking als END
          USR      De parameter kan alleen maar een integer zijn.
          VARPTR   File-nummer kan niet gegeven worden als parameter.
          
          
          Er zijn ook een paar nieuwe opties toegevoegd:
          
          '#I      Staat voor INLINE. Hiermee kunt u stukjes 
                   machinetaal eenvoudig tussenvoegen. Bijv.:
          
                  10 CALL TURBO ON
                  20 DEFINT I
                  30 I=1
                  40 '#I &H2A,I%
                  50 '#I &HF3,&HCD,@70,&HFB
                  60 END
                  70 'SUBROUTINE
                  80 ..
                  90 RETURN
                  100 CALL TURBO OFF
          
          Regel 40 betekent: LD HL,(i)  ; I moet integer zijn.)
          Regel 50 betekent: DI
                             CALL @70   ; CALL naar regel 70. Let op! 
                                        ; Wordt niet mee geRENUMd!
                             EI
          
          
          '#C +/-  Zet het zogenaamde CLIPPEN aan of uit. Clippen 
                   houdt in dat als de waarde van een y-coordinaat 
                   groter is dan het beeld aankan, b.v. groter dan 
                   211, er net gedaan wordt alsof de y-waarde gelijk 
                   is aan de maximale y-waarde, in het voorbeeld dus 
                   211. Geldt niet voor PAINT en CIRCLE
          
                   Voorbeeld:
          
                  10 CALL TURBO ON
                  20 SCREEN 5
                  30 '#C-
                  40 LINE(0,0)-(255,255)       (y geCLIPt)
                  50 IF INKEY$="" THEN 50
                  60 '#C+
                  70 LINE(0,0)-(255,255)       (y niet geCLIPt)
                  80 IF INKEY$="" THEN 80
                  90 CALL TURBO OFF            (Kan weggelaten worden)
          
          
          #N +/-   Kijkt of de waarde van de variabele tijdens een 
                   NEXT overloopt. Als #N+ gegeven is, wordt de fout 
                   net zoals in normaal BASIC afgehandeld.
          
                   Voorbeeld:
          
                  10 CALL TURBO ON
                  20 FOR I%=0 TO 100000
                  30 NEXT I%
                  40 CALL TURBO OFF
          
          Als  u  dit programma  RUNt blijft  het oneindig  doorlopen, 
          omdat I%  (een integer)  nooit 100000 kan bereiken. Als u 15 
          '#N+  invoegt eindigt het net zoals normaal in een "Overflow 
          in  30"   Deze  switch   vertraagt  de  uitvoering  van  het 
          programma, gebruik het dus alleen als het nodig is. '#N- zet 
          deze controle-fuctie weer uit.
          
          
                     T I P S   E N   O P M E R K I N G E N 
          
          In   normaal  BASIC   kunnen  "oneindige"   programma's  met 
          [CTRL][STOP] toch gestopt worden. Dit kan tijdens de uitvoer 
          van een gecompileerd programma niet! Voorbeeld:
          
                  10 PRINT"Hallo! ";
                  20 GOTO 10
          
          Als  u  dit  gewoon  RUNt  kunt  u  het altijd  afbreken met 
          [CTRL][STOP],  maar als  u dit  CALL RUNt  kan dat  niet. De 
          enige  mogelijkheid  om toch  te stoppen  is de  computer te 
          resetten of  uit te  zetten! En  daarmee het  verlies van uw 
          programma. Hier wat oplossingen van dit probleem:
          
                  10 PRINT"Hallo! ";
                  20 IF INKEY$="" THEN GOTO 10
          
          Nu  zal  het programma  altijd afbreken  zodra er  een toets 
          wordt ingedrukt.  Natuurlijk werkt "ON STOP GOSUB" ook, maar 
          dit vertraagt de snelheid van het programma.
          
          
          Andere oplossing:
          
          Tik  voor dat  u begint de POKE&HFBB0,1 in. Nu kunt u altijd 
          met [CTRL][SHIFT]  [GRAPH][CODE] het  programma verlaten  en 
          valt  u terug in BASIC. Let op als u het programma onderbrak 
          terwijl u  NIET in  een tekstscherm  was! Dan  ziet u  niets 
          meer. Tik dan SCREEN 0.
          
          
          Nog een oplossing:
          
          Dit is  meer een  nood-oplossing, voor  als er toch nog iets 
          mis  is gegaan en u de computer hebt moeten resetten: dus na 
          een  reset,  tik  dan  in:  POKE32770,128:  POKE32769,1:LIST 
          [ENTER]  Nu  ziet  u  dat telekens  de eerst  regel van  het 
          programma  weergegeven  wordt. Druk  op [CTRL][STOP]  om het 
          listen te  onderbreken en tik dan ALLEEN het regelnummer van 
          de  eerste regel in en druk op ENTER. Nu kunt u uw programma 
          normaal listen en op diskette wegschrijven. Als u dit gedaan 
          heeft,  kunt  u  het  beste weer  resetten en  het programma 
          gewoon  met  "LOAD"  in  te  laden,  voordat u  verder gaat. 
          Voorbeeld:
          
                  [reset]
                  POKE32770,128:POKE32769,1:LIST
                  10 PRINT"Hallo! ";
                  10 PRINT"Hallo! ";
                  10 PRINT"Hallo! ";
                  10 PRINT"Hallo! ";   [CTRL][STOP]
                  Ok
                  10 [ENTER]
                  list
                  10 PRINT"Hallo! ";
                  20 GOTO 10
                  Ok
                  save "naam.bas",a
          
          
                          V R E E M D E   D I N G E N 
          
          De BC vindt een string-formule al snel te complex en zal dan 
          ook snel met de "String formula too complex"-error aankomen.
          
          Als er bij een goto naar een regel gesprongen wordt die niet 
          bestaat, zal  de BC  altijd de  als fout-regel  de regel met 
          CALL TURBO OFF geven. B.v.
          
                  10 CALL TURBO ON
                  20 GOTO 25
                  30 PRINT "TJA..."
                  40 CALL TURBO OFF
          
                  RUN
                  Undefined line number in 40
          
          Daar  staat   echter  niets   verkeerds.  Zeker  bij  groter 
          programma's  is de fout vaak moeilijk op te sporen. Hier een 
          truuk om de fout snel te vinden.
          
                  RENUM 55555,55555,55555
          
          Zorg ervoor  dat er  geen regelnummers  na 55555  voorkomen, 
          anders  gebeurt  er  wel wat.  Er zal  niets met  uw listing 
          gebeuren,   maar  er  wordt  wel  gemeld  dat  er  naar  een 
          niet-bestaande regel gesprongen wordt in regel 20.
          
          De disk-versies van de BC nemen de memory-map page 2 en 3 in 
          beslag, dus let op met een RAM-disk. Bij mijn ROM-versie heb 
          ik die problemen niet gehad.
          
          Nvdr:  de ROM  versie zet  zichzelf ook  in pagina  2 van de 
          memory mapper.  Daarom is CALL BC nodig om te initialiseren. 
          KUN  zit bij  MSX2+'en op  page 2 van het ROM. Dus van #8000 
          tot en  met #bfff.  Bij de  turbo R kun je met het programma 
          READKANA (zie deze disk) een programma in de DRAM laden, dan 
          kost het alleen je Kanji ROM, als je de DRAM mode aan hebt.
          
          Het gecompileerde  programma wordt ook in page 2 en 3 gezet, 
          net  als het  bron-BASIC programma,  maak het bron-programma 
          dus niet te groot.
          
          Alle variabelen BUITEN het TURBO blok zijn niet toegankelijk 
          IN  het TURBO blok en vice versa. Doc integers (zowel enkele 
          variabelen als arrays) kunnen wel meegenomen worden naar het 
          TURBO blok en weer naar buiten. Voorbeeld:
          
                  10 DIM A%(5)
                  20 B%=4:C%=3
                  30 FOR I=1 TO 5:A%(I)=4-I:NEXT I
                  40 CALL TURBO ON ( A%() , B% )
                  50 B%=B%+1
                  60 FOR I=1 TO 5:A%(I)=I+3:NEXT I
                  70 PRINT B%,C%
                  80 FOR I=1 TO 5:PRINT A%(I);
                  90 CALL TURBO OFF
                  100 PRINT B%,C%
                  110 FOR I=1 TO 5:PRINT A%(I);
          
                  RUN
                  5 0
                  4 5 6 7 8
          
                  5 3
                  4 5 6 7 8
          
          
          Dubbele  precisie-variabelen  IN het  TURBO blok  hebben een 
          nauwkeurigheid van 4,5 decimalen. Voorbeeld:
          
                  10 CALL TURBO ON
                  20 PI=3.1415926535897
                  30 PRINT PI
                  40 CALL TURBO OFF
          
                  RUN
                  3.1416
          
          
          CALL TURBO  ON en  CALL TURBO  OFF mogen niet genest worden. 
          Voorbeeld:
          
                  10 CALL TURBO ON
                  20 I=1
                  30 IF I=1 THEN CALL TURBO OFF:GOTO 300
                  40 ..
                  50 CALL TURBO OFF
                  ..
                  ..
                  300 ..
          
          Als  dit  programma  geRUNd  wordt  zal  er  een foutmelding 
          verschijnen.
          
          
                              V E R S N E L L E N 
          
          Om  de snelheid van uitvoer op te voeren kunt u het volgende 
          doen:
          - Gebruik zoveel mogelijk integer variabelen.
          - Voor   grafisch  werk   zonder  sprites:   voeg  dit   in: 
          VDP(9)=VDP(9)  OR  2. Dit  schakelt het  sprite-systeem uit, 
          zodat de  VDP meer tijd over heeft om de andere zaken uit te 
          voeren.  Dit kan juist het laatst beetje extra snelheid zijn 
          dat uw  programma nodig  heeft om  "smooth" te  zijn. Om het 
          sprite-systeem weer aan te schakelen: VDP(9)=VDP(9) AND 253.
          
          
                                 N A D E L E N 
          
          Een  nadeel is dat het gecompileerde programma niet apart op 
          disk gezet  kan worden, zoals MCBC wel kan. Een ander nadeel 
          is  de  vrij  grote onnauwkeurigheid  van de  reals (dubbele 
          precisie variabelen). Voor het tekenen van fractals e.d. kan 
          met  niet al  te nauwkeurig de coordinaten ingeven. (Zie ook 
          deze disk).
          
          De  lijst van  niet te compileren opdrachten zie ik niet als 
          nadeel. Het  zijn meestal  de I/O  opdrachten en de snelheid 
          van uitvoeren ligt dan vaak aan het apparaat zelf.
          
          De BC is (volgens mij) vooral geschikt voor grafisch werk in 
          BASIC. Je  kunt er zelfs op een normale MSX2 een horizontale 
          smooth-scroll over het hele scherm in SCREEN 5 mee maken.
          (zie vorige SRS, ALPHA.BAS)
          
          Ook de  maximale grootte  van het bron-programma is toch wel 
          beperkt.  Op MSX2  en hoger  valt dat  enigzins op te vangen 
          door slim  gebruik te  maken van  de memory-mapper, maar dan 
          zit je met de variabelen te kijken, die moeten dan in page 4 
          of in het VRAM, wat de snelheid niet bevordert.
          
          Na,  dat  was  het  wel.  Zijn  er vragen  over (KUN-)BASIC, 
          schrijf   dan   gerust  naar   de  Sunrise   postbus  t.a.v. 
          ondergetekende.
          
          Tot    de     volgende    keer,    met    (misschien)    een 
          vector-graphics-maker  in KUN.  Programmeer ze!  En je  weet 
          het: programmeren in BASIC: moet KUNnen!
          
                                                         Randy Simons
