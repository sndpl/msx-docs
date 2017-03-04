# D E   P R O G R A M M E E R T A A L   C  - D E E L   3 

## I N L E I D I N G 

Voor  BASIC programmeurs  is het  gebruik van strings net zo 
gemakkelijk als  getallen: BASIC zoekt zelf wel uit hoe lang 
strings  zijn  en  waar ze  in het  geheugen terecht  moeten 
komen.   PASCAL  programmeurs  hebben  het  wat  moeilijker, 
aangezien PASCAL  eigenlijk geen strings kent (alleen ARRAYS 
OF  CHAR). In  Turbo Pascal  hebben ze dit probleem opgelost 
door een  van ARRAY  OF CHAR  afgeleid type,  de STRING (hoe 
verzinnen  ze het, he?) te introduceren. Wel moet nog steeds 
de maximale lengte worden gespecificeerd.

Wat dat  betreft lijkt C wel op Turbo Pascal, alleen zijn er 
wat  andere conventies  wat betreft het gebruik van strings. 
Strings in  C zijn  zo verweven met array's en pointers, dat 
het  zinloos zou zijn te proberen deze dingen los van elkaar 
te behandelen.  Maar om  te beginnen  zullen we moeten weten 
hoe we moeten werken met array's in C.


## D E   A R R A Y 

Een  array is een rij variabelen van hetzelfde type, die elk 
afzonderlijk kunnen  worden aangesproken door middel van een 
index.  (Onthoud  dit  woord;  het  zal  nog  vaak  gebruikt 
worden!)  Een array  laat zich  nog het best vergelijken met 
een rij  huizen, die allemaal identiek zijn. Als je naar een 
bepaald  huis  in  de  rij  wilt verwijzen  zul je  naast de 
straatnaam (de naam van het array) ook het huisnummer moeten 
gebruiken (de index). Ook in C is de index een getal.

Om  een array binnen C te declareren gaan we bijna hetzelfde 
te  werk  als bij  gewone variabelen.  Zo algemeen  mogelijk 
geformuleerd:

        <type> <naam>[<grootte>];

Hierbij is <type> de al aan ons bekende datatypen (int, char 
enz.), <naam>  is de  naam van het array en <grootte> is het 
aantal elementen van het array. Met

        int getallen[10];

creâren we dus een array 'getallen', bestaande uit 10 int's. 
Met

        char buffer[80];

maken  we  een  array 'buffer'  van 80  char's. In  C is  de 
laagste  index altijd  0, en  in bovenstaand voorbeeld is de 
hoogste index  79. Dan hebben we totaal 80 elementen. Dus in 
een  array van N elementen loopt de index van 0 t/m N-1. LET 
HIER GOED  OP! In C wordt niet gecontroleerd of de index die 
we  gebruiken binnen  deze grenzen valt, en dit kan tot zeer 
venijnige  bug's   leiden,  vooral   als  we  iets  in  zo'n 
niet-bestaand element willen opslaan!

Overigens,  de vierkante haakjes slaan alleen op de naam die 
er direkt voor staat. Met

        int veel[40], enkel;

declareren  we  dus een  array 'veel'  met 40  int's en  een 
gewone  int-variabele,  'enkel'.
Een element  uit een  array gebruiken in een uitdrukking kan 
als volgt:

        <arraynaam>[<index>]

De  <index> is  een gewone  uitdrukking, en het is uiteraard 
geen bezwaar  als daarin  ook weer  elementen van  een array 
worden  gebruikt. De  volgende uitdrukkingen  in C  zijn dus 
allemaal   correct   (ervan  uitgaande   dat  de   gebruikte 
variabelen zijn gedeclareerd):

        buffer[0]

        getallen[x]

        getallen[x-1]

        buffer[getallen[x-1] + 1]

Een   en   andere   kan   zo  ingewikkeld   worden  als   de 
omstandigheden  vereisen,  zolang  de  index maar  binnen de 
juiste  grenzen  valt.  Verwijzingen  naar  elementen  mogen 
overal gebruikt worden als gewone variabelen. Dus met

        getallen[x+1] = getallen[x] * 2;

heeft C geen enkele moeite.


## P O I N T E R S 

Verwant met  de array  (nauwer zelfs  dan je  op het  eerste 
gezicht  zou  denken)  is  de  pointer. Een  pointer is  een 
variabele  die 'wijst'  naar een  andere variabele. (Vandaar 
pointer, want  'to point' is 'wijzen naar'). Om de variabele 
waarnaar  zo'n pointer  wijst aan  te kunnen  spreken, of  - 
omgekeerd  -  van een  variabele de  verwijzing (d.w.z.  het 
adres) op  te vragen  zijn er  in C  een tweetal operatoren, 
resp.  de '*'  en de '&'. Maar laten eerst eens beginnen met 
het declareren van een pointer. Dit doen we met:

        <type> *<naam>;

Hiermee wordt  dus een pointer gecreeerd die naar variabelen 
van <type> wijst. Een voorbeeld: met

        int *intptr;

krijgen  we een pointer 'intptr' die naar variabelen van het 
type int  wijst. Het sterretje heeft alleen betrekking op de 
naam die er direkt achter staat. De declaratie

        int *intptr, x;

creeert   dus   een   gewone  int-variabele   'x',  en   een 
pointer-naar-int  'intptr'. We  kunnen dus 'intptr' naar 'x' 
laten wijzen! Dit kan met de '&' operator, die het adres van 
een variabele levert. Met

        intptr = &x;

wijst 'intptr'  dus naar  'x'. Wil  dit enig  nut hebben dan 
moeten  we iets  met die  verwijzing kunnen doen. Met de '*' 
operator krijgen  we toegang  tot de  variabele waarnaar  de 
pointer wijst. Na

        intptr = &x;
        *intptr = 3;

zal 'x'  dus de  waarde 3  hebben gekregen. Dit is misschien 
een   beetje   verwarrend,   omdat  we   de  '*'   ook  voor 
vermenigvuldiging  wordt gebruikt. Bij een vermenigvuldiging 
heeft  de  '*' twee  waarden nodig  (en is  dus een  binaire 
operator) en als verwijzing slechts 1 (een unaire operator). 
Vergelijk dit  bijvoorbeeld met  de '-'  operator, die zowel 
'negatief  maken' (unair) als 'aftrekken' (binair) betekent. 
Omdat  het   vermenigvuldigen  van  pointers  geen  zinvolle 
bezigheid is, kan er eigenlijk geen verwarring ontstaan.

We kunnen ook naar een arrayelement wijzen. Dus met

        int getallen[10], *getal;

declareren    we   een    int-array   'getallen'    en   een 
pointer-naar-int 'getal', waarna we met

        getal = &getallen[5];

'getal' naar  het zesde (!) element van het array 'getallen' 
zal  wijzen. In sommige gevallen kan het echter nog simpeler 
als we  in element  0 -  het eerste  element van het array - 
zijn geinteresseerd:

        getal = &getallen[0];

en

        getal = getallen;

doen  namelijk precies  hetzelfde. Als  we namelijk in C een 
array declareren  gebeuren er  twee dingen: ten eerste wordt 
er  ruimte  gereserveerd  voor de  elementen van  het array. 
Vervolgens  wordt de pointer naar element 0 gekoppeld aan de 
naam van  het array.  Een array  is dus  een bijzonder soort 
pointer,  maar  dan  een  pointer  die niet  van waarde  kan 
veranderen!   (Proberen  we  dit  toch  te  doen  dan  volgt 
(meestal) de  wat cryptische foutboodschap: "Must be lvalue" 
of: "Can't be rvalue". Dit is nog overgebleven uit de vroege 
jaren van de taal C, en heeft te maken met de interne opslag 
van informatie in de compiler zelf.)

De  slimmeriken onder  ons zullen zich nu afvragen: "Als een 
array een bijzonder soort pointer is, kunnen we dan pointers 
net zo  gebruiken als  array's?" Het  antwoord hierop luidt: 
ja!  De vierkante  haken zijn  eigenlijk niets anders dan de 
zoveelste operator  in C,  maar dan  eentje die - net als de 
unaire '*' operator - alleen met pointers werkt.

Een  bijzonder geval  is als een pointer NERGENS naar wijst. 
Hiermee  wordt   niet  bedoeld:   naar  een   niet-bestaande 
variabele,  maar echt  naar niets. Hiervoor is in STDIO.H de 
waarde NULL gedefinieerd, en we kunnen met bijvoorbeeld

        getal = NULL

de pointervariabele  getal 'nergens'  naar laten  wijzen, en 
met

        getal == NULL

of

        getal != NULL

kan getest  worden of dit wel, resp. niet zo is. Het gebruik 
van  NULL berust in feite op een afspraak, en is zeer nuttig 
in bepaalde  algorithmen. Overigens  zijn de '==' en de '!=' 
operatoren  de enige  zinvolle vergelijkingsoperatoren  voor 
pointers.  Men  noemt  twee  pointers  gelijk  als  ze  naar 
dezelfde variabele  wijzen, of  allebei NULL  zijn. Pointers 
kunnen  natuurlijk eigenlijk alleen vergeleken worden als ze 
ook  naar  hetzelfde type  wijzen, als  zijn er  maar weinig 
compilers die  dit testen.  Alleen NULL  mag altijd gebruikt 
worden, ongeacht het type waar de pointer naar wijst.

Met  pointers kan  in zeer beperkte mate worden gerekend, en 
dan  alleen  nog  als  pointers naar  elementen van  array's 
wijzen. Kijk eens naar het volgende stukje C:

        int getallen[20], *ptr1, *ptr2;
        ptr1 = &getallen[5];
        ptr2 = &getallen[10];

hierna doet

        *(ptr1 + 1)

hetzelfde als

        getallen[6]

En

        *(ptr1 + 3)

komt overeen met

        getallen[8]

Tenslotte, de uitdrukking

        ptr2 - ptr1

moet de  waarde 5 opleveren. De meest gebruikte operator bij 
pointers  is  echter  de '++'.  Na gebruik  hiervan zal  een 
pointer naar het volgende element in het array wijzen, en in 
een  lus wordt  dit zeer  vaak gebruikt  om de elementen een 
voor een  af te  lopen. Dit is vaak handiger en bijna altijd 
sneller  dan met  een index werken, al ziet het er voor niet 
C-programmeurs soms wat onoverzichtelijk uit.


## S T R I N G S 

Tot  nu  toe  is  het een  'zware' aflevering  geweest, maar 
daaraan viel niet te ontkomen. Wees getroost: wie het tot nu 
toe allemaal  begrepen heeft  zal de  rest van de cursus ook 
begrijpen.

Strings  in C  zijn gelukkig  heel simpel:  het zijn  gewoon 
array's van  het type  char. De  eerste letter van de string 
vind  je in element 0, de tweede in element 1, enzovoort. De 
lengte  van  een  string  wordt dus  nergens opgeslagen,  in 
plaats  daarvan   wordt  na   het  laatste  karakter  een  0 
opgeslagen. Dit wordt een 'sentinel' (wachtpost) genoemd, en 
zegt  ons zoiets als: "Halt! Einde string." Dit houdt tevens 
in dat  we het  array 1  groter moeten  maken dan de maximum 
lengte van de string, anders past het niet.

Een andere consequentie is dat 0 nooit deel mag uitmaken van 
de  string, maar  omdat strings  eigenlijk bedoeld zijn voor 
tekst is dit niet echt een probleem.

In C  kunnen we  niet met  hele array's (en dus ook strings) 
tegelijk  werken, in plaats daarvan gebruiken we pointers om 
naar deze array's te verwijzen. De standaardbibliotheek kent 
meer  dan   tien  functie's   die  uitsluitend   op  strings 
betrekking  hebben,  en  strings  kom  je  ook  in veel  I/O 
routines tegen. In al die routines maakt men dus gebruik van 
pointers, en de functie-return is meestal ook een pointer.

## P R O G R A M M A V O O R B E E L D   D E   S O U N D E X   C O D E 

In de  Verenigde Staten zijn in de loop der tijden een groot 
aantal  volkstellingen  gehouden,  met het  doel een  beetje 
overzicht  te houden op de groeiende bevolking van dit land. 
Vooral  in  het  verleden  was een  groot percentage  van de 
mensen  analfabeet,  en veel  mensen konden  hun eigen  naam 
zelfs niet schrijven.

Dit  leidde ertoe  dat ook  de spelling  van die  namen niet 
bekend was,  en ambtenaren moesten vaak ter plekke zelf maar 
verzinnen  hoe een naam gespeld moest worden. Hierdoor waren 
analfabeten  vaak  onder verschillende  namen bij  officiâle 
instanties bekend.

De   ambtenaren  van   het  volkstellingsbureau  losten  dit 
probleem grotendeels op door voor namen een coderingsmethode 
te verzinnen  die voor namen met ongeveer dezelfde uitspraak 
dezelfde  code opleverde. Hierdoor was het mogelijk geworden 
in bestanden  namen te  achterhalen zonder  dat de  precieze 
spelling bekend hoefde te zijn.

Deze  coderingsmethode   is  vrij   simpel,  en   laat  zich 
uitstekend   tot   een   klein  C-programmaatje   maken.  De 
toepassingsmogelijkheden  zijn legio, en is overal inzetbaar 
waar we  naar namen of woorden zoeken waarvan we de precieze 
spelling niet kennen.

De  eerste  letter  van  de  code wordt  gewoon uit  de naam 
overgenomen.   Alle  vorgende   letters  worden  in  klassen 
verdeeld. Letters  van klasse 0 (klinkers en bijna-klinkers) 
worden genegeerd, andere klassen (1 t/m 6) worden gewoon als 
nummer  aan  de  code  toegevoegd.  Indien er  twee of  meer 
letters  van  dezelfde  klasse direkt  achter elkaar  staan, 
wordt  het nummer  maar 1  keer toegevoegd.  De totale  code 
wordt na  vierde karakter  afgekapt, of  met nullen tot vier 
karakters  aangevuld. Om een voorbeeld te geven: zowel "jan" 
als  "johan"  hebben  de  code "J500",  "scheur" en  "sguer" 
hebben de code "S250".
```
 #include <STDIO.H>                                       
                                                          
 #define STRLEN 80                                        
                                                          
 char naam[STRLEN];                                       
                                                          
 char code[5];                                            
                                                          
 char *soundex(src, dst)                                  
 char *src, *dst;                                         
 {                                                        
   unsigned cur_len;                                      
   int alfa_pos;                                          
   char new_code, old_code, *old_dst;                     
                                                          
   /* Onthoud begin van dst */                            
   old_dst = dst;                                         
                                                          
   cur_len = 0;                                           
                                                          
   /* Als string niet leeg is... */                       
   if (*src) {                                            
                                                          
     /* Neem eerste letter over */                        
     *dst++ = toupper(*src++);                            
     cur_len++;                                           
     old_code = ' ';                                      
                                                          
     /* Doe tot codelengte = 4, of string ten einde */    
     while (*src && (cur_len < 4)) {                      
                                                          
       /* Bereken positie in het alfabet */               
       alfa_pos = toupper(*src++) - 'A';                  
                                                          
       /* Als het een letter betreft, maak code */        
       if ((alfa_pos >= 0) && (alfa_pos <= 25)) {         
         new_code="01230120022455012623010202"[alfa_pos]; 
                                                          
      /* indien code niet '0' en niet dubbel, voeg toe */ 
       if ((new_code != '0') && (new_code != old_code)) { 
           *dst++ = new_code;                             
           cur_len++;                                     
         } /* if ((new_code... */                         
         old_code = new_code;                             
       } /* if ((alfa_pos... */                           
     } /* while */                                        
   } /* if (*src) */                                      
                                                          
   /* Vul aan tot 4 karakters met '0' */                  
   while (++cur_len <= 4)  *dst++ = '0';                  
                                                          
   /* Sluit de string af */                               
   *dst = '\0';                                           
                                                          
   /* Geef originele pointer terug */                     
   return old_dst;                                        
                                                          
 } /* soundex */                                          
                                                          
 main()                                                   
 {                                                        
   printf("\n\nBereken soundex-code\n\n");                
                                                          
   while (TRUE) {                                         
     printf("Geef te coderen naam: ");                    
     gets(naam, STRLEN);                                  
                                                          
     if ((*naam == '\n')  (*naam == '\0')             
                                          (*naam == ' ')) 
       break;                                             
                                                          
     printf("\nDe soundex-code is: %s\n\n",               
                                    soundex(naam, code)); 
   } /* while */                                          
                                                          
   printf("\n\nEinde programma\n\n");                     
 }                                                        
```

Na het  doorlezen van  bovenstaande listing zal menigeen met 
een  groot  aantal  vragen  zitten.  Laten  we,  net als  de 
computer, bij 'main' beginnen:

Om te beginnen de

    while (TRUE)

In "STDIO.H"  wordt de constante TRUE ('waar') gedefinieerd. 
Deze  heeft in  de meeste  systemen de  waarde 1, maar is in 
ieder   geval   niet  0.   De  tegenhanger   hiervan,  FALSE 
('onwaar'), is  wel 0.  Deze constanten worden gebruikt waar 
we  willen aangeven  dat we  logische waarden gebruiken. (In 
tegenstelling tot PASCAL kent C geen speciaal logisch type.) 
Deze constanten  zijn alleen  bedoeld om de leesbaarheid van 
het programma te verbeteren.

Betekent dit dat de 'while' lus eeuwig wordt doorlopen? Nou, 
niet  echt.   In  deze  lus  is  gebruikt  gemaakt  van  een 
ontnappingsmogelijkheid   die  C  ons  biedt,  namelijk  het 
'break' statement.  Als dit  wordt uitgevoerd springen we de 
lus  uit, naar  het eerste statement na de lus. Deze 'break' 
is ook in alle andere lussen bruikbaar.

Dit  'break'  statement is  hier toegepast  binnen een  'if' 
statement.  Hier   kunnen  we   meteen  zien   hoe  we   met 
karakterconstanten  kunnen werken:  gewoon tussen '' zetten. 
Net als  de strings die tussen "" staan kunnen we bijzondere 
karakters  aangeven met het escape-karakter '\' (backslash). 
Ook hier  betekent '\n'  een newline,  en met  '\0' wordt de 
ASCIIwaarde  nul aangeduid.  (Gewoon een  0 had ook gemogen, 
maar zo is het duidelijker dat het een karakter betreft.)

De  vreemd uitziende  '||' is  een operator  die staat  voor 
logisch-of  (logical   or).  Als   1  van  de  afzonderlijke 
uitdrukkingen  niet-nul (ofwel  logisch waar) is, is de hele 
uitdrukking waar.  We springen dus uit de lus als het eerste 
karakter  van het  array 'naam' een spatie OF een newline OF 
een nul  is. Het bijzondere van de '||' vergeleken met de OR 
uit  BASIC of PASCAL, is dat er niet meer gerekend wordt dan 
strikt noodzakelijk  is: na  de eerste  deeluitdrukking (van 
links  gezien) die  logisch waar is moet de hele uitdrukking 
namelijk waar zijn, en hoeven we niet verder te rekenen.

De tegenhanger van de logische-of is de logische-en (logical 
and),  die  met  '&&'  wordt  aangeduid.  Hier echter  is de 
uitdrukking onwaar als 1 van de deeluitdrukkingen onwaar is. 
Ook hiervoor  geldt dat  er zo  min mogelijk wordt gerekend: 
zodra  een onware  deeluitdrukking is  gevonden, is  de hele 
uitdrukking onwaar,  en stopt de berekening. Deze '&&' komen 
we elders in het programma nog tegen!

Even een stukje teruggaand zien we:

    gets(naam, STRLEN);

STRLEN  is  een  helemaal  in  het  begin van  het programma 
gedefinieerde constante  met de  waarde 80.  Hier is ook het 
char  array  'naam'  gedeclareerd, met  dezelfde lengte.  De 
standaardfunctie   'gets'  leest   een  regel   in  van  het 
toetsenbord t/m de return, maar maximaal STRLEN-1 karakters, 
omdat de  tweede parameter de lengte van het array aangeeft. 
Vervolgens  wordt er nog een nul achtergeplakt, om het einde 
van de  string aan te geven. Als functie-return geeft 'gets' 
de  eerste parameter  terug (een char pointer). In dit geval 
doen we niets met deze functie-return.

Na het 'if' statement staat tenslotte:

    printf("\nDe soundex-code is: %s\n\n", soundex(naam, code));

Hier zien we een nog onbekende format-specificatie, de '%s'. 
Het  zal  niemand wel  zeer verbazen  dat dit  voor 'string' 
staat. De  erbij behorende  parameter moet  naar een  string 
wijzen,  een char pointer dus. De functie 'soundex' berekent 
de soundex-code,  en heeft  als functie-return de waarde van 
de  tweede  parameter,  een  char  pointer.  Op  deze manier 
pointers  onveranderd  teruggeven  is heel  gebruikelijk bij 
string   functies  in   C,  zodat   er  meteen   kan  worden 
doorgerekend. (Let wel: de pointer mag dan ongewijzigd zijn, 
de  string  waarnaar  die  pointer wijst  is dat  natuurlijk 
niet!)

De  functie 'soundex'  berekent hier  de soundex-code van de 
string  'naam'  en plaatst  deze code  in de  string 'code'. 
Natuurlijk moet  'code' dan wel voldoende ruimte bieden voor 
de code, 'soundex' kan dit namelijk niet controleren. Terug- 
bladerend  naar het  begin van het programma zien we dat het 
array 'code'  5 char groot is, precies voldoende dus voor de 
vier karakters en een afsluitende nul.

Binnen  'soundex' zelf  wordt - na de declaraties - eerst de 
char pointer (dit korten we in het vervolg maar af tot: char 
*) 'old_dst'  gelijk gemaakt  aan 'dst'.  (Een afkorting van 
'destination", oftewel 'bestemming'. Maar goed, "what's in a 
name?")  Dit moeten we doen omdat de pointer 'dst' veranderd 
zal worden  in de  routine, en  we willen de oorspronkelijke 
waarde als functie-return gebruiken.

Nadat  'cur_len' -  het aantal codesymbolen tot nu toe - nul 
is gemaakt, zien we:

    if (*src) {

Met '*src'  halen we  het karakter op waar 'src' naar wijst. 
Als  dit karakter de waarde nul heeft is de string blijkbaar 
leeg, en hoeft er niets gecodeerd te worden. Met

    *dst++ = toupper(*src++);

wordt er 1 karakter van '*src' naar '*dst' gekopieerd, nadat 
het tot hoofdletter is gemaakt. Ook worden beide pointer met 
1 opgehoogd.  Dat is nogal veel voor een enkel statement; zo 
compact programmeren is typisch voor C. Laten we eens kijken 
wat precies wat doet in dit statement.

Om een van char - indien van toepassing - een hoofdletter te 
maken  is de  standaarfunctie 'toupper' gebruikt. Deze heeft 
een char parameter, en ook een char functie-return.

Bij de  '*dst++' en  de '*src++'  moet ik  eerst wat over de 
'++' operator vertellen: het maakt namelijk nogal wat uit of 
de  '++' voor  of na de variabelenaam gebruikt wordt: in het 
verleden waren  we alleen  geinteresseerd wat het effect was 
op  de variabele,  en dat is in beide gevallen hetzelfde. De 
waarde van de uitdrukking is echter heel anders:

    x = ++y;

heeft hetzelfde effect als

    ++y;
    x = y;

en:

    x = y++;

heeft hetzelfde effect als

    x = y;
    ++y;

Dus als we de '++' voor een variabelenaam gebruiken wordt de 
waarde teruggegeven  die de variable krijgt na het verhogen, 
in  het andere geval wordt de oude waarde teruggegeven (maar 
de variabele wordt natuurlijk wel verhoogd!) Het statement

    *dst++ = toupper(*src++);

heeft dus hetzelfde effect als

    *dst = toupper(*src);
    ++dst; ++src;

De  eerste  manier  is  natuurlijk bondiger  en efficiânter, 
hoewel niet  echt begrijpelijker.  De meeste  C-programmeurs 
zullen toch de korte methode kiezen.

Even een  technisch puntje:  uit het bovenstaande blijkt dat 
de  operator '++'  eerst wordt uitgevoerd, en daarna de '*'. 
Je zou  dus kunnen  denken dat de '++' een hogere prioriteit 
heeft  dan de  '*', maar  dit is  niet waar, ze hebben beide 
dezelfde:  alleen  worden  dit  soort unaire  operatoren van 
rechts-naar-links uitgevoerd. Dit lijkt misschien onlogisch, 
maar heeft meestal tot gevolg dat de unaire operator die het 
dichtst bij  de variabele  staat het eerst wordt uitgevoerd. 
Alleen  het feit dat de '++' in bovenstaand voorbeeld achter 
de  variabele  is  gebruikt  doet  het  wat onoverzichtelijk 
lijken!

Na  dit statement  komen we in een while-lus, die net zolang 
doorlopen zal  worden tot de lengte van de code-string 4 is, 
of  er geen  karakters meer  te coderen  zijn. Het  volgende 
statement dat misschien enige uitleg behoeft is:

    alfa_pos = toupper(*src++) - 'A';

Hier  zien  we  weer  zo'n  haal-en-ophoog  constructie  als 
voorheen. Wat we echter ook zien is dat we met karakters net 
zo kunnen  rekenen als  getallen! De variabele 'alfa_pos' is 
van  het  int-type,  en  we  zien  dus  ook  dat  dit  soort 
omzettingen  door C  automatisch worden gedaan. De variabele 
'alfa_pos' bevat  nu de plaats in het alfabet (van 0 t/m 25) 
als  het karakter een letter was. Zo niet, dan is 'alfa_pos' 
kleiner dan 0 of groter dan 25. Door hierop te testen kunnen 
we niet-letters negeren.

De bij  de positie  in het  alfabet behorende  code wordt nu 
berekend met:

    new_code = "01230120022455012623010202"[alfa_pos];

Ai!  Dat ziet  er vreemd  uit, nietwaar?  Een string met een 
array-index erachter,  hoe kan  dat? Wel,  zo vreemd  is dat 
niet.  Op het moment dat C iets tussen "" tegenkomt zal deze 
string ergens  in het  geheugen worden gezet (de afsluitende 
nul wordt automatisch toegevoegd). In de expressie waarin de 
string  werd gevonden  wordt met een char * naar deze string 
verder gewerkt.  En een  pointer met  een index erachter kan 
natuurlijk  zonder problemen  worden gebruikt. Het resultaat 
lijkt dus  wat op  de MID$-functie in BASIC, alleen wordt er 
maar een enkel karakter uitgelicht, en tellen we vanaf 0.

De  het begrijpen  van de  rest van de functie mag nu weinig 
problemen  opleveren,  vooral omdat  er in  commentaarregels 
wordt aangegeven wat er gebeurt.

Nog  een  laatste  opmerking:  bij  het  gebruik  van  lange 
compound  statements  (tussen  {  },  remember?) na  whileen 
ifstatements  e.d.  is  het  een  goede gewoonte  om na  het 
afsluitende  } een  kort commentaar  te gebuiken  om aan  te 
geven bij  welke 'while'  of 'if'  etc. het  hoort. Ook  het 
telkens   inspringen  na   'while',  'if',   etc.  komt   de 
leesbaarheid zeer  ten goede!  Dat maakt  het weer  een stuk 
gemakkelijker  om fouten  in het programma te vinden. Vooral 
nietgepaarde  {  en  }  maken  compilers  vaak nogal,  eh... 
spraakzaam.

Robert Amesz
