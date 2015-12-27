A S S E M B L E R S   O P   M S X


Het schrijven van machinetaalprogramma's  op  MSX  is  nooit
echt moeilijk geweest en is daarom ook een prima computer om
het op te leren. Er is in het verleden al veel geklaagd over
het feit dat men wel  een  modernere  processor  had  kunnen
kiezen om in de MSX te stoppen, maar daarbij werd  dan  vaak
over het hoofd gezien dat de Z80 een makkelijk te  begrijpen
microprocessor is (waarschijnlijk  omdat  hij  een  volledig
achterhaalde architektuur heeft).

Dit verhaal is er op gericht om u kennis te laten maken  met
de twee meest gebruikte assemblers op MSX.  Deze  assemblers
zijn WBASS-2 en GEN80.


           G E N   8 0

Dit is een redelijk oude assembler  die  nog  uit  het  CP/M
tijdperk stamt. Het is echter in technisch opzicht een prima
assembler omdat hij redelijk flexibel is en  omdat  hij  met
hele grote listings om kan gaan.

De hele assembler zit in 1 .COM file en maakt gebruik van de
DOS  commando  regel  om  invoer  en  uitvoer  te   regelen.
Bij de aanroep kunnen  verschillende  dingen  gespecifiseerd
worden zoals de maximale breedte van een label en dingen met
betrekking tot de uitvoer.

Voordelen van GEN 80:
- Het is mogelijk om met GEN 80 gebruik te maken van include
files en van macro files. Vooral  deze  laatste  optie  is
makkelijk omdat je zo je eigen persoonlijke BIOS bij  kunt
houden met routines die je vaak nodig hebt.
- De maximale filelengte van een te assembleren files  hangt
feitelijk alleen van de hoeveelheid RAM van je computer af
en van het feit dat er 'maar' 720 kB op 1 disk past.

Nadelen:
- Omdat de assembler  steeds  geladen  moet  worden  is  het
feitelijk ondoenlijk alleen van disk te  werken  omdat  je
dan gewoon niets opschiet. Het gebruik van een RAM-disk is
eigenlijk onontbeerlijk omdat je anders het grootste  deel
van de tijd staat te wachten.
- Omdat GEN 80 onder DOS loopt is het niet echt  handig  als
je iets wilt testen. Meestal moet je dan eerst naar  BASIC
voordat je de creatie kunt testen. Je berijpt dat debuggen
zo een vervelende bezigheid wordt omdat je konstant tussen
DOS en BASIC zit te klooien.
- Het eigen gebruik van de memory mapper is ook niet aan  te
bevelen als je op hetzelfde moment alles in  een  RAM-disk
hebt staan. De kans is dan  groot  dat  je  dingen  in  de
RAM-disk gaat overschrijven waardoor je alles weer opnieuw
moet installeren.

GEN 80 is  een  prachtige  assembler  maar  is  helaas  niet
speciaal voor MSX geschreven. Vooral bij het gebruik van  de
memory mapper stoot je  hier  dus  op  problemen.  Voor  het
schrijven van demo's of andere soorten  programma's  die  in
een grafische mode werken is het gebruik van GEN  80  af  te
raden om  de  doodsimpele  reden  dat  hij  daar  niet  voor
geschreven is.


          W B A S S - 2

Dit is bij mijn weten de enige assembler die  speciaal  voor
MSX geschreven is.  Ik  moet  hierbij  meteen  opmerken  dat
WBASS-2 niet een assembler is, maar dat  het  een  assembler
bevat.  Verder  behoren  een  monitor,  een  editor  en  een
disassembler tot de standaard uitrusting. Het gekke  is  dat
de voordelen en nadelen van GEN  80  precies  omgekeerd  van
toepassing zijn voor WBASS-2. Zo is het bij WBASS-2 oppassen
dat je niet over je source code heen assembleert, maar  daar
staat tegen over dat het gebruik  van  de  memorymapper  een
fluitje van een cent is.

Voordelen van WBASS-2:
- Het is een ge�ntegreerd pakket van  ontwikkelsoftware  dat
goed met elkaar samenwerkt en waarmee weinig tijd verloren
gaat aan het omschakelen tussen editten en bv.  het testen
van een programma.
- Het is mogelijk om tegelijk een assembly programma en  een
BASIC programma te gebruiken en te bewerken.
- De assembler is snel (op een turbo R zelfs hemels).
- De assembler is heel flexibel. Zo is het mogelijk  om  met
verschillende notaties voor getalrepresentatie te werken.
(Zie verder).
- Het pakket  biedt  een  volledige  ondersteuning  van  het
memory-map systeem van de MSX.
- De editor  is  speciaal  geschreven  voor  de  invoer  van
assembly (alhoewel er  dan  altijd  nog  mensen  zijn  die
klagen over deze lovenswaardige eigenschap).
- Het pakket biedt een  goede  ondersteuning  van  de  MSX
file-formaten. Zowel .BIN als .DAT files zijn probleemloos
te laden.
- De editor kan ASCII files genereren, maar het opslaan  van
files kan beter in het WBASS-2 formaat gebeuren omdat deze
vorm korter en sneller is.
- Het is met een simpele aanpassing  van  het  laadprogramma
mogelijk om de plaats van het WBASS-2 programma in het RAM
zelf te bepalen (Zet hem dus altijd in de  hoogste  vrije
RAM page die je hebt).

Nadelen:

- De lengte van de te bewerken file is beperkt tot ong. 23kB
- Je moet oppassen dat je niet over  je  eigen  source  code
heen schrijft. (Zie voor meer details bij Tips.)
- De Z80 mnemonics liggen redelijk goed  vast,  behalve  die
van  de  instruktie  EX  AF,AF'  Volgens  offici�le  Zilog
informatie is deze notatie de enige goeie, maar een aantal
andere Z80 fabrikanten vond dat zeker niet belangerijk  en
hebben er EX AF,AF van gemaakt. Om verwarring te voorkomen
kunnen de meeste assemblers beide notaties wel  aan,  maar
WBASS-2 kan dit niet en zal  bij  gebruik  van  de  eerste
variant een foutmelding geven.
- WBASS-2 heeft nog een paar kleine bugs die op  het  eerste
gezicht niet opvallen (zie Tips).
- WBASS-2 kent geen macro's en het include  file  mechanisme
is niet echt denderend.

WBASS-2 bevat enkele bugs ! Zo is  het  gelijktijdig  werken
met zowel enkel- als dubbelzijdige disks af te  raden  omdat
het programma hierdoor in de war kan raken met alle gevolgen
vandien. Dit is bij mijn weten de grootste bug van  WBASS-2.
Er zijn geloof ik nog enkele, maar die  zijn  mij  nog  niet
opgevallen.

De nadelen van WBASS-2 zijn met enige oefening en handigheid
op te heffen. Het is gewoon een kwestie van hoe je  met  het
programma om gaat. Voor MSX is WBASS-2 zonder enige  twijfel
het beste pakket omdat het speciaal voor MSX geschreven  is.
Hierbij zijn de twee  belangerijkste  kriteria  de  snelheid
waarmee tussen testen en assembleren geschakeld  kan  worden
en de sublieme manier van geheugen management dat het pakket
de gebruiker biedt.


 Tips voor het gebruik van WBASS-2

Tips voor het instellen van het programma:
- In het laadprogramma wordt op een gegeven moment een
OUT &HFD instruktie gegeven vlak voordat de WBASS-2  file
geladen wordt. Deze OUT  bepaald  op  welke  RAM  page het
programma geladen wordt. Indien je bv. 256 kB hebt zou  je
dus het best OUT &HFD,15 kunnen geven omdat het  programma
dan niet in de weg zit  tijdens  het  ontwikkelen  van  je
software.
- De edit data van je eigen programma komt altijd op  mapper
page 1 te staan (net als een BASIC programma) en om nu  te
voorkomen dat je per ongelijk een programma over je  eigen
edit data assembleert kan je het  beste  een  andere  page
selekteren (met het PAGE commando) zodat de  assembler  de
code niet over je source data heen kan schrijven.
Dit werkt echter alleen voor  page  2  (&H8000  /  &HC000)
omdat je page 3 eigenlijk moet laten staan. Je  edit  data
kan nl. makkelijk over adres &HC000 heen staan, maar  daar
kan je code ook staan. Pas dus altijd op met wat je  doet.
(Persoonlijk vind ik  dit  het  allergrootste  nadeel  van
WBASS-2.)  Het  is  gelukkig  mogelijk  om  op   disk   te
assembleren inplaats van in RAM zodat je dit  probleem  op
kunt heffen.
- Het is in het laadprogramma ook  mogelijk  om  het  eerste
adres  te  kiezen  voor  editdata.  Dit  gebeurt  met   de
instruktie: BE=&H9200 Indien je dit adres  op  &H8000  zet
moet je wel oppassen dat je absoluut niet meer  een  BASIC
programma gaat invoeren omdat je dan meteen over je source
code heen schrijft. Je  kunt  deze  waarde  het  beste  op
&H8100 zetten zodat je naast je  edit  data  ook  nog  een
klein BASIC programma in het RAM kunt hebben.
- De include optie van WBASS-2 is zoals  vermeld  niet  echt
denderend. Het houdt feitelijk in dat je  in  een  listing
kunt vermelden dat hij nog een file  moet  laden  die  ook
geassembleerd moet worden. Dit kan bij WBASS-2 alleen  als
je de include opdracht onderaan de file neer zet omdat  de
oude file uit de editbuffer  gewist  zal  worden.  Het  is
daarom ook niet handig om deze optie  te  gebruiken  omdat
het alleen maar ellende veroorzaakt.

Voor verdere tips moet ik naar  de  uitstekende  handleiding
verwijzen.


G E T A L R E P R E S E N T A T I E

Voor Z80 mnemonics  bestaan  4  verschillende  manieren  van
getalsrepresentatie en wel:
- de binaire      ( 2 tallig stelsel)
- de oktale       ( 8 tallig stelsel)
- de decimale     (10 tallig stelsel)
- de hexadecimale (16 tallig stelsel)

Het is met een zgn. 'prefix' of 'postfix' mogelijk om aan de
compiler te vertellen  welk  talstelsel  bedoeld  wordt.  In
BASIC zijn dit:
- &Bxxxxxxxx voor een binair getal
- &Oxxx      voor een oktaal getal
- xx         voor een decimaal getal
- &Hxx       voor een hexadecimaal getal

De meeste Z80 assemblers kunnen deze getalsrepresentatie ook
gebruiken. Er is  echter  een  oudere  notatievorm  (die  ik
persoonlijk wat eleganter vind) en die is als volgt:

- xxxxxxxxB  voor een binair getal
- xxxO       voor een oktaal getal
- xx         voor een decimaal getal
- xxH        voor een hexadecimaal getal

Hierbij moet opgemerkt worden dat bv. &H7FFF  in  de  tweede
notatie als 07FFFH geschreven moet worden. Die  extra  0  is
eigenlijk flauwekul omdat zo'n groot getal niet  in  een  16
bits register past. (Nvdr. Die 0 is bij de meeste assemblers
alleen  verplicht als  het getal  met een letter begint (dus
bijvoorbeeld 0F000H).  Zo kan  de assembler  zien dat het om
een getal gaat en niet om een label.)

WBASS-2 heeft als standaardinstelling de eerste  notatievorm
maar kan ook de tweede gebruiken (met het SET commando in te
stellen). Dit is een hele handige optie  als  je  bv.  GEN80
files om wilt zetten naar WBASS-2  files  en  dat  gaat  als
volgt.

Geef op de WBASS-2 commandoregel de commando's:
SET/H a,"H"
SET/B a,"B"

De getallen zullen  nu  bekeken  worden  volgens  de  tweede
notatievorm. Laad nu de om  te  zetten  file  die  in  ASCII
formaat moet staan.

Doe nu:
SET/H v,"&H"
SET/B v,"&B"

Als je nu weer EDIT in typt  zullen  alle  getallen  omgezet
zijn naar de eerste notatievorm. Nu hoef je alleen nog  maar
het ' teken achter alle EX AF,AF' instrukties weg  te  halen
en de hele file is omgezet. Andersom  werkt  dit  natuurlijk
ook.


Uitleg van begrippen die in dit artikel genoemd zijn

Voor het geval je niet weet wat macro's zijn  komt  nu  even
een korte uitleg. Macro's zijn  vormen  van  labels  die  je
gewoon in je assembly code kunt zetten  waarna  de  computer
tijdens het assembleren inplaats van dat  label  een  aantal
instrukties neer gaat zetten. Meestal gebeurt dit in de vorm
van een subroutine. Op deze manier voorkom je  dat  je  voor
elk programma de code  van  eigen  BIOS  routines  hoeft  te
kopi�ren, maar inplaats daarvan vermeld  je  aleen  de  file
waar je eigen BIOS calls in staan en hoef je alleen nog maar
de macro in je programma neer te zetten inplaats van de hele
routine.


        C O N C L U S I E

Ik heb gemerkt dat mensen die met GEN 80 werken liever  niet
omschakelen naar WBASS-2 en andersom. Ik heb in ieder  geval
geprobeerd om een objektief oordeel over  beide  programma's
te geven. Aan GEN 80 is  weinig  meer  te  veranderen,  maar
WBASS-2 zou nog uitgebreid kunnen worden. Indien  de  totale
ruimte voor edit data met 100 kB zou groeien en de gebruiker
precies zou kunnen bepalen waar deze  data  staat,  dan  zou
dat een zeer waardevolle aanvulling  van  het  pakket  zijn.
Vooral bij de wat grotere programma's wordt  het  lastig  om
toch nog in het RAM te assembleren, maar  voor  de  rest  is
WBASS-2 de beste keus voor je MSX assembly creativiteit.

                              Alex van der Wal
