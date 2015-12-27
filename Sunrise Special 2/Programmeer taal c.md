      D E   P R O G R A M M E E R T A A L   C   ( 1  )
                                                       


              O V E R   D E Z E   C U R S U S 

Deze  cursus  is  bedoeld  voor  MSX'ers  die  nader  willen 
kennismaken    met     de    programmeertaal     C.    Enige 
programmeerkennis  in  BASIC  of  PASCAL  kan  nuttig  zijn, 
aangezien  de overeenkomsten  en verschillen  tussen C en de 
andere twee  talen uitgebreid  aan bod  zullen komen. Tevens 
hoop  ik dat  het aantal  C-programmeurs door dit initiatief 
nogal  zal  stijgen,  zodat  een  gezonde  uitwisseling  kan 
ontstaan  van  ide�en,  technieken,  routines of  zelfs hele 
bibliotheken.


       D E   C - C O M P I L E R S   V O O R   M S X 

Natuurlijk  is het  nuttig om een C-compiler te bezitten, om 
de  programmeervoorbeelden  zelf te  kunnen compileren.  Bij 
mijn   weten   zijn   de   volgende  C-compilers   voor  MSX 
beschikbaar:

+  HiSoft C++  CP/M  produkt,  is  echter  in  een voor  MSX 
               aangepaste  vorm beschikbaar.  Geen PD,  maar 
               toch ruim in BBS'en te downloaden. Overigens, 
               C++ is  niet de C++ zoals het ANSI-comitee de 
               object-oriented  versie van  C gedoopt heeft. 
               Kent geen float's en long's.

+  ASCII-C     Speciaal voor  MSX. Wat  bewerkelijker in het 
               gebruik  dan C++, maar levert zeer efficiente 
               code  op,  en  stimuleert  bovendien modulair 
               programmeren. Verder zeer flexibel. Kent geen 
               float's en long's. Ook geen PD, maar eveneens 
               goed  verspreid   onder  de   MSX'ers.  Werkt 
               voortreffelijk samen met assembler.

+  SMALL C     CP/M    produkt.   Een   Public   Domain   C, 
               oorspronkelijk gepubliceerd  in Dr. Dobbs. Er 
               zijn  vele  varianten  van in  omloop. (Nvdr: 
               Natuurlijk     omdat    de    source    wordt 
               meegeleverd!) Levert helaas weinig efficiente 
               of compacte code, en kent in vergelijking met 
               de  voorgaande   twee  compilers   de  meeste 
               beperkingen,  met name de datastructuren zijn 
               zeer beperkt.  Is daardoor niet echt geschikt 
               voor wat grotere programma's.
                    
   GST-C       Speciaal  voor MSX. Lijkt van SMALL-C te zijn 
               afgeleid    en    kent   ongeveer    dezelfde 
               beperkingen. Geen PD.

   BDS-C       CP/M produkt.  Een wat  oudere compiler,  die 
               wel  echter  het  volledigst  de  C-standaard 
               lijkt  te volgen.  Verder bij  mij weinig van 
               bekend.

Verder zijn  andere C-compilers  voor CP/M ook te gebruiken, 
zoals  AZTEC-C,  MIX-C,  enzovoort.

De met een + gemerkte compilers heb ik zelf. Wellicht dat er 
nog mensen  zijn die  een van  de andere  C-compilers in hun 
bezit  hebben, of  compilers die  zelfs niet  in het lijstje 
staan: in  dat geval zou ik die graag eens ontvangen, om een 
vergelijkend  onderzoekje  te  doen. (Snelheid,  grootte van 
programma's, volledigheid).


       G E B R U I K   V A N   C - C O M P I L E R S 

C-compilers zijn  wat omslachtiger in het gebruik dan BASIC, 
of  Turbo PASCAL (bij kleine programma's, tenminste). Om een 
programma   te   compileren   zijn   de   volgende   stappen 
noodzakelijk:

      + Editen van het ASCII-file met de programmatekst
     (+ Pre-compileren)
      + Compileren
     (+ Assembleren)
     (+ Optimaliseren)
     (+ Linken)

De  stappen  tussen haakjes  zijn niet  altijd noodzakelijk. 
ASCII-C  heeft  vier  van  de  vijf  stappen  nodig  (alleen 
optimaliseren  niet),  HiSoft C++  darentegen maar  twee. De 
meeste compilers zitten tussen deze twee uitersten in.

Na  de  laatste  stap  hebben  we  als alles  goed ging  een 
.COM-file  gecre�erd,  wat direkt  onder MSX-DOS  kan worden 
uitgevoerd. Als het werkt: prima! Maar werkt het niet: "back 
to the drawing-board".

 
                     I N L E I D I N G 

In de  loop der  tijden hebben  computerdeskundigen heel wat 
manieren gevonden om computers te programmeren. De oudste en 
primitiefste manier was programmeren in machinetaal door het 
leggen  van draadverbindingen!  Onnodig te zeggen dat zoiets 
alleen gaat  bij de allerkleinste prorammaatjes. Ook uit die 
tijd  stamt de  term "bug"  (= fout in het programma), omdat 
insekten  toen   daadwerkelijk  voor  sluitingen  -  en  dus 
veranderingen  in het programma - in de grote panelen konden 
zorgen. In  die tijd  was programmeren  dan ook voorbehouden 
aan de experts.

Deze manier van programmeren moest gelukkig al snel het veld 
ruimen  voor wat  geavanceerdere methodes:  de assembler was 
geboren, en programmeren werd al een heel stuk genoeglijker. 
Toch bleven  er problemen.  In die tijd volgde het ene model 
computer  het andere  snel op, en ieder nieuw model had weer 
een wat  andere machinetaal,  en dus  moesten de programma's 
iedere  keer  door  programmeurs  vertaald  worden. Duur  en 
omslachtig.

Dus  deden de  eerste hogere  programmeertalen hun  intrede: 
COBOL (COmmon  Business Oriented Language), FORTRAN (FORmula 
TRANslation)  en  ALGOL  (ALGOrithmic Language).  De laatste 
twee  zijn de  stamvaders van  de moderne  programmeertalen. 
BASIC (Beginners  All-purpose Symbolic  Instruction Code) is 
duidelijk  afgeleid van (een vroege vorm van) FORTRAN. BASIC 
was in principe alleen bedoeld als oefentaal voor beginnende 
programmeurs, en  de ontwerpers  ervan hebben  dan ook  geen 
moeite  gedaan om  er structuur in aan te brengen, iets waar 
ook de moderne BASIC's nog steeds last van hebben.

ALGOL was  de eerste  programmeertaal die met enig beleid is 
ontworpen.  Een speciaal  comitee, voornamelijk bestaand uit 
wiskundigen, heeft begin zestiger jaren de defintie van deze 
taal  in  een  rapport  vastgelegd. Uit  deze taal  is later 
PASCAL voortgekomen. (PASCAL is gelukkig eens geen acroniem: 
de naam komt van Blaise Pascal, een Frans wiskundige van een 
paar eeuwen  terug.) De  syntax van PASCAL is bijna identiek 
aan  die van ALGOL, en is op sommige punten zelfs versimpeld 
om het  de compiler makkelijk te maken. Wel kent PASCAL meer 
data-typen dan ALGOL.

De taal  C is  in principe ook afgeleid van ALGOL, maar niet 
direct. Ertussen zitten nog de talen BCPL en B, die speciaal 
voor  de relatief  kleine computers  in de PDP-serie bedoeld 
waren.  Ook   C  was   in  eerste   instantie  bedoeld  voor 
systeemprogrammatuur   op   een   PDP-11   onder   UNIX,  en 
instructies  als "++"  zijn in  de taaldefinitie  gekomen om 
optimaal  gebruik  te maken  van de  instructieset van  deze 
processor.  (Overigens,  de instructieset  van de  "moderne" 
68000 processor vertoont opmerkelijke overeenkomsten met die 
van de PDP-11.)

In  C  kun  je  heel  abstract  programmeren, maar  ook heel 
effici�nt en  machinespecifiek. Dit  maakt C  zelfs in  zijn 
basisvorm  een krachtige en prettige programmeertaal. Andere 
talen  -   en  met  name  Turbo  PASCAL  -  moeten  allerlei 
niet-gestandaardiseerde    uitbreidingen   maken    om   die 
flexibiliteit  te   benaderen.  Evenaren   is  zelfs   bijna 
onmogelijk, zeker waar het het gebruik van pointers betreft. 
Het nadeel is wel dat er hele obscure bug's kunnen ontstaan, 
en  dat is  waarschijnlijk de reden dat er zo weinig MSX-ers 
in C programmeren.

Veel C-compilers staan ook toe om zelf bibliotheken te maken 
met  nieuwe  functies, en  die ook  APART te  compileren. In 
ASCII-C    is    zelfs    het    grootste   deel    van   de 
standaardbibliotheek in  C zelf  geschreven. Dit alles maakt 
het  mogelijk om  zgn. "modulair"  te programmeren,  wat wil 
zeggen: een  groot programma  in kleinere deelprogramma's op 
te   splitsen  en   apart  te  programmeren.  Dit  houdt  de 
programmeerarbeid niet alleen overzichtelijk, maar het maakt 
het ook  gemakkelijk om meerdere mensen aan een programma te 
laten werken door het werk eenvoudig te verdelen.

De   taal   C   is  -   net  als   PASCAL  -   een  "kleine" 
programmeertaal,  wat inhoudt  dat de taaldefinitie klein en 
compact  is  gehouden.  I/O  valt  in  principe buiten  deze 
definitie, maar  er zijn in de loop der tijden toch bepaalde 
conventies en standaardbibliotheken ontstaan, waar we ons in 
de  cursus maar  van zullen bedienen. Eveneens is C een zgn. 
"free  format"   taal,  wat   inhoudt  dat   je  de   in  de 
programmatekst   willekeurig   spaties,   tabs   of   crlf's 
(newlines)  mag gebruiken, mits er op die manier geen namen, 
strings  of  symbolen  in  stukken worden  gesneden. Dit  in 
tegenstelling  tot   bijvoorbeeld  COBOL,  waar  voor  ieder 
programmaelement  is  vastgelegd  op  welke  kolom die  moet 
beginnen.

Ook  in de  cursus zal  er aan  de hand  van voorbeelden  de 
verschillende taalelementen  worden uitgelegd. Dus eerst een 
voorbeeldprogramma,  gevolgd door  de uitleg. Tussendoor zal 
wat dieper op bepaalde onderwerpen worden ingegaan.


| /*                                                       |
| De listings  in de  tekst staan  tussen deze  tekens. Ze |
| maken  geen deel  uit van  het programma. Ze zijn alleen |
| bedoeld om  duidelijk te  maken waar  de listings  in de |
| tekst  staan. Waar een enkel statement in de tekst wordt |
| besproken,  zijn   die  dingen   niet  gebruikt,  alleen |
| volledige programma's!                                   |
| */                                                       |


       M I J N   E E R S T E   C - P R O G R A M M A 

|                                                          |
| #include <STDIO.H>                                       |
|                                                          |
| /* Mijn eerste programma */                              |
|                                                          |
| main() {                                                 |
|                                                          |
| printf("Hello, world!\n");                               |
|                                                          |
| }                                                        |
|                                                          |

Dit is het equivalent van het BASIC-programma:

     10 REM Mijn eerste programma
     20 PRINT "Hello, world"
     20 END

Om een of andere reden wordt dit C-programma altijd gebruikt 
voor de  beginner, en  met zo'n  oude traditie  wil ik  niet 
breken.

De eerste regel

     #include <STDIO.H>

is  bedoeld om  een aantal standaardfuncties, -definities en 
-constanten  beschikbaar  te maken  voor de  programmeur. In 
feite betekent het alleen dat het file "STDIO.H" (staat voor 
STandarD I/O) wordt opgenomen in het programma. Dit file kun 
je ook  gewoon bekijken met TYPE (onder DOS, natuurlijk). De 
'<'  en de '>' stellen een soort quotes voor, maar de gewone 
'"' kan  meestal ook.  Met '#include  "file"' kan  overigens 
iedere  file  in  de  programmatekst  worden opgenomen,  met 
bijvoorbeeld  eigen definities  of constanten. Wel moet zo'n 
'#include' helemaal  aan het begin van een regel staan! (Dit 
is dus een uitzondering op het bovengenoemde free-format).

De  volgende  regel  is  een  commentaarregel.  De  compiler 
negeert alles tussen '/*' en '*/'.

Vervolgens staat er

     main() {

Dit   is   in  feite   een  functiedefinitie,   maar  zonder 
parameters, zodat  er niets  tussen de  haakjes staat. In de 
ruimte  tussen  ')'  en  '{'  moeten  de  datatypes  van  de 
parameters  worden gegeven,  dus die ruimte blijft ook leeg. 
De naam 'main' is een bijzondere in C; het geeft aan dat dit 
het hoofdprogramma  (main program)  is. Welke volgorde je je 
functies  ook hebben, de uitvoering van het programma begint 
altijd  bij  'main'.  Na  de  '{'  volgt  de  daadwerkelijke 
functie,  de  zgn.  'body'.  Hier bestaat  die body  uit een 
statement, namelijk:

     printf("Hello, world!\n");

'printf'  is  een  I/O  functie, de  naam staat  voor "print 
formatted", zoiets  als PRINT USING in BASIC, maar je kan er 
ook  gewoon strings  mee afdrukken. De '\n' aan het eind van 
de string  betekent dat er naar een nieuwe regel moet worden 
gesprongen.   De  '\'   (backslash)  is   een  zgn.  'escape 
character',  wat  aangeeft  dat  het  volgende  karakter een 
bijzondere  betekenis  heeft. '\n'  staat dus  voor newline, 
meestal een  'linefeed' met  ASCII code  10, '\t' staat voor 
tab,  code 9,  en zo  zijn er  nog een  aantal. Met  '\nnn', 
waarbij nnn  een OCTALE constante is kun je alle ASCII codes 
van  0 tot  255 (0  tot 377 octaal, is dat) gebruiken, zelfs 
als de editor daar moeite mee heeft!

De ';'  aan het  eind van  de regel  geeft het  eind van het 
statement aan. Hoewel dit dus veel lijkt op PASCAL is er een 
belangrijk  verschil: in  PASCAL scheidt  de puntkomma  twee 
statements, en  in C is het de afsluiting van een statement! 
Als  je  daar  niet  op  verdacht bent  kan dat  onverwachte 
foutmeldingen opleveren!

De  '}' aan  het eind  van het  programma geeft  aan dat  de 
functie   ten    einde   is.    PASCAL-programmeurs   kunnen 
eenvoudigweg  voor de '{' 'BEGIN' lezen en voor de '}' 'END' 
om een en ander duidelijker te laten worden.

Nu  is  het  nog  zaak  om  het programma  te compileren  en 
eventueel  te   linken,  en  we  hebben  een  programma  dat 
vriendelijk   hallo  zegt.   Niet  het  meest  spectaculaire 
programma ter wereld, maar alle begin is eenvoudig.


        S I M P E L E   D A T A T Y P E N   I N   C 

Net als BASIC en PASCAL kent C een aantal simpele datatypen, 
die  -  net  als  PASCAL  -  als  basis  kunnen dienen  voor 
ingewikkelder datatypen. In deze paragraaf worden deze typen 
besproken. Met deze datatypen kunnen we variabelen cre�ren.

De basistypen zijn:

TYPE      GROOTTE        BEREIK              OMSCHRIJVING
------------------------------------------------------------
char      1 byte   0..255 of -128..127              karakter
int       2 bytes  -32768..32767             getal met teken
unsigned  2 bytes  0..65535               getal zonder teken
long      4 bytes  -2147483648..2147483647   getal met teken
float     4 bytes  +/- 1.0E+/-36  glijdende komma, 6 cijfers
double    8 bytes  +/- 1.0E+/-36 glijdende komma, 13 cijfers

De grootte  en de  precieze nauwkeurigheid van de 'float' en 
de  'double' hangen  nogal af van de implementatie, het zijn 
dus slechts richtwaarden. Bovendien onbreekt er in de meeste 
C-compilers voor  MSX ieder  spoor van floating-point typen, 
waardoor  e.e.a. nogal  academisch is.  Ook het  'long' type 
ontbreekt  meestal,  wat eigenlijk  veel lastiger  is, omdat 
sommige   standaard    bibliotheekfuncties   daarmee    zijn 
gedefinieerd.

De gegeven  groottes en bereiken zijn overigens minima, maar 
worden  op 8-bitters,  zoals onze  Z-80 bij mijn weten nooit 
overschreden.  Het  char-type moet  overigens altijd  1 byte 
groot zijn.  De precieze  grootte van elk datatype is altijd 
op  te  vragen  met  de  'SIZEOF'  pseudo-operator.  Zo moet 
'SIZEOF(char)'  dus altijd de waarde 1 opleveren. (Het wordt 
een pseudo-operator genoemd omdat er niets wordt uitgerekend 
door het  programma: de  compiler vult  er eenvoudigweg  een 
constante voor in).

Sommige  datatypen hebben  ook pseudoniemen.  Hier volgt een 
lijstje:

     short int      = int (op 8-bit computers, tenminste)
     unsigned int   = unsigned
     long int       = long
     long float     = double

(Nvdr. Deze  tekst was  langer dan  16 kB, u kunt het tweede 
gedeelte in het submenu vinden.)



                    - TWEEDE GEDEELTE -

         P R O G R A M M E R E N   I N   C   ( 1  )
                                                    


(Nvdr.  De tekst  was langer  dan 16  kB, u  kunt het eerste 
gedeelte in het submenu vinden.)


               V A R I A B E L E N   I N   C 

Dat  we  nu  de  simpele  data-types  kennen  in  C is  heel 
plezierig, maar we moeten natuurlijk daar ook variabelen mee 
kunnen declareren, d.w.z. cre�ren. Dit moeten we, net als in 
PASCAL, zelf doen. BASIC cre�ert een nieuwe variabele op het 
moment dat die voor het eerst gebruikt wordt.

De naam  van variabelen  mag bestaan uit letters, cijfers en 
'_', maar mag niet met een cijfer beginnen. Ook kan het zijn 
dat  de lengte  van een naam beperkt is, maar dat hangt weer 
van de compiler af.

Het declareren van een variabele gaat als volgt:

     <datatype> <naam>;

of:

     <datatype> <naam1>,<naam2>,.......<naamx>;

In  het  laatste  geval  worden  er  een  aantal  variabelen 
gedeclareerd  van  hetzelfde type.  De ';'  aan het  eind is 
wederom verplicht!

Voorbeelden:

     int x;
     int y, z;
     char lange_naam;
     unsigned catch22;
     float blub, blubblub;  


          V O O R B E E L D P R O G R A M M A   2 
      E E N   K W A D R A T E N T A B E L   M A K E N 

In dit  programma kan  kennis worden  gemaakt met wat simpel 
gereken  in C  en het  gebruik van  variabelen. Ook  komt de 
eerste lus aan bod, de do..while lus.

|                                                          |
| #include <STDIO.H>                                       |
|                                                          |
| #define MAXWAARDE 10                                     |
|                                                          |
| /* Rekenen en de do..while lus */                        |
|                                                          |
|  main() {                                                |
|                                                          |
|    int waarde,kwadraat;                                  |
|                                                          |
|    waarde = 0;                                           |
|                                                          |
|    do {                                                  |
|      kwadraat = waarde * waarde;                         |
|      printf("Waarde = %4d Kwadraat = %4d\n", waarde,     |
|                                              kwadraat);  |
|      ++waarde; }                                         |
|    while (waarde <= MAXWAARDE);                          |
|                                                          |
|    printf("\n\nThat's all folks!\n");                    |
|  }                                                       |
|                                                          |

De '#include'  kennen we  nog van het vorige programma, maar 
de  '#define'  is  nieuw. Het  is echter  een simpele,  doch 
krachtige   manier  om  de  ene  tekst  door  de  andere  te 
vervangen. Iedere  keer als  de compiler  dus het  hele (!!) 
woord  'MAXWAARDE' tegenkomt, wordt het door '10' vervangen. 
De computer "ziet" dus:

     while (waarde <= 10);

Maar 'MAXWAARDES'  zou niet veranderd worden, omdat per heel 
woord  gekeken wordt.  Hiervoor geldt  hetzelfde als voor de 
namen van  variabelen: letters,  cijfers en '_' mogen worden 
gebruikt.  De tekst,  waarin dat woord wordt verandert hoeft 
daar niet aan te voldoen. Met de regel

     #define ACCOLADE_OPEN {

is dus niets verkeerds.

Hoewel je  woorden op  die manier  door willekeurige  andere 
tekst  kunt vervangen,  wordt het meestal alleen gebruikt om 
constanten  te  definieren,  zoals  hierboven.  Een  van  de 
conventies  in   C  is   om  zulke   constanten  altijd   in 
hoofdletters  te schrijven,  vanwege de duidelijkheid. Zulke 
definities mogen  ook weer binnen andere '#define's gebruikt 
worden. Dus

     #define MAXKWADRAAT MAXWAARDE*MAXWAARDE

is prima.

Nadat  we  in  'main' zijn  aangeland, worden  de variabelen 
'waarde' en 'kwadraat' gedeclareerd, beide van het int-type. 
Vervolgens wordt met

     waarde = 0;

de  variabele  'waarde'  nul  gemaakt.  In  PASCAL  zou ':=' 
gebruikt  worden in  plaats van de '=', maar verder is alles 
hetzelfde.

Vervolgens gaan we de do..while lus binnen, maar daar heb ik 
het later over. Dan staat er:

     kwadraat = waarde * waarde;

De  '*'  betekent  dus  niets anders  dan vermenigvuldiging, 
tenminste als  binaire operator. (Dat wil zeggen dat er twee 
getallen  nodig zijn  bij een  vermenigvuldiging, want  'bi' 
betekent  'twee'.)   Als  unaire  operator  -  'un'  is  '1' 
-betekent  dat sterretje iets heel anders, maar daarover een 
andere keer.

Hierna komt:

 printf("Waarde = %4d  Kwadraat = %4d\n", waarde, kwadraat);

wat  ons  leert  hoe  je  met  'printf'  ook  getallen  kunt 
afdrukken. Hiervoor  is in  de string  tweemaal het gedeelte 
'%4d'  opgenomen, om  respectievelijk 'waarde' en 'kwadraat' 
op het  scherm te krijgen. Het procentteken geeft aan dat er 
iets moet worden afgedrukt, de '4' geeft het aantal posities 
aan  waarin dat moet gebeuren, en de 'd' tenslotte geeft aan 
dat  het  decimaal moet  gebeuren. Er  kan ook  'x' gebruikt 
worden voor hexadecimale uitvoer, of 'u' als er een unsigned 
moet worden afgedrukt. En dat is nog maar een klein deel van 
alle  mogelijkheden,  maar voorlopig  genoeg, lijkt  me. Als 
'waarde'  5  is en  'kwadraat' dus  25, komt  er dit  op het 
scherm:

     Waarde =    5  Kwadraat =   25

gevolgd door een newline. Let goed op de lege ruimte voor de 
getallen, doordat we een veldbreedte van 4 hebben opgegeven. 
Als het  getal te  breed is  voor het  veld, wordt het getal 
niet  smaller gemaakt maar toch helemaal afgedrukt. Door als 
veldbreedte dus  '0' op  te geven,  of de veldbreedte weg te 
laten  (wat  op  hetzelfde  neerkomt)  wordt  dus  zo weinig 
mogelijk ruimte gebruikt.

De functie printf mag een variabel aantal parameters hebben, 
maar  de format-string  is verplicht. Daaraan kan de functie 
aflezen hoeveel parameters er nog volgen. Twee maal de '%4d' 
in  bovenstaand   voorbeeld  gaf   dus  aan  dat  er  na  de 
format-string  nog  twee  parameters  volgden  van het  type 
'int'.  Een functie met een variabel aantal parameters wordt 
een 'variadic  function' genoemd. Een aantal compilers staan 
toe  zelf dit  soort functies te schrijven, maar de preciese 
details hoe je dat kan doen verschilt per compiler.

Daarna staat er

     ++waarde;

Dit is  voor niet-C programmeurs onbegrijpelijk koeterwaals. 
Het betekent echter alleen: tel 1 bij 'waarde' op. We hadden 
dus ook kunnen schrijven:

     waarde = waarde + 1;

De  andere schrijfwijze  is korter  en sneller,  wat in  dit 
voorbeeld misschien  niet zo  erg naar  voren komt,  maar in 
iets ingewikkelder berekeningen wel.


Dan nu de do..while lus. Als algemene vorm heeft zij:

 do <statement> while (<expression>);

Vertaald in het Nederlands:

 HERHAAL een statement ZOLANG (deze uitdrukking NIET nul is);

Dan  hebben we  meteen een probleem, want we hebben niet een 
enkel  statement,   maar  drie!  De  oplossing  is  gelukkig 
eenvoudig  want als  we een  aantal statements achter elkaar 
tussen  '{'  en  '}'  zetten,  "ziet"  C het  als een  enkel 
statement. Dit wordt een 'compound statement' genoemd.

Aan het einde van de lus staat:

     while (waarde <= MAXWAARDE);

Lees  voor  '<=' 'kleiner  of gelijk  aan'. De  '<=' is  een 
vergelijkingsoperator  die  0 oplevert  als de  vergelijking 
niet  waar   is,  en  een  waarde  ongelijk  aan  0  als  de 
vergelijking klopt.

Zolang  'waarde' in  ons voorbeeld  kleiner of gelijk is aan 
10, klopt de vergelijking, en levert dus een waarde ongelijk 
aan 0  op. Hierdoor wordt het programma weer vanaf het begin 
van  de  lus  doorlopen.  Zodra  'waarde' groter  dan 10  is 
geworden,  wordt niet meer naar het begin van de lus gegaan, 
en  vervolgt  het  programma  gewoon.  Hierdoor  krijgen een 
keurig lijstje  met de  getallen 0  t/m 10  en de  kwadraten 
daarvan.

Na de boodschap

     That's all folks!

stopt nu het programma.

Nog  even een  lijstje met  de vergelijkingsoperatoren die C 
rijk is:

     >    groter dan
     >=   groter dan, of gelijk aan
     <    kleiner dan
     <=   kleiner dan, of gelijk aan
     ==   gelijk aan (jawel, twee is-gelijk tekens!)
     !=   ongelijk aan



Volgende keer meer.

Vragen, opmerkingen of dreigbrieven aan:

                     Stichting Sunrise
                    T.a.v. Robert Amesz
                        Postbus 2146
                  2400 CC  Alphen a/d Rijn

Of plaats een bericht in dit BBS (wel zo handig, nietwaar?):

     Future Base
     Online ma-vr 22:00-06:00  za+zo 21:00-06:00
     Sysop: Johan Gijsman
     Tel. 071-222380
     

Een nuttig  naslagwerkje over  C dat voor plm. � 7,-- bij de 
Slegte te koop is:

     F. Wagner-Dobler
     "Zakboek C-language"
     Delfia-Press, Rijswijk, 1987
     ISBN 90-6449-024-4

Dit  is  geen  leerboek!  Er  staat  echter  bijzonder  veel 
informatie in.

                                               Robert Amesz