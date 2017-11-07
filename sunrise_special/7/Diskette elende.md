                        D I S K E T T E   E L L E N D E 
                                                         
          
          Iedere MSX'er heeft dit waarschijnlijk al  eens  meegemaakt;
          een disk die na het saven van een file  'kapot'  is  doordat
          tijdens  de  save  aktie  van  diskette  gewisseld  is.  Dit
          probleem  is  al  heel  lang  bekend  (de  oplossingen   ook
          overigens) en ik vertel dan ook weinig nieuws, maar  ik  heb
          nog nooit ergens na kunnen lezen  wat  er  allemaal  precies
          fout gaat.
          
          Ik wil me hier vooral op MSXDOS-1  richten  omdat  dit  voor
          diskettes  de  grote  boosdoener  is.  Diskettes  kunnen  in
          tegenstelling tot hun grotere broers, de harddisks  uit  hun
          gleuf geknikkerd worden. Dat is op zich  heel  handig,  maar
          het probleem is dat de BDOS hier praktisch niet  achter  kan
          komen.  De  BDOS  is  namelijk  blind  voor  onaangekondigde
          diskwisselingen. Snapt u nu waarom een Apple Macintosh  geen
          eject knop heeft?  Het enige wat de BDOS kan zien is dat  er
          tijdens een diskaktie geen disk in de drive zit, waarna deze
          kan vragen om het ding weer terug te plaatsen.  Het  woordje
          'deze' in de vorige zin  is  in  dit  verhaal  van  cruciaal
          belang. De BDOS gaat er  nl.  vanuit  dat  tijdens  kritieke
          akties altijd de fysiek gelijke diskette in de drive zit.
          
          Theoretisch is het mogelijk dat de  BDOS  ook  daadwerkelijk
          controleert of dit het geval is, maar dat zou een  diskaktie
          zo traag maken dat iedereen nog  vrolijk  met  cassettetapes
          zou werken omdat die sneller zouden zijn. De BDOS  eist  dus
          een bepaalde discipline van de gebruiker om tijdens kritieke
          diskakties NOOIT van diskette te wisselen. Ik ben echter van
          nature een onderzoeker (lees: eigenwijs) en heb eens gekeken
          naar wat er nu precies mis kan gaan.
          
          
                  H E T   L E Z E N   V A N   E E N   F I L E 
          
          Het inlezen van een file begint met het  openen  ervan.  Bij
          DOS1 worden nu de 3  FAT sektoren en de directorysektor waar
          de te laden file in genoemd is in het  geheugen  geladen  en
          die blijven daar ook. Vervolgens kan de file gelezen  worden
          waarbij in het geheugen  1  sektor  met  laadgegevens  wordt
          bijgehouden. Dit wordt gedaan om bv. 4 bytes uit de file  te
          kunnen lezen. Eerst wordt de sektor ingeladen en  vervolgens
          worden de 4 bytes naar het DMA gekopieerd.
          
          Tijdens het inladen kan de gebruiker tot de ontdekking komen
          dat dit een oude versie van de file is waarna hij de disk in
          een reflex uit de drive neemt. Indien hij deze na  protesten
          van de BDOS terug stopt, dan is er niets aan de hand.  Stopt
          hij echter een andere disk terug in de drive met de nieuwere
          versie van de file, dan gaat het fout. Alle informatie  over
          waar de file precies op de disk te vinden is staat nl. al in
          het geheugen (FAT + dir.sektor) en op deze nieuwe  disk  kan
          deze file wel op een heel andere positie staan. In dit geval
          gaat het lezen dus  wel  door,  maar  wordt  alleen  rotzooi
          gelezen.
          
          Geconcludeerd mag worden  dat  het  wisselen  van  diskettes
          tijdens een laadaktie resulteert in een fout ingelezen file,
          maar dat de diskettes zelf heel blijven. Hoe anders  is  dit
          bij een schrijfaktie.
          
          
              H E T   S C H R I J V E N   V A N   E E N   F I L E 
          
          Tijdens het cre�ren van een file zoekt de BDOS de  directory
          sektoren af naar een lege positie voor de nieuwe file. Zodra
          een  plaats  gevonden  is  wordt  o.a.  de  filenaam  alvast
          ingevuld. De sektor waar de directory entry in terecht  komt
          blijft in het geheugen staan. Nu wordt in de  FAT  naar  een
          geschikte startpositie voor de file gezocht.  De  FAT  wordt
          overigens ook in het geheugen gelezen.
          
          Laten we zeggen dat de file 1kB lang is en dat de eerste 128
          bytes nu weggeschreven worden waarbij niets  fout  gaat.  Nu
          haalt een onverlaat echter de disk uit de  drive  waarna  de
          volgende 128 bytes geschreven worden. Wat zal er volgens jou
          gebeuren tijdens deze tweede schrijfaktie?  Een disk-offline
          melding misschien?
          
          FOUT!!  De schrijfaktie gaat wonder boven wonder  goed.  Dat
          komt  omdat  ook  tijdens  een  schrijfaktie ��n sektor  met
          informatie in het geheugen wordt bijgehouden  en  omdat  een
          sektor 512 bytes groot is, wordt deze buffer nog  niet  naar
          disk geflushed.
          
          Nu wordt het resterende deel van  de  1kB  file  in  1  keer
          weggeschreven en komt  de  diskdrive  dus  wel  degelijk  in
          aktie. Hier doen zich drie potenti�le foutsituaties voor, te
          weten:
          - Disk offline
          - Disk full
          - Physical I/O error (kras op de magnetische laag etc.)
          
          In een eerder artikel op Sunrise Magazine Special #4 heb  ik
          al eens geprobeerd uit te leggen hoe de error  handling  van
          de BDOS werkt (met die dubbele pointer constructie etc.) Bij
          een disk offline wordt nu naar een eventueel  ge�nstalleerde
          error handler gesprongen. Zo ook bij een fysieke I/O  error.
          Bij een disk  full  wordt  echter  alleen  register  A  =  1
          (MSXDOS-1 call) gemaakt en wordt naar de aanroepende routine
          terug gesprongen.
          
          De vraag op dit punt is, wat moet je nu doen?  Indien er een
          disk offline is geweest die nu is opgelost, maar de  persoon
          een andere disk in de drive heeft gestopt, dan  is  de  kans
          99.99% dat deze nieuwe  disk  bij  de  minste  of  geringste
          volgende diskaktie onherstelbaar  beschadigd  raakt  doordat
          zowel de FAT als de directory sektor van de andere  disk  op
          de huidige geschreven worden.
          
          Even terug naar de vraag: "wat nu te doen ?". Indien de file
          wordt gesloten, dan wordt  de  in  het  geheugen  opgeslagen
          directorysektor teruggeschreven naar de disk. Dit wordt  ook
          met de FAT sektoren gedaan. De nieuwe  disk  wordt  dus  met
          voor  hem  onzinnige  informatie  gevuld,  maar  indien   de
          originele disk is  teruggestopt  dan  gaat  het  (uiteraard)
          goed.
          
          Indien de hele schrijfaktie domweg afgebroken wordt, dan zal
          de nieuwe disk niet vernield worden, of wel ?? Het  antwoord
          daarop is dat deze disk in eerste instantie  inderdaad  heel
          blijft. Tijdens mijn testjes heb ik in die situatie eens  de
          directory opgevraagd en zag tot  mijn  stomme  verbazing  de
          directory van de schijf die ik net  gewisseld  had  op  mijn
          beeldscherm verschijnen. (Opm: De totale directory  van  die
          schijf pastte in 1 sektor) De BDOS had kennelijk  niet  door
          dat er een andere disk in de  drive  was  gestoken.  Of  dit
          onder DOS-2 ook het geval is weet ik overigens niet, maar ik
          kan me voorstellen dat het hier wel goed gaat omdat hier met
          Volume-IDs gewerkt wordt.
          
          Waarom kijkt de BDOS in deze situatie niet naar de  disk  om
          te kijken of deze misschien gewisseld is?  Anders  doet  hij
          het immers altijd (het zou  niet  best  zijn  als  dat  niet
          gebeurde). Het enige wat de oorzaak  van  dit  probleem  kan
          zijn is het feit dat de file waarop de schijfaktie fout ging
          nooit gesloten is. De BDOS neemt immers aan dat een diskette
          alleen tussen het lezen  en  schrijven  van  komplete  files
          gewisseld wordt. Het domweg  afbreken  van  de  schrijfaktie
          omdat er iets fout ging is dus NOOIT goed.
          
          Iets anders is BDOS call 13; de  drive  reset.  Indien  deze
          call wordt uitgevoerd voordat een gecre�erd bestand gesloten
          is, dan gaat het toch fout doordat o.a. de FAT sektoren weer
          worden teruggeschreven. In deze situatie dus niet gebruiken.
          
          
                               D E   M O R A A L 
          
          De moraal van dit verhaal is dat indien je een file cre�ert,
          je deze ook moet sluiten. Doe je het niet, dan verschuif  je
          de ramp alleen maar in tijd naar voren, maar hij zal komen !
          Dat  hierdoor  diskjes  vernield  kunnen  raken  doodat   ze
          tussentijds gewisseld worden is jammer, maar doe je het niet
          dan biedt alleen het uitzetten van de computer een  garantie
          dat de disk bij de volgende diskaktie in  orde  blijft.  Het
          zal hem wel aan mij liggen, maar de  melding  "Please  reset
          your MSX before continuing"  lijkt me alleen in  Metal  Gear
          grappig, maar niet in een tekstverwerker.
          
          Wat dient er nu te gebeuren  indien  een  disk  schrijfaktie
          afgebroken  wordt  door  de  gebruiker  (disk  offline   met
          (A)bort) of de BDOS (disk vol)?  Welnu, als eerste  moet  de
          file gesloten worden. Daarna moet de  gemaakte  rotzooi  nog
          opgeruimd worden, door de aangemaakte file te  deleten.  Als
          dat gebeurt is, is er niets aan de hand en kan  de  komplete
          aktie later nog eens herhaald worden op een  disk  die  meer
          kans van slagen biedt.
          
          
               P R O G R A M M E U R S   E N   D I S K E T T E S 
          
          Gebruikers moeten weten dat ze tussen het  cre�ren  van  een
          file en het sluiten ervan met hun tengels van  de  diskdrive
          af moeten blijven. Programmeurs moeten er met wat praktische
          programmeer psychologie voor zorgen dat mensen  niet  in  de
          verleiding komen om het  toch  te  doen.  Tevens  moeten  ze
          aangeven wanneer het laad/save proces begint en eindigd.
          
          Neem  als  voorbeeld  MoonBlaster.  Het  filemenu  van   dit
          meesterwerk laat weinig te wensen over. Het  is  voor  zowel
          eencellige  wezens,  Stefan  Boer  en  mensen  overduidelijk
          wanneer de computer bezig is met laden of saven. Het  lampje
          van de diskdrive knippert nl. uiterst vrolijk  tijdens  zo'n
          aktie waarbij  (samples  buiten  beschouwing  gelaten)  geen
          verwarrende   pauzes   voorkomen.   Dat   laatste   voorkomt
          vroegtijdige eject neigingen.
          
          Een puntje van kritiek is  wel  dat  MoonBlaster  'mislukte'
          files niet delete bij bv. een disk full error.
          
          Abort en Retry opties tijdens een schrijfaktie mogen dus ook
          wel. Het schrijfproces mag immers zonder het sluiten van  de
          file toch niet afgebroken worden en dus mag je de  gebruiker
          best de kans geven om  de  fout  te  herstellen.  Dergelijke
          opties zijn echter geen must. Een retry omdat iemand de disk
          uit de drive haalt en  deze  daarna  weer  terug  stopt  zou
          eigenlijk met een hatelijke "Blijf daar van af met je fikke"
          sample beantwoord moeten worden en  niet  met  het  redelijk
          beschaafde "Eh sorry, zal ik het nog eens proberen?".
          
          
                      T E C H N I S C H E   D E T A I L S 
          
          Even resumerend:
          - Bij het laden van een file mag je van de gebruiker
            verwachten dat hij van de eject knop af blijft. Doet hij
            het toch, dan kan het geheugen corrupt raken, maar de disk
            blijft tenminste heel.
          - Bij het saven waarbij fouten optreden moet je niet gewoon
            doen alsof je neus bloed, maar moet je netjes de rotzooi
            opruimen. Zorg er m.b.v. je programma voor dat de
            gebruiker niet in de verleiding komt om van diskette te
            wisselen, zodat dit zo goed als altijd goed zal gaan.
          
          Het  valt op  dat het  eigenlijk ontzetend  vaak goed  gaat. 
          Blijkbaar komt  men niet  zo snel op het idee om tijdens een 
          schijfaktie  van disk te wisselen. Dit is dus blijkbaar niet 
          zo'n probleem. Er is echter een situatie denkbaar waarbij in 
          verhouding veel  meer diskettes  vernield raken. Stefan Boer 
          heeft deze situatie op Sunrise Special #1 ook al beschreven. 
          De  situatie  treedt  op wanneer  het cre�ren  van een  file 
          mislukt  door een  'write protected'  diskette. De gebruiker 
          stopt nu  een nieuwe  disk in  de drive en drukt op (r)etry. 
          Deze  nieuwe disk  wordt nu vernield indien de error handler 
          verkeerd  geschreven  is.  Hier  is  een  (r)etry  dus  niet 
          ongevaarlijk!
          
          
                      ! ! !   B E L A N G R I J K   ! ! ! 
                                                           
          
          Een vroeger (ook door mij) sterk  gepropageerde  methode  om
          een retry uit te voeren in de BDOS error  handler  was  door
          register C=1 te maken, gevolgd door een RET instruktie.  Dit
          systeem werkt vaak goed, maar  in  de  hierboven  beschreven
          situatie gaat het hopeloos fout. Het geval wil  nl.  dat  de
          FAT en directory sektor van de  originele  disk  al  in  het
          geheugen staan op het moment dat een "write protected" error
          optreed. Na een (R)etry wordt echter niet meer gekeken of de
          diskette gewisseld is. Tijdens het  schrijven  van  de  file
          worden  dus  met  hoge   waarschijnlijkheid   andere   files
          overschreven  en  na  het  sluiten  van  de  file  staat  er
          plotseling een verkeerde  FAT  en  directory  sektor  op  de
          nieuwe disk.
          
          Samengevat: DOE NIET LD C,1 gevolgd door RET  om  een  retry
          optie te implementeren, maar doe het zoals  de  abort  optie
          werkt!!!!  Herstel de stack en belangrijke  registers  zoals
          ze voor de aanroep naar de BDOS waren en JP  terug  naar  de
          BDOS. Nu worden nl. eerst weer de FAT en  directory  van  de
          potentieel onbekende disk ingelezen. Ik heb  dit  geprobeerd
          en het klopt als een bus.
          
          
                              C O N C L U S I E S 
          
          Deze hele discussie over wat er nu precies moet gebeuren  om
          disk crashes  te  voorkomen  loopt  al  een  tijdje.  Binnen
          Sunrise leeft bij monde van Stefan Boer al geruime  tijd  de
          overtuiging om diskakties bij foutsituaties zo snel mogelijk
          af te breken (waarbij ik niet weet of hij adviseert ze eerst
          te sluiten, maar dat zal wel), maar dat lijkt me dus  alleen
          maar gevaarlijker dan een keurige afhandeling.
          
          Kortom, neem aan dat de gebruiker niet gek is en handel  een
          schrijffout beschaafd af (sluiten en deleten).  Zorg  echter
          wel  voor  een  beetje  transparantie   van   akties   zodat
          gebruikers niet in het ongewisse zijn over het feit  wanneer
          de diskaktie echt klaar is (lees: wanneer de  file  gesloten
          is). Een tekstje "saving file..." op een statusbalk  is  bv.
          een hele goeie. Verder MOET je er dus  voor  zorgen  dat  de
          retry optie in de error handler goed ge�mplementeerd is.
          
          Op Sunrise  Special #4  staat ook  een listing van mijn hand 
          over  diskerror  afvanging  onder  DOS1  (en  dus  ook  DOS2 
          alhoewel  deze eigen en betere routines heeft). Deze listing 
          werkt wel  aardig en  is leuk voor demo's en zo, maar het is 
          wat  beperkt en  de LD C,1 ; RET bug in de error handler zit 
          er  ook  in.  Ik  heb (met  behulp van  dit onderzoekje)  de 
          routines nog  eens dunnetjes  over gedaan, waarbij vooral de 
          veiligheid  van disk  I/O prioriteit heeft gekregen. Affijn, 
          ik ben  van mening  dat deze nieuwe routines heel veel beter 
          en  veiliger zijn  dan de  vorige en raad het gebruik van de 
          oude af.  Lees echter  ook de  tekst op  de volgende submenu 
          optie  die  wat  uitgebreidere  uitleg  over  de  listing en 
          routines geeft.
          
          Ik  vind het  redelijk om van gebruikers het besef te vragen 
          dat ze  zolang het  disklampje brandt  niet aan de ejectknop 
          moeten  komen. Je  kunt immers  ook aan mensen vragen om bij 
          een rood  verkeerslicht te  wachten. Diegenen  die dat  niet 
          doen  lopen dan het risiko om 5 liter rood lichaamsvocht aan 
          het  ZOAB   asfalt  kwijt  te  raken,  maar  dat  wisten  ze 
          vantevoren.
          
                                                     Alex van der Wal
