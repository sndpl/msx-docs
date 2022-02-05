                            T U R B O   L O A D E R 
                                                     
          
          Demo's  op de  Picturedisk bestaan vaak uit een groot aantal 
          files,  en  omdat  de  BDOS  voor  elke file  opnieuw in  de 
          directory  en  FAT  moet  gaan kijken  waar de  file precies 
          staat, duurt het laden relatief lang.
          
          De laadtijd  kan enorm worden verkocht door voor de demo een 
          zogenaamde  "turbo loader"  te maken.  Je zet dan alle files 
          achter elkaar  in ��n  grote file,  en je maakt een klein ML 
          programmaatje  dat alles inlaadt en op de juiste plek zet en 
          vervolgens het hoofdprogramma start.
          
          De naam  "turbo" is  zeker niet ten onrechte, want het laden 
          gaat  zo veel  en veel  sneller dan wanneer alle files apart 
          moeten worden  geladen. De  enige manier  om nog  sneller te 
          laden  is door  alles op  sector te  zetten zoals ook bij de 
          Special wordt  gedaan met  de teksten,  muziek en  graphics, 
          maar dat is voor de Picturedisk erg onhandig.
          
          Ik  zal het principe van de turbo loader, heel toepasselijk, 
          uitleggen aan  de hand  van de  Witch's Revenge promo die ik 
          voor Sunrise Picturedisk #9 had gemaakt.
          
          
                                 P L A K K E N 
          
          We  gaan de  files met  het DOS2  commando CONCAT aan elkaar 
          plakken. Zet  eerst de  benodigde files netjes bij elkaar op 
          een  disk en  geef ze zulke namen dat je ze met ��n filenaam 
          met wildcards exclusief kunt beschrijven.
          
          Dit klinkt misschien moeilijk, maar dat valt reuze mee. Noem 
          de files bijvoorbeeld DEMO.MBM, DEMO.MBK, DEMO.SC5, etc. Met 
          DEMO.*  heb  je dan  alle files  te pakken.  Met "exclusief" 
          bedoel ik dat er geen andere files op de disk staan die niet 
          bij de demo horen maar die wel aan DEMO.* voldoen.
          
          In het voorbeeld had ik de volgende files:
          
                     DRUMKIT  DAT      5760 03-11-93 20:21 
                     GFX1     DAT     13568 03-11-93 20:18 
                     GFX2     DAT     12672 03-11-93 20:19 
                     MUSIC    DAT      2304 03-11-93 20:22 
                     TEXT     DAT      2048 04-11-93 16:49 
                     MBREPLAY DAT      4608 18-08-93 11:32 
          
          
          We  plakken  deze files  nu aan  elkaar tot  ��n grote  file 
          WRPROMO.ALL van  ca. 40  kB met het volgende commando (onder 
          DOS2!):
          
                          CONCAT /B *.DAT WRPROMO.ALL
          
          Nu  zien we  de reden waarom ik de files zo heb hernoemd dat 
          ze  met   *.DAT  beschreven   kunnen  worden,  want  met  de 
          oorspronkelijke filenamen had het er zo uitgezien:
          
          CONCAT /B DRUMKIT.MBK + GFX1.SC7 + GFX2.SC7 + ...
          
          De  /B is  nodig om  de files binair achterelkaar te zetten, 
          als je  dat weglaat  dan worden  de files  bij de  eerste ^Z 
          afgekapt en dat is natuurlijk niet de bedoeling.
          
          Het  is zometeen belangrijk dat je de volgorde en de lengtes 
          van de  files bij de hand hebt, dit kan (wederom onder DOS2) 
          heel makkelijk met:
          
                              DIR *.DAT > DIR.TXT
          
          In DIR.TXT  staat nu  de directory  van de  .DAT files.  Met 
          behulp  van deze file gaan we nu een ML progje schrijven dat 
          WRPROMO.ALL inleest.
          
          
                               D E   L O A D E R 
          
          Om  de file  in te laden gebruiken we de BDOS. We defini�ren 
          nu eerst een aantal functienummers en het BDOS aanroepadres:
          
          
          BDOS:   EQU   &HF37D
          SETDMA: EQU   &H1A
          OPEN:   EQU   &H0F
          READ:   EQU   &H27
          
          
          Een   BDOS   functie   kan   worden  aangeroepen   door  het 
          functienummer  in  register C  te zetten  en vervolgens  het 
          adres &HF37D aan te roepen.
          
          We moeten  nu eerst de file WRPROMO.ALL openen. We gebruiken 
          hiervoor  de BDOS functie OPEN, waarbij DE naar het FCB moet 
          wijzen. FCB  staat voor  File Control  Block. Een FCB is een 
          blokje van 37 bytes, dat als volgt is opgebouwd:
          
          FCB+0   drive (0 = default, 1 = A:, 2 = B:, etc.)
          FCB+1   filenaam (8 karakters)
          FCB+9   extensie (3 karakters)
          FCB+14  record size (16 bits waarde)
          FCB+33  random record (32 bits waarde)
          
          Er  staat nog  meer informatie  in, maar  die is voor ons nu 
          niet  interessant. In  de ML source ziet de FCB er als volgt 
          uit:
          
          
          FCB:    DB    0
                  DM    "WRPROMO ALL"
                  DB    0,0,0,0,0,0,0,0,0,0,0,0,0,0,0
                  DB    0,0,0,0,0,0,0,0,0,0
          
          
          Het openen van de file gaat nu als volgt:
          
          
                  LD    C,OPEN          ; open de file
                  LD    DE,FCB
                  CALL  BDOS
                  LD    HL,1
                  LD    (FCB+14),HL     ; record size
          
          
          De  recordsize  wordt  op  1 byte  gezet, zo  kunnen we  elk 
          willekeurig stuk  van de  file inlezen.  Door de  waarde van 
          "random record" te veranderen kunnen we de diverse gedeeltes 
          van  WRPROMO.ALL in  een willekeurige volgorde inlezen, maar 
          het is het makkelijkste om het op de volgorde te doen waarin 
          ze in de file staan, want dan gaat het vanzelf goed.
          
          De  eerste file is de drumkit, deze file is 5760 bytes lang. 
          Dit is een ingekorte drumkit van MoonBlaster, ik heb de rest 
          gewoon "weggegooid" omdat dat niet werd gebruikt.
          
          De eerste  56 bytes  bevatten de sample adressen, die straks 
          in  de MoonBlaster replayer moeten worden ingevuld. We laden 
          eerst  deze  56  bytes  in  en  bewaren  ze  zolang  in  een 
          buffertje.
          
          
                  LD    DE,SMPBUF       ; sample adressen
                  LD    C,SETDMA
                  CALL  BDOS
                  LD    HL,56           ; aantal te lezen bytes
                  LD    DE,FCB
                  LD    C,READ
                  CALL  BDOS
          
          
          De functie  SETDMA stelt  het adres  in waar de data naartoe 
          moet,  dit adres  moet in DE staan. De BDOS functie &H27 die 
          we hier  gebruiken heeft als invoer HL en DE nodig. DE wijst 
          weer naar het FCB en in HL moet het aantal bytes staan. 
          
          Nu  gaan we  de rest  van de  samplekit inladen. Omdat we de 
          MoonBlaster replayer  nog niet  hebben ingeladen,  zetten we 
          die zolang op mapper page 2.
          
          
                  LD    A,2
                  OUT   (&HFE),A
          
                  LD    DE,&H8000       ; samplekit
                  LD    C,SETDMA
                  CALL  BDOS
                  LD    HL,5760-56
                  LD    DE,FCB
                  LD    C,READ
                  CALL  BDOS
          
          
          Dit  gaat weer precies hetzelfde. Nu zijn de graphics aan de 
          beurt. Ik  gebruik mapper  page 3  als laadbuffer. De lengte 
          van de eerste file met graphics is 13568 bytes, zo kunnen we 
          in de directory zien.
          
          
                  LD    A,3
                  OUT   (&HFE),A
          
                  LD    DE,&H8000       ; gfx1
                  LD    C,SETDMA
                  CALL  BDOS
                  LD    HL,13568
                  LD    DE,FCB
                  LD    C,READ
                  CALL  BDOS
                  LD    HL,0
                  CALL  DCRNCH
          
          
          De  graphics waren gecruncht en worden met de routine DCRNCH 
          (die ik hier niet bij heb gezet) gedecruncht naar VRAM-adres 
          &H00000. Het inladen van het tweede gedeelte van de graphics 
          gaat net zo:
          
          
                  LD    DE,&H8000       ; gfx2
                  LD    C,SETDMA
                  CALL  BDOS
                  LD    HL,12672
                  LD    DE,FCB
                  LD    C,READ
                  CALL  BDOS
                  LD    HL,&H8000
                  CALL  DCRNCH
          
          
          Deze  graphics  worden  naar  adres &H08000  gedecruncht. Nu 
          laden we het muziekje in, dat op adres &H8000 in mapper page 
          3 moet staan (mapper page 3 was al geselecteerd).
          
          
                  LD    DE,&H8000       ; muziek
                  LD    C,SETDMA
                  CALL  BDOS
                  LD    HL,2304
                  LD    DE,FCB
                  LD    C,READ
                  CALL  BDOS
          
          
          Nu  is  de  tekst  aan  de  beurt.  Eerst  wordt  de  mapper 
          teruggezet.
          
          
                  LD    A,1
                  OUT   (&HFE),A
          
                  LD    DE,&HB000       ; tekst
                  LD    C,SETDMA
                  CALL  BDOS
                  LD    HL,2048
                  LD    DE,FCB
                  LD    C,READ
                  CALL  BDOS
          
          
          Tot slot wordt de MoonBlaster replayer ingeladen, die in dit 
          geval op adres &HC800 begint.
          
          
                  LD    DE,&HC800       ; MoonBlaster replay
                  LD    C,SETDMA
                  CALL  BDOS
                  LD    HL,4608
                  LD    DE,FCB
                  LD    C,READ
                  CALL  BDOS
          
          
          Nu  is  de  MoonBlaster  replayer  ingeladen  en  kunnen  de 
          sampleadressen  erin worden  gezet en de samples kunnen naar 
          de MSX-AUDIO worden gekopieerd.
          
          
                  LD    A,2
                  OUT   (&HFE),A
                  CALL  MOVSMP
                  LD    A,1
                  OUT   (&HFE),A
          
                  LD    HL,SMPBUF
                  LD    DE,SMPADR
                  LD    BC,56
                  LDIR                  ; zet sampleadressen in
                                        ; replayer
          
          
          In  dit  geval  had ik  de hoofdroutine  hier direct  achter 
          gezet,  eventueel kan  die natuurlijk  ook in  de grote file 
          worden gezet.  Tot slot moeten we nog even ruimte reserveren 
          voor de tijdelijke opslag van de sample adressen:
          
          
          SMPBUF: DS    56              ; buffer sample adressen
          
          
          Zoals  je  ziet  is het  laden vanuit  ML dus  helemaal niet 
          moeilijk! Nu nog even iets over ruimtebesparing.
          
          
                                C R U N C H E N 
          
          De  redactie van  de Picturedisk  is altijd zeer blij als de 
          demo's zo  klein mogelijk zijn, want dan past er des te meer 
          op  de Picturedisk. Als er met losse files wordt gewerkt kan 
          de redactie  meestal zelf  de graphics  wel crunchen als dat 
          nog  niet  gedaan  is,  maar  met  een  turbo loader  is dat 
          natuurlijk  onbegonnen werk.  Zorg er dus altijd voor dat de 
          graphics worden  gecruncht voordat  je ze  in de  grote file 
          zet!
          
          
                               T E N S L O T T E 
          
          Als resultaat krijg je een file WRPROMO.BIN met de loader en 
          een  file WRPROMO.ALL  met alle  muziek, graphics,  etc. Het 
          laadt  niet   alleen  sneller  maar  is  ook  nog  een  stuk 
          overzichtelijker!  Ik roep  dan ook iedereen die demo's voor 
          de Picturedisk  maakt om  ze in de vorm van een turbo loader 
          aan te leveren!
          
                                                          Stefan Boer
