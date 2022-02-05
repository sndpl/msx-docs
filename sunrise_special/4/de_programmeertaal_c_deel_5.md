                    D E   P R O G R A M M E E R T A A L   C 
          
                                  D E E L   5 
                                               
          
          
          N.B.: Dit is het vijfde deel van de serie. Bekendheid met de 
          eerste vier delen wordt verondersteld!
          
          
                 C O M P L E X E   D A T A T Y P E N   I N   C 
          
          
                               I N L E I D I N G 
          
          De  kracht van C zit hem in het feit dat er zowel 'dichtbij' 
          de  machine  geprogrammeerd  kan  worden  (waardoor  er veel 
          minder vaak  naar assembler  hoeft te  worden gegrepen)  als 
          heel  abstract, iets  wat PASCAL  programmeurs vaak  als hun 
          specifieke terrein beschouwen.
          
          In  feite   heeft  PASCAL   maar  twee  dingen  voor  op  C: 
          enumeraties   en  sets.   Enumeraties  zijn  gemakkelijk  te 
          vervangen door  constanten, sets  geven wat  meer problemen, 
          maar  in C zijn wel uitgebreide bitmanipulaties mogelijk die 
          dit gemis  kunnen opvangen  - en  een stuk  efficienter. (In 
          ANSI   C  heeft   men  de   taal  overigens  uitgebreid  met 
          enumeraties, maar ANSI C is niet voor de MSX te krijgen.)
          
          Bij voorbaat  wil ik  de gebruikers  van Small C waarschuwen 
          dat  het meeste  van wat in deze aflevering staat niet geldt 
          voor deze compiler: buiten het enkelvoudige array kent Small 
          C   geen   samengestelde   datatypen,   geen   casts,   geen 
          initializers en is het geheugenbeheer - mogelijk afhankelijk 
          van de verschillende versies - beperkt.
          
          
                     S A M E N G E S T E L D E   T Y P E N 
          
          Of we  abstract kunnen programmeren hangt er vooral vanaf of 
          we  onze gegevens astract kunnen maken. Blijven we steken op 
          het niveau  van bits,  bytes of words (machinetaal, maar ook 
          BASIC)   dan  maken   we  het   onszelf  heel   moeilijk  om 
          begrijpelijke en  dus foutloze  programma's te schrijven als 
          we wat meer gecompliceerde algorithmen willen gebruiken. 
          
          
            A R R A Y 'S ,   P O I N T E R S   E N   D E   "C A S T "
          
          Array's   en  pointers  zijn  natuurlijk  in  deel  drie  al 
          behandeld, toch  komen hier  nog een paar dingen aan de orde 
          die niet eerder aan bod zijn gekomen.
          
          In  de eerste  plaats moet  ook binnen  declaraties rekening 
          worden   gehouden   met  de   prioriteit  en   volgorde  van 
          "evaluatie"   van   de   "operatoren".   Ik   gebruik   hier 
          aanhalingstekens omdat er natuurlijk niet echt sprake is van 
          een rekenkundige uitdrukking, maar het lijkt er veel op.
          
          In declaraties  komen gelukkig alleen de '()', de '[]' en de 
          '*'  "operator" voor.  De '()'  en de '[]' hebben een hogere 
          prioriteit  dan  de  '*'.  (De  '()'is  de  'functie-return' 
          operator;  deze  valt  echter  buiten  het  bestek van  deze 
          aflevering.) Gewone  haakjes kunnen gebruikt worden als deze 
          prioriteit  ons niet bevalt. De "evaluatie" verloopt voor de 
          '()' en  '[]' van  links naar rechts, die van de '*' precies 
          omgekeerd.   Gelukkig  hoeven   we  met   dat  laatste   bij 
          declaraties  geen  rekening te  houden, omdat  het de  enige 
          "operator"  is  met die  prioriteit die  in declaraties  kan 
          worden gebruikt.
          
          Zo is na
          
                    char *vb1[10];
          
          'vb1' een array van 10 pointers naar char, en na
          
                    char (*vb2)[10];
          
          'vb2' een  pointer naar  een array  van 10  chars, iets heel 
          anders. In het laatste geval hadden we ook kunnen schrijven:
          
                    char (*vb2)[];
          
          Er wordt namelijk geen array gedeclareerd (waarvoor geheugen 
          gereserveerd  zou moeten  worden), maar een pointer naar een 
          array. Binnen  C hoef  je in  zo'n geval  de grootte van het 
          array niet op te geven, wat logisch is: in C wordt toch niet 
          gecontroleerd of een array index binnen de grenzen valt!
          
          Het is in principe mogelijk de declaraties zo ingewikkeld te 
          maken  als je  zelf wilt, als je bovenstaande regels maar in 
          acht neemt. (In het bijzonder kun je dus een array maken met 
          twee of meer dimensies!) Het is echter bijzonder gemakkelijk 
          om hierin  fouten te  maken, of  het overzicht te verliezen, 
          vooral omdat lang niet alle compilers meldingen geven als we 
          dingen  (bijvoorbeeld pointers  en int's)  doorelkaar halen. 
          ASCII-C vormt hierop gelukkig een uitzondering.
          
          Maar stel  dat we  juist wel  bepaalde datatypen  doorelkaar 
          willen  gebruiken? Dit  komt vaker voor dan je op het eerste 
          gezicht denkt.  Zo verwachten  sommige standaardfuncties een 
          unsigned  als een van hun parameters, maar vaak is de waarde 
          alleen als  int beschikbaar. (Zie ook 3.1 voor een voorbeeld 
          hiervan.) Hoe gaan we dan te werk?
          
          Allereerst  moet  de  de waarde  van de  int natuurlijk  wel 
          positief  zijn, want unsigned's zijn altijd positief. Is dat 
          het geval  dan volstaat het voor de uitdrukking '(unsigned)' 
          te  zetten. In  meer algemene termen: zet een declaratie van 
          het gewenste  type waarin een variabelenaam ontbreekt tussen 
          haakjes  en voor  de uitdrukking,  en voila!  We hebben  van 
          datatype gewisseld. Deze operatie wordt een 'cast' genoemd.
          
          Een  'cast'  kan alleen  worden uitgevoerd  bij enkelvoudige 
          datatypes (unsigned, int, char en pointers), wat logisch is, 
          want alleen  deze mogen  gebruikt worden  bij een assignment 
          ('='  operator), of  als functieparameter en -return. Ook de 
          'cast' operator  heeft een  prioriteit, uiteraard, en die is 
          dezelfde is als die van '++', '--', enz.
          
          Overigens  wordt bij een assignment meestal een automatische 
          cast uitgevoerd naar het gewenste type, bijvoorbeeld:
          
                    int g1;
                    char g2;
          
                    g1 = 65;
          
                    g2 = g1;
          
          Hierna bevat  g2 een  'A', als  het systeem waarop we werken 
          tenminste   de  ASCII-code   gebruikt.  Waar   echter  nooit 
          automatische casts worden uitgevoerd zijn functie-parameters 
          en -returns, wat bij ASCII-C tot problemen kan leiden, omdat 
          char's aan de ene kant, en unsigned's, int's en pointers aan 
          de andere  kant op  verschillende manieren  worden door-  en 
          teruggegeven.  De  functie  parameter  checker  van  ASCII-C 
          (FPC.COM)  ontdekt dit  soort fouten  wel, maar  het is niet 
          verplicht deze te gebruiken!
          
          Bij HiSoft C moet een cast op een iets andere manier gegeven 
          worden: voor  de haakjes  moet nog  het woord  'cast' worden 
          gebruikt.   Het  al  eerder  gebruikte  voorbeeld  zou  dan: 
          'cast(unsigned)' worden.  Naar eigen zeggen is dit gedaan om 
          de  compiler simpeler  te houden, maar het komt niet overeen 
          met enige standaard.
          
          
          
                                 S T R U C T S 
          
          Structs  gebruiken  we  als  we  verschillende  gegevens bij 
          elkaar willen opslaan. Als voorbeeld nemen we een punt in de 
          ruimte: deze  heeft een X, Y en Z coordinaat. Gesteld dat we 
          genoegen nemen met int's voor deze coordinaten dan hebben we 
          na
          
                    struct point { int x,y,z; } pnt1;
          
          een  variabele 'pnt1'  gecreeerd die  uit drie int's bestaat 
          met  de  namen  'x',  'y'  en  'z'. Deze  struktuur (vandaar 
          'struct') is  nu bekend  onder de naam 'point', zodat we met 
          bijvoorbeeld
          
                    struct point pnt2;
          
          de  variabele 'pnt2'  in het  leven roepen,  die op  precies 
          dezelfde  manier  is  samengesteld.  We zijn  overigens niet 
          verplicht een  struct een naam (we spreken in C ook wel over 
          'tag  name')  te  geven,  maar  het  is  vaak  heel  handig. 
          Omgekeerd  kun je  ook de naam van de variabele weglaten. Er 
          wordt dan uiteraard geen variabele gecreeerd, maar de struct 
          is  dan   wel  gedefinieerd.   Dit  gebeurt  nogal  eens  in 
          '#include'  files waarin  we dit  soort definities  voor een 
          aantal modules tegelijk willen vastleggen.
          
          Om toegang  te krijgen  tot het "inwendige" van een struct - 
          de  'members'  (=  leden)  -  gebruiken we  de '.'  operator 
          gevolgd  door de  naam van  het lid.  Als we de Z-coordinaat 
          willen bereiken van 'pnt1' is
          
                    pnt1.z
          
          voldoende,  en  dit  samenstel  is als  gewone variabele  te 
          gebruiken.  Veel  algorithmen maken  gebruik van  lijsten of 
          boomstrukturen. Hiervoor is nodig dat er in de de struct een 
          of meer pointers zitten naar andere variabelen van hetzelfde 
          type. Als  voorbeeld kunnen we denken aan een lijst woorden, 
          of  namen. De  pointer in de struct wijst dan naar de struct 
          met  de   volgende  naam,  en  de  pointer  daarin  naar  de 
          daaropvolgende, enzovoort. (In de laatste struct maken we de 
          pointer NULL.)
          
                    struct name { struct name *next;
                                  char print_name[20]; } firstname;
          
          Dit geeft aan hoe we zoiets kunnen doen. Nu we het toch over 
          pointers hebben: er bestaat een verkorte schrijfwijze om een 
          member  van een struct te bereiken uitgaande van een pointer 
          naar een  struct. Hou  bovenstaande struct  in gedachten, en 
          bekijk het stukje C hieronder:
          
                    struct name *pointer;
          
                    pointer = &firstname;
          
                    pointer = pointer->next;
          
          Het  gaat hier  natuurlijk om de '->'. Volledig identiek met 
          de laatste regel zou zijn:
          
                    pointer = (*pointer).next;
          
          (De  haakjes  zijn nodig  omdat de  '.' operator  een hogere 
          prioriteit  heeft  dan de  '*' operator.)  Het behoeft  geen 
          betoog dat  de andere  schrijfwijze korter  is, en  zeker zo 
          duidelijk.
          
          Als  een  struct  eenmaal  'bekend'  is,  of  beter  gezegd: 
          gedefinieerd  is, kan  een struct precies zo gebruikt worden 
          als een  gewone variabele,  al zijn er een paar restricties. 
          Zo  kan een struct niet als parameter van een functie worden 
          gebruikt, maar  een pointer  naar een struct mag wel, en dat 
          voldoet in bijna alle gevallen even goed.
          
          Een vervelender  restrictie is  het feit dat de '=' operator 
          niet gebruikt mag worden op structs: we moeten de leden stuk 
          voor  stuk kopieren,  of gebruik  maken van  de 'movmem'- of 
          'memcpy'-functie.  (Deze  functies worden  verderop in  deze 
          tekst besproken.)
          
          Wat  wel weer  mag is  in de  definitie van  een struct  een 
          andere struct  gebruiken. Een  uitgebreid voorbeeld  hiervan 
          staat aan het eind van het volgende gedeelte.
          
          
          
                                  U N I O N S 
          
          Unions  lijken een  beetje op  structs. De  manier waarop ze 
          gedefinieerd en  gebruikt worden is zelfs precies gelijk aan 
          structs,  als je het woord 'union' voor 'struct' invult. Het 
          grote  verschil  is dat  in een  struct alle  leden tegelijk 
          bruikbaar zijn, en in een union slechts een lid tegelijk. Of 
          om het anders te zeggen: in een union zijn de leden 'bovenop 
          elkaar' gelegd, en in een struct 'naast elkaar'.
          
          Een voorbeeldje maakt alles misschien duidelijker:
          
                    union char_int { char letter;
                                     int getal; } uvar;
          
          We kunnen nu bijvoorbeeld doen:
          
                    uvar.letter = 'A';
          
          Nu bevat uvar.letter het karakter 'A', en is uvar.getal niet 
          gedefinieerd. Na
          
                    uvar.getal = 123;
          
          bevat  uvar.getal  de  waarde  123,  en  is  de  waarde  van 
          uvar.letter niet gedefinieerd.
          
          Hadden we  een struct gebruikt, in plaats van een union, dan 
          had uvar.letter nu nog steeds de waarde 'A', maar een struct 
          gebruikt  daarom  wel  meer  geheugen:  een struct  gebruikt 
          evenveel  geheugen als de som van het geheugengebruik van de 
          afzonderlijke  leden,  een  union  gebruikt  slechts  zoveel 
          geheugen als het grootste lid.
          
          Bovenstaand voorbeeld  zal wel  het gebruik, maar zeker niet 
          de zin van de union duidelijk maken. Als leden van een union 
          worden   dan  ook   meestal  zelf  ook  samengestelde  typen 
          (array's, structs,  unions) gebruikt,  al dan niet samen met 
          simpeler  typen.  Als  voorbeeld  een  door  mijzelf  in een 
          programma gedefinieerde struct:
          
          struct node { char nodetype;
                     union {
                       struct { struct node *left, *right; } ptr_node;
                       CONSTANT con_node;
                       VARIABLE var_node; } nodeinfo ; };
          
          CONSTANT  en  VARIABLE  zijn  met  een  'typedef'  (zie 2.4) 
          gedefinieerde  types. Binnenin  de struct  'node ' worden op 
          hun  beurt  een  struct  en  een  union gedefinieerd.  Om de 
          variabele  'left'   te  bereiken   is  na  de  naam  van  de 
          struct-variabele  nodig:  '.nodeinfo.ptrnode.left',  waarbij 
          'nodeinfo'  de naam  is van de union, het tweede lid van van 
          de struct  'node', 'ptrnode'  de naam wan het eerste lid van 
          de  union, en  zelf een  struct, en  'left' de  naam van het 
          eerste lid van de binnenste struct. Het is even wennen.
          
          Overigens, omdat  ik juist 'left' en 'right' heel vaak moest 
          gebruiken, heb ik de volgende twee #defines gemaakt:
          
                    #define NLEFT nodeinfo.ptrnode.left
                    #define NRIGHT nodeinfo.ptrnode.right
          
          Dat voorkomt kramp in de typevingers.
          
          
                 N I E U W E   T Y P E N   D E F I N I E R E N 
          
          In  het geval  van structs  en unions  kunnen we een eenmaal 
          gedefinieerd type  telkens opnieuw gebruiken, als we er maar 
          voor  zorgen dat  we het  een 'tag name' hebben gegeven. Bij 
          arrays  of   ingewikkelde  pointer   types,  of  erger  nog: 
          combinaties ervan, zou het handig zijn (en fouten voorkomen) 
          als we het type slechts een keer moesten definieren. Dat kan 
          met een zogenaamde 'typedef'.
          
          Het  gebruik van 'typedef' is heel eenvoudig. Als je uitgaat 
          van de  declaratie van  de variabele  van het gewenste type, 
          volstaat  het om er het woordje 'typedef' voor te zetten, en 
          de naam  van de  variabele te  vervangen door de naam die je 
          het nieuwe type wilt geven.
          
          Als  je  bijvoorbeeld  een type  LONG wilt  maken (omdat  je 
          compiler - zucht - geen long's kent) kun je bijvoorbeeld als 
          volgt te werk gaan:
          
                    typedef char LONG[4];
          
          Een  LONG is dan gedefinieerd als een array van 4 char's, en 
          kan nu in declaraties worden gebruikt, zoals:
          
                    LONG lang_getal1, lang_getal2;
          
          Natuurlijk heft  dat geen van de beperkingen op die voor het 
          specifieke datatype gelden en dus zal bijvoorbeeld
          
                    lang_getal1 = lang_getal2;
          
          tot  een foutmelding  leiden, zoiets  als "must  be lvalue", 
          omdat we hier in feite arraynamen, en dus pointerconstanten, 
          gebruiken! (Dat  heeft weer  wel als voordeel dat we bij ons 
          nieuwe  type deze  arraynaam zonder  meer als  parameter aan 
          functies kunnen doorgeven, zonder dat we specifiek het adres 
          hadden moeten doorgeven; alles heeft voor- en nadelen.)
          
            - Het volgende tekstdeel kunt u in het submenu vinden -
          
          
          
             - Het eerste tekstdeel kunt u in het submenu vinden -
          
                    D E   P R O G R A M M E E R T A A L   C 
          
                                  D E E L   5 
                                               
          
          
                     G E H E U G E N B E H E E R   I N   C 
          
          PASCAL programmeurs zullen zich bij al die pointers wel even 
          het hoofd hebben geschud. Immers, de enige manier waarop zij 
          met pointers  kunnen werken  is na  een NEW, die een nieuwe, 
          anonieme   variabele   cre�ert,   en   een   pointer  ernaar 
          teruggeeft.  Dat is (officieel) de enige manier waarop je in 
          PASCAL aan een pointer kunt komen.
          
          Deze  beperkingen  zijn  waarschijnlijk  opzettelijk,  omdat 
          pointers tevens  een belangrijke bron van fouten zijn. Zelfs 
          binnen   het  keurslijf  van  PASCAL  kunnen  pointers  naar 
          niet-meer-bestaande objecten overblijven na een MARK/RELEASE 
          of DISPOSE.
          
          Toch  is de  keuze die  men voor PASCAL gemaakt heeft zo gek 
          nog niet:  bij anonieme  variabelen moet  je immers  wel met 
          pointers  werken,  omdat  je  er  niet  bij  naam  naar  kan 
          verwijzen,  en bij  veel algorithmen  die gebruik  maken van 
          bomen  ('trees')  en  lijsten  ('linked  lists')  -  en  dus 
          pointers -  moet je  nieuwe objecten  kunnen cre�ren en oude 
          vernietigen.   Vandaar  dat   men  die   link  (woordspeling 
          opzettelijk!) tussen geheugenbeheer en pointergebruik legde.
          
          Inderdaad, geheugenbeheer.  Voor wie  het nog niet door had: 
          als  je  een  nieuwe  variablele  wilt  cre�ren  dan heb  je 
          (ongebruikt)  geheugen nodig,  en dat  geheugen moet  ergens 
          vandaan komen.  In C  zijn daar verschillende manieren voor, 
          de meest gebruikelijke zijn echter:
          
                    calloc(aantal, grootte)
                    malloc(grootte)
                    alloc(grootte)
          
          De   functie  'calloc'  reserveert  geheugen  voor  'aantal' 
          objecten van  'grootte' bytes,  'malloc' en 'alloc' een blok 
          van  'grootte'  bytes.  Deze twee  laatste doen  dus precies 
          hetzelfde, tenminste op byte-georienteerde systemen zoals de 
          Z80. Alle parameters van alledrie de functies zijn unsigned, 
          en  alledrie geven  ze een char * terug naar het eerste byte 
          in het  blok geheugen,  of NULL  als er onvoldoende geheugen 
          beschikbaar is.
          
          In  tegenstelling tot  PASCAL kunnen  we dus niet direct een 
          bepaald  object  cre�ren, we  zijn er  zelf verantwoordelijk 
          voor  om  de  juiste  hoeveelheid  geheugen  te  reserveren. 
          Gelukkig is dat niet zo moeilijk. Willen we bijvoorbeeld een 
          nieuw object van het type 'struct name' (zie eerder) dan kan 
          dat met
          
                    malloc(sizeof(struct name))
          
          en bij  wat strengere  compilers moeten  we bovendien  casts 
          gebruiken, en dan wordt het bovenstaande:
          
                  (struct name *)malloc((unsigned)sizeof(struct name))
          
          Dat   ziet  er  lelijk  -  en  onbegrijpelijk  -  uit.  Zo'n 
          wanproduct kunnen we binnen een aparte functie plaatsen (wat 
          we kunnen  combineren met  een actie  wanneer er onvoldoende 
          geheugen beschikbaar is) zoals:
          
          struct name *newname()
           {
             char *temp;
          
             if ((temp = malloc((unsigned)sizeof(struct name))) ==
                NULL) { fputs("Onvoldoende geheugen!\n", stderr);
               exit(1); } /* einde programma! */
          
             return (struct name *)temp;
           }
          
          Als   onze  preprocessor   voldoende  krachtig  is,  en  ook 
          #define's met  parameters accepteert  (zoals ASCII-C) kunnen 
          we ook het volgende doen:
          
                  #define NEW(x) (((x) *)malloc((unsigned)sizeof(x)))
          
          en  dan reserveren  we altijd de juiste hoeveelheid geheugen 
          en krijgen we altijd het juiste type pointer terug, wat voor 
          data-type we  ook voor x invullen. Nu kunnen we eenvoudig "a 
          la PASCAL"
          
                    NEW(struct name)
          
          doen.  Et voila! Deze manier is heel efficient, vooral omdat 
          het ons  geen byte aan code extra kost: al die casts zijn er 
          tenslotte voor de interne administratie van de compiler.
          
          Omdat  'calloc(aantal, grootte')  precies hetzelfde doet als 
          'malloc(aantal  *  grootte)'  zal  'calloc'  hier  wel  geen 
          verdere  toelichting  behoeven.  Waar  in  de  rest van  het 
          verhaal  'malloc'  staat kan  dus ook  - mutatis  mutandis - 
          'calloc' of 'alloc' worden gelezen.
          
          Het geheugen dat we met een van bovenstaande functies hebben 
          gekregen kunnen  we ook  aan het  systeem teruggeven, waarna 
          het  weer voor beschikbaar wordt voor een volgende 'malloc', 
          'alloc' of 'calloc'. Hiervoor is de functie
          
                    free(pointer)
          
          beschikbaar.  De  parameter 'pointer'  is een  char *  en we 
          moeten hier  dezelfde pointer  gebruiken die we via 'malloc' 
          e.d.  hebben gekregen.  De functie  'free' geeft geen waarde 
          terug.
          
          Meer voor  de volledigheid  dan om praktische redenen wil ik 
          hier  nog vermelden  dat C nog een functie heeft om geheugen 
          te  reserveren,   namelijk  'sbrk'.  Niet  alle  C-compilers 
          ondersteunen deze functie, en bovendien kan met deze functie 
          gereserveerd  geheugen  niet  terug worden  gegeven aan  het 
          systeem. Wederom een Unix-relikwie.
          
          Ook  kent  C  nog  een  paar, weinig  gebruikte functies  om 
          blokken geheugen te verplaatsen of te vullen. Deze zijn:
          
                    memcpy(dst, src, len)
                    movmem(src, dst, len)
                    memset(dst, byte, len)
                    setmem(dst, len, byte)
          
          De  functies  'memcpy'  en  'movmem'  verplaatsen  een  blok 
          geheugen  dat  'len'  bytes  groot is  van 'src'  naar 'dst' 
          ('len'  is  unsigned,  'dst'  en  'src'  zijn char  *). Deze 
          functies zijn identiek, op de volgorde van de argumenten na. 
          Overigens mogen  de 'src' en 'dst' gebieden niet overlappen. 
          De  functies 'memset'  en 'setmem' vullen een geheugengebied 
          vanaf 'dst', dat 'len' bytes groot, is met de waarde 'byte', 
          een char.  Ook bij  deze twee functies is alleen de volgorde 
          van  de argumenten  verschillend. Aangezien  het ANSI comite 
          aan de  functies 'memcpy'  en 'memset'  de voorkeur geeft is 
          het misschien verstandig dit zelf ook te doen.
          
          
               S T R A T E G I E E N   V A N   G E H E U G E N - 
          
                B E H E E R :   E E N   V E R G E L I J K I N G 
          
          Vooral  assemblyprogrammeurs zullen  bij bovenstaand verhaal 
          de oren  hebben gespitst  en denken:  "maar zijn er dan geen 
          restricties in de volgorde waarin geheugen wordt aangevraagd 
          en  teruggegeven, en moet bij 'free' niet de grootte van het 
          blok geheugenblok worden vermeld?" Op beide vragen luidt het 
          antwoord:  nee!  Het  geheugenbeheersysteem  van  C  is  erg 
          flexibel wat  dat betreft,  maar voor  alles moet  een prijs 
          worden betaald.
          
          In  de eerste plaats moet het systeem zelf bijhouden hoeveel 
          geheugen er  in een blok zit. De meestgebruikte manier omdat 
          te  doen is  de grootte van een blok direct voor dat blok op 
          te slaan, wat op zich al wat geheugen kost.
          
          In  de  tweede plaats  moet een  blok een  bepaalde minimale 
          grootte hebben,  of soms zelfs een veelvoud van een bepaalde 
          waarde,  omdat er  zelfs in  een ongebruikt geheugenblok nog 
          bepaalde informatie moet worden opgeslagen. Deze twee feiten 
          maken het  onaantrekkelijk om  zeer veel  kleine blokken  te 
          gebruiken, omdat anders het verlies erg groot wordt. In zo'n 
          geval  kan het  aantrekkelijk zijn een of meer grote blokken 
          geheugen aan  te vragen,  en daar  zelf een  beheerstrategie 
          voor  uit te  denken. Vooral als we met stukjes geheugen van 
          allemaal  gelijk  formaat willen  werken kan  dit vaak  heel 
          eenvoudig.
          
          Als   laatste   probleem  wil   ik  noemen   het  (mogelijk) 
          versnipperen van  het geheugen  in allemaal  kleine blokken, 
          wat  tot gevolg  kan hebben dat er geen blokken geheugen van 
          een bepaalde  minimale grootte meer beschikbaar zijn, hoewel 
          de totale hoeveelheid vrij geheugen ruim voldoende kan zijn. 
          Een  en ander  hangt af  van in welke volgorde we de blokken 
          aanvragen  en  weer  vrijmaken,  en hoe  het formaat  van de 
          verschillende blokken  uiteenloopt. In  de praktijk valt het 
          echter allemaal wel mee.
          
          Als vergelijking  wil ik  hier noemen de manier waarop BASIC 
          met  strings  omgaat.  Strings  in  BASIC zijn  variabel van 
          lengte,  en voor  een string  kan dus geen vaste hoeveelheid 
          geheugen worden gereserveerd. Maar de geheugenallocatie voor 
          strings wordt veel simpeler aangepakt, en geheugen wordt ook 
          niet expliciet  weer vrijgegeven.  Als gevolg daarvan is het 
          geheugen  dan  ook  vrij  snel  vol, en  vervolgens gaat  de 
          interpreter  een zogenaamde  'garbage collection' uitvoeren, 
          die uiteindelijk  tot gevolg  heeft dat  alle bezette  delen 
          achterelkaar  worden geplaatst,  en er  een enkel  vrij blok 
          geheugen ontstaat.
          
          Dit klinkt op het eerste gezicht aantrekkelijk, maar er zijn 
          een  paar   problemen.  Het  belangrijkste  is  wel  dat  de 
          informatie  verschuift, en alle pointers naar die informatie 
          aangepast moeten worden. In BASIC gaat dat nog wel, omdat de 
          interpreter  het  beheer  over  die  pointers  (lees: string 
          descriptors) voert,  en precies  weet waar ze zich bevinden. 
          In  C is  dit niet mogelijk, omdat de pointers (en mogelijke 
          kopieen daarvan)  over het  gehele geheugen verspreid kunnen 
          zijn, vaak in de geheugenblokken zelf!
          
          Dat zo'n  garbage collection  ook nog  eens bijzonder  traag 
          gaat  is de BASIC programmeur wel bekend, hoewel dat laatste 
          voor een  niet onbelangrijk  deel aan de 'gierigheid' van de 
          BASIC-interpreter  ligt: als  ze twee bytes extra per string 
          hadden geinvesteerd hadden ze de garbage collection een orde 
          sneller kunnen  maken, en  hadden bovendien niet alle string 
          descriptors  in  hetzelfde  geheugendeel  hoeven  liggen. Ik 
          spreek  hier  uit  ervaring,  want  ik  heb  zelf  eens  een 
          stringuitbreiding  voor FORTH  geprogrammeerd, en ik heb het 
          systeem nooit  zichtbaar zien  vertragen tijdens een garbage 
          collection.
          
          Maar  het feit  dat de informatie in zo'n systeem van plaats 
          kan  veranderen  kan  in  veel gevallen  toch tot  problemen 
          leiden, en  vraagt de nodige discipline van de gebruiker. In 
          bovenstaand FORTH systeem worden strings dan ook in een keer 
          naar  de stack  gekopieerd voor ermee gewerkt mag worden, en 
          ook weer  in een  keer weggeschreven,  en dat  maakt een  en 
          ander   toch  weer  wat  minder  efficient.  (Al  past  deze 
          werkwijze wel bij uitstek in de FORTH-filosofie!)
          
          Binnen TURBO  PASCAL kan  op ongeveer dezelfde manier worden 
          gewerkt  als in C met NEW/DISPOSE of GETMEM/FREEMEM, al moet 
          bij FREEMEM  in tegenstelling  tot C  wel de grootte van het 
          geheugenblok  worden  opgegeven.  Binnen  TURBO  PASCAL  kan 
          echter  ook met  MARK/RELEASE worden  gewerkt (maar dan niet 
          met DISPOSE  of FREEMEM!)  om alle  blokken die  aangevraagd 
          zijn  na een  bepaald eerder blok weer vrij te geven. Een en 
          ander   functioneert   dan   ongeveer   als  een   stack.  C 
          programmeurs die  hetzelfde willen  kunnen doen  moeten daar 
          zelf een routine voor schrijven, die echter bijzonder simpel 
          is als we in een enkel, aaneengesloten blok geheugen werken.
          
          
          
                            I N I T I A L I Z E R S 
          
          Een  van de meestgebruikte statements in BASIC is 'DATA'. Er 
          zijn nauwelijks  programma's van  enige omvang die niet zijn 
          gezegend(?!)  met  een  grote hoeveelheid  van deze  dingen, 
          meestal   aan  het  eind.  Zelfs  als  we  die  DATA's  niet 
          meerekenen die gebruikt zijn om een stukje machinetaal in te 
          zetten -  omdat het programma anders niet vooruit te branden 
          is  - blijkt  dat we er nog een hoop overhouden met tabellen 
          en dergelijke.
          
          Handige programmeurs  maken vaak gebruik van tabellen, omdat 
          het  programma  er  vaak een  stuk simpeler  en kleiner  van 
          wordt.  Ook op  dit gebied laat C niemand in de steek, omdat 
          we al  onze variabelen  mogen intialiseren met een waarde of 
          waarden  bij de  declaratie ervan.  Dit kan  met een simpele 
          variabele als volgt:
          
                    int var1 = 0;
                    char var2 = 'N';
                    unsigned var3 = 5 * 4;
                    char *var4 = "En dit is ook een constante!";
          
          We mogen  variabelen alleen initialiseren met constanten, of 
          constante  uitdrukkingen. Samengestelde  typen mogen  echter 
          alleen  initialiseren  als  het  statische  variabelen  zijn 
          (buiten een  functie is  dat altijd  zo). Voor  automatische 
          variabelen (die komen alleen binnen een functie voor) gelden 
          dezelfde  beperkingen als die voor een assignment. (In feite 
          wordt het  ook naar een assignment vertaald, wat logisch is: 
          bij het aanroepen van de functie moet de initialisa-
          tie telkens opnieuw plaatsvinden!)
          
          In  het het  laatste voorbeeld van bovenstaand rijtje moeten 
          we ons realiseren dat we een pointer variabele initialiseren 
          zodat hij  naar die  string wijst; de string zelf zit op een 
          andere plaats in het geheugen. Doen we echter dit:
          
                    char var4[] = "En nu vullen we een array!";
          
          Dan  wordt var4 een array, en gevuld met boventaande string. 
          Deze methode  is wat  efficienter want we sparen een pointer 
          variabele  uit. We hoeven ook niet op te geven hoe groot het 
          array  moet  worden, want  dat is  gewoon de  lengte van  de 
          string met  afsluitende nul:  dat zoekt de compiler zelf wel 
          uit.
          
          Een   string  kan   dus  twee  betekenissen  hebben  in  een 
          initializer. De  tweede manier  gebruikt de  string als  een 
          blok  gegevens, maar dit kan ook door een aantal constanten, 
          of constante  uitdrukkingen, gescheiden door komma's, tussen 
          accoladen  te zetten.  De volgende twee declaraties zijn dan 
          ook volledig identiek:
          
                    char str[] = "String";
                    char str[] = { 'S', 't', 'r', 'i', 'n', 'g', 0 };
          
          In het  algemeen is de eerste vorm wat duidelijker, maar die 
          kunnen  we natuurlijk  alleen gebruiken  als het  om strings 
          gaat.  In  alle andere  gevallen en  bij alle  samengestelde 
          typen  moeten   we  van  dit  soort  gegevensblokken  tussen 
          accoladen  gebruiken. In  het geval  we samengestelde  typen 
          binnen  samengestelde   typen  gebruiken   moeten  we  zelfs 
          gegevensblokken   binnen  gegegevensblokken   gebruiken.  We 
          moeten zo'n  blok dus  eigenlijk zien  als een samengestelde 
          constante. Hier een aantal voorbeelden:
          
                    int rij[] = { 1, 2, 3 };
                    int matrix[][] = { { 1, 2, 3 }, { 4, 5, 6 },
                                       { 7, 8, 9 } };
                    struct point pnt3 = { 0, 1, -1 };
                    struct point vlak[] = { { 1, 0, 0 }, { 0, 1, 0 },
                                            { 0, 0, 1} };
                    char *teksten[] = { "tekst1", "tekst2", "tekst3",
                                        "tekst4" };
          
          
          In  het laatste  geval hebben we een array van char pointers 
          gemaakt,  en  elk van  die pointers  wijst naar  een van  de 
          teksten. Verder  nog vragen?  Geen vragen!  En dus wordt dit 
          het einde van deze aflevering!
          
                                                         Robert Amesz
          
          
          P.S.:  Het zal  misschien een  aantal mensen opgevallen zijn 
          dat een voorbeeldprogramma deze aflevering ontbreekt. Sorry. 
          Dat  komt  pas  bij  aflevering  6,  wanneer het  over zulke 
          diverse onderwerpen  zal gaan als modulair programmeren, het 
          maken  van  functiebibliotheken,  debuggen,  combineren  met 
          machinetaal, enz. enz.
