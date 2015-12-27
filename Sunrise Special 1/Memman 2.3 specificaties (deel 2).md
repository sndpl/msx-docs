M E M M A N   S P E C I F I C A T I E S


(Nvdr.  Dit is het tweede gedeelte, het eerste gedeelte kunt
u in het submenu vinden.)


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
