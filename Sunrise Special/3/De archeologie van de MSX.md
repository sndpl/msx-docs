# D E   A R C H E O L O G I E   V A N   D E   M S X


Ofwel:  waarom  een  vooruitziende  blik achteraf  beter was
geweest

## I N L E I D I N G 

De tegenwoordige  MSX2, MSX2+  of turbo  R computers  lijken
maar  in weinig  opzichten op hun MSX1-voorgangers van bijna
tien jaar  geleden. De vroege MSX1 computers waren ontworpen
om  vooral  zo  goedkoop mogelijk  te zijn,  en maakten  dus
uitsluitend  gebruik van  goedkope standaardcomponenten, die
goed verkrijgbaar  waren. De belangrijkste (LSI) componenten
waren:

Type        Taken                        Fabrikant(en)
------------------------------------------------------------
Z80A        processor                    Zilog, SGS, etc.
AY-3-8910   geluid, joystickpoorten      General Instruments
8255 PPI    toetsenbord, slotselectie    Intel
          cassette, diverse led's
9918A VDP   display                      Texas Instruments

Voor  de  rest  kon  gebruik  gemaakt  worden van  standaard
(LS)TTL-, RAM-  en EPROM  IC's, aangevuld met wat ander spul
(zoals  weerstanden, condensatoren,  etc.) zodat een ervaren
hobbyist in  principe zijn  eigen MSX1 had kunnen bouwen met
componenten die bijna iedere electronicazaak kon leveren.

Hoewel  de gebruikte  LSI-componenten niet  erg duur  waren,
bleken ze  toch duur  genoeg om er zo weinig mogelijk van te
gebruiken,  hoewel bijvoorbeeld  een extra  8255 de hardware
eenvoudiger had  gehouden en de prestaties had verbeterd. Zo
is  er  bijvoorbeeld  tussen de  joystickpoorten en  de 8910
extra  hardware nodig om tussen de twee joysticks (of wat er
ook  aan  die  poorten  zit) te  schakelen, domweg  omdat er
onvoldoende I/O lijnen beschikbaar zijn.

Het lijkt  er wel  op of die tweede joystickpoort pas in een
later  stadium aan  de standaard is toegevoegd, en hetzelfde
kun  je  zeggen  van  de  printerpoort,  die op  veel vroege
MSX-computers  ontbrak.   (Philips  heeft   ooit  eens   een
cartridge  uitgebracht  met  alleen  een  printeraansluiting
erop!)


##H E T   G E H E U G E N - P R O B L E E M

De  Z80  kan  helaas  niet  meer  dan  64  kB  aan  geheugen
adresseren,  en dat  was zelfs toendertijd te weinig, vooral
omdat  er  naast  een  verplichte BASIC-interpreter  ook nog
software in  cartridges moest  kunnen lopen, en aan het toen
nog toekomstige MSX-DOS zal men ook wel gedacht hebben.

Dit  probleem is  - zoals  iedereen wel zal weten - opgelost
door de  PPI-poort op  0A8h te gebruiken om voor de selectie
van  de  geheugenslots  te  dienen.  Omdat we  vier pagina's
moeten  schakelen, en  dat in  een enkel  byte moeten kunnen
doen, beperkt  dat ons  tot 2  bits per  pagina, en  dus een
maximum van vier slots. Dit lijkt het adresseerbare geheugen
uit te breiden to 4 x 64 = 256 kB, maar schijn bedriegt: ten
eerste  mag pagina  3 nooit worden weggeschakeld, omdat daar
de systeemvariabelen  in zitten  en (meestal en hopelijk) de
stack,  en ten tweede omdat het wegschakelen van pagina 0 de
BIOS onbereikbaar  maakt, tenzij  je ervoor  zorgt dat er in
die  pagina ook slotselectieroutines aanwezig zijn. (MSX-DOS
doet dit, maar daarover later meer.)


##H E T   S E C U N D A I R E   S L O T R E G I S T E R

Achteraf gezien  zal het dan ook niemand verbazen dat dit te
weinig bleek, maar wat wel verbazend is, is de manier waarop
men  het heeft opgelost: het secundaire slotselectieregister
op 0FFFFh!

Op  zich is  het natuurlijk  al vreemd  om een memory-mapped
register te  gebruiken als  er nog voldoende ongebruikte I/O
adressen    zijn,   maar    dat   kun   je   hoogstens   een
schoonheidsfoutje noemen.  Nee, waar het echte probleem ligt
is   het  feit   dat  ieder  geâxpandeerd  slot  een  eigen,
afzonderlijk slotselectie-register op 0FFFFh heeft! Dit feit
en wat  de consequenties  hiervan zijn  realiseren maar heel
weinig  mensen zich  ten volle,  dus laat  ik ze hier op een
rijtje zetten:

In  pagina  3  zit  een  deel  van  de  slotselectieroutines
(behalve voor  ENASLT), en  om een  goede reden:  als we van
(primair)  slot veranderen is dat de enige pagina waarvan we
zeker weten  dat hij  op z'n plaats blijft na het schakelen.
Het  ongelukkige van  het secundaire  slotregister op 0FFFFh
zit hem hierin, dat we juist pagina 3 moeten wegschakelen om
het secundaire slotregister te kunnen bereiken, waardoor een
secundaire  slots  selecteren  nogal  omslachtig  wordt  (de
stappen met een * gebeuren in pagina 3):

1  - Schakel het gewenste primaire slot in, maar op pagina 3
2  - Lees FFFF, complementeer, en onthoud de waarde
3  - Bereken de nieuwe waarde, en schrijf naar FFFF
4  - Herstel primair slot (situatie als voor 1)
5  - Bereken nieuw primair slot op de gewenste pagina
6* - Schrijf primair slotregister
7* - De gewenste actie (lezen, schrijven of CALLen)
8* - Herstel primair slotregister (situatie als voor 1)
9  - Schakel primair slot als bij 1
10 - Schrijf oude waarde (uit 2) terug naar FFFF
11 - Herstel primair slotregister (situatie als voor 1)


Voor ongeâxpandeerde slots gaat 't simpeler:

1  - Bereken nieuw primair slot op de gewenste pagina
2* - Schrijf primair slotregister
3* - De gewenste actie (lezen, schrijven of CALLen)
4* - Herstel primair slotregister (situatie als voor 1)


##D E   C O N S E Q U E N T I E S

Die  ingewikkelde  secundaire  slotselectie  is niet  zonder
gevolgen. Zo  moet iedere  slotselectieroutine heen  en weer
hinken  tussen pagina  3 en  (meestal) 0,  terwijl het  heel
handig (en  een stuk  sneller) zou  zijn als  we de complete
routine  in  pagina  3  kwijt  zouden kunnen.  MSX-DOS heeft
hetzelfde probleem, en heeft het leeuwendeel van de routines
in pagina 3 zitten, maar een deel zit verplicht in pagina 0.

Een tweede  gevolg is  dat binnen een geâxpandeerd slot geen
CALSLT ed. gedaan kan worden naar pagina 0, of het moest via
aan  omweg zijn!  Dit kan  uiterst onaangename consequenties
hebben  voor  computers  met  RAM  en  SUBROM  in  hetzelfde
geâxpandeerde  slot,  of (erger  nog!) met  een geâxpandeerd
slot 0.  In het  eerste geval moet een omweg via de BIOS (de
routine  EXTROM) gemaakt  worden, wat  lastig wordt  gemaakt
door het feit dat CALSLT ook het IX-register gebruikt.

Dus waarom  hebben ze  het zichzelf  zo ingewikkeld gemaakt?
Een  deel  van  het  antwoord  ligt  in  het  feit  dat  men
secundaire  sloten herkent  doordat bij teruglezen de waarde
wordt gecomplementeerd. Als men van dit gemak had afgezien -
de RAM  en ROM check bij het opstarten van de computer wordt
dan   iets  ingewikkelder   -  dan   hadden  de   secundaire
slotselectie registers  write-only kunnen worden uitgevoerd.
In  dat geval  hoefde de hardware rond die registers er niet
op  te   letten  of  het  desbetreffende  primaire  slot  is
ingeschakeld,   en  bovendien   kon  de  hardware  voor  het
teruglezen vervallen. (Het leuke is namelijk dat niet alleen
alle     in     het     systeem     aanwezige     secundaire
slotselectieregisters beschreven worden, maar ook het gewone
RAM  op  0FFFFh,  zodat  teruglezen nog  steeds zonder  meer
mogelijk is!)

Dus  ten  koste  van  iets  ingewikkelder  software  kan  de
hardware eenvoudiger  en bovendien  vervallen dan bijna alle
bovenstaande bezwaren! (Naar het secundaire slotregister kan
dan  nl.  altijd  geschreven  worden,  ook vanuit  pagina 3.
Secundaire  slotselectie  wordt  dus veel  eenvoudiger.) Een
onbegrijpelijke  misser! Je  moet hier  wel de  conlusie uit
trekken  dat   het  secundaire   slot  een  late  en  weinig
doordachte toevoeging aan het MSX-systeem is.



##H E T   I N T E R R U P T S Y S T E E M

Ook  het  interruptsysteem  draagt  de sporen  van spaarzaam
denken. Interrupts  veroorzaken altijd een sprong naar adres
38h,  wat tot  gevolg heeft  dat daar natuurlijk altijd iets
aanwezig moet  zijn om  de interrupts af te handelen. (En er
moeten   ook  slotselectieroutines   aanwezig  zijn!)   Deze
interrupt wordt altijd in de BIOS afgehandeld voor hij de zo
bekende hooks op FD9A en FD9F passeert. Dit heeft tot gevolg
dat een  echt snelle  interruptafhandeling niet mogelijk is,
zeker  in DOS,  waar er  nog een extra interslot-call aan te
pas moet komen.

Nvdr.   BIOS  wegschakelen   en  zelf  iets  op  38h  zetten
misschien?

Dat vaste  interruptadres veroorzaakt  bovendien nogal  eens
problemen  met CP/M debuggers, omdat adres 38h vaak door die
programma's wordt  gebruikt. Beter  ware het  geweest om met
behulp  van een PIC (priority interrupt controller) zoals de
8259 van  Intel interrupt-vectoring  te verzorgen,  iets wat
nauwelijks  extra kosten  met zich had meegebracht, maar het
systeem wel een stuk sneller en flexibeler had gemaakt.


##D E V I C E S

Aan de  andere kant  is het  vreemd dat  juist een doordacht
systeem  als  het  device-systeem op  de MSX  nooit gebruikt
wordt;  iets dat  waarschijnlijk alleen  toe te schrijven is
aan  gebrek   aan  informatie  (hoewel  gebrek  aan  devices
natuurlijk  ook kan).  Om vooral geen slapende honden wakker
te  maken  zal  ik  volstaan met  te zeggen  dat dit  (onder
andere) gaat via de hook B.EXT (FFCA), een hook die ook door
MemMan wordt  gebruikt, hoewel  deze beslist geen device is!
Buiten  de devices  die standaard in BASIC zitten (CRT: GRP:
CAS:  LPT:)  kunnen RS232-uitbreidingen  (COM:) hiermee  ook
worden gebruikt.


##C O N C L U S I E

Bij het  uitdenken van  het MSX-systeem  is goed  nagedacht,
maar  er  zijn  ook  blunders  gemaakt.  Iedere  computer is
natuurlijk   een  compromis  tussen  wensen  en  kosten,  en
persoonlijk verbaast  het mij  dat het  systeem telkens  kon
worden  uitgebreid zonder  dat de  compatibiliteit tussen de
systemen in belangrijke mate verloren ging.

Wat betreft  het secundaire  slot systeem kun je je afvragen
of  alle programmeurs  wel precies weten hoe het werkt. Alle
Philips MSX2  computers hebben  het RAM in het geâxpandeerde
slot  3 zitten,  wat inhoudt dat het secundaire slotregister
altijd bereikbaar  is. Programma's die rucksichtslos op FFFF
lezen  of schrijven zullen op die computers wel werken, maar
kunnen op andere voor grote problemen zorgen.

Robert Amesz
