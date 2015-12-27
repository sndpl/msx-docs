# Pascal Course (2)

 Wat is die Koen toch een etter. Nauwelijks ben ik een PASCAL
 cursus begonnen of hij begint al met klagen. Vorige keer had
 ik een leuke programmaatje op de disk laten zetten om je CD's
 beter op te kunnen nemen, maar nee, voor meneer Dols is het
 weer niet goed.

 Koen   : Kan ik niet aangeven dat ik een 60 bandje heb?
 Jeroen : Nee Koen, dat kan niet, dat was wat veel werk.
 Koen   : Wat is dat dan voor een stom programma, zorg er maar
          voor dat dat volgende keer verbeterd is!

 Tja, en ik als goede onderdaan (Sahib, sahib) heb dat dus ook
 maar gedaan. Ik hoop dus maar dat hij dit keer niets te
 zeiken heeft(NvdR: ik heb thuis ook nog 100 minuten bandjes
 Jeroen!)

 Maar genoeg over onze kleine sadist Koen. Nu gaan we over
 naar het echte werk: programmeren. Als ik mij goed kan
 herinneren, dan hebben we het vorige keer over de globale
 structuur van PASCAL gehad. Nu gaan we het over variabelen
 hebben.

 Een groot verschil tussen BASIC en PASCAL is dat het in
 PASCAL verplicht is om alle variabelen die je gebruikt van te
 voren aan te melden en te specificeren. Wat ik hiermee bedoel
 is dat je de compiler duidelijk maakt hoe je variabelen heten
 (jan, piet of klaas) en wat voor variabelen het zijn. Een
 variabele declaratie zou er als volgt kunnen uitzien (de
 variabele declaratie moet altijd vooraf gegaan worden door
 het gereserveerde woord VAR):

   VAR
     teller : integer;
     getal  : real;
     status : boolean;

 De opbouw van deze declaratie mag wel duidelijk zijn. Eerst
 komt er het gereserveerde woord VAR. Op de volgende regels
 volgt de naam van de variabele die je gaat gebruiken gevolgd
 door een dubbele punt en het type van de variabele. Aan de
 naam van de variabele is eigenlijk maar een grote restrictie,
 de naam mag geen gereserveerd woord (zoals BEGIN, END, VAR,
 etc) zijn. De dubbele punt is verplicht. Het type van de
 variabele kan vanalles zijn, maar ik beperk me hier tot de
 standaard types :

 - boolean (kan de waarde TRUE of FALSE hebben)
 - byte (spreekt voor zich)
 - integer (16 bits geheel getal, ook word)
 - longint (32 bits geheel getal, ook longword)
 - real (een gebroken getal, lengte hangt af van de machine)
 - char (een ascii karakter)
 - string (een reeks ascii karakters)
 - text (een file met ascii karakters)

 De punt-comma op het einde van de regel is verplicht (bijna
 alle programma regels in PASCAL worden afgesloten met een
 punt-comma). Naast bovengenoemde standaard types kun je ook
 zelf types ontwikkelen (dagen van de week bijvoorbeeld), maar
 daar kom ik later op terug. Voor ik het vergeet, de laatste 2
 types zouden wel eens niet kunnen werken, daar ze niet vanaf
 de eerste PASCAL versies ge◊mplementeerd waren. Vanaf versie
 3 of hoger zouden ze echter aanwezig moeten zijn.

 Dan komen we nu bij de toewijzingen. Deze zijn zeer
 gemakkelijk :

   getal:=2;
   letter:='A';
   teller:=teller+1;

 Bovenstaande voorbeelden zijn erg simpel, maar geven het
 principe goed weer. Een toewijzing wordt als volgt opgebouwd:

 Als eerste komt de variabele waar iets aan toegekend moet
 worden. Dan volgt een dubbele punt met een "is-gelijk"
 teken (spreek := uit als 'wordt'). En als laatste volgt de
 waarde die de variabele moet krijgen. Deze waarde wordt
 samengesteld zoals in BASIC, met een kleine restrictie: het
 type mag (uiteraard) niet wijzigen. Dus als getal eerst een
 integer was, dan mag deze niet door 'getal:=1.5' veranderd
 worden in een real. Genoemde toewijzing zal resulteren in
 een compiler fout of een run time error.

 Naast de gebruikelijke operators (+, -, *, (), AND, OR, XOR,
 NOT) zijn er ook nog functies zoals CHR, ORD, ROUND, TRUNC,
 ABS, ARCTAN, COS, EXP, FRAC, INT, LN, SIN, SQR, SQRT, etc. De
 meeste namen duiden op soortgelijke functies uit BASIC, voor
 meer details verwijs ik naar boeken over PASCAL. Genoemde
 functies kunnen gewoon gebruikt worden in een toewijzing, als
 men er maar aan denkt dat het type van de hele toewijzing
 gelijk moet zijn aan het type van de variabele.

 Laatstgenoemde punt impliceerd dat er twee functies moeten
 zijn om een getal te delen. Waarom? Omdat delen per definitie
 een gebroken getal opleverd en dat dus niet een integer is.
 Gehele getallen kunnen echter wel op elkaar gedeeld worden en
 daarom is er een speciale functie nodig om integers te delen.
 Deze speciale functie is de DIV functie (DIVide). De logisch
 bijbehorende functie is MOD (MOD geeft de rest van de
 deling).

 Zo, dat was het voor deze keer. Maar voor ik jullie weer
 verlaat (ooohh) geef ik jullie nog een voorbeeldje en een
 opgave om zelf te maken. Van het voorbeeld programmaatje ga
 ik niet veel verklappen, bekijk het maar eens en probeer
 (voor- dat je het laat uitvoeren) te beredeneren wat het
 doet. Het voorbeeld programmaatje staat op de disk onder
 PASCAL2.PAS (dan hoeven jullie het niet over te tikken).

 De opgave daar wil ik iets anders mee doen. Als iemand zo gek
 is om het te proberen, het op te lossen en de oplossing op te
 sturen, dan beloof ik die persoon dat zijn oplossing volgende
 keer geplaatst wordt. En om het nog aantrekkelijker te maken
 vermelden we de naam van de persoon die de oplossing heeft
 ingestuurd (HÇ Koen, wedje dat nu niemand meer iets instuurt.
 Kun je tenminste niet weer kankeren dat je niet alles op de
 disk kunt plaatsen wegens plaatsgebrek).

 Zo, genoeg getypt, nu de opgave :
 Schrijf een programma dat om 3 netto prijzen van artikelen
 vraagt. Bereken aan de hand van die prijzen : De BTW per
 artikel, het totaal betaalde BTW bedrag, de totale BRUTO en
 NETTO prijs van de drie artikelen.

 Aangezien ik nog niets heb gezegd over in- en uitvoer van
 variabelen via toetsenbord en beeldscherm, verwijs ik naar
 het voorbeeld programma. Door dit goed te bekijken moet het
 mogelijk zijn om iets soortgelijks te construeren.

 Veel plezier en tot volgende keer,

Jeroen 'KUBIE' Smael
Galopiahof 6
6215TK Maastricht
Tel : 043-3437778 ('s weekends)
Tel : 040-2261521 (door de week)

Stuur de oplossingen van de puzzle ook naar dit adres!!

 P.S. (1) Om een nieuwe programmeertaal te leren moet je zelf
          een heleboel oefenen en uitzoeken, daar leer je
          tenslotte het meeste van. Daarom kauw ik niet alles
          voor, maar laat een heleboel aan jezelf over.
      (2) Boeken die je kunt raadplegen zijn onder de VELE
          andere : K.L. Boon / PASCAL voor iedereen / Kluwer /
          ISBN 90.201.1425.5 --- Findley / PASCAL Inleiding
          tot gestructureerd programmeren / Kluwer / ISBN
          90.201.1991.5
      (3) Let ook op de programmeerstijl die ik in het
          voorbeeldprogramma gebruik. Er is geen standaard
          voor, maar mijn stijl is wel gestructureerd en dat
          bevorderd de leesbaarheid.
      (4) Dankzij Koen staat er dus ook een verbeterde versie
          van het CD programma op de disk (CD2.PAS / CD2.COM).
          Veel plezier daarmee (nog iets te mekkeren, Koen?).
          (NvdR: Ja, Jeroen: Bl◊◊◊◊◊◊◊◊◊◊◊◊◊◊◊◊◊◊◊◊◊◊◊◊◊◊◊h!)