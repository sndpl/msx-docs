                         V A L B U R G   R E A K T I E 
                                                        
          
          Ik  ben geroerd. Tegen mijn verwachting in heeft iemand (Jan 
          van Valburg)  de moeite  genomen om  eens te reageren op ��n 
          van mijn literaire brouwsels (Sunrise Special #6). Dankjewel 
          Jan, het komt namelijk zelden tot nooit voor dat er reakties 
          komen   op  artikelen   die  door   het  gilde  van  Sunrise 
          medewerkers geschreven worden, wat wij overigens zeer jammer 
          vinden. Het lijkt wel alsof alle abonnementen van de Special 
          op kerkhoven worden bezorgd wat de stilte van de kant van de 
          lezers zou verklaren.
          
          Daar komt nog bij dat het artikel van Jan de 'totaal  andere
          aanpak' een uitermate frisse kijk geeft op  een  manier  van
          programmeren die zijn vruchten sinds het CP/M tijdperk heeft
          afgeworpen. Jan omschrijft in zijn artikel het bijhouden van
          een standaard kernel als leuk en  interessant,  maar  weinig
          nuttig en dat kan ik natuurlijk niet zomaar over  mijn  kant
          laten  gaan.  Dergelijke  uitspraken   schreeuwen   om   een
          weerwoord, maar daarover later meer.
          
          
              D E   B E S T E   O N T W I K K E L O M G E V I N G 
          
          Laat ik beginnen met het introduceren van  twee  termen  die
          nogal eens zullen vallen in de rest  van  dit  artikel.  Een
          programmeeromgeving definieer ik als de feitelijke assembler
          of compiler, met  eventueel  in  combinatie  een  linker  en
          librarian (een programma dat libraries onderhoud).  Met  een
          ontwikkelomgeving  bedoel  ik  alle  andere   software   die
          daarnaast nog nodig is, zoals een editor, een debugger,  een
          RAMdisk en (heel belangrijk) het Operating System.
          
          Iemand die een softwareprojekt aan het uitwerken is  waarbij
          een aanzienlijke hoeveelheid code en data komt kijken  heeft
          behoefte aan een goede ontwikkelomgeving. Deze omgeving moet
          ondersteunend werken wat zoveel betekent als  het  voorkomen
          van ergernissen die in principe niets  met  het  projekt  te
          maken  hebben.  Denk   hierbij   aan   resource   (geheugen,
          diskruimte  etc.)  conflicten  tussen  de  compiler  en  het
          project. Ook het omschakelen tussen  programmeeromgeving  en
          het programma zelf moet vlekkeloos en snel gaan.
          
          DOS kan als  ontwikkelomgeving  gebruikt  worden.  Het  hele
          concept van DOS draait om een 'single user / single process'
          omgeving. Op ��n moment kan er dus maar 1 programma draaien,
          door ��n gebruiker. Dat betekent dus dat bv.  een  assembler
          en linker niet tegelijkertijd aktief kunnen zijn  tenzij  ze
          onderdeel zijn  van  hetzelfde  programma.  Indien  je  geen
          Harddisk hebt is dit een  hel,  omdat  de  ontwikkelomgeving
          plotseling tegen je werkt door  fikse  tijdsvertragingen  te
          introduceren.
          
          Jan noemt  in zijn  artikel een  bij hem ontwikkelde aversie 
          tegen  WB-ASS2 en  ik begrijp  dat best. GEN80 in combinatie 
          met een  Linker en  Librarian is  inderdaad heel,  heel veel 
          beter.  Indien je  daarnaast echter niet de beschikking hebt 
          over een  turbo R  met Harddisk worden deze voordelen teniet 
          gedaan  door de  enorme traagheid van de omgeving. Jan noemt 
          dit niet  met zoveel  woorden, maar  ik kan uit zijn artikel 
          opmaken  dat hij er net zo over denkt (denk ik). Een RAMdisk 
          biedt  daarbij  niet  zoveel  soelaas omdat  je dan  nl. erg 
          voorzichtig moet zijn met welk geheugen je gebruikt.
          
          Aangenomen dat je in het  bezit  bent  van  een  super  MSX-
          configuratie, dan nog stuit ik op ��n aanzienlijk nadeel van
          de GEN80 / DOS omgeving.  De  assembler,  de  linker  en  de
          librarian (de programmeeromgeving) vormen samen een redelijk
          nauw  samenwerkend  geheel.  Ze  begrijpen  elkaars  in-  en
          uitvoer prima waardoor de gebruiker hier geen enkel probleem
          zal tegenkomen. Elk ander onderdeel van de omgeving  (editor
          en  debugger)  is  echter  niet  verwant   aan   de   andere
          programma's. Dat hoeft op zich geen probleem te zijn en  het
          zal op  MSX  nauwelijks  voor  problemen  zorgen,  maar  het
          beperkt de theoretische mogelijkheden toch wat.
          
          Waarom is de assembler van WB-ASS bv. zo snel?  Het antwoord
          schuilt in de aanverwante editor. Deze produceert  nl.  geen
          ASCII bestanden, maar een getokeniseerde versie zoals BASIC.
          De assembler kan zijn werk veel sneller doen doordat  er  in
          de editor reeds een vorm van 'pre-processing' is gedaan. Het
          zijn dit soort 'kleine' dingen die een ontwikkelomgeving tot
          ongekende doelmatigheid doen stijgen. WB-ASS2 mag  dan  zijn
          nadelen  en   beperkingen   hebben,   maar   de   combinatie
          editor/assembler levert aanzienlijke tijdwinsten tijdens het
          assembleren die met GEN80 onmogelijk te halen zijn.
          
          Ik  kan  het  niet  laten  om  hier  ook  even  bij   andere
          computersystemen stil te staan. Overal om  me  heen  zie  ik
          advertenties   in   bladen   die   een   samensmelting   van
          programmeer- en ontwikkelomgeving propageren. Een uitstekend
          voorbeeld is bv. Borland-C voor de PC.
          
          Borland-C is een ge�ntegreerde C++ compiler met de editor,
          compiler, linker en librarian in ��n programma.
          Voordelen:
          - De editor is zeer geschikt om programma's mee te schrijven
          - De linker in nagenoeg onzichtbaar voor de gebruiker omdat
            alles automatisch gaat.
          - Dit geldt ook voor de librarian
          - De ontwikkelde software werkt voor het oog van de gebrui-
            ker vlekkeloos met de omgeving samen. De gebruiker hoeft
            bv. niet zelf tussen verschillende programma's heen en
            weer te springen.
          
          Nadelen:
          - Het is een drug, want je wilt nooit meer iets hebben dat
            minder lekker werkt (alhoewel er wel betere C compilers
            zijn).
          
          Om nog eens aan te tonen  wat  ik  bedoel  zal  ik  nog  een
          voorbeeld geven van de enorme invloed die een kleine ingreep
          kan  hebben.   In   Borland-C   kunnen   meerdere   listings
          tegelijkertijd open staan. Deze listings staan  in  vensters
          die je met de muis kunt besturen. Op het moment dat  je  het
          programma verlaat kan je het programma opdracht geven om  de
          'desktop' te saven. Alle posities  van  vensters  en  inhoud
          worden op deze manier bewaard. Start  je  het  programma  nu
          opnieuw op, dan wordt de situatie  van  voor  het  afsluiten
          hersteld en kan je meteen verder werken. Natuurlijk zou  het
          niet veel tijd kosten om al die bestanden weer even met  het
          handje te openen, maar daar wordt je behoorlijk moe  van  na
          30 keer. Een uiterst kleine geste van het programma kan  dus
          een grote invloed  op  de  funktionaliteit  van  het  geheel
          hebben.
          
          
                    T O   D O S   O R   N O T   T O   D O S 
          
          Wat  is  nu het  fundamentele verschil  tussen de  beste DOS 
          assembler (GEN80)  en de beste BASIC assembler (WB-ASS)? Het 
          antwoord  luidt, de  bron en bestemming van data. GEN80 moet 
          en zal  een file  als input  hebben en  genereert ook alleen 
          maar  files  als  uitvoer. WB-ASS  is wat  dat betreft  veel 
          flexibeler,  maar  je  kunt  praktisch  stellen  dat  WB-ASS 
          geheugen  als  invoer  neemt  en  geheugen of  een file  als 
          uitvoer heeft.
          
          Wat is nu het fundamentele verschil tussen DOS en BASIC? Het 
          antwoord  luidt hier  dat DOS  programma's op een vast adres 
          laadt en  de gebruiker  bij BASIC  alles zelf  maar uit moet 
          zoeken.  Dat eist in de eerste plaats een behoorlijke kennis 
          van de  gebruiker (lijkt  me niet  onredelijk om dat van een 
          programmeur  te vragen),  maar cre�ert  ook een  waanzinnige 
          flexibiliteit  en  controlevermogen over  de inhoud  van het 
          geheugen. Nadeel  is wel dat dit controlevermogen maar zover 
          reikt  als  de  kennis van  de gebruiker  over de  gebruikte 
          programma's.
          
          Stel dat bv. TED of DD-Graph vanuit BASIC  opgestart  zouden
          kunnen worden en  je  deze  programma's  in  combinatie  met
          WB-ASS tegelijkertijd in het  geheugen  wilt  hebben  om  er
          zonder te laden  mee  te  kunnen  werken.  Dat  lukt  alleen
          wanneer  je  heel  precies  zou  weten  welk   geheugen   de
          respektievelijke programma's gebruiken. Conclusie:  met  het
          hudige Operating System van MSX is dit praktisch onmogelijk.
          Dat wil niet zeggen dat het  een  onoplosbaar  probleem  is,
          maar helaas voor MSX  is  het  te  laat.  Het  zou  nl.  een
          allesomvattend geheugenbeheer programma moeten  krijgen  dat
          onlosmakelijk met het Operating System is verbonden.
          
          Nu komt daar toch heel stiekem de door Jan afgeraden  Kernel
          om de hoek kijken. De door ons geschreven F-Kernel (want  zo
          heet het ding) heeft integraal  geheugenbeheer  en  al  onze
          programma's houden er rekening mee. Dat  betekent  praktisch
          dat geheugen niet zomaar genomen kan worden,  maar  dat  het
          netjes aangevraagd moet  worden.  Dat  lijkt  omslachtig  en
          i.v.m. vaste adressen in code ook heel onhandig, maar ik kan
          uit ervaring meedelen  dat  het  juist  het  omgekeerde  is.
          WB-ASS is daarbij zo verbouwd  dat  het  vlekkeloos  met  de
          F-Kernel samenwerkt. Andere  programma's  kunnen  natuurlijk
          niet zomaar opgestart worden (misschien  een  idee  voor  de
          toekomst), maar dat is niet anders.
          
          Ondertussen  groeit  het  aantal  programma's  dat  van   de
          F-Kernel gebruik maakt gestaag. Utilities  en  uitbreidingen
          van het Operating System zelf; allen houden ze  zich  netjes
          aan de afspraken en kunnen zonder  problemen  in  combinatie
          met andere F-Kernel programma's  in  het  geheugen  aanwezig
          zijn. Het heeft natuurlijk veel werk gekost om dit alles  te
          bereiken, maar het verandert een MSX  met  een  aanzienlijke
          hoeveelheid geheugen in een  ware  tovermachine.  Dat  neemt
          niet weg dat het feest pas compleet is wanneer  we  ook  een
          assembler hebben met betere mogelijkheden.
          
          De F-Kernel is niet een file met een hoop standaard routines
          die je net zo goed in een library  kunt  stoppen,  maar  een
          Operating System met een specifiek doel dat de hele computer
          naar  zijn  hand  zet   en   integratie   van   projekt   en
          ontwikkelomgeving mogelijk maakt.
          
          In  dit  verband  ben  ik  ook  uitermate  benieuwd naar  de 
          aangekondigde  assembler van  Compjoetania (Compass)  die de 
          voordelen van  WB-ASS en  GEN80 zou  moeten gaan combineren. 
          Indien   dit  een   pakket  wordt   dat  van  de  specifieke 
          mogelijkheden van  MSX gebruik maakt, met alle mogelijkheden 
          van  GEN80 en  L80 waarbij  in het geval van DOS2 netjes met 
          geheugen wordt omgesprongen etc. dan kan het niet anders dat 
          straks iedere zichzelf respekterende programmeur hiermee zal 
          werken. [Nvdr.  Helaas, linken  is (nog?)  niet mogelijk met 
          Compass.]  Heerlijk, een  file laden  door alleen de muis te 
          besturen via  pull-down menu's.  Debug faciliteiten  waarmee 
          tussentijdse  breakpoints gezet  kunnen worden  waardoor het 
          debuggen van kleine routines in een groter geheel doodsimpel 
          wordt.
          
          Alweer hoor  ik mensen klagen: "Ja maar dat kost veel teveel 
          geheugen".  Onzin, een  programma kan  niet teveel  geheugen 
          kosten. Het  is een  historisch feit dat programmeurs altijd 
          meer  geheugen  moeten  hebben  als  dat  het  te  schrijven 
          programma  nodig heeft  en dus  is dit geen bezwaar. De tijd 
          dat een MSX met 128kB voldoende zou zijn is voorbij. Om echt 
          lekker uit  de voeten  te kunnen  is 512kB toch zeker nodig. 
          Dit  natuurlijk wel  in combinatie met programma's die weten 
          hoe ze met geheugen om moeten gaan.
          
          Zou   ik  mijn   geluk  kunnen  rekken  door  de  heren  van 
          Compjoetania te  vragen alvast  hun visie op het te ontstane 
          produkt toe te lichten ? Indien jullie dat doen dunkt me dat 
          een aanzienlijke hoeveelheid Special lezers (moi incluis) de 
          volgende keer watertandend het desbetreffende artikel zullen 
          verslinden, uitprinten en inlijsten.
          
          
                                T O T   S L O T 
          
          Jan  beveelt (wat  een dwingend woord eigenlijk) het gebruik 
          van  GEN80  met  de  alleskunnende linker  aan. Ik  deel die 
          mening, maar alleen zolang je een turbo R met Harddisk hebt. 
          In alle andere gevallen moet je het zelf maar weten. Wij van 
          Fuzzy  Logic  hebben gekozen  voor een  door ons  behoorlijk 
          verbouwde versie  van WB-ASS  (Shadow voegt  tegenwoordig al 
          eigen  commando's aan  WB-ASS toe)  waarmee we in combinatie 
          met onze  F-Kernel minstens zo lekker werken als anderen met 
          GEN80.
          
          Jan  (en natuurlijk  ook vele anderen) gebruiken een externe 
          linker waarmee  zowel code als data aan elkaar kunnen worden 
          geknoopt.  De F-Kernel is nog niet zo ver en kan alleen maar 
          data aan elkaar linken, maar we zijn op de goede weg.
          
          Jan heeft  nooit niet gebruikte routines in zijn programma's 
          en  wij wel,  maar dat  zal de gemiddelde koper van software 
          een rotzorg zijn.
          
          Indien  het produkt  van de heren van Compjoetania echt goed 
          wordt  en  een zeer  volledig uitgevoerde  ontwikkelomgeving 
          cre�ert, dan  schakelen wij  ongetwijfeld over. Het zijn nl. 
          niet   de  mogelijkheden   van  de  programmeeromgeving  die 
          doorslaggevend zijn  voor de  keuze van  een pakket, maar de 
          mogelijkheden   en  handigheid   van  de  ontwikkelomgeving. 
          Flexibliteit  van   de  laatst   genoemde  is   daarbij  van 
          essentieel  belang omdat  iedereen zijn  eigen inhoud aan de 
          omgeving moet kunnen geven.
          
                                                     Alex van der Wal
