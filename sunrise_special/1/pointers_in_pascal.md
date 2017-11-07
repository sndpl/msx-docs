#P O I N T E R S


Ik wil in dit stukje wat uitleg geven over pointers; wat kun
je er  zoal mee doen, hoe ga je er mee om. Ik ga in principe
uit  van een  de taal  Pascal, maar  dit verhaal  is met wat
fantasie  om  te  zetten  naar  bijvoorbeeld  machinetaal of
BASIC.


## W A T   I S   E E N   P O I N T E R

Een  pointer is  in feite een adres en op dat adres staat de
inhoud van een variabele. De variabele waar een pointer naar
wijst is  meestal alleen  via deze  pointer toegankelijk. Om
het  allemaal moeilijk te maken kun je ook nog pointers naar
pointers (naar pointers naar pointers enz.) hebben.


## W A A R O M   P O I N T E R S   G E B R U I K E N

Voor  het  structureren van  'gelijksoortige data-elementen'
(dus bv. integers, arrays, bytes, records) zijn er in Pascal
in principe 3 mogelijkheden: de array, de file en pointers.

De file  is vooral bedoeld als in- en uitgangsmedium en niet
in  de eerste  plaats voor interne verwerking (daarvoor zijn
de mogelijkheden gewoon te klein).

Arrays van  een bepaald  type bieden  wel ruime verwerkings-
mogelijkheden,  maar er  kleven ook een aantal bezwaren aan,
waarvan de  belangrijkste is  dat we verplicht zijn de array
in   een   preciese   omvang  te   declareren  (bijvoorbeeld
array[1..100] of byte; deze array heeft een vaste omvang van
100  bytes), terwijl  de omvang van de omvang van de rij die
in de  array moet  komen bijna nooit van te voren bekend is.
Hierdoor is bijna altijd de array te groot of te klein.

Pascal  biedt nog  een andere  structureringsmogelijkheid en
wel door  middel van  pointers. Hierbij zijn geen van de bij
de  file  en  array  genoemde  bezwaren van  toepassing. Aan
pointers  kleven echter  weer andere  bezwaren, o.a. dat het
netwerk van pointers waarmee je werkt soms enorm complex kan
worden,  maar  aan  de  andere  kant  zijn er  ook erg  veel
voordelen.


## D E   S P E L R E G E L S

* TYPE-DEFINITIE
Een pointer-type wordt in pascal als volgt gedefinieerd:

<pointertype> = ^<data-element>

Voorbeeld:
type arint  = array[1..100] of integer;
rek    = record
a    : integer;
x, y : byte;
z    : array[1..24] of char
end;
PTRint = ^integer;
PTRari = ^arint;
p_rek  = ^rek;


* POINTER-VARIABELE
We kunnen nu pointer-variabelen declareren aan de hand van
de zojuist gemaakte type-definities.

Voorbeeld:
var p1, p2, p3 : PTRint;
p4, p5     : p_rek;

Het bovenstaande  creeert de  pointers (zoals var ook alle
andere  variabelen  creeert).  Een  pointer  heeft op  een
MSX-computer een  'grootte' van  2 bytes, want daarin past
precies het gehele geheugen- bereik van 64 kB.

* POINTER-OPERATIES
Er zijn drie manieren om aan een pointer-variabele een
waarde te geven:
- Met de standaard-procedure NEW.
Aanroep: NEW(p), waarbij p de betreffende
pointer-variabele is.

Door  de aanroep van NEW(p) wordt er in het geheugen van
de  computer  ruimte  gereserveerd ter  grootte van  het
data-type  waarnaar  de  pointer wijst.  Als ik  dus een
pointer  naar een  byte heb (^byte) dan wordt er dus ook
werkelijk 1 byte geheugen vrijgemaakt voor dat byte.

Na  de  NEW-instructie  kun  je de  pointervariabele een
waarde  geven. Dit  gaat bijna  zoals bij  iedere andere
variabele;  via de  pointer geef je de pointer variabele
nu een waarde.
scrijfwijze: <pointervariabele>^ := <waarde>

Voorbeeld:
NEW(p1);     { maak ruimte voor p1^ }
p1^ := 25;   { geef pointervariabele p1^ de waarde 25 }

- We kunnen  een pointer-variabele ook de waarde geven van
een andere pointer-variabele via een assignment: Stel p1
en   p2  zijn  pointers  naar  hetzelfde  type  pointer-
variabele  (dit is erg belangrijk). Met p2 := p1 laat je
deze poinbters  naar dezelfde variabele wijzen. p1 en p2
zijn  nu  dus verschillende  aanduidingen voor  dezelfde
variabele...

Er is een groot verschil tussen:
1)  new(p1);              2)  new(p1);
p1^ := 25;                p1^ := 25;
new(p2);                  p2 := p1;
p2^ := 25;

Er geldt na afloop:
1)  p1^ = p2^             2)  p1^ = p2^
twee variabelen           een variabele
p1 <> p2                  p1 = p2

- De derde mogelijkheid is om de pointer de waarde nil te
geven; hij wijst dan nergens naar.

Voorbeeld:
p1 := nil;


Een gecreeerde variabele kunnen we ook weer vernietigen en
wel met de standaard-procedure DISPOSE.

Voorbeeld:
dispose(p);

Hierna  is de  variabele p^  vernietigd; de geheugenruimte
die door  deze variabele bezet werd is weer vrijgegeven en
is dus weer beschikbaar. De pointer p is ongedefinieerd.


## T E N   S L O T T E

Je  kunt  met  pointers  leuke  dingen uithalen.  Zo kun  je
bijvoorbeeld  een  'gelinkte  lijst'  opzetten.  Dit is  een
array-achtig  iets, waarbij  je echter  geen indexen en geen
vaste omvang  hebt (wat je bij een array juist wel hebt). Je
creeert  een record  waarin een  pointer zit  die naar 'zijn
eigen' record wijst.

Hierbij krijg je echter een kip-en-ei-probleem: als ik eerst
de pointer definieer, dan is het record nog niet bekend maar
als  ik  eerst het  record definieer  dan heb  ik weer  geen
pointer om naar dat record te laten wijzen.

Dit is  in pascal  de enige  toestand waarbij je een pointer
mag definieren naar een nog (!) niet-bestaand type.

De definitie van de data-typen ziet er bijvoorbeeld zo uit:
type PTRrek : ^rek; { pointer naar type rek }
rek    : record
A    : array[1..7] of byte;
B    : boolean;
next : PTRrek;  { pointer naar het volgende
            element in de lijst }
end;

var startpunt, huidig : PTRrek;
^          ^
|          |
|          +-- op deze plek in de lijst ben ik nu
|
+------------- eerste element uit de lijst (de 'wortel')


Bij lijsten wordt de waarde NIL gebruikt om het einde van de
lijst  aan te  geven (je  moet de pointer(s) zelf die waarde
geven; standaard staat er alleen onzin in).

Rudy Oppers
