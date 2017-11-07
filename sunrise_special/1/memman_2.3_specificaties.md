# M E M M A N   2 . 3   S P E C I F I C A T I E S


File : MM23SPEC.TXT
Datum: 19 september 1991
Door : Ries Vriend / Ramon van der Winkel - c  MST

Deze  tekst  bevat  de  informatie  die  nodig  is  voor het
schrijven  van   MemMan  2.3   toepassingsprogramma's.  Voor
specifieke  specificaties omtrend het programmeren van TSR's
wordt echter  verwezen naar  de technische documentie die te
vinden  is  op  de `TSR-Development  disk'. Hierop  staan de
volledige  TSR specificaties  en enkele TSR ontwikkel tools.
Deze disk  kan besteld  worden bij  het MST,  zie voor  meer
informatie   hierover  de  LezersService  van  MSX  Computer
Magazine.


Wijzigingen in MemMan 2.3 ten opzichte van versie 2.2:

- De functie XTsrCall (61) is toegevoegd. Deze functie werkt
identiek  aan  de  functie  TsrCall (63),  het Tsr-ID  wordt
echter verwacht  in register  IX in  plaats van BC. Hierdoor
komt  register BC  vrij om  als invoerparameter  gebruikt te
worden.

- Door  middel van de functie Info (50) kan het adres worden
opgevraagd   waarop   XTsrCall   rechtstreeks   kan   worden
aangeroepen.

-  De funtie status (31) is verbeterd. De totale hoeveelheid
bruikbaar  werkgeheugen  in  de  computer  wordt  nu correct
gemeld, ook onder MSX-DOS2.

-  De  Alloc  (10)  functie  herkent  nu  ook  geheugen  dat
beschikbaar komt wanneer de DOS2 RAMdisk wordt verwijderd of
verkleind! Het  maakt daarbij  niet meer  uit of  de RAMdisk
wordt aangemaakt voor- of nadat MemMan werd ge�nstalleerd.

-  De  interne  stack  van  MemMan  die  gebruikt worden  om
functieaanroepen  te verwerken is vergroot tot 240 bytes, in
plaats van 160. In de praktijk bleek dat de functiestack van
MemMan 2.2 te krap was om geneste "tsrCalls" te verwerken.


  G E B R U I K T E   T E R M I N O L O G I E

Segment -  Geheugenblok van  16kB. Segmenten  komen voor  in
Pagina  specifieke segmenten  (PSEG) en  Flexibele segmenten
(FSEG). De  Flexibele segmenten kunnen op de pagina's 0,1 en
2  worden  aangeschakeld.  De  Pagina  specifieke  segmenten
alleen  op hun  eigen pagina.  Er zijn  drie soorten  pagina
specifieke segment:  PSEG0000, PSEG4000 en PSEG8000. Ze zijn
op respectievelijk pagina 0,1 en 2 aanschakelbaar.

Heap  - Blok  geheugen in  pagina 3 (ergens tussen &HC000 en
&HFFFF) waarvan  MemMan toepassingsprogramma's  een stuk aan
kunnen vragen en daarna vrij mogen gebruiken.

FastUse  -  Zelfde  als Use,  maar dan  het adres  waarop de
routine direct aan te roepen is in pagina 3.

UnCrash  - Om te voorkomen dat segmenten aangevraagd zijn en
door  een  crash van  een programma  nooit meer  vrij zouden
worden gegeven,  voert de  IniChk routine  een unCrash  uit.
Hierbij   worden  alle   segmenten  weer   vrijgegeven.  Het
unCrashen van  een segment  is te voorkomen door een segment
de  Reserved status  te geven. Dit kan met de functie SetRes
(11). Normaal  gesproken hoeft  een segment niet de Reserved
status gegeven te worden.


            D E   P R I N C I P E S

MemMan  verdeelt het  aanwezige geheugen in segmenten van 16
kB. Voordat  een segment gebruikt mag worden moet het worden
aangevraagd.   Na   gebruik   dient  het   weer  te   worden
vrijgegeven.  Er zijn  twee soorten segmenten: de zogenaamde
pagina-specifieke ofwel PSEG's en de flexibele FSEG's.

PSEG's  zijn  segmenten  die  aangevraagd  worden  voor  het
gebruik   op   een   bepaalde   pagina,   bijvoorbeeld   van
&h4000-&h7FFF  of  van  &h8000-&hBFFF.  Wanneer er  een PSEG
aangevraagd wordt  zal MemMan  zo mogelijk geheugensegmenten
toewijzen die niet in een memory-mapper zitten.

FSEG's zijn segmenten die op elke willekeurige pagina kunnen
worden  ingeschakeld. Deze segmenten komen altijd uit memory
mappers. Welk soort segment er ook aangevraagd wordt, MemMan
zal een  16-bits 'segmentcode'  teruggeven. Deze segmentcode
is  weer nodig bij het inschakelen of het weer vrijgeven van
het segment.  Wie alleen  maar geheugen  nodig heeft  in het
gebied  van  &h8000  tot  &hBFFF  kan dus  het beste  PSEG's
aanvragen.   MemMan  gebruikt   dan  eerst  zoveel  mogelijk
geheugen uit  de 'oude'  16- en 64 Kb modules en gaat dan de
mapper gebruiken.

Met  behulp van MemMan hoeft er dus nooit meer naar geheugen
gezocht te worden. Simpelweg een pagina aanvragen, gebruiken
en uiteindelijk weer vrijgeven. Zo eenvoudig is dat.

Overigens is  er een  pagina die  zich met  MemMan niet laat
schakelen. Pagina 3 bevat behalve de MemMan code zelf ook de
stack  (meestal) en een grote hoeveelheid systeemvariabelen.
Er zitten  nogal wat  haken en ogen aan het wegschakelen van
dat alles.


     F U N C T I E O M S C H R I J V I N G

MemMan  functies kunnen  worden uitgevoerd  door een aanroep
van de  `Extended BIOS' of EXTBIO hook, op adres &HFFCA. Het
device ID van MemMan - 'M' oftewel &H4D - moet in register D
worden  geplaatst. Register E dient het MemMan functienummer
te bevatten.  Na aanroep  van een MemMan functie kunnen alle
registers gewijzigd zijn, behalve indien het tegendeel wordt
vermeld bij de functie-omschrijving.

Omdat  de EXTBIO  hook gebruikt  wordt voor  diverse systeem
uitbreidingen  zoals  Kanji  en  RS232  interfaces,  is  het
mogelijk  dat  MemMan  functie-aanroepen  bijzonder langzaam
verwerkt    worden.    De   prestaties    van   de    MemMan
toepassingsprogramma's kunnen  aanmerkelijk worden  verhoogd
door functie afhandelingsroutine van MemMan rechtstreeks aan
te roepen, in plaats van de EXTBIO hook. Het adres waarop de
functie  afhandelingsroutine  aangeroepen  kan  worden,  kan
worden opgevraagd via de info functie (50).

De  meeste  MemMan  functies  bevinden  zich  in  een  apart
geheugensegment in pagina 1. Deze functies schakelen over op
een      een     interne      stack,     waardoor     MemMan
toepassingsprogramma's  met  een  betrekkelijk  kleine stack
kunnen  volstaan.  Door een  MemMan functie  worden maximaal
twintig  bytes  op  de  stack  van  het toepassingsprogramma
geplaatst.  Dit   geldt  echter  alleen  indien  de  functie
rechtstreeks,  of via  de MemMan functie-afhandelingsroutine
wordt aangeroepen.

Een  functie-aanroep  via  de  EXTBIO  hook  kan echter  een
bijzonder  grote   stack  vereisen.  Dit  wordt  veroorzaakt
doordat  alle uitbreidings-modules  die aan  de EXTBIO  hook
gekoppeld  zijn  elkaar aanroepen,  net zo  lang totdat  ��n
module de  functieaanroep herkent. Wanneer er tussendoor ook
nog  interrupts  afgehandeld  worden,  kan  het stackgebruik
sterk  oplopen. Al  met al kan gesteld worden dat er bij een
aanroep van  de EXTBIO  hook minimaal  150 bytes stackruimte
beschikbaar moet zijn.

Het  is derhalve  verstandig om via de info functie (50) het
adres op  te vragen  van de  routine die  de MemMan  functie
aanroepen   afhandelt.  Wanneer   deze  routine   vervolgens
rechtstreeks aangeroepen wordt, wordt de verwerkingssnelheid
verhoogd en blijft het stack-gebruik beperkt.

De interruptstand  blijft na  een MemMan  functie-aanroep in
meeste  gevallen  ongewijzigd.  Sommige  functies  zoals  de
diverse  (Fast)Use functies  schakelen de  interrupts echter
uit.  Wanneer  een MemMan  functie werd  aangeroepen met  de
interrupts  uit,   zal  MemMan   nooit  terugkeren   met  de
interrupts ingeschakeld.

Deze   eigenschap  is   bijvoorbeeld  van  belang  voor  TSR
programma's   die   slechts  een   zeer  kleine   stack  ter
beschikking hebben.  Zo lang de interrupts uit staan, kunnen
alle  MemMan  functies  zonder problemen  worden uitgevoerd,
mits  de functie  verwerkingsroutine van MemMan rechtstreeks
wordt aangeroepen. Wanneer de interrupts echter aan staan is
een      grote       stack      vereist,       omdat      de
interrupt-verwerkingsroutine  enkele tientallen  bytes op de
stack plaatst.


Naam   : Use0
Nummer : 0
Functie: Aanschakelen van een segment op pagina 0
   (adresgebied 0000..3FFF)
In     : HL = Segmentcode
Uit    : A  = Resultaatcode (-1 = Mislukt, 0 = Gelukt)

Het  inschakelen  van  een  segment  in  pagina 0  is alleen
mogelijk indien  het segment  de MSX-standaard  slot-schakel
entry points bevat.

Opm:  Deze functie mag niet worden aangeroepen via de EXTBIO
hook. Deze  functie mag  alleen worden  uitgevoerd door  een
rechtstreekse     aanroep    van     de    MemMan    functie
afhandelingsroutine  of  de  FastUse0  functie. De  adressen
waarop deze  routines aangeroepen  kunnen worden, kunnen via
de info functie (50) worden verkregen.


Naam   : Use1
Nummer : 1
Functie: Aanschakelen van een segment op pagina 1
   (adresgebied 4000..7FFF)
In     : HL = Segmentcode
Uit    : A  = Resultaatcode (-1 = Mislukt, 0 = Gelukt)

Opm:  Deze functie mag niet worden aangeroepen via de EXTBIO
hook. Deze  functie mag  alleen worden  uitgevoerd door  een
rechtstreekse     aanroep    van     de    MemMan    functie
afhandelingsroutine  of  de  FastUse1  functie. De  adressen
waarop deze  routines aangeroepen  kunnen worden, kunnen via
de info functie (50) worden verkregen.


Naam   : Use2
Nummer : 2
Functie: Aanschakelen van een segment op pagina 2
   (adresgebied 8000..BFFF)
In     : HL = Segmentcode
Uit    : A  = Resultaatcode (-1 = Mislukt, 0 = Gelukt)

Opm:  Deze functie mag niet worden aangeroepen via de EXTBIO
hook. Deze  functie mag  alleen worden  uitgevoerd door  een
rechtstreekse     aanroep    van     de    MemMan    functie
afhandelingsroutine  of  de  FastUse2  functie. De  adressen
waarop deze  routines aangeroepen  kunnen worden, kunnen via
de info functie (50) worden verkregen.


Naam   : Alloc
Nummer : 10
Functie: Aanvragen van een segment
In     : B  = Segment voorkeuze code
Uit    : HL = Segmentcode. (0000 = Geen segment meer vrij)
   B  = Segmentsoort code (-1 = FSeg, 0 = PSeg)

Segment voorkeuze code overzicht (Register B):

Bit 7 6 5 4 3 2 1 0
^ ^ ^ ^ ^ ^ ^ ^
| | | | | | | |
| | | | | | +-+--> Segment Type. 00 = PSEG0000
| | 0 0 0 0                      01 = PSEG4000
| |                              10 = PSEG8000
| |                              11 = FSEG
| +--------------> 1 = Prefereer TPA oftewel het
|                      standaard MSXDOS RAM slot
+----------------> 1 = Prefereer onge�xpandeerd (dus
                       snel) slot

De bits 5 tot en met 2 zijn niet gebruikt en moeten 0 zijn.

Mocht  een PSEG type aangevraagd, maar niet beschikbaar zijn
wordt -  indien mogelijk  - een FSEG ter beschikking gesteld
die dan het PSEG kan vervangen.


Naam   : SetRes
Nummer : 11
Functie: Segment de Reserved status geven
In     : HL = Segmentcode

Geeft  een segment  de `Reserved-status';  zodat het segment
niet automatisch wordt vrij gegeven na aanroep van de IniChk
routine. Normaal  gesproken hoeven  programma's de  reserved
status   niet  te   zetten,  behalve  als  een  programma  -
bijvoorbeeld een  Ramdisk -  een segment  voor eigen gebruik
zeker wil stellen.


Naam   : DeAlloc
Nummer : 20
Functie: Teruggeven van een segment
In     : HL = Segmentcode

Bij  het  verlaten  van  een  programma  dient  deze functie
gebruikt te worden om alle aangevraagde segmenten weer terug
te  geven aan  MemMan. De  eventuele reserved status van het
terug  te  geven  segment  wordt  door  DeAlloc  automatisch
opgeheven.

Segmenten die  ook door  DOS2 beheerd worden, worden door de
DeAlloc functie weer ter beschikking gesteld van DOS2.


Naam   : ClrRes
Nummer : 21
Functie: Reserved status van het segment opheffen
In     : HL = Segmentcode

Het  is niet  nodig deze  functie vlak  voor DeAlloc  aan te
roepen. DeAlloc heft zelf de Reserved status van het segment
op.


Naam   : IniChk
Nummer : 30
Functie: Initialisatie MemMan voor een programma
In     : A  = Controle code
Uit    : A  = Controle code + "M"
   DE = Versie nummer (format: Versie #D.E)

Deze routine  telt de  ascii-waarde van de letter "M" op bij
de  inhoud  van  register  A.  Hierdoor  kan  er een  MemMan
aanwezigheids  controle uitgevoerd  worden. Verder  wordt er
een  unCrash  uitgevoerd en  worden de  segmentcodes van  de
actief  aangeschakelde  sloten berekend  en opgeslagen  voor
CurSeg.

De IniChk  functie mag  slechts ��n  keer door  ieder MemMan
toepassings  programma aangeroepen worden. Dit aanroepen van
IniChk dient  te gebeuren  voordat de  overige functies  van
MemMan  aangroepen worden.  TSR programma's  mogen de IniChk
functie nooit aanroepen.


Naam   : Status
Nummer : 31
Functie: Status gegevens van MemMan ophalen
Uit    : HL = Aantal aanwezige segmenten
   BC = Aantal nog vrije segmenten
   DE = Aantal segmenten in dubbel beheer bij DOS2 en
        MemMan
   A  = Connected Status van de aangesloten hardware.
        Bit   Functie
         0    1=Dos2 Mapper Support Routines aanwezig
        1-7   Gereserveerd, altijd 0

Als  bit  0  van  de  Connected  status  gezet  is, zijn  de
geheugenbeheer functies van DOS 2.20 aanwezig.

Het  aantal  nog  vrije  segmenten  kan  lager  zijn  dan is
aangegeven in  register BC,  omdat sommige  segmenten na  de
installatie   van  MemMan  door  DOS2  gebruikt  zijn  -  om
bijvoorbeeld een ramdisk te installeren.

Naam   : CurSeg
Nummer : 32
Functie: Segmentcode van een aangeschakeld segment opvragen.
In     : B  = Paginanummer (0,1,2,3)
Uit    : HL = Segmentcode
A  = Segmentsoort code (255 = FSeg, 0 = Pseg)

Deze routine  geeft de huidige segmentcode terug van een van
de vier pagina's.

TSR  programma's mogen  deze functie  niet gebruiken  om het
actieve segment  in geheugen pagina 0 te bepalen. Om tijd te
sparen   wordt  deze   stand  niet  automatisch  bepaald  en
opgeslagen bij  de aanroep van een TSR. De actieve segmenten
in  de pagina's 1 en 2 worden echter bij iedere hook-aanroep
opnieuw bepaald  en kunnen  ten alle  tijde via deze functie
opgevraagd  worden.  Omdat  in  pagina  3  altijd  hetzelfde
segment actief is, is ook de segmentcode van pagina 3 altijd
opvraagbaar.

Een snellere variant van deze functie is FastCurSeg routine.
gebruikt.  Het  adres  waarop deze  routine aangeroepen  kan
worden is via de Info functie (50) op te vragen.


Naam   : StoSeg
Nummer : 40
Functie: Huidige segmenten stand opslaan
In     : HL = Buffer adres (9 bytes groot)

De   voor  MemMan   bekende  segmentcodes   van  de   actief
aangeschakelde sloten  worden opgeslagen in het buffer. Deze
segmentcodes  zijn in beginsel door IniChk berekend en later
door de  Use functies  geupdate. De opgeslagen stand is niet
de  huidige stand,  maar de  voor MemMan  bekende stand. TSR
kunnen hiermee dus niet de actieve stand opslaan.

Opm: Deze  functie mag niet worden aangeroepen via de EXTBIO
hook.  Deze functie  mag alleen  worden uitgevoerd  door een
rechtstreekse    aanroep     van    de     MemMan    functie
afhandelingsroutine.   Het   adres   waarop   deze   routine
aangeroepen  kan worden, kan via de info functie (50) worden
verkregen.

Natuurlijk  kunnen  ook  de  (Fast)CurSeg functies  gebruikt
worden om de momentele segment-stand op te vragen.


Naam   : RstSeg
Nummer : 41
Functie: Opgeslagen segment-stand actief maken
In     : HL = Buffer adres

De in  het buffer opgeslagen segment-stand wordt weer actief
gemaakt en wordt opgeslagen voor CurSeg.

Opm:  Deze functie mag niet worden aangeroepen via de EXTBIO
hook. Deze  functie mag  alleen worden  uitgevoerd door  een
rechtstreekse     aanroep    van     de    MemMan    functie
afhandelingsroutine.   Het   adres   waarop   deze   routine
aangeroepen kan  worden, kan via de info functie (50) worden
verkregen.

Natuurlijk  kunnen ook de (Fast)Use functies gebruikt worden
om een segment-stand te herstellen.


Naam   : Info
Nummer : 50
Functie: Geeft informatie over onder andere aanroep-adressen
van MemMan functies
In     : B  = Informatie nummer (0..8)
Uit    : HL = Informatie

Informatie  nummer   overzicht.  Tussen   haakjes  staan  de
equivalente MemMan functie codes:

0 - Aanroepadres van FastUse0 (functie 0)
1 - Aanroepadres van FastUse1 (functie 1)
2 - Aanroepadres van FastUse2 (functie 2)
3 - Aanroepadres van TsrCall  (functie 63)
4 - Aanroepadres van BasicCall
5 - Aanroepadres van FastCurSeg (functie 32)
6 - Aanroepadres van MemMan, de functie-afhandelingsroutine
7 - Versienummer van MemMan, format: Versie #H.L
8 - Aanroepadres van XTsrCall (functie 61)

De    bovengenoemde   functie-adressen    mogen   door   een
toepassingsprogramma of TSR rechtstreeks aangeroepen worden.
Alle entry adressen liggen gegarandeerd in pagina 3.

De functies worden snel uitgevoerd omdat de MemMan CALL naar
de EXTBIO hook vervalt en de functie-codes in registers D en
E niet  uitgeplozen hoeven worden. Een ander voordeel is dat
parameters  ook  via  het  register  DE  doorgegeven  kunnen
worden, dit is vooral van belang bij de TsrCall en BasicCall
functies.

Bijvoorbeeld,  de initialisatie  routine van  een TSR kan de
benodigde functieadressen  via de  INFO functie  opvragen en
deze  vervolgens voor  later gebruik in de TSR programmacode
opslaan,  wat  de snelheid  van het  TSR programma  zeer ten
goede kan komen.

Een  exacte  beschrijving van  de bovenstaande  functies kan
gevonden worden  bij de  MemMan functie  waarvan het  nummer
tussen haakjes is aangegeven.

Houd echter onder de aandacht dat de `snelle' functies op de
volgende punten van de gewone MemMan functies verschillen:

fastUse0-2: Schakelt een segment in in een bepaalde geheugen
pagina. Zie de omschrijving bij de memMan `Use' functies.

tsrCall:  Register   [DE]  wordt   ongewijzigd  aan  de  TSR
doorgegeven.  Dit in tegenstelling tot functie 63 (TsrCall),
register DE  is dan  al bezet om het MemMan functienummer in
op te slaan.

xTsrCall:    Alle    mainregisters    (AF,HL,BC,DE)   worden
ongewijzigd  aan de TSR doorgegeven. De TSR-ID code dient in
register IX te worden geplaatst.

basicCall : Heeft geen MemMan functie nummer.
  Functie: Aanroepen van een routine in de BASIC
           ROM.
  In:      IX = Call address in pagina 0 of 1
           AF,HL,BC,DE = dataregisters voor de
           BASIC-ROM
  Uit:     AF,HL,BC,E = dataregisters van de
           BASIC-ROM
           Interrupts disabled

Via deze functie kunnen TSR's een routine aanroepen die zich
in pagina  0 en/of  pagina 1  van het  BASIC-ROM bevindt. De
bios  moet al  in pagina  0 aangeschakeld  zijn. In pagina 1
wordt de BASIC ROM door MemMan aangeschakeld.

Dit is  bijvoorbeeld noodzakelijk  om de  math-pack routines
aan  te  kunnen  roepen die  in pagina  0 van  de BASIC  ROM
zitten,  maar tussendoor ook een aantal routines in pagina 1
aanroepen.

De H.STKE  (stack error)  hook wordt afgebogen, zodat na een
eventueel  op  getreden  BASIC error  de interne  stacks van
MemMan gereset kunnen worden.

fastCurSeg:  In register [A] komt geen zinnige waarde terug.
De MemMan CurSeg functie (32) geeft aan of het een FSEG/PSEG
betreft.

memMan: Heeft geen MemMan functienummer
  Functie: Rechtstreeks aanroepen van een MemMan
           functie.
  In:      E=MemMan functienummer
           AF,HL,BC = Dataregisters afhankelijk
           van de aan te roepen functie.
  Uit:     AF,HL,BC,DE = dataregisters afhankelijk
           van de aangeroepen functie.

Een aanroep  van deze routine heeft hetzelfde effect als het
aanroepen van een MemMan functie via de EXTBIO hook. Doordat
echter  de aanroep  naar de  EXTBIO hook  vervalt, worden de
overige uitbreidingen  die aan deze hook gekoppeld zijn niet
aangeroepen.  Hierdoor blijft  het stack  gebruik beperkt en
wordt de verwerkingssnelheid verhoogd.


Naam   : XTsrCall
Nummer : 61
Functie: Roep het driver-entry van een TSR aan
In     : IX = ID code van de aan te roepen TSR
AF,HL,DE,BC worden ongewijzigd doorgeven aan de TSR.
Uit    : AF,HL,BC,DE komen ongewijzigd terug van de TSR.

Deze functie is een verbeterde versie van de functie TsrCall
(63). Omdat  met deze functie alle main-registers aan de TSR
kunnen  worden doorgegeven, verdient het aanbeveling om deze
functie te gebruiken in plaats van functie 63.

Opm: Deze  functie mag niet worden aangeroepen via de EXTBIO
hook,  omdat  bij  een  aanroep  via EXTBIO  het IX-register
verminkt  wordt. Roep  deze functie daarom rechtstreeks aan,
of   gebruik   de  MemMan   functie-afhandelingsroutine.  De
adressen  waarop  deze routines  aangeroepen kunnen  worden,
kunnen via de info functie (50) worden opgevraagd.


Naam   : GetTsrID
Nummer : 62
Functie: Bepaal TSR ID code
In     : HL = Pointer naar de TsrNaam (12 tekens).
    Ongebruikte posities opvullen met spaties.
Uit    : Gevonden: Carry clear (NC)
         BC = TSR ID code
Anders  : Carry set (C)


Naam   : TsrCall
Nummer : 63
Functie: Roep het driver-entry van een TSR aan
In     : BC = ID code van de aan te roepen TSR
AF,HL,DE worden ongewijzigd doorgeven aan de TSR.
Uit    : AF,HL,BC,DE komen ongewijzigd terug van de TSR.

Merk op  dat alhoewel het DE register ongewijzigd aan de TSR
wordt  doorgegeven, het niet voor parameter-invoer benut kan
worden. De Extended BIOS functiecode van MemMan (D='M' E=63)
moet namelijk in dat register geplaatst worden.

Bij de Fast-TsrCall routine treedt deze complicatie niet op;
het  adres  van  deze  routine kan  middels de  info functie
opgevraagd worden.


Naam   : HeapAlloc
Nummer : 70
Functie: Alloceer ruimte in de heap
In     : HL = Gewenste grootte van de ruimte (in bytes)
Uit    : Genoeg ruimte: HL = Startadres van de ruimte
Anders       : HL = 0000

Door  middel   van  deze   functie  kan  een  stuk  geheugen
gealloceerd  worden. Het  geheugenblok zal zich gegarandeerd
in pagina 3 bevinden.

De  heap  is  vooral  nuttig voor  TSR programma's,  die hem
bijvoorbeeld als  tijdelijke of permanente diskbuffer kunnen
gebruiken.   Ook  andere  buffers  -  waarvan  het  absoluut
noodzakelijk is dat ze zich in pagina 3 bevinden - kunnen op
de heap worden geplaatst.

Aangevraagde   blokken   geheugen   uit   de  heap   blijven
onbruikbaar voor andere programma's totdat een `HeapDeAlloc'
is uitgevoerd (functie 71).

De grootte  van de heap kan worden ingesteld door middel van
het configuratie programma CFGMMAN.


Naam   : HeapDeAlloc
Nummer : 71
Functie: Geef geAlloceerde ruimte van de heap weer vrij
In     : HL = Startadres van de ruimte


Naam   : HeapMax
Nummer : 72
Functie: Geef de lengte van het grootste vrije blok geheugen
in de heap terug
Uit    : HL = Lengte van het grootste vrije blok


D E   S T A C K   O N D E R   M E M M A N

MemMan  toepassingsprogramma's dienen  de stack pointer (SP)
bij voorkeur  in pagina  2 of 3 (tussen &h8000 en &HFFFF) te
plaatsen.  Indien MemMan  door een  hook-aanroep geactiveerd
wordt, wordt  het huidige  segment in  pagina 1  (&h4000 tot
&h8000)  namelijk weggeschakeld  om plaats  te maken voor de
TSR-Manager en  de eventuele  TSR's. Indien de stack zich op
dat moment in pagina 1 bevindt zal de computer vastlopen.

Indien TSR's na een BDOS call of interrupt via een BIOS-hook
worden  aangeroepen treden  geen stackproblemen op; ook niet
indien de  stack van  het toepassingsprogramma  in pagina  1
staat.  De BDOS  en interruptfuncties gebruiken namelijk hun
eigen stack in pagina 3. De stack bevindt zich dan alsnog in
pagina 3 op het moment dat de hook aangeroepen wordt.

Bestaande  CP/M  en  MSX-DOS  programmatuur  is  dus  zonder
problemen in  combinatie met  MemMan 2  te gebruiken  - maar
alleen  indien  de  standaard  BDOS  calls gebruikt  worden.
Wanneer  echter  via  een  interslot  call een  BIOS routine
rechtstreeks  aangeroepen wordt,  dient de stack in pagina 2
of 3  te staan.  Reserveer in  dat geval  minimaal 150 bytes
voor de stack.


Appendix 1: BIOS aanroepen onder Turbo Pascal

Indien in een Turbo Pascal programma interslot-calls naar de
BIOS  gebruikt worden,  is het  belangrijk dat  de stack  in
pagina 2  of 3 staat. Op het moment dat de BIOS dan een hook
aanroept  kan MemMan  veilig de  TSR's aktiveren. De positie
van de  stack is afhankelijk van het maximum programma adres
dat  tijdens de  compilatie in Turbo Pascal is ingesteld. De
stack  bevindt   zich  in  Turbo  Pascal  direkt  onder  het
variabelen  geheugen.  Het  variabelen  geheugen  dient  bij
programma's  die  de  BIOS  aanroepen  dus ruim  boven adres
&h8000 geplaatst te worden.

Is  geen source  voorhanden, dan  is het mogelijk om met een
debugger het stack adres van Turbo Pascal programma's aan te
passen. De  initialisatie code  van een TP programma ziet er
als volgt uit:

start: jp init
...
...
init:  ld sp,100h
ld hl,nn
ld de,nn
ld bc,nn
call yy
ld hl,nn
ld de,stack    ;DE bevat het stack adres, hoeft
ld bc,nn       ; alleen aangepast te worden als het
call zz        ; lager is dan &h80A0
...

Het  stackadres in  register DE  kan bijvoorbeeld  op &hC100
gezet worden.
