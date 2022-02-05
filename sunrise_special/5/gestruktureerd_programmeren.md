             G E S T R U C T U R E E R D   P R O G R A M M E R E N 
                                                                    
          
          Vanaf de volgende Special ga ik een aantal teksten schrijven 
          over  gestructureerd   programmeren  in   machinetaal.  Veel 
          programmeurs,  zowel beginners als gevorderden, programmeren 
          niet  gestructureerd  en  denken  dat  zij  daardoor sneller 
          programmeren  en  dat hun  programma's sneller  zijn. Hoewel 
          mijn  artikelen  juist voor  die programmeurs  zijn bedoeld, 
          zullen zij daarom vaak aan hun oude stijl vasthouden.
          
          Ik  was  daarom  van  plan  om  een  uitgebreid  verhaal  te 
          schrijven  waarom   het  beter   is  om   gestructureerd  te 
          programmeren.  Ik heb echter (erg) weinig tijd momenteel, en 
          kwam daarom  het idee  om de tekst die Alex van der Wal ruim 
          twee  jaar geleden  schreef over  dit onderwerp  hier "af te 
          drukken". Ik hoop dat iedereen na het lezen van onderstaande 
          tekst "bekeerd" is!
          
          Vanaf de volgende Special kunt u van mij een serie artikelen 
          verwachten   over  gestructureerd  programmeren  volgens  de 
          methode van  stapsgewijze verfijning (oftewel top-down). Tot 
          dan!
          
                                                          Stefan Boer
          
          
                               I N L E I D I N G 
          
          Er  zijn   nogal  wat   mensen  onder  jullie  die  zichzelf 
          programmeur  noemen, maar vaak blijkt dan dat ze dat heilige 
          woord  misbruiken.  Een programmeur  is nl.  iemand die  een 
          probleem onder  de loep neemt en die daarna via vaste regels 
          en instrukties tot een oplossing komt. Dat betekent dat zijn 
          programma aan een aantal standaard voorwaarden voldoet.
          
          De  belangerijkste afspraak waar hij zich aan moet houden is 
          dat het  programma dat  hij geschreven heeft ook voor andere 
          programmeurs  ontcijferbaar moet  zijn zonder  dat je  dagen 
          bezig bent  om alles  te begrijpen. Het is echter helaas een 
          feit  dat  mensen  die  bezig  zijn  om  de  kunst  van  het 
          programmeren  te leren deze afspraak negeren. Vaak loopt het 
          zo uit  de hand  dat deze  mensen hun eigen programma's niet 
          eens   meer  snappen   die  ze   twee  weken  eerder  hebben 
          geschreven. (Ik spreek uit ervaring.)
          
          
                               S P A G H E T T I 
          
          Denk nou niet meteen dat ik een arrogante zak ben  omdat  ik 
          kritiek op andermans programmeerknoeisels heb, want ik  geef 
          van harte toe dat ik ook zo'n prutser  ben  geweest.  Er  is 
          echter eens een moment in mijn leven geweest dat  iemand  me 
          op het feit wees dat mijn programma's  de  vergelijking  met 
          een hoop stront niet overleefden. Vanaf dat moment heb ik al 
          mijn aandacht gericht op het leren van de schone  kunst  van 
          het gestructureerd programmeren.
          
          Deze teksten  die ik  over dit  inderwerp ga  schrijven zijn 
          niet  bedoeld als cursus, omdat dat helemaal geen effect zou 
          hebben. Dat zou nl. alleen maar betekenen dat ik jullie mijn 
          programmeerstijl zou  opdringen en dat is niet de bedoeling. 
          Ze  zijn meer bedoeld om jullie aan te sporen ook op zoek te 
          gaan naar  een standaardisatie  van jullie  programma's. Het 
          enige  dat ik  kan doen is jullie een aantal tips geven over 
          wat  voor  standaardisatie  geschikt  is  en  hoe dat  in de 
          praktijk werkt.  [Noot van Stefan: die serie artikelen is er 
          nooit gekomen, daar ga ik dus verandering in brengen!]
          
          Er  zullen ongetwijfeld mensen zijn die al een eigen methode 
          van  werken  hebben  en  die  in  staat zijn  programma's te 
          schrijven die best voor andere mensen leesbaar zijn, maar ik 
          neem nu  de stelling  dat de  methode die ik ga bespreken de 
          beste is. Dit omdat hogere programmeertalen ook allemaal uit 
          gaan  van deze  methode, dus  als je deze methode al kent op 
          een moment  dat je  met een hogere taal geconfronteerd wordt 
          dan kan dat alleen maar voordeel opleveren.
          
          De methode van werken die ik (en met mij  vele  informatici) 
          gebruiken is niet een strakke methode en dat  is  maar  goed 
          ook omdat je de denkwijze van mensen maar heel moeizaam kunt 
          be�nvloeden.  Bovendien  zou  programmeren  een  hele  saaie 
          bezigheid  zijn  als  iedereen   zich   aan   een   absolute 
          programmeerstandaard hield.
          
          Goed, ik denk dat het tijd wordt om maar eens te beginnen en 
          ik  hoop  dat  deze  artikelen  veel mensen  die nu  nog een 
          spaghetti    programmeerstijl    gebruiken    (oftewel   een 
          'kbenblijdattutwerkt' stijl) zich tot het ware geloof zullen 
          bekeren. (Waar heb ik dat eerder gehoord?)
          
          
                                T O P - D O W N 
          
          De methode die ik altijd gebruik heet de top-down methode en 
          is uit te leggen met een  voorbeeld.  Stel  je  zit  in  een 
          wolkenkrabber op  de  100ste  verdieping.  Vanuit  die  hoge 
          positie kan je de hele stad overzien, maar  nu  ga  je  naar 
          beneden zodat je steeds minder van  het  geheel  gaat  zien, 
          terwijl de detaillering van het deel dat  je  nog  wel  kunt 
          zien steeds groter wordt.
          
          De stad in het voorbeeld is het op te  lossen  probleem.  Je 
          kan het hele probleem van boven overzien, waarbij je  alleen 
          de verschillende deelproblemen die je op  moet  lossen  kunt 
          overzien. Als je lager komt kan je steeds beter de oplossing 
          van de verschillende deelproblemen zien die op hun beurt ook 
          weer uit deelproblemen kunnen bestaan. Op het moment dat  de 
          lift beneden is zijn  voor  alle  deelproblemen  oplossingen 
          gevonden, oftewel het programma is  af.  Natuurlijk  is  het 
          handig af en toe even met de lift omhoog te  gaan  zodat  je 
          weer helemaal precies weet waar je mee bezig  bent.  Ook  is 
          het handig om even naar de verschillende deelproblemen kijkt 
          voordat je met programmeren begint om er achter te komen  of 
          je alle problemen wel op kan lossen. Lukt dit nl.  niet  dan 
          ben je dus niet in staat om het programma te schrijven en is 
          het raadzaam om eerst wat minder hooi op je vork te nemen.
          
          
                             D E   P R A K T I J K 
          
          Goed, we zullen nu eerst de puur  theoretische  oplosmethode 
          bekijken voor machinetaal.
          
          Bij de top-down methode is het heel handig  om  modulair  te 
          programmeren. Dat betekent dat je deelproblemen apart oplost 
          zodat ze in principe ook los van de rest van  het  programma 
          werken en dat je ze in andere programma's  in  kunt  bouwen. 
          Het beste voorbeeld van een modulair geschreven  routine  is 
          de BIOS. Deze routines zijn zo standaard dat  ze  door  alle 
          verschillende programma's gebruikt kunnen worden. Een andere 
          eigenschap van een modulaire routine is dat  informatie  die 
          de ene routine maakt ook door andere routines bruikbaar  is. 
          De BIOS voldoet ook aan deze eis, zo wordt b.v. het  huidige 
          schermnummer op een vaste plaats bewaard. Het blijkt dat het 
          standaardiseren van eigen routines een zeer taaie klus is en 
          het is voor de meeste eigen geschreven  routines  vaak  niet 
          echt belangerijk. De eigenschap dat eigen routines  ook  los 
          van de rest van programma  kunnen  werken  en  uitwisselbaar 
          zijn is veel belangerijker.
          
          Goed, een programmeur is dus blijkbaar  iemand  die  volgens 
          een vaste methode (in ons  geval  de  top-down  methode)  en 
          volgens het principe van modulair  programmeren  programma's 
          schrijft.
          
          Als er mensen onder jullie zijn die later programmeur willen 
          worden dan kan ik ze aanraden om de bovenstaande alinea goed 
          te onthouden want deze regel geldt in de praktijk voor  alle 
          programmeertalen die ooit ontworpen zijn sinds de  invoering 
          van de CPU.
          
          Programma's die volgens de top-down methode zijn  geschreven 
          zijn heel eenvoudig herkenbaar. Ze beginnen nl. vaak met een 
          lijstje EQU  instrukties  waarna  een  hoofdprogramma  volgt 
          waarin   de   instruktie   CALL   overheerst.   Achter   dat 
          hoofdprogramma staan altijd  de  routines  waar  die  CALL's 
          heenspringen en achter deze routines staan dan vaak nog  een 
          lading data's.
          
          De routines waar de CALLs heenspringen zijn altijd modulair, 
          maar  dat betekent in de praktijk vaak dat ze niet in andere 
          programma's bruikbaar  zijn omdat  ze te  specifiek bij  het 
          programma  horen waar ze voor geschreven zijn. Een voorbeeld 
          hiervan is  b.v. een  routine bij  een lichtkrant (scroller) 
          die  de plaats  van het  volgende stukje  letter berekend en 
          zoekt,  terwijl  een routine  die een  lijn trekt  heel goed 
          uitwisselbaar is.
          
          Als je mij gelooft dan moet je nu dus ook  geloven  dat  een 
          programma dat met de top-down methode geschreven is ook door 
          andere programmeurs ontcijferd kan worden. Dit is wel  waar, 
          maar het komt niet door de top-down methode want  die  stelt 
          eigenlijk niets voor.  Nee,  het  komt  door  een  bijzonder 
          aangenaam bijverschijnsel dat  altijd  optreedt  als  je  de 
          top-down  methode  gebruikt.  Want   het   programmeren   in 
          spaghetti stijl wordt nl. onmogelijk gemaakt.
          
          
                    I T A L I A A N S E   K O O K K U N S T 
          
          De spaghetti stijl is de stijl waar de meeste  prutsers  mee 
          beginnen. Er wordt konstant heen en weer  gesprongen  tussen 
          verschillende  routines   waardoor   het   programma   wordt 
          gedegradeerd tot doolhof voor iemand die  de  werking  ervan 
          moet uitvissen. Bovendien slaan dit soort programma's  nogal 
          snel (en heel heel vaak)  vast  als  er  kleine  (of  grote) 
          veranderingen in worden aangebracht.  Je  begrijpt  dat  het 
          vinden van bugs een zeer slopende en  tijdrovende  bezigheid 
          is (gniffel gniffel). Het enige voordeel dat deze misbaksels 
          hebben is dat ze inderdaad  redelijk  kraakvrij  zijn,  maar 
          helaas geldt dat ook vaak voor de  programmeur.  (Ik  spreek 
          trouwens nog steeds uit ervaring.)
          
          De spaghetti stijl wordt onmogelijk gemaakt  omdat  je  heel 
          goed op je stack moet letten omdat een routine die  met  een 
          CALL wordt aangeroepen met een RET weer terug moet,  terwijl 
          de stack er bij het ingaan van de routine hetzelfde uit moet 
          zien als bij het  terug  gaan  om  hele  vreemde  errors  te 
          voorkomen. Het is natuurlijk altijd mogelijk om spaghetti te 
          maken, maar de top-down methode  heeft  een  sterk  remmende 
          werking op deze gang van zaken.
          
          
          Je moet dus een aantal regels gebruiken:
          
          - Je mag nooit van de  ene  routine  in  de  andere  routine 
            springen. Wel mag je in de eigen routine naar hartelust de 
            meest vreemde bokkensprongen maken. Moet je in een routine 
            een andere aanroepen, dan moet dit met een CALL  statement 
            gebeuren.
          
          - Als je in een routine iets op de stack zet, dan  moet  dit 
            in dezelfde routine weer van de stack gehaald worden.  Dus 
            niet alleen PUSH en POP, maar ook  CALL  of  iets  anders. 
            (Deze regel kan in een hogere  programmeertaal  niet  eens 
            ontdoken worden.)
          
          Er blijken uitzonderingen mogelijk op deze regels, maar  een 
          goede programmeur laat zich daar  niet  door  uit  het  veld 
          slaan  en  bedenkt  een  manier  zodat  deze   regels   toch 
          gehandhaafd blijven.
          
          
                               V O O R D E L E N 
          
          - Programma's zijn goed leesbaar en begrijpbaar
          - Programma's zijn snel bugvrij te maken en te houden.
          - Hoe langer je oefent, des te sneller je in staat zult zijn 
            om een foutloos programma te schrijven.
          - Je maakt een fantastische opstap om aan een hogere 
            programmeertaal te beginnen. (Overigens is BASIC geen 
            hogere programmeertaal.)
          - Je mag jezelf programmeur noemen
          - Je kunt eens lachend op prutsers neerkijken (even 
            vergetende dat je er zelf ook een bent geweest).
          
          
                                 N A D E L E N 
          
          - Het is lastig om je alles aan te wennen.
          - Tja, eh...  Er zijn eigenlijk geen nadelen van belang.
          
          Goed, dat was het voorlopig. Ik zal zeer zeker een  volgende 
          keer een meer praktische benadering van deze methode geven.
          
                       A A N   A L L E   P R U T S E R S 
          
          Lig over deze tekst maar eens een nachtje wakker en maak een 
          besluit of je in de toekomst door wilt gaan met je gepruts, 
          of dat je bereid bent om iets anders te proberen.
          
          
          Hopelijk heb ik niet al  te  veel  mensen  op  hun  (meestal 
          uitschuifbare)  tenen  getrapt,  maar  ik  kan  deze  mensen 
          verzekeren dat ze vanzelf zullen struikelen als  ze  blijven 
          zweren bij de Italiaanse kookkunst (spaghetti).
          
                                                     Alex van der Wal
          
          
          (Nu  al 1.5 jaar geen prutser meer, alhoewel ik dit jaar het 
          meeste geleerd  heb.) [Nvdr. Klopt niet, omdat deze tekst al 
          2 jaar oud is.]
