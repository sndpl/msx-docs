M E M M A N   2 . 3   S P E C I F I C A T I E S


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

(Nvdr.  Deze tekst  was langer  dan 16 kB, en is dus in twee
gedeelten gesplitst.  U kunt  het tweede deel in het submenu
vinden.)
