# M U I Z E N I S S E N


Het valt me telkens weer  op  dat  er  op  MSX  maar  weinig
gebruik gemaakt wordt van de muis. Dit in tegenstelling  tot
andere systemen  waar  de  muis  een  absolute  noodzaak  is
geworden. Het is dan ook een heel slim  apparaatje  dat  een
gigantische  brug  kan  slaan  tussen  de  software  en   de
gebruiker. Het is toch fantastisch dat er zo'n doosje is dat
als je het  beweegt  een  gelijksoortige  beweging  van  een
voorwerp op het scherm teweeg brengt!


## D E   G E S C H I E D E N I S

De muis is ��n van de belangerijkste  computerontwikkelingen
van de afgelopen 15 jaar. Voorheen moesten  alle  commando's
domweg ingetikt worden en dat maakte de computer  niet  echt
toegankelijk voor het grote publiek omdat de  besturing  van
die dingen alleen maar door een klein elitair groepje mensen
gedaan kon worden. Daar is met de komst van de muis  nu  een
radikaal einde aan gekomen. De muis is zelfs zo  'makkelijk'
dat een kind dat nog maar amper praten al met  een  computer
overweg kan omdat de besturing zo eenvoudig is geworden.

De muis op zich is niet duur, maar  de  verdere  (grafische)
weergave bleek  ontzettend  duur  te  zijn.  Daar  kwam  pas
verandering in toen  computers  goedkoper  werden  omdat  de
produktie  omhoog  ging.  Plotseling  werden  massaal  video
kaarten (en losse VDP's) gemaakt die de muis nu ook voor het
grote publiek toegankelijk maakten.


## D E   M U I Z E N   V A N   N U

Nu, zoveel jaar later  kost  een  muis  niet  meer  dan  150
gulden. Er zijn muizen in alle soorten en  maten  en  er  is
gelukkig   een   zekere   standarisatie   van   aansluiting.
Natuurlijk had IBM in haar oneindige wijsheid besloten om de
zaken zo aan te pakken dat er een behoorlijke connector voor
nodig is om een muis aan een PC te  hangen,  maar  er  waren
gelukkig ook verstandige mensen die de muis  gewoon  aan  de
bekende 'Joystick' poort connector gehangen hebben.

Bij mijn weten gebruiken MSX, Amiga en Atari deze  connector
en ze zijn ook volledig uitwisselbaar (alhoewel de standaard
meegeleverde muizen van de laatste twee matig tot slecht van
kwaliteit zijn).

## D E   W E R K I N G

Er zijn ruwweg 3 soorten muizen, waarvan  er  twee  weer  op
elkaar lijken, nl:
- de elektro mechanische muis
- de opto mechanische muis
- de optische muis

De eerste vorm is meteen  ook  de  oudste  en  gebruikt  het
overbekende 'balletje tegen  asje'  systeem.  Aan  die  twee
assen zitten echter twee  pot-meters  (variabele  weerstand)
die 360 graden kunnen draaien. Door nu de weerstand te meten
kon de positie van de muis berekend worden.  Deze  vorm  had
echter als nadeel dat het  balletje  makkelijk  over  de  as
gleed waardoor de as niet draaide.  Dit  slippen  werd  weer
veroorzaakt doordat de pot-meter  teveel  wrijvingsweerstand
genereerde. Verder sleten de  pot-meters  snel  waardoor  de
weerstandsmeting niet meer goed  ging.  Deze  muizen  worden
tegenwoordig niet meer gemaakt en wie er ��n heeft moet  dat
ding maar in de kast leggen,  want  het  kon  wel  eens  een
museumstuk worden.

De tweede soort muis is de meest gebruikte  muis,  en  werkt
precies hetzelfde als de eerste vorm met het verschil dat de
pot-meters vervangen zijn door twee  'wieltjes'  met  gaten.
Affijn, omdat de meeste mensen de rest van  de  werking  wel
weten zal ik er verder geen woorden  over  vuil  maken.  Het
voordeel van deze muis boven de eerste is dat hij niet slijt
en dat er heel weinig wrijvingsenergie op de asjes komt.

Deze eerste twee vormen hebben echter een gezamelijk nadeel;
als ze vuil worden gaan ze haperen (oftewel  wrijving).  Dit
nadeel is opgelost met de komst van  de  volgende  generatie
muizen, de optische muis. Deze  muis  heeft  geen  bewegende
onderdelen  en  wrijving  heeft  dus  geen  invloed  op   de
prestaties. Het komt er op  neer  dat  er  in  de  muis  een
lichtje brandt samen met een element dat licht  opvangt.  De
muis wordt bewogen over een mat met horizontale en vertikale
lijnen  die  licht  weerkaatsen.  Affijn,  als   het   licht
weerkaatst vangt het andere element het op en zo herkent  de
muis dat hij beweegt.

Al deze muizen werken echter wel met  het  principe  dat  ze
verplaatsing  meten  en  niet  plaats.  Ze  leveren  dus  de
afgelegde afstand tussen twee metingen aan  de  computer  af
die deze meting dan in een positie omzet. Dit  gebeurd  door
de afstand bij de vorige positie op te tellen.


## D E   M U I S   L E E S A K T I E

Er  is in  het verleden nogal wat verwarring geweest over de
vraag hoe  een MSX2  de muis leest omdat de VDP van een MSX2
een  hardwarematische muisleesroutine bevat, die echter niet
gebruikt  wordt.  De  twee  joystickpoorten  zitten  op alle
MSX'en aan  register 14  en 15 van de PSG en de VDP heeft er
geen bal mee te maken.

Om een komplete leesaktie te doen is nogal wat  nodig  omdat
de muis maar de beschikking heeft over een 4  bits  databus.
Om dus 2 keer een 8 bits offset te lezen (X en  Y)  moet  er
dus 4 keer gelezen worden waarbij de muis eerst verteld moet
worden dat er gelezen  gaat  worden.  Aan  de  hand  van  de
routine (GTMOUS) zal ik nu het algoritme verklaren.

We nemen even aan dat de muis in poort 1 zit. In  dat  geval
staat in (PORT) de waarde 10111111B en in (PORT+1) de waarde
00010000B

GTMOUS:
- Lees PSG register 15
- Wis bit 6, dit geeft aan dat de muis in poort 1 zit
(Zet bit 6 om aan te geven dat de muis in poort 2 zit)
- Zet bit 4, zodat de muis weet dat nu XH klaargezet moet
worden. Bit 5 doet hetzelfde voor poort 2
Opm: XH/YH = Bit 4-7 van X/Y offset
XL/YL = Bit 0-3 van X/Y offset
- Wacht even op muis (het is een relatief traag ding)
- Lees XH (Verdere details volgen)
- Lees XL
- Lees YH
- Lees YL
Dit lezen is niet eenvoudig omdat de muis  niet  automatisch
weet wanneer ��n van de waarden gelezen is. Om aan  de  muis
te vertellen dat je de  waarde  gelezen  hebt,  moet  bit  4
(indien de muis in poort 2 zit bit 5) van  PSG  register  15
ge�nverteerd  worden.  Als  tijdens  de  volgende  leesaktie
register  15  weer  geschreven  wordt  zal   de   muis   dit
ge�nverteerde bit zien waarop het  beestje  de  volgende  te
lezen waarde op zijn 4  bits  databus  zet.  Na  een  kleine
wachttijd wordt deze waarde dan uit register 14 gelezen.


## E E N   M U I S B E S T U R I N G S P R O G R A M M A

Laat ik beginnen met het bedanken van Stefan  Boer  die  het
basisprogramma voor de muisbesturing heeft gemaakt  waar  ik
op voortgeborduurd heb.

De routine is vrij universeel en werkt gegarandeerd op  alle
MSX'en (vanaf MSX2). Als uitvoer  zal  een  sprite  op  het
scherm getekend worden die de beweging van  de  muis  volgt.
Wat meteen opvalt  is  dat  het  programma  nogal  groot  is
geworden, maar dat komt omdat er maar heel weinig BIOS calls
gebruikt worden. De muisroutine van de MSX2 en turbo R staat
nl. in een subslot en dat maakt de routine  relatief  traag.
Voor de meeste routines is dit niet zo'n bezwaar,  maar  als
een routine vaak aangeroepen wordt is het  toch  wel  handig
dat alles zo vlot mogelijk verloopt.

Verder bevat het programma  een  schat  aan  andere  nuttige
informatie. Wat dacht je bv. van de vuurknop  uitleesroutine
die feitelijk  een  kopie  is  van  de  routine  die  in  de
interruptafhandelingsroutine van FD9FH staat.  Deze  routine
kijkt naar alle vuurknoppen (0 t/m 5) en doet dit 5 keer  zo
snel als de BIOS routine omdat je die in dit  geval  5  keer
aan zou moeten roepen. In principe is deze routine overbodig
omdat er in het systeem RAM ook een byte is waar de  huidige
vuurknop status in staat, maar ik heb hem er toch bij  gezet
omdat er onder MSX programmeurs de trend bestaat om het hele
systeem plat te leggen zodat er geen processortijd  verloren
gaat aan -voor hen- overbodige dingen en in dat geval  wordt
dit byte niet meer gezet.

Verder  bevat het  programma een  routine die voor SCREEN 5,
page 0 twee sprites initialiseert en alle andere sprites weg
definieert. Je  merkt het  al, dit  programma bevat meer dan
alleen maar een simpele muisbesturing.


## H O E   W E R K T   H E T ?

Als het programma opgestart wordt zullen eerst de  benodigde
sprites ge�nitialiseerd worden. Daarna  zal  naar  een  muis
gezocht worden (CHKPRT) die in de variabele (PORT &  PORT+1)
aangeeft of en in welke poort de muis zich bevindt. Is  geen
muis aangesloten dan zal de waarde 0  in  (PORT)  staan.  Nu
wordt nog een interruptroutine ge�nstalleerd  die  bij  elke
interrupt de sprites zal herplaatsen.

Verder zal de routine ook altijd met  de  cursorkeys  werken
(logisch nietwaar??). Het uiteindelijke  resultaat  is  dus
een hele snelle routine die zelf wel uitzoekt of en waar een
muis is aangesloten  en  die  zowel  met  muis  als  met  de
cursorkeys tegelijkertijd een 'arrow' over  het  scherm  kan
bewegen in een van tevoren opgegeven venster.

Een  ander leuk  detail is dat de muisroutine op zich ook op
MSX1 werkt,  maar dat  dan wel de muisplaats en initialiseer
routines herschreven moeten worden.

Voor meer informatie over het hoe en wat van sprites moet ik
jullie naar ��n van mijn eerdere artikelen verwijzen die  op
de GENIC ClubGuide Special #2 staat.

Het programma staat los op  deze  disk  (hoop  ik)  en  heet
MOUSE.ASC  Als u niet zo'n ster bent in het bedienen van een
assembler  kunt  u  ook  gewoon  het   programma   MOUSE.BIN
opstarten.

Ik hoop dat ik wat duidelijkheid heb gebracht in  de  wereld
van MSX muizen en natuurlijk  wil  ik  in  de  toekomst  een
stormvloed van programma's zien die de muis  gebruiken,  dat
spreekt voor zich (ik bedoel dus dat ik hoop dat dit  muisje
nog een staartje krijgt).

Alex van der Wal                
