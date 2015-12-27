- DEEL 1 -

H Y B R I D E   P R O G R A M M E R E N   ( 1  )


I N L E I D I N G

Dit is het eerste deel van een serie artikelen waarin ik wat
dieper in  ga op  het zogenaamde "Hybride programmeren", een
combinatie  van BASIC  en machinetaal.  In de  artikelen zal
vooral aandacht  worden besteed  aan de  hulpmiddelen die de
MSX  standaard  biedt  om  de  samenwerking  beter  te laten
verlopen,  en waar  men bij het hybride programmeren op moet
letten.

Maar  eerst zal  ik uitleggen  waar de  naam hybride vandaan
komt.


H Y B R I D E

Het woord  "hybride" komt  zowel in de Nederlandse als in de
Engelse  taal  voor  en  betekent  hetzelfde,  namelijk  het
resultaat   van  een   kruising  tussen  twee  verschillende
diersoorten.  Bekende  hybriden zijn  muilezels, muildieren,
scheiten en  gapen. Hybriden zijn altijd steriel, en bij het
fokken  moeten  afstotingsverschijnselen  worden onderdrukt.
Maar genoeg biologie, terug naar MSX.

Spreken  we  het  woord op  z'n Engels  uit (haibraid),  dan
bedoelen  we er  in de  computerwereld iets  anders mee: een
kruising tussen twee programmeertalen. Bij MSX is dat altijd
BASIC en machinetaal. Ik ga er in deze artikelen van uit dat
de  lezer  zowel  BASIC  als machinetaal  in redelijke  mate
beheerst.


G R E N Z E N

MSX BASIC  heeft een  werkgebied nodig  voor het opslaan van
een  BASIC programma en de daarbij behorende variabelen. Dit
werkgebied begint  normaal gesproken op &H8000, maar dit kan
worden  veranderd.  Het  adres  waarop  het BASIC  programma
begint wordt opgeslagen op de volgende locatie:

TXTTAB (&HF676, 2 bytes) Begin van "BASIC text area"

Op (TXTTAB)-1  moet altijd een 0 staan. Normaal gesproken is
TXTTAB  gevuld met  de waarde  &H8001, en bevat adres &H8000
dus een 0.

Na het veranderen van TXTTAB moet er een NEW worden gegeven,
zodat  ook  andere  adressen  (zoals  het beginadres  van de
variabelentabel)  kunnen worden  aangepast. Ditzelfde  wordt
gedaan  door  een  RUN"FILENAAM.BAS"  of  LOAD"FILENAAM.BAS"
instructie, er wordt dan door de BASIC interpreter eerst een
NEW uitgevoerd,  en daarna wordt het BASIC programma geladen
en (bij  RUN) ge�xecuteerd. U kunt dus na het veranderen van
TXTTAB   ook  een  RUN"FILENAAM.BAS"  of  LOAD"FILENAAM.BAS"
commando geven.

U  kunt   in  BASIC  het  startadres  van  BASIC  als  volgt
veranderen: (veranderen naar &HC000)

POKE&HF677,&HC0
POKE&HF676,1
POKE&HC000,0
NEW

Het  kan  ook  vanuit  een  programma. Stel  u wilt  dat het
programma  "EXAMPLE.BAS"  op  &HC000 staat.  U kunt  dat als
volgt programmeren:

10 'EXAMPLE.BAS
20 IF PEEK(&HF677)<>&HC0 OR PEEK(&HF676)<>1 THEN
POKE&HF677,&HC0:POKE&HF676,1:POKE&HC000,0:
RUN"EXAMPLE.BAS"
30 .....

Vanaf regel  30 kunt u uw eigen programma invullen, save het
programma  als  "EXAMPLE.BAS".  Wordt dit  programma geladen
terwijl  het adres al goed staat, dan wordt er direct verder
gegaan met  regel 30.  Anders wordt  het adres goed gezet en
laadt  het programma  zichzelf nogmaals in, alleen nu op het
goede adres.

Het gebied  vanaf &H8000 tot (TXTTAB)-1 mag door machinetaal
worden gebruikt, hier kan BASIC nooit komen.

Ook  achter het  BASIC programma  is nog een gebied dat door
machinetaal gebruikt  kan worden.  Dit kan voor BASIC worden
beschermd met de CLEAR instructie.


C L E A R

Dit   is  een   veelgebruikte  (en   meestal  noodzakelijke)
instructie bij  hybride programmeren.  Eerst maar  de syntax
van het CLEAR commando:

CLEAR [<stringruimte>][,<adres>]

Voor  de stringruimte  moet een  grootte in  bytes voor  het
datagebied voor  strings worden  ingevuld. Standaard  is dit
200.  Bij programma's die veel strings gebruiken is dat niet
genoeg, er  volgt dan een "Out of string space" foutmelding.
Zo'n  foutmelding kan  worden voorkomen door de stringruimte
met  het  CLEAR  commando  te  vergroten.  Alleen een  CLEAR
commando, dus zonder parameters, wist alle variabelen.

Voor het  adres moet  het hoogste  adres dat  door BASIC mag
worden  gebruikt worden  ingevuld. Lees  dit goed, het adres
zelf mag  dus nog door BASIC worden gebruikt. Wilt u dus een
werkgeheugen voor machinetaal vanaf adres &HD000 reserveren,
dan moet het commando als volgt luiden:

CLEAR 200,&HCFFF

En niet  CLEAR 200,&HD000,  omdat dan de eerste byte van het
machinetaalgedeelte toch door BASIC kan worden veranderd. In
een  hybride  programma  dat  het  gebied  boven  het  BASIC
programma  gebruikt moet  ALTIJD een  CLEAR commando  worden
gezet!

Met  het CLEAR commando wordt de inhoud van HIMEM veranderd,
maar CLEAR  doet nog  veel meer.  Zoals het  wissen van alle
variabelen en het verplaatsen van een aantal werkgebieden.

HIMEM (&HFC4A, 2 bytes) Bevat het hoogste adres dat door
BASIC mag worden gebruikt.

De  stack verhuist  dan mee  naar het  gebied onder (HIMEM).
(Tussen  haakjes  betekent "de  inhoud van".)  Zoals u  weet
groeit de  stack aan  de onderkant  aan. De top van de stack
wordt ook in het systeem gebied opgeslagen:

STKTOP (&HF674, 2 bytes) Bevat de top van de stack.

Behalve de stack worden ook de File Control Blocks (niet die
van de BDOS, maar van Disk BASIC, 267 bytes per stuk, aantal
in te stellen met MAXFILES) verplaatst.


D E   B O V E N G R E N S

De ondergrens  van het  machinetaalprogramma wordt  dus door
het CLEAR commando bepaald. Maar wat is de bovengrens?

Bij  een MSX computer zonder diskdrive is dat erg eenvoudig.
Het  systeemgebied  van  de  MSX  begint  namelijk op  adres
&HF380,  dus  het  gebied  tot  &HF37F  is  vrij  voor eigen
gebruik.

Bij ��n  of meer  aangesloten diskdrives  wordt de  situatie
anders. De diskROM gebruikt namelijk een stuk geheugen onder
&HF380  als werkgeheugen,  en de  grootte van dat werkgebied
kan  behoorlijk vari�ren.  Dit is afhankelijk van het aantal
diskdrives  en  de  grootte  (enkelzijdig  of  dubbelzijdig)
daarvan.

Meestal  heeft een MSX2 computer 2 softwarematige drives, en
��n  hardwarematige.   Sommige  luxe  modellen  hebben  twee
hardwarematige drives (een hardwarematige drive is een echte
diskdrive,  waar  je  een diskette  in kunt  steken, en  een
softwarematige  drive is een drive die door de diskROM wordt
ondersteund.  Heeft  een  computer  maar ��n  hardwarematige
drive,  dan  zal  de  diskROM  de  softwarematige  B:  drive
simuleren met  een "Insert  diskette for  drive B:" op de A:
drive).   De   tweede   softwarematige   drive   kan  worden
afgekoppeld  door  met  de  [CTRL]  toets  ingedrukt  op  te
starten.  Hierdoor komt  het werkgeheugen  dat eerst voor de
softwarematige B: drive werd gebruikt vrij voor gebruik door
BASIC of machinetaal.

Het  maakt   ook  uit   of  de   diskdrive  enkelzijdig   of
dubbelzijdig  is. Er  kunnen ook  nog meer diskdrives actief
zijn, als  er bijvoorbeeld  een extra losse interface in een
cartridgeslot wordt gestoken.

Tot  slot speelt  ook het actieve disk besturingssysteem een
rol. Bij Disk BASIC 1.0 (DOS1) is een veel groter werkgebied
nodig dan bij Disk BASIC 2.01 (DOS2). Kortom, u mag er nooit
vanuit  gaan  dat het  werkgeheugen van  de diskROM  op elke
computer hetzelfde is.

Waar mag  u dan  wel van uit gaan? Als u de computer opstart
bevat  HIMEM altijd de juiste waarde. Het geheugen onder dat
adres wordt niet door de diskROM gebruikt, en kan dus veilig
voor machinetaal  worden gebruikt.  BASIC zorgt  er zelf wel
voor   dat  het  werkgeheugen  van  de  diskROM  niet  wordt
overschreven.

Lees direct na het opstarten dus HIMEM uit, en controleer of
uw  machinetaal niet  boven dat  adres uit komt. Is dat toch
zo,  geef  dan  een  melding  "Reset  met  [CTRL]"  of  iets
dergelijks, en  stop de  executie van  het programma.  Is er
niets  aan de  hand, dan  kunt u  rustig verder gaan met het
programma. Als  zelfs het  opstarten met  [CTRL] niet helpt,
dan  is  kan  het programma  helemaal niet  op die  computer
worden gedraaid, in ieder geval niet in die configuratie. De
gebruiker kan dan bijvoorbeeld de C: en D: drive afkoppelen.

Het is  belangrijk om HIMEM uit te lezen voordat u een CLEAR
geeft,  omdat CLEAR de waarde van HIMEM verandert. Het CLEAR
commando hoort  trouwens vooraan  het BASIC programma, omdat
het alle variabelen wist.


V E I L I G E   B O V E N G R E N S

Bij bovenstaande  methode moet er toch een bovengrens worden
gekozen.  En het  liefst een die op alle computers werkt, en
het  liefst  een  waarbij  niet met  [CTRL] hoeft  te worden
opgestart.  Houd  de  bovengrens  altijd  zo  laag mogelijk.
&HD7FF  is  een zeer  veilige waarde,  die met  vrijwel geen
enkele configuratie problemen zal geven.


S T A C K

Houd wel  rekening met  de stack. In principe geeft die geen
problemen,  maar als  u eigenwijs  toch geen  CLEAR commando
gebruikt, dan  staat de  stack ergens een paar honderd bytes
onder de waarde van HIMEM bij het opstarten.

Misschien  is het  even handig  om de geheugenindeling onder
BASIC te geven:

- BASIC programma
- Variabelen
- Arrays
- Vrij
- Stack
- Strings
- File Control Block
- Machinetaalprogramma
- Werkgebied Disk BASIC
- Werkgebied BIOS


H I M E M   O P S L A A N

Als  de  besturing  aan  het  eind  van  het programma  weer
volledig aan BASIC wordt overgedragen, zorg dan dat HIMEM en
TXTTAB weer terug worden gezet. Zet TXTTAB daarbij op &H8001
(vergeet  niet de  POKE &H8000,0)  en zet HIMEM op de waarde
die er  bij het opstarten in stond. Om dit te kunnen doen is
het nodig dat de waarde van HIMEM direct na het opstarten op
een veilige plaats wordt bewaard.

We  weten  nu  hoe  we  ervoor kunnen  zorgen dat  het BASIC
werkgebied,   het  machinetaal  werkgebied,  het  BIOS/BASIC
werkgebied en het diskROM werkgebied elkaar niet storen, dus
we kunnen beginnen met het eigenlijke hybride programmeren.


  U S R

Bij  hybride programmeren  wordt het  USR commando van BASIC
het  meeste  gebruikt.  Met  DEFUSR  kunnen  10 machinetaal-
routines worden gedefinieerd. Dit gaat als volgt:

DEFUSR<nummer>=<adres>

Waarbij  voor <nummer>  een getal van 0 t/m 9 wordt ingevuld
(weglaten  =  0)  en  voor  <adres>  het  beginadres  van de
machinetaalroutine. Voorbeeld:

DEFUSR3=&HD000

De  machinetaalroutine  die op  adres &HD000  begint kan  nu
worden opgeroepen met USR3. De DEFUSR adressen worden in het
werkgebied  opgeslagen  in de  tabel USRTAB,  die begint  op
adres &HF39A.  Deze tabel  bevat 10 adressen van 2 bytes per
stuk,   en  is  dus  20  bytes  lang.  POKE  &HF39A,0:  POKE
&HF39B,&HC0 is dus hetzelfde als DEFUSR=&HC000.

Het  aanroepen  van  een  machinetaalroutine  gaat  door het
opvragen van  een waarde!  Het USR commando is dus eigenlijk
een  functie. Aan  het USR commando kan ��n parameter worden
meegegeven. De syntax luidt als volgt:

USR<nummer>(<parameter>)

Voor  het  <nummer>  moet  weer  een  getal  van  0-9 worden
ingevuld. De  parameter kan  niet worden weggelaten. Het USR
commando wordt als functie gebruikt. Een paar voorbeelden:

U=USR(0)
PRINT USR3("HALLO")
A$=USR2(X$)+USR5(3)
PRINT USR6(3)*USR7("D")
P=3+USR(8.8354743)
X=LEN(USR9(5))

Als  de  waarde van  USRx wordt  opgevraagd, wordt  naar het
adres  dat   met  DEFUSRx  is  gedefinieerd  gesprongen.  De
parameter   kan   door   het   machinetaalprogramma   worden
uitgelezen,   en  het   machinetaalprogramma  kan   ook  het
resultaat weer  aan BASIC  doorgeven. Dit  is dus de "waarde
van  USRx". Dit  kan een  getal zijn (INT, SNG of DBL), maar
ook een  string. Hoe gaat dit doorgeven van parameters nu in
zijn werk?

(Nvdr.  Om dat  te weten  te komen zult u eerst terug moeten
gaan naar  het submenu  om daar het tweede deel in te laden.
De oorspronkelijke tekst was langer dan 16 kB.)

                       Stefan Boer
