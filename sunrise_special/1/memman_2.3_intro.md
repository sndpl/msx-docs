# M E M M A N   2 . 3   I N T R O D U C T I E


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

        T S R S   L A D E N

TSR  programma's zijn  te herkennen  aan de  extensie van de
bestandsnaam: ze eindigen op .TSR. Deze files bevatten naast
de eigenlijke programmacode ook alle informatie die nodig is
om de TSR goed in het geheugen te installeren.

Om bijvoorbeeld  de demonstratie  TSR 'CAPS' - die overigens
niets  anders doet  dan het Caps lampje laten knipperen - te
laden moet onder MSX-DOS ingetikt worden:

TL CAPS

Er  mogen  meerdere TSR-bestandsspecificaties  achter elkaar
worden opgegeven. Indien MSX-DOS2 gebruikt wordt, mag ook de
subdirectory worden  opgegeven, waaruit  de TSR geladen moet
worden.  Bijvoorbeeld, door middel van het volgende commando
worden zowel  de TSR  `CAPS' ingeladen,  als ook  alle TSR's
waarvan de bestandsnaam begint met `DEMO'.

TL CAPS DEMO*

`TL'  betekent `TSR-Load',  het is  het programma dat de TSR
laadt en  in het  geheugen plaatst.  Ook onder  BASIC is een
versie  van TSR-Load beschikbaar, zie hiervoor het hoofdstuk
`MemMan BASIC statements'.

Zodra TL  de TSR in het geheugen geinstalleerd heeft zal het
programma  actief worden.  In bovenstaand  voorbeeld wil dat
zeggen dat  het CAPS lampje zal gaan knipperen en bij iedere
toetsaanslag het cassetterelais aan of uit geschakeld wordt.

TL  is een  slim programma.  Zolang er in het MemMan segment
nog ruimte  is voor  TSR's zullen  ze daar geplaatst worden.
Alleen  als dat  ook echt  nodig is  wordt een nieuw segment
gebruikt,  dat  voor overige  toepassingen dan  onbereikbaar
gemaakt  wordt.   Dat  gebeurt   bijvoorbeeld  als   er  een
uitzonderlijk  groot TSR programma wordt geladen. Wanneer er
vervolgens weer een kleinere wordt geladen zal TL eerst alle
bestaande TSR  segmenten aflopen om te zien of er ergens nog
ruimte  is. De  volgorde waarin  de TSR's geladen worden zal
dan ook geen invloed hebben op het geheugengebruik.


     T S R S   B E K I J K E N

Het is  ten aller tijde mogelijk te kijken welke TSR's er op
dit  moment in  het geheugen  actief zijn. Daartoe bevat het
MemMan pakket  de utility  TV, TSR-View.  Het gebruik  is de
eenvoud zelf: gewoon achter de DOS prompt intikken:

TV

Er  zal  een  overzicht  verschijnen  van de  op dit  moment
actieve  TSR's, compleet  met hun  volledige naam. Deze naam
moet voor  iedere TSR  uniek zijn,  en zal  dan ook  vrijwel
altijd  de initialen  van de programmeur bevatten. Deze naam
is dus een andere dan de bestandsnaam!

Het is  deze volledige  naam - het TSR ID - die nodig is als
een  TSR  uit  het  geheugen  verwijderd  moet  worden.  Ook
programma's  die direct  met TSR's  samenwerken kunnen  deze
naam  gebruiken  om  te  zien  of  een TSR  in het  geheugen
aanwezig is.



  T S R S   V E R W I J D E R E N

Zoals gezegd  is het  ook mogelijk  TSR programma's weer uit
het  geheugen te  verwijderen. Het  benodigde programma heet
TK, TSR-Kill. TK zorgt er voor dat een TSR netjes verwijderd
wordt. Alle  andere TSR's blijven vlekkeloos doorwerken, als
de TSR als enige in een segment stond wordt dat segment weer
vrijgegeven voor gebruik door overige toepassingen.

Om  bijvoorbeeld het  het Caps  lampje weer normaal te laten
werken en  het knipperen uit te schakelen is het verwijderen
van de betrokken TSR voldoende. Daartoe tikt u:

TK "MJVcapsblink"

Hierbij   dient  de   volledige  naam   van  de  TSR  tussen
aanhalingstekens  opgegeven  te worden.  Er kunnen  meerdere
TSR's in  ��n keer  worden verwijderd,  door de namen achter
elkaar in te voeren. Bijvoorbeeld:

TK "TSR naam 1" "TSR naam 2" "TSR naam 3"

TSR-Kill  kan behalve  geheugen weer vrijmaken voor gebruik,
ook gebruikt worden om vastgelopen TSR's uit het geheugen te
verwijderen. Een  TSR die  om welke  reden dan ook niet meer
vlekkeloos  functioneert zal  met behulp  van TK meestal nog
wel verwijderd  kunnen worden.  Vervolgens kan de TSR met TL
weer  geladen  worden,  op dezelfde  manier als  het opnieuw
starten van gewone programma's nog wel eens wil helpen geldt
dat ook voor TSR's.


M E M M A N   B A S I C - S T A T E M E N T S

Vanaf  versie 2.3 bevat MemMan enkele statements en functies
die  vanuit  MSX-BASIC toegepast  kunnen worden.  Hiertoe is
MemMan van  een systeem  TSR voorzien,  met de  ID-naam "MST
TsrUtils".  Deze TSR  heeft volgnummer  0 en  kan niet  door
TSR-Kill verwijderd worden. Hieronder volgt een beschrijving
van de  beschikbare BASIC-instructies.  De betekenis  van de
gebruikte symbolen is als volgt:

[]   Wat tussen vierkante haken staat mag worden weggelaten.
<>   Omschrijvingen van parameters staan tussen gehoekte
haken.
()", Ronde haken moeten worden ingetikt, evenals leestekens
en comma's.


Commando: TSR-Load
Syntax:   CMD TL("<filename>",[T],[<N$>],[<F>])
Soort:    Statement
Voorbeeld:CMD TL("PB")
CMD TL("ALARM",T,,A)
Functie:  Laadt een TSR-bestand in het geheugen.
<filename> = Naam   van  het   TSR-bestand.  Onder
           MSX-DOS2 mag  een subdirectory worden
           opgegeven.
T          = Toon de intro-tekst van de TSR.
<N$>       = Naam  van een string-variabele waarin
           de TSR ID-naam zal worden opgeslagen.
<F>        = Variabele waarin  een foutcode  wordt
           opgeslagen.   Indien  deze  variabele
           wordt  weggelaten, zal  in geval  van
           laadfouten   een   standaard   BASIC-
           foutmelding   worden    gegeven.   De
           foutcodes zijn als volgt:

        0 = TSR met succes ingeladen
        1 = Installatie door de TSR afgebroken
        2 = Structuurfout in TSR bestand
        3 = TSR-tabel vol
        4 = Hook-tabel vol
        5 = Geen vrij MemMan segment
        6 = Te weinig vrij BASIC werkgeheugen


Commando: TSR-Kill
Syntax:   CMD TK("<TSR ID-naam>")
Soort:    Statement
Voorbeeld:CMD TK("MJV printbuf")
Functie:  Verwijdert een TSR uit het geheugen.
<TSR ID-Naam> = Identificatienaam van de te
              verwijderen  TSR.  Deze  naam  kan
              worden  opgevraagd door middel van
              TSR-View of Find-TSR.


Commando: TSR-View
Syntax:   CMD TV
Soort:    Statement
Functie:  Toont een overzicht van de ID-namen van alle
actieve TSR's.


Commando: Find-TSR name
Syntax:   ATTR$ FT("<TSR ID-naam>")
Soort:    Functie
Voorbeeld:IF ATTR$ FT("CAPS") THEN CMD TK("CAPS")
Functie:  Levert de waarde -1 indien de opgegeven TSR is
ge�nstalleerd, levert anders de waarde 0.
<TSR ID-Naam> = Identificatienaam van de TSR.


Commando: Find-TSR number
Syntax:   ATTR$ FT(<TSR volgnummer>)
Soort:    Functie
Voorbeeld:N$ = ATTR$ FT(0)
Functie:  Levert de  ID-naam van  de TSR  met het  opgegeven
volgnummer. Indien  het volgnummer  groter is  dan
het  aantal  actieve  TSR's,  dan  wordt  een lege
string met lengte 0 teruggeven.