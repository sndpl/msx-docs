               S O F T W A R E   P R O J E K T E N   D E E L   1 
                                                                  
          
          Welkom bij deel 1 van de 'cursus' software projekten.  Zoals
          beloofd in de inleidende tekst gaan we vanaf nu  wat  dieper
          in op de vraag wat men in elk systeem tegen komt en wat daar
          aan te programmeren valt. In dit deel zal ik de problemen en
          mogelijkheden van een standaard filesysteem behandelen.  Een
          filesysteem is een verzameling routines waarmee data van elk
          denkbaar type geladen  en  verwerkt  kan  worden.  Het  moge
          duidelijk zijn dat dit een zeer belangrijk onderdeel van elk
          softwareprojekt is. Zo belangrijk zelfs,  dat  de  kwaliteit
          van de rest van het systeem  sterk  afhankelijk  is  van  de
          mogelijkheden van het filesysteem.
          
          Ik zal waarschijnlijk wel weer wat woorden door elkaar  heen
          gebruiken.   Woorden   als   softwareprojekt   en   systeem,
          softwarelagen en niveaus zijn synoniemen van elkaar.
          
          Een wat groter softwareprojekt heeft:
          - invoer
          - uitvoer
          - programma code
          - data
          
          
                                  I N V O E R 
          
          Dit kan natuurlijk van alles zijn. Bij een tekstverwerker is 
          het invoer  van een  toetsenbord, muis of joystick, maar ook 
          van een achtergrondgeheugen (diskette of harddisk etc.). Dit 
          zelfde  geldt voor  een tekenprogramma.  Je kunt wel stellen 
          dat  ELK   groter  programma  achtergrondgeheugen  gebruikt. 
          Daarom   zal  een   eigen  systeem   een  goed  uitgedokterd 
          filesysteem nodig hebben.
          
          
                                 U I T V O E R 
          
          ELK programma  [Nvdr. TSR's soms niet!] zal uitvoer naar het 
          scherm  hebben. Zelfs het meest pietepeuterige programmaatje 
          heeft het al, dus dit is iets om rekening mee te houden. Als 
          het  al  mogelijk  is  om iets  van achtergrond  geheugen te 
          lezen, dan  zal het  schrijven daar  naartoe ook  wel handig 
          zijn.  Bij  een  tekstverwerker  of  tekenprogramma  is  dit 
          natuurlijk  vanzelfsprekend,  maar  het gemiddelde  spel zal 
          daarentegen maar weinig naar achtergrond-geheugen schrijven.
          
          
                   P R O G R A M M A C O D E   E N   D A T A 
          
          Ook dit is iets wat elk programma zal hebben.  De  problemen
          die deze datavormen opleveren zijn per projektsoort  anders.
          Bij een tekstverwerker zal over  het  algemeen  maar  weinig
          extra code  bijgeladen  hoeven  te  worden  tijdens  normaal
          bedrijf, maar bij spellen is dat andere koek.
          
          
                           F I L E   H A N D L I N G 
          
          Uit  deze  beschrijvingen  blijkt  dat  er  altijd  behoefte
          bestaat om data van en/of naar  een  achtergrondgeheugen  te
          transporteren  d.m.v.   een   filesysteem.   Een   standaard
          filesysteem is daarom ook enorm handig is om altijd  bij  de
          hand te hebben. Hoe vaak is het ons ook  al  niet  overkomen
          dat we voor elke demo nieuwe laadroutines hebben  geschreven
          omdat de  oude  plotseling  niet  meer  handig  bleken.  Het
          schrijven van dit soort code is 'dom' werk dat feitelijk  zo
          standaard hoort te zijn dat het maar ��n keer  gedaan  hoeft
          te worden. Een filesysteem is zo'n  cruciaal  onderdeel  van
          elk softwareprojekt dat het daarom niet verkeerd is  om  dit
          als eerste te schrijven van waaruit de rest van het  systeem
          kan groeien. Een filesysteem is als het ware  de  ruggegraat
          van de rest van de software.
          
          Verder zijn laad- en saveroutines natuurlijk per  direkt  te
          gebruiken om andere delen van je eigen systeem mee te maken.
          Dus tijdens de ontwikkelfase  kan  je  meteen  al  van  deze
          routines gebruik maken wat natuurlijk ontzettend handig is.
          
          
          Wat zal het gemiddelde filesysteem moeten kunnen?
            1) Laden van data
            2) Schrijven van data
          
          Dat lijkt simpel genoeg,  ware  het  niet  dat  de  data  zo
          verschillend van aard kan zijn. Mogelijke  datasoorten  zijn
          bijv.:
            - Algemene data
            - Code
            - Graphics
            - Paletten
            - Sprite data
            - Muziek
            - Samples
          
          Samples en muziek zullen voor de  gemiddelde  tekstverwerker
          van ondergeschikt belang zijn, maar ik  vind  het  toch  een
          goed idee om een gezonde  'link'  naar  een  eigen  spel  te
          leggen.
          
          Algemene data en code zijn eigenlijk ��n en dezelfde  tenzij
          codefiles van het BLOAD type zouden  zijn  en  de  datafiles
          niet. Het is verstandig om beide typen gelijk te kiezen, wat
          nl. in code scheelt.
          
          Graphics hebben twee  eigenschappen  waardoor  ze  zich  van
          normale data onderscheiden:
            - Ze kunnen gecruncht zijn
            - Ze moeten uiteindelijk in VRAM terecht komen.
          
          Of  je wel  of niet gecrunchte graphics gebruikt, je zal RAM 
          nodig hebben  om de  grafische data tijdelijk in op te slaan 
          voordat  het naar  VRAM gekopieerd wordt. Praktisch kost dit 
          minimaal 16  kB, die  voor de  rest van  de tijd voor andere 
          doeleinden  te gebruiken  is. Overigens is het ook heel goed 
          mogelijk dat er meerdere grafische formaten zijn. Dit kan je 
          het beste vergelijken met het COPY "filenaam.ext" en BLOAD,S 
          commando in  BASIC. Beide  zijn grafische  laadroutines, die 
          echter verschillende dataformaten gebruiken.
          
          De  andere  filetypes:  paletten,  sprite  data,  muziek  en
          samples; het enige wat ze gemeen hebben is dat  ze  allemaal
          een eigen specifieke behandeling nodig hebben.
          
          
                 B E S T E   M E T H O D E   V O O R   D A T A 
          
          Er blijken net zoveel datatypen als algoritmen om deze  data
          te verwerken te zijn. Of je nu dus wel of niet een standaard
          filesysteem maakt, er zullen hoe dan ook routines geschreven
          moeten worden om de verschillende datatypen af te  handelen.
          Met dat gegeven kunnen we meteen een stap verder gaan,  want
          wat zal er nu  theoretisch  moeten  gebeuren  als  een  file
          geladen wordt?
          
          1) Geef opdracht om de file te laden
          2) Ga naar de betreffende afhandelingsroutine
          3) Laad de file
          4) Handel laadfouten af
          5) Verwerk de geladen data
          
          Stel je voor dat je bij elk  te  laden  file  zelf  al  deze
          akties uit zou moeten voeren, daar wordt  je  toch  helemaal
          krankjorem van. Nee, dan de goede oude  BASIC  tijd  waarbij
          het laden van een grafische file en het afhandelen  van  het
          daarbij horende palet een fluitje van een  cent  was  m.b.v.
          twee eenvoudige commando's.
          
          Het zou buitengewoon wenselijk zijn om elke  mogelijke  file
          van elk mogelijk type even simpel te  kunnen  laden  als  in
          BASIC het geval is. In assembly krijg je dan zoiets als:
          
                  LD   DE,FILADR ; Adres van filenaam string
          ; Geef met andere registers de laadpositie aan
                  CALL GETFLE    ; HAAL file!!
                  JP   C,oeps    ; Uh oh, een laadfout.
                       :
                       :
          
          FILADR: DM   "FILENAME","EXT"
          
                  END
          
          
          De  routine GETFLE  moet nu  het genoemde rijtje van 5 taken 
          uit gaan  voeren. Het soort file is bv. via de extensie door 
          te  geven (.PIC  voor graphics, .COD voor code, .MBM voor de 
          muziek  etc.).  Dit systeem  is al  zo oud  als DOS  zelf en 
          waarom zouden  we hier  recalcitrant gaan doen? Bovendien is 
          het  ook  een  prima  manier  om  de  dingen voor  jezelf te 
          administreren.
          
          De laad-algoritmen  hangen natuurlijk sterk af van de manier 
          waarop  je  de  data opgebouwd  hebt. Laat  ik een  grafisch 
          voorbeeld  geven. Bij  ons [Nvdr.  Fuzzy Logic] is grafische 
          data ALTIJD  gecruncht en  bestaat uit blokken van 16 kB die 
          per blok gedecruncht kunnen worden. Het laadalgoritme is dan 
          bv. als volgt:
          
          - Zet het VRAM decrunchadres
          - Laad een blok in het RAM
          - Decrunch dat blok vanuit de tijdelijke RAM opslag.
          - Herhaal dit tot de file volledig in het VRAM
            gedecruncht is.
          
          Hier  komt nu  het grote voordeel van een routine als GETFLE 
          naar voren.  Zodra deze standaard routine eenmaal geschreven 
          is, kan het je niet meer schelen HOE een file geladen wordt, 
          maar  alleen nog  maar DAT dit gebeurt. Laden, decrunchen en 
          foutafhandeling; dit alles gebeurt automatisch zonder dat je 
          er  verder   nog  last   van  hebt.  Men  noemt  zoiets  een 
          'hi�rarchisch  niveau' of  'software laag'.  Het komt  er op 
          neer  dat  het routines  binnen verschillende  softwarelagen 
          niets kan  schelen HOE  de ander  nu precies zijn werk doet, 
          maar er gewoon vanuit kan gaan dat het gebeurt. In het geval 
          van  graphics  moet  de  gebruiker  van  de  routine GETFLE: 
          natuurlijk  wel weten  dat een  bepaald blok  RAM van  16 kB 
          gebruikt wordt,  maar dat is nog altijd een stuk eenvoudiger 
          dan dat je alles tot in detail moet weten en regelen.
          
          
              Deel 2 van deze tekst kunt u in het submenu vinden.
          
          
          
                                   - PART B -
          
               S O F T W A R E   P R O J E K T E N   D E E L   1 
                                                                  
          
                   H I E R A R C H I S C H E   N I V E A U S 
          
          Hi�rarchische niveaus heb je in vele soorten  en  maten.  In
          principe is je computer zelf  al  een  verzameling  niveaus,
          maar het hangt van je uiteindelijke doel af hoeveel  daarvan
          van belang zijn. Een mogelijk overzicht van deze niveaus  is
          bijv. als volgt:
          
          Niveau 0, Elektronen binnen componenten
          Niveau 1, Transistoren, weerstanden etc.
          Niveau 2, Logische bouwstenen (AND en OR funkties etc.)
          Niveau 3, Complexe bouwstenen (zoals de CPU)
          Niveau 4, CPU Microcode
          Niveau 5, Machinetaalprogramma's
          (Niveau 6, Hogere talen)
          Niveau 7, Operating System
          Niveau 8, Applicaties
          
          Overigens bestaat  er niet  een gedefinieerde toewijzing van 
          niveaus,  en de  toewijzing ervan  is dan  ook een twistpunt 
          tussen  computerguru's.  Een leuk  aspekt is  echter dat  de 
          scheiding waar de hardware eindigt en de software begint per 
          type computer verschillend is. De ene computer bijv. kan wel 
          hardwarematig vermenigvuldigen en de andere niet.
          
          Je begrijpt dat de manier waarop de elektronen over de print
          van je computer lopen voor de gemiddelde software applicatie
          niet echt interessant is. Dit  voorbeeld  laat  alleen  maar
          even zien hoe  de  verschillende  niveaus  opgebouwd  KUNNEN
          zijn. GETFLE zou in dit voorbeeld een niveau zijn binnen het 
          Operating  System  niveau,  maar  zelfs  daar  kan  je  over 
          twisten.
          
          Het hebben van dergelijke niveaus is  echter  cruciaal  voor
          het welslagen van een groot software projekt. Het  stelt  je
          nl. in staat om de complexiteit van het probleem als  geheel
          te verdelen over meerdere (in principe ook wel los werkende)
          modules. Indien je dit niet doet, zie je na een  paar  weken
          programmeren de bomen door het bos niet meer en is  je  code
          gegarandeerd een puinhoop geworden. Is dat het  geval,  stop
          dan twee maanden (om je hoofd even leeg te maken)  en  begin
          opnieuw; deze keer met een goede  opzet.  Overigens  is  het
          onderscheiden van niveaus binnen je software niet  expliciet
          hetzelfde als modulair programmeren  (waar  ik  het  in  het
          verleden al eens over gehad heb  en  waarbij  de  Italiaanse
          kookkunst uitgebreid aan de orde is geweest), maar  de  twee
          gaan wel hand in hand samen.
          
          
               A F H A N D E L I N G   V A N   F I L E T Y P E S 
          
          Ik kan voor bepaalde standaard filetypes wel gaan behandelen
          hoe je deze verwerkt,  maar  de  meeste  verwerkingsroutines
          zijn gewoon  sterk  afhankelijk  van  hoe  deze  files  zijn
          opgebouwd. Deze routines zal je dus zelf  moeten  schrijven,
          waarbij je van tevoren goed moet nadenken hoe je het  jezelf
          zo gemakkelijk mogelijk kunt maken.
          
          Neem  nu een  MoonBlaster muziekfile.  Een datafile  van dit 
          type mag  in principe overal in het geheugen komen te staan. 
          Indien je dus geen vaste plaats voor .MBM files aanwijst zal 
          je  dus elke  keer aan  de MoonBlaster afspeelroutine moeten 
          vertellen  waar   deze  de  data  kan  vinden.  Het  is  een 
          peuleschil  om  dit  binnen  GETFLE  te doen,  het moet  per 
          definitie gebeuren, dus waarom zou je dit dan niet standaard 
          doen  binnen de  afhandelingsroutine? Nadat nu een .MBM file 
          geladen  is  met  GETFLE  hoeft  de  muziek alleen  nog maar 
          aangezet te  worden en  klaar is  Kees. Het  is hierbij  wel 
          belangrijk  dat de afspeelroutine standaard onderdeel van je 
          verzameling  routines  is,  en  dat geldt  bv. ook  voor een 
          mogelijke grafische  data decruncher.  Het voorbeeld  met de 
          MoonBlaster  file laat  duidelijk zien  dat je de verwerking 
          van zo'n  file niet  meer exact  hoeft te  kennen, omdat het 
          automatisch gaat.
          
          Hetzelfde  geldt  voor  samples  die  naar  de Music  Module 
          gekopieerd moeten  worden. Waarom  zou je  het zelf doen als 
          het  ook automatisch  kan? Laad  via GETFLE  gewoon een .MBK 
          file en  laat GETFLE  maar uitzoeken of er een MSX-AUDIO met 
          sample-geheugen aanwezig is en indien ja, de sampledata naar 
          deze herrieschopper kopi�ren.
          
          Als  je de  zaken een beetje slim aanpakt is het mogelijk om 
          dingen waar  je in het verleden dagen mee hebt lopen stoeien 
          ('ik  kan die  samples niet  laden omdat  ik geen 16 kB vrij 
          geheugen heb' of 'waar staat die decruncher ook alweer?') nu 
          met ��n  CALL instruktie  vlekkeloos uit  te voeren. Als dat 
          geen vooruitgang is weet ik het ook niet meer.
          
          
                     F I L E S Y S T E E M   N I V E A U S 
          
          Een routine als GETFLE: staat natuurlijk op  een  vrij  hoog
          niveau binnen je filesysteem. Daaronder zullen zich toch nog
          minimaal twee niveaus moeten bevinden om je  filesysteem  te
          completeren.
          
          Filesysteem niveaus:
          
          Niveau 0: Interface naar de BDOS en foutafhandeling.
          Niveau 1: Laad- en verwerkingsroutines voor de verschillende
                    filetypes.
          Niveau 2: GETFLE-achtige routine(s)
          Niveau 3: Mogelijke batchverwerker
          
          
          N I V E A U   0 
          
          In  een eerder  artikel op Sunrise Special #4 heb ik al eens 
          lopen  zeuren  over  de  moeizame interfacing  met de  BDOS. 
          Ikzelf heb  de routines  die in  de bijbehorende listing van 
          dat  artikel horen  bijna letterlijk  gebruikt als  niveau 0 
          voor mijn  eigen filesysteem. In het algemeen kun je stellen 
          dat  in niveau  0 routines  moeten komen  die delen  van, of 
          complete files  kunnen laden.  Hierbij is de foutafhandeling 
          natuurlijk van groot belang.
          
          Wat je ook schrijft voor niveau 0, de routines  zullen  toch
          ongeveer hetzelfde doel  moeten  hebben  als  de  bij  SRS#4
          gegeven routines (anders in het geen niveau 0 meer).
          
          
          N I V E A U   1 
          
          Hier komen de routines die files  van  een  bepaald  formaat
          laden en verwerken. In een stuk hierboven is al het  ��n  en
          ander vermeld over  de  routines  die  hier  terecht  moeten
          komen.
          
          
          N I V E A U   2 
          
          In  niveau 2  staat op dit moment maar ��n routine; te weten 
          GETFLE  (of  soortgelijke).  Het  is  echter  helemaal  niet 
          ondenkbaar  dat  hier  meer  routines komen.  In mijn  eigen 
          systeem heb  ik nog  een eigen  RAMdisk zitten en een aantal 
          van  de routines  om de RAMdisk te besturen zijn in niveau 2 
          te vinden (de meeste echter in niveau 1).
          
          
          N I V E A U   3 
          
          Een   batchverwerker?  Wat   is  dat?   Welnu,  dit  is  een 
          verzameling routines  waarmee het  mogelijk is  om niet  ��n 
          file (zoals bij GETFLE) maar meerdere files achter elkaar te 
          laden  en te verwerken (denk aan .BAT files in DOS). Dit kan 
          vooral bij  spellen wel  eens enorm  makkelijk blijken, waar 
          van  tijd  tot  tijd  meerdere  files geladen  moeten worden 
          voordat  verder  gespeeld  kan  worden.  De  'Batch'  is een 
          tekstbestand  dat in de teksteditor van je keuze gemaakt kan 
          worden.  In   deze  tekst   staan  dan   commando's  die  de 
          batchverwerker  interpreteert en  uitvoert. Afijn, of en hoe 
          je zoiets  wilt implementeren  moet je  zelf maar weten. Bij 
          mijn  systeem  is  de  batchverwerker  wat  uit  de  kluiten 
          gegroeid  en  bevat  naast  file  laadcommando's nu  ook een 
          aantal commando's om data op een hoger niveau te verwerken.
          
          
             N I V E A U   G E G E V E N S U I T W I S S E L I N G 
          
          Het aantal gegevens  dat  tussen  de  verschillende  niveaus
          uitgewisseld wordt moet zo klein mogelijk  gehouden  worden,
          maar  we kunnen ook niet zonder. Hoe zou je anders in GETFLE 
          een foutmelding  terug kunnen krijgen als deze helemaal niet 
          weet wat er in de onderliggende niveaus in gebeurt? Wat voor 
          parameters  er ook  aan een  niveau doorgegeven of ontvangen 
          worden, ze  zullen in  elk geval  door moeten  geven of iets 
          goed of fout gegaan is. Dat kan het beste met de Cy (=Carry) 
          gebeuren.  Om nu  conversieproblemen tussen de verschillende 
          niveaus tegen te gaan is het van belang dat alle niveaus hun 
          foutmeldingen via de Cy doorgeven.
          
          Een Cy gegenereerd bij een fysieke laadfout in niveau 0 moet 
          dus  bij  alle  andere  niveaus  ook bekend  gemaakt worden. 
          Niveau  2 weet dan bijv. wel DAT er iets fout is gegaan maar 
          nog  niet  precies WAT.  Dat is  echter ook  niet belangrijk 
          omdat het in niveau 2 helemaal niet interessant is wat er is 
          fout gegaan.  Het enige dat telt voor de routine op niveau 2 
          (GETFLE)  is dat deze nu weet dat het niet 100% zeker is dat 
          de data  die geladen  had moeten worden ook daadwerkelijk op 
          de  plaats van bestemming is aangekomen. Met dit gegeven zal 
          een  beslissing  genomen  moeten  worden  of  het  nog  eens 
          geprobeerd wordt, of dat het programma moet stoppen.
          
          Alleen binnen  niveau  0  is  bekend  wat  de  bron  van  de
          foutmelding  moet  zijn  geweest.   Hier   zit   immers   de
          foutafhandling en het kan best zijn dat alleen maar de  disk
          offline is wat binnen niveau 0 prima op te lossen is door de 
          gebruiker  te  vragen  deze  als  de  bliksem weer  terug te 
          stoppen.  Een  foutmelding  naar  hogere  niveaus  hoeft dan 
          natuurlijk niet meer gegeven te worden.
          
          Of het nu om een filesysteem of andere vorm van hi�rarchisch
          systeem gaat,  het  teruggeven  van  foutmeldingen  betekent
          altijd  hetzelfde.   Een   routine   die   een   foutmelding
          terugkrijgt weet dat de gewenste verandering aan het systeem
          (geheugen, disk, registers binnen  een  chip  of  elk  ander
          onderdeel van de computer) niet gelukt is.  We  praten  hier
          over 'inconsistentie' van het systeem; het niet langer weten
          HOE elk systeemonderdeel ingesteld is.
          
          Wat er precies moet gebeuren als dit gebeurt is  afhankelijk
          van de programmeur en de situatie.  Bij  een  echte  fysieke
          laadfout kan maar het beste met het programma gestopt worden
          waarna een boodschap op het scherm moet komen dat je  diskje
          kapot is. Daarentegen zal een fout m.b.t.  het  niet  kunnen
          vinden van een muis in de meeste gevallen wel anders  op  te
          lossen zijn.
          
          
                                T O T   S L O T 
          
          Simpel is dit alles zeker niet. Een goede  voorbereiding  is
          essentieel voor het welslagen van elk programma. Ga nu  niet
          als een bezetene  ondoordachte  code  schrijven,  maar  werk
          eerst op papier uit wat er in de verschillende niveaus  moet
          komen en hoe dat er per niveau ongeveer  uit  zal  komen  te
          zien. Vooral de manier  van  gegevens  doorgeven  (en  welke
          gegevens) tussen de verschillende niveaus is verschrikkelijk
          belangrijk.
          
          In  dit  eerste  deel  zijn  de  basisonderdelen   van   een
          filesysteem besproken alsmede de  manier  waarop  je  zoiets
          moet struktureren. Het is  helaas  maar  het  topje  van  de
          ijsberg, maar ik hoop toch een idee gegeven te  hebben  over
          de  globale  opbouw.  Ik  geef  expliciet  geen  code  omdat
          praktisch blijkt dat NIEMAND daar belangstelling voor toont.
          Zelfs de grootste luiaards vinden dat ze nogmaals  het  wiel
          moeten uitvinden, en wie ben ik  om  dat  tegen  te  houden.
          Bovendien, zelf zou ik waarschijnlijk hetzelfde doen  en  de
          gegeven code hooguit als voorbeeld gebruiken.
          
          Volgende keer komen de programmeer-  en  ontwikkelomgevingen
          aan de beurt. Deze twee zaken zijn nl.  innig  verweven,  en
          dat is iets om terdege rekening mee te houden.
          
          Tot de volgende keer!
          
                                                     Alex van der Wal
