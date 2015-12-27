      D E   P R O G R A M M E E R T A A L   C   ( 2  )
                                                       


                     I N L E I D I N G 

In  de vorige  aflevering hebben we wat ge�xperimenteerd met 
wat simpel  rekenwerk in C. Deze aflevering gaan we eens wat 
nader kijken naar het rekenen in C, want dat verschilt nogal 
van  PASCAL en  BASIC. Vooral de filosofie erachter is nogal 
anders.

Zo kent C geen rekenkundige statements, alleen uitdrukkingen 
(expressions)! Om van een uitdrukking een statement te maken 
volstaat het  er een  ';' achter  te zetten. Deze manier van 
werken  maakt het  mogelijk allerlei onzinnige statements te 
maken,  die  zonder  problemen  door  de  compiler  verwerkt 
worden. Kijk dus uit, want

     1;

is foutloos C, alleen gebeurt er weinig.


    R E K E N K U N D I G E   U I T D R U K K I N G E N 

Een expression  levert altijd  een waarde op, maar als je er 
een statement van maakt wordt er verder niets met die waarde 
gedaan.  De '='  (toewijzing, oftewel assignment) is ook een 
operator, en geen (deel van) een statement. De waarde van

     x = 35

is gewoon  35, dus  de uitdrukking  die rechts  van het  '=' 
teken  staat. Dit  heeft grote voordelen als je effici�nt en 
compact wil programmeren. Zo maakt de uitdrukking

     y = (x = 35) + 2

x gelijk  aan 35,  en y gelijk aan 37, wat ook het resultaat 
van  de hele  uitdrukking is.  De haakjes  zijn hier  nodig, 
omdat de '+'-operator een hogere prioriteit heeft dan de '=' 
operator. de uitdrukking

     y = x = 2 + 35

zou  zowel x  als y gelijk maken aan 37. De prioriteiten van 
de vele operators die C kent - en er zijn er ruim 40! - zijn 
zelfs voor geoefende C-programmeurs nauwelijks uit het hoofd 
te leren, vandaar dat het gebruik van haakjes, zelfs als dit 
niet  strikt  nodig  is,  vaak is  aan te  raden. De  gewone 
rekenoperaties ('+' '-' '*' '/') hebben echter de prioriteit 
die je ervan verwacht.

Een grote  bron van  fouten is het verwisselen van de '=' en 
de '==' operator. Laatstgenoemde is een vergelijkingoperator 
(zie  de vorige  aflevering) die  veel gebruikt  wordt, maar 
soms wordt  er wel  eens een  enkele '='  getypt als  er een 
dubbele bedoeld wordt. Vergelijk eens de volgende lussen:

     do { /* hier allerlei statements */ }
     while (x == 10);

en

     do { /* hier allerlei statements */ }
     while (x = 10);

Je  leest gemakkelijk over het verschil heen, maar de eerste 
lus wordt  be�indigd als  x ongelijk  aan 10 wordt, en in de 
tweede lus wordt x altijd 10 gemaakt. Bovendien is de waarde 
van de uitdrukking 'x = 10' gelijk aan 10 (niet nul, dus) en 
wordt  de lus nooit be�indigd! De compiler echter heeft geen 
enkele  moeite  met  deze  twee  lussen,  en geeft  ook geen 
waarschuwing of  foutmelding. De  prijs van de flexibiliteit 
van C is dus zorgvuldiger nakijken van het programma door de 
programmeur.



    D E   A L G O R I T H M E   V A N   E U C L I D E S 

|                                                          |
| #include <STDIO.H>                                       |
|                                                          |
|                                                          |
| /* Bepaal de grootste gemene deler van twee getallen */  |
| int ggd(grootste, kleinste)                              |
|   int grootste, kleinste;                                |
|   {                                                      |
|                                                          |
|   int temp, rest;                                        |
|                                                          |
| /* Zorg ervoor dat 'grootste' het grootste getal bevat */|
| /* Verwissel ze zonodig                                */|
|   if (grootste < kleinste) {                             |
|   temp = kleinste; kleinste = grootste;                  |
|     grootste = temp;                                     |
|     }                                                    |
|                                                          |
| /* Het daadwerkelijke algorithme */                      |
|                                                          |
|  while (rest = (grootste % kleinste)) {                  |
|     grootste = kleinste;                                 |
|     kleinste = rest; }                                   |
|                                                          |
|     return kleinste;                                     |
|                                                          |
| } /* einde van functie ggd() */                          |
|                                                          |
|                                                          |
| /* Hoofdprogramma */                                     |
| main() {                                                 |
|                                                          |
|   int getal1, getal2;                                    |
|                                                          |
|   getal1 = 69;                                           |
|   getal2 = 253;                                          |
|                                                          |
|   printf("De grootste gemene deler van %d en %d is %d\n",|
|           getal1, getal2, ggd(getal1, getal2));          |
|                                                          |
|   printf("\nEinde programma\n\n");                       |
| }                                                        |
|                                                          |


In  dit   programma  worden  heel  wat  nieuwe  taalelemenen 
ge�ntroduceerd:  allereerst  het  gebruik van  functies, het 
if-statement,  een iets  andere versie  van de reeds bekende 
do..while lus, en het return-statement.

Om te  beginnen iets over functies, ofwel subroutines, in C. 
In  tegenstelling tot  PASCAL kent C geen procedures. Dit is 
geen  groot  gemis,  aangezien  we  functies  als procedures 
kunnen gebruiken.  De functiewaarde,  die de  functie geeft, 
wordt dan eenvoudig niet gebruikt.

Met de regels

     int ggd(grootste, kleinste)
     int grootste, kleinste;

geven we  aan dat  de definitie  van de functie 'ggd' volgt. 
(Voor  functienamen  gelden  dezelfde regels  als namen  van 
variabelen). Het 'int' voor 'ggd' laat de compiler weten dat 
de waarde die de functie teruggeeft van het type 'int' is.

De  namen na  het '('  zijn de parameters van de functie. Er 
mag  een   willekeurig  aantal   parameters,  door   komma's 
gescheiden,  volgen (dus  ook 0).  In ons  geval zijn dat er 
twee, 'grootste' en 'kleinste'.

In de  volgende regel  (of regels)  worden de  typen van  de 
parameters  gespecificeerd. Dit gebeurt alsof het variabelen 
zijn. Zo  vreemd is  dat niet, want binnen de functie (na de 
'{')  mogen de  parameters precies  als variabelen  gebruikt 
worden.  In  het  bovenstaande  geval  hebben  we  dus  twee 
parameters van het type 'int'.

Vervolgens worden  er twee  variabelen gedeclareerd  van het 
type 'int'. Net als in PASCAL zijn variabelen die binnen een 
functie   worden  gedeclareerd  -  en  daar  rekenen  we  de 
parameters  van  de  functie  ook onder  - alleen  binnen de 
functie bekend. Dus 'grootste', 'kleinste', 'rest' en 'temp' 
kunnen alleen  binnen 'ggd'  gebruikt worden, en zijn vanuit 
'main'  onbereikbaar. Wie  denkt dit te kunnen omzeilen door 
binnen  'main'  deze variabelen  opnieuw te  declareren komt 
bedrogen uit:  de variabelen hebben weliswaar dezelfde naam, 
maar zijn toch andere variabelen!

Dit  laatste betekent  dat de C-programmeur zich geen zorgen 
hoeft te  maken of  een bepaalde  naam al  binnen een andere 
functie  gebruikt  wordt.  Wil  hij  toch  dat  een bepaalde 
variabele  door verschillende  functies gebruikt kan worden, 
dan   moet   die   variabele   buiten  een   functie  worden 
gedeclareerd, d.w.z.  aan het  begin van  het programma,  of 
tussen de functies in.

De regels

  if (grootste < kleinste) {
    temp = kleinste; kleinste = grootste; grootste = temp; }

is een if-statement in C. Het lijkt veel op dat van BASIC of 
PASCAL,  alleen ontbreekt  hier 'THEN'. Het 'THEN' is echter 
impliciet! De algemene vorm van een if-statement is:

     if (<expression>) <statement>

Als <expression>  een waarde oplevert die niet gelijk is aan 
nul,  dan wordt  <statement> uitgevoerd. In het bovenstaande 
voorbeeld  moesten   we  dus   weer  een  compound-statement 
gebruiken. C kent ook een if..else statement:

     if (<expression>) <statement1> else <statement2>

Als  <expression> een waarde oplevert die niet gelijk is aan 
nul, dan  wordt <statement1> uitgevoerd, in het andere geval 
<statement2>.

Wat   de   bedoeling   is  van   het  if-statement   in  het 
voorbeeldprogramma   staat   al   in   de   ervoor   staande 
commentaarregel!

Nu  krijgen  we  een  nieuwe  vorm van  het while-statement, 
namelijk:

     while (<expression>) <statement>

Dit  werkt precies als het do..while statement, alleen wordt 
er in het begin van de lus getest, en niet aan het eind. Dit 
houdt tevens  in dat  <statement> niet wordt uitgevoerd, als 
<expression> meteen al de waarde nul oplevert. Het programma 
gaat  dan verder  na <statement>.  In de do..while lus wordt 
<statement> tenminste 1 keer uitgevoerd.

De <expression>,  zoals die  in het voorbeeld wordt gebruik, 
maakt al aardig gebruik van de mogelijk heden van C. Het '%' 
in

     rest = (grootste % kleinste)

betekent  hetzelde als  MOD in  BASIC of  PASCAL, de rest na 
deling dus.  Deze waarde  wordt, heel  toepasselijk, aan  de 
variabele  'rest' toegewezen,  wat ook  de uitkomst van deze 
uitdrukking is.  Als 'rest' dus nul wordt, wordt de lus niet 
(meer) doorlopen.

Na  het  compound  statement na  'while' volgt  een onbekend 
statement, namelijk

     return kleinste;

BASIC-programmeurs  zullen  nu  de  oren spitsen.  Inderdaad 
be�indigt een  return statement een functie (subroutine). De 
algemene vorm is:

     return;

of

     return <expression>;

In  het eerste  geval wordt de functie be�indigd, zonder dat 
er een  functiewaarde wordt  teruggegeven aan  het programma 
dat  de functie aanriep, in het tweede geval gebeurt dat wel 
(namelijk  de  waarde  van  <expression>). In  ons voorbeeld 
wordt de inhoud van 'kleinste' gereturnd. Een kale 'return;' 
aan het  einde van  een functie is overigens nooit nodig: de 
compiler regelt dat zelf wel.

De  functie  wordt  nu  afgesloten  met  een  '}'.  Het  kan 
eenvoudig  bewezen  worden  dat  deze  functie inderdaad  de 
grootste  gemene deler  van twee  getallen levert,  maar dat 
valt buiten  het bestek van deze reeks. Een oefening voor de 
lezer  wellicht? Het  is misschien  interessant te weten dat 
dit de  oudst bekende  algorithme ter wereld is. Zij dateert 
van ruim voor het begin van onze jaartelling!

Het  hoofdprogramma -  na 'main' - moet voor de trouwe lezer 
weinig problemen  opleveren. In  'printf' kun je zien hoe de 
functie  'ggd' kan worden aangeroepen. Overigens mogen zelfs 
een achter  een functie  met nul  parameters de haakjes niet 
worden  weggelaten, dit  in tegenstelling tot PASCAL. Doe je 
dit  toch   dan  krijg  je  bij  de  meeste  compilers  geen 
foutmelding!  (Voor de  ge�nteresseerden: in  plaats van  de 
functiewaarde  krijg  je  dan  de pointer  naar de  functie, 
oftewel het  adres van  de functie. Over pointers wil ik het 
echter pas in de volgende aflevering hebben).

                                               Robert Amesz