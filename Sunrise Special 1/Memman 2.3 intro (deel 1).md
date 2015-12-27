M E M M A N   2 . 3   I N T R O D U C T I E


File : MM23INTRO.TXT c  MST
Datum: 18 september 1991
Door : Ries Vriend/Ramon van der Winkel/Robbert Wethmar


   MST's MemMan 2, de MSX Memory Manager

Begin  1990 riep  MSX Computer  Magazine voor  het eerst  de
beste  MSX  programmeurs  van  Nederland bij  elkaar met  de
bedoeling  de  MSX  wereld  nieuw  leven  in  te  blazen. De
programmeursgroep  maakte   kennis  en   er  werden   ideeen
uitgewisseld.  Er bleek behoefte aan een Memory Manager, een
programma dat het geheugen van de MSX beheert.

Met de Memory Manager worden twee doelen nagestreefd:

1) Het zoeken  en gebruiken  van geheugen wordt eenvoudiger.
Het  zoeken wordt  door MemMan gedaan terwijl het gebruik
van  geheugen zoveel  mogelijk wordt  losgekoppeld van de
configuratie:  'oude'  uitbreidingen,  een, twee  of meer
mappers, MemMan heeft er geen moeite mee.
2) Het wordt mogelijk meerdere programma's tegelijkertijd in
het  geheugen te  laden zonder  dat ze  elkaar in  de weg
zitten.    Hierbij   wordt    gedacht   aan    RAMdisk's,
printerbuffers en op de achtergrond werkende programma's.

Met versie 1 van MemMan - ge�ntroduceerd op 9 september 1990
-  is  de  eerste  doelstelling  bereikt.  Nu  is  de tweede
doelstelling ook bereikt.

MemMan versie  2 kan  meerdere programma's  'ergens' in  het
geheugen  laden laten  werken, zonder dat ze van elkaar last
hebben.  Op  andere  computer  merken was  deze techniek  al
langer  bekend.  Dergelijke  programma's  worden daar  TSR's
genoemd,  vandaar  ook  hier:  Terminate  and  Stay Resident
programma's.

Hopelijk zullen nog meer programma's van MemMan gebruik gaan
maken en bestaande programma's voor MemMan worden aangepast.
Een direct  voordeel is dat het programma dan ook direct met
bijvoorbeeld  64  kB  modules en  zelfs met  meerdere memory
mappers kan werken, iets dat de meeste bestaande programma's
niet of niet goed doen.

MemMan  versie 2  zal net  als de  eerste versie  als Public
Domain  de  wereld in  gestuurd worden.  Dat wil  zeggen dat
iedereen vrij  van MemMan  gebruik mag  maken. Het  is zelfs
toegestaan  MemMan als  onderdeel van een commercieel pakket
te verkopen.  Alleen zo kan het programma uitgroeien tot een
aanvulling  op de  MSX standaard.  Er zullen  twee pakketten
uitgebracht  worden.  De  eerst  is  voor  de gebruiker  van
MemMan. Dit  pakket zal  MemMan en  een aantal tools voor de
TSR's  bevatten. Het  tweede pakket bevat ontwikkel tools en
technische documentatie over het programmeren van TSR's. Dit
laatste pakket is geen Public Domain.


Aan MemMan werkten en dachten mee:

Ramon van der Winkel
Ries Vriend
Robbert Wethmar
Paul te Bokkel
Markus The
en een aantal anderen die met hun opbouwende kritiek MemMan
hielpen worden tot wat het is.


      H E T   C O N F I G U R E R E N

MemMan is er in twee versies: een .BIN en een .COM file. Het
zal duidelijk  zijn dat  de .BIN versie vanuit BASIC met een
BLOAD"MEMMAN.BIN",R  instructie geladen  kan worden, terwijl
de  .COM  vanuit MSXDOS  gestart kan  worden door  simpelweg
MEMMAN in  te tikken. Beide versies keren na het laden - via
een zogenaamde warm boot - automatisch terug naar BASIC. Als
de  .COM versie  vanuit MSXDOS  opgestart wordt, dan kan een
commandline mee  gegeven worden.  Dit zijn  commando's welke
uitgevoerd  zullen worden alsof ze ingetikt zijn. Een Return
kan met  het @  teken worden opgegeven. Er kunnen meerdere @
tekens  in de  commandline worden  opgegeven, zodat meerdere
commando's na elkaar uitgevoerd kunnen worden.

Bijvoorbeeld:

A>MEMMAN _SYSTEM@TL CAPS@

Na het  opstarten van  MEMMAN zal  naar MSXDOS  teruggekeerd
worden en de TSR "CAPS" ingeladen worden.

Met   behulp  van   CFGMMAN  is   het  mogelijk  een  aantal
instellingen van  MemMan en  een default  commandline op  te
geven.  CFGMMAN  kan  zowel  de  .COM  als  de  .BIN  versie
configureren. Met betrekking tot de TSR's kunnen de volgende
instellingen veranderd worden:

- Default command line
Hier  kan de standaard commando-regel ingevoerd worden. Deze
commando-regel wordt  uitgevoerd nadat  MemMan ge�nstalleerd
is.  Na het laden van de .BIN versie van MemMan wordt altijd
de   standaard   commando-regel  uitgevoerd.   De  standaard
commando-regel wordt  niet uitgevoerd  indien MemMan  vanuit
DOS  opgestart wordt met een commando regel als argument. Na
foutmeldingen   van   MemMan  wordt   geen  commando   regel
uitgevoerd.

- Heap grootte
Sommige  TSR  programma's  hebben  extra  geheugen  nodig in
geheugen pagina  3, waar  ze normaal  gesproken geen toegang
toe hebben. De heap is een stuk geheugen in pagina 3 dat wel
voor  TSR's toegankelijk is. Wanneer een TSR meldt dat er te
weinig  heap  geheugen  beschikbaar  is  dient  deze  waarde
verhoogd te  worden. Meestal zal het toevoegen van 100 extra
bytes  heap-geheugen  de  problemen  uit  de wereld  helpen.
Wanneer  een  TSR  meer  heap-ruimte  nodig  heeft  dient de
handleiding dat te vermelden.

Elke verandering van de heap-grootte heeft slechts effect na
het opnieuw laden van MemMan.

- Maximum aantal TSR's dat tegelijk aanwezig kan zijn
Het  aantal TSR's  dat onder  MemMan 2 geladen kan worden is
beperkt. Wanneer  de TSR  Loader (TL)  de melding 'TSR Table
Full' geeft dient deze waarde verhoogd te worden.

- Maximum aantal hooks dat tegelijk afgebogen kan zijn
Het  aantal hooks  dat door  alle in  het geheugen aanwezige
TSR's kan worden afgebogen is beperkt. Wanneer de TSR Loader
de  melding  'Hook  Table  Full'  geeft  dient  deze  waarde
verhoogd te worden.

- Recursiediepte
Wanneer TSR's  elkaar of  zichzelf te vaak aanroepen zal het
systeem  op een  gegeven moment  vastlopen. Door de maximale
recursiediepte te  verhogen kunnen  deze problemen voorkomen
worden.  TSR's die  zichzelf aanroepen  dienen dat  - met de
benodigde recursiediepte - te vermelden in de handleiding.


       H E T   I N S T A L L E R E N

Om  MemMem  vanuit  MSX-DOS  te  laden  is het  intikken van
`MEMMAN' achter  de `>'-prompt voldoende. Vanuit BASIC dient
het commando BLOAD "MEMMAN.BIN",R ingevoerd te worden. Na de
installatie   van  MemMan  wordt  in  beide  gevallen  BASIC
gestart,  waarna  de  standaard  commando  regel  uitgevoerd
wordt,  zoals   opgegeven  in   het  configuratie  programma
CFGMMAN.  De standaard  commando-regel wordt niet uitgevoerd
indien MemMan  vanuit DOS  gestart wordt met een vervangende
commando-regel als argument.

Versie  2 van  MemMan neemt  behalve een stuk BASIC-geheugen
ook een  16 kB  segment in  beslag. Hierdoor blijft er onder
BASIC  en MSXDOS  zoveel mogelijk  geheugen beschikbaar.  De
ruimte  die  in  het  16  kB  segment over  is wordt  indien
mogelijk  gebruikt   om  TSR's   in  onder  te  brengen.  De
hoeveelheid BASIC-geheugen die MemMan gebruikt kan be�nvloed
worden door middel van het configuratieprogramma CFGMMAN.

Wanneer  MemMan onder  DOS2 ge�nstalleerd wordt blijven alle
segmenten -  behalve die  ene die  MemMan zelf nodig heeft -
ook  voor DOS2  beschikbaar. Het is dus zonder meer mogelijk
eerst MemMan te installeren en daarna een DOS2 RAMdisk.

Alvorens zichzelf  in het RAM te nestelen controleert MemMan
natuurlijk  of er  al een  versie van MemMan aanwezig is. In
dat laatste  geval verschijnen de info-regels, aangevuld met
de  mededeling  dat  MemMan reeds  ge�nstalleerd is.  Verder
gebeurt er niets. De commandoregel wordt gewoon uitgevoerd.


  Terminate and Stay Resident programma's

Gewoonlijk  zal  een  programma  na  uitvoering niet  in het
geheugen  achterblijven. Programma's die dat wel doen worden
aangeduid met de afkorting TSR: Terminate and Stay Resident.
Voorbeelden van  dergelijke programma's zijn: printerbuffers
en   RAMdisks.  Maar  ook  andere  toepassingen,  zoals  een
rekenmachine of  een kalender  die met  een enkele toetsdruk
opgeroepen kan worden zijn denkbaar.

In het verleden zijn TSR's voor de MSX een tamelijk zeldzaam
verschijnsel  geweest.  Het  probleem was  namelijk dat  het
geheugen  dat de  TSR gebruikt  ook door  andere programma's
gebruikt kan  worden. Er  zijn in  een standaard MSX machine
geen  mogelijkheden om  een stuk  geheugen voor  een TSR  te
reserveren. Dit  probleem wordt  door MemMan  uit de  wereld
geholpen.  MemMan beheert  het geheugen en zorgt er voor dat
er geen geheugenconflicten optreden.

Dankzij MemMan  is het  mogelijk meerdere  TSR's tegelijk in
het  geheugen te  hebben, waarbij  elke TSR  maximaal 16  kB
groot kan  zijn. Op  de standaard  MSX is het laden van meer
dan  ��n TSR al lastig en alleen mogelijk als de TSR niet al
te groot  is. Met  de invoering  van MemMan  2 krijgt de MSX
betere  TSR  mogelijkheden  dan  de  alom  gewaardeerde  PC.
Bovendien  doen ze  niet onder  voor de `Desktop Accesoires'
zoals die op de Macintosh en de Atari ST gebruikt worden.

Bij MemMan  worden twee eenvoudige voorbeeld TSR's geleverd.
Ze  doen weinig  zinvols, maar  demonstreren wel degelijk de
kracht  vn  Terminate  and  Stay  Resident  programma's.  De
voorbeelden zijn  CAPS.TSR en  BEEP.TSR. De  eerste laat het
Caps  lampje  knipperen,  de  tweede  maakt  dat  het  Basic
commando  CMD BEEP een pieptoontje genereerd. In de toekomst
zullen  er  echter  meer  en  meer  TSR's  verschijnen,  met
mogelijkheden waar  de MSX  gebruiker tot  voor kort  alleen
maar van kon dromen.

(Nvdr.  De tekst  was langer  dan 16  kB, en  is dus in twee
delen gesplitst.)
