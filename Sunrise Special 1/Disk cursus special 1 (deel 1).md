- DEEL 1 -

D I S K   C U R S U S   S P E C I A L   ( 1  )


I N L E I D I N G

In  de diskcursus op Sunrise Magazine had ik al aangekondigd
dat er op de Sunrise Special een diskcursus voor gevorderden
zou  komen.  De cursus  op het  magazine is  voornamelijk op
BASIC gericht, terwijl we hier juist de machinetaal aspecten
van het diskgebruik gaan bespreken. Ik ga er hier vanuit dat
de lezer  een behoorlijke  basiskennis heeft, ik zal dan ook
niet meer gaan uitleggen wat bijvoorbeeld een file is.

Veel   programmeurs   vinden   diskgebruik   in  machinetaal
moeilijk. Toch  is het  tamelijk eenvoudig,  mits men in het
bezit  is van  duidelijke uitleg  en documentatie.  Elke MSX
computer met  diskdrive heeft  namelijk dezelfde  serie BDOS
system  calls tot  zijn beschikking,  waarmee vrijwel al het
werk uit handen wordt genomen.

In deze  cursus op  de Special  zullen (onder voorbehoud) de
volgende onderwerpen aan bod komen:

- BDOS system calls
- BIOS calls (PHYDIO en FORMAT)
- Opbouw en werking bootsector
- Opbouw en werking FAT
- Opbouw en werking directory
- Zelf een programma schrijven dat vanuit de bootsector
opstart
- Zelf .COM files schrijven

Dit is alles voor MSX-DOS 1/Disk BASIC 1.0. Misschien dat ik
aan het  eind van  de cursus  verder ga  met MSX-DOS  2/Disk
BASIC 2.01, maar dat is nu nog niet zeker.

In  dit eerste deel de werking van FAT en directory, oftewel
hoe staan files op een disk.


  F A T

Meteen  al  een  ingewikkeld onderwerp:  de File  Allocation
Table.  Bij het  MSX Disk Operating System (kortweg MSX-DOS)
worden  files  behandeld als  een verzameling  clusters. Een
cluster bestaat  bij een  normale MSX  disk (3.5")  uit twee
sectoren.  Een cluster  is dus  2 *  512 = 1024 bytes (1 kB)
groot.

Een  file beslaat altijd minimaal ��n cluster, maar een file
die groter  is dan ��n cluster worden over meerdere clusters
verdeeld. Dit betekent dat een file van slechts 1 byte op de
diskette  toch maar  liefst 1024  bytes in  beslag neemt  (1
cluster), en  een file van 2049 bytes 3 clusters (met in het
derde cluster slechts 1 byte).

De FAT zorgt ervoor dat de ruimte op de disk altijd optimaal
wordt  benut.  Als  er  vaak  op  een diskette  files worden
toegevoegd  en   andere  files  gewist,  zullen  de  "volle"
clusters  niet meer  aaneengesloten op de disk staan. Zonder
de FAT zouden deze "gaten" niet meer kunnen worden benut.

Als op  een diskette  waarop de  vrije ruimte  niet aan  een
gesloten  is een  (grote) file wordt gezet, dan zullen eerst
de vrije  ruimtes worden opgevuld, pas daarna wordt de vrije
ruimte  "achteraan" benut. Zonder de FAT zou deze file later
nooit meer  in te  laden zijn, omdat het niet bekend is waar
de file verder gaat. In de FAT wordt dit bijgehouden.

Stel u  heeft de  volgende files  achter elkaar  op een disk
staan:

SUNRISE.001     cluster 2 t/m 4
SUNRISE.002     cluster 5 t/m 6
SUNRISE.003     cluster 7 t/m 10

Nu  geeft  u  een  KILL"SUNRISE.002"  opdracht. De  volgende
situatie ontstaat:

SUNRISE.001     cluster 2 t/m 4
vrij            cluster 5 t/m 6
SUNRISE.003     cluster 7 t/m 10

Nu zet u de file "SUNRISE.004" op de disk, die acht clusters
groot is. Nu gebeurt het volgende:

SUNRISE.001     cluster 2 t/m 4
SUNRISE.004     cluster 5 t/m 6 en 11 t/m 16
SUNRISE.003     cluster 7 t/m 10

De file  "SUNRISE.004" staat  nu niet meer aaneengesloten op
de  disk.  Toch  zal MSX-DOS  de file  zonder enig  probleem
inlezen.  En dat  is waar  de FAT om de hoek komt kijken. In
deze  File  Allocation Table  (de naam  zegt het  al), wordt
namelijk opgeslagen waar (met welke cluster) een file verder
gaat.


D E   S T R U C T U U R

De eerste  byte van  de FAT  is de  "FAT ID" byte. Dit is de
gewoon  de "media  ID", waarmee  het formaat  diskette wordt
aangegeven. Deze informatie staat ook in de bootsector, maar
het is  natuurlijk makkelijk  dat het  ook in  de FAT staat,
zodat  de bootsector niet bij elke diskactie hoeft te worden
ingelezen. Bij MSX zijn de volgende twee ID's gebruikelijk:

Naam:           Aantal zijden:  ID:

MF-1DD          1               &HF8
MF-2DD          2               &HF9

De volgende  twee bytes  van de fat zijn altijd &HFF en doen
niet ter zake. Op de vierde byte begint de eigenlijke tabel.
De  informatie is  opgeslagen in  een vreemd  formaat van 12
bits per  FAT entry.  De eerste  FAT entry  is nummer 2. Het
nummer van de FAT entry is ook het nummer van de cluster die
ermee correspondeert. Een voorbeeld FAT in hexadecimaal:

F9 FF FF 03 40 00 FF 6F 00 FF .F .. .. ..
0  1  2  3  4  5  6  7  8  9  10 11
-------  -------  --------
entry    entry    entry
2 en 3   4 en 5   6 en 7

De bytes 0 t/m 2 bevatten de FAT ID (F9) en twee dummy bytes
(FF  FF), het  betreft hier  een dubbelzijdige  diskette. De
bytes 3 t/m 5 bevatten de informatie van cluster 2 en 3. Dit
moet als volgt worden gelezen:

waarde cluster 2 = RECHTER nibble byte 4 + byte 3 = 003
waarde cluster 3 = byte 5 + LINKER nibble byte 4  = 004

De  waarde in  een FAT entry wijst naar de cluster waarin de
file  verder  gaat.  In FAT entry 2 staat "003", dus de file
gaat verder in cluster 3. In FAT entry 3 staat "004", dus de
file gaat verder in cluster 4. Zo eenvoudig is dat!

Het einde  van een  file wordt  aangegeven met "FFF". In fat
entry 4 staat "FFF", dus daar eindigt de file. De file staat
dus op cluster 2 t/m 4. We herhalen het stukje FAT nogmaals:

F9 FF FF 03 40 00 FF 6F 00 FF .F .. .. ..
0  1  2  3  4  5  6  7  8  9  10 11
-------  -------  --------
entry    entry    entry
2 en 3   4 en 5   6 en 7

We  zien dat  FAT entry  5 de waarde 006 bevat, de file gaat
dus verder met cluster 6. In FAT entry 6 staat FFF, dus daar
eindigt de file. Dit is dus een korte file, die slechts twee
clusters beslaat (5 en 6).

Omdat  de volgorde  van de  bytes nogal vreemd is zal ik nog
een duidelijk voorbeeld geven:

21 43 65

Deze FAT entry's bevatten respectievelijk &H321 en &H654.

Een leeg  cluster wordt  aangegeven met  "000". Indien u een
bepaald  gedeelte van  een diskette wilt beschermen tegen de
BDOS  (omdat  u  daar  zelf de  disk rechtstreeks  op sector
niveau  wilt  lezen/schrijven), kunt  u het  overeenkomstige
deel van de FAT vullen met FFF.


P L A A T S   V A N   D E   F A T

De  FAT  staat  direct na  de bootsector,  en begint  dus op
sector  1. Het aantal sectoren dat de FAT in beslag neemt is
afhankelijk van  de grootte  van de diskette. De grootte van
de  FAT staat  in de bootsector. Bij MSX-DOS wordt er direct
achter de  FAT ook  nog een kopie van de FAT bewaard. (In de
bootsector staat ook het aantal FATs vermeld.) Deze kopie is
nodig  omdat de  diskette niet  meer normaal te gebruiken is
als de FAT niet meer in orde is.

Rekent  u  even mee?  Een dubbelzijdige  diskette heeft  713
clusters  (7   clusters  =   14  sectoren  zijn  nodig  voor
bootsector  (1), 2  FATs (6) en directory (7).) De FAT heeft
aan het  begin ook  nog twee  clusters met de FAT-ID, dus in
totaal  zijn er  715 FAT  entry's. Elke entry vergt 12 bits,
dus er zijn 8.580 bits nodig. Een sector telt 4096 bits, dus
op een  dubbelzijdige diskette  neemt de  FAT 3  sectoren in
beslag. De FAT staat dus op sector 1 t/m 3 en de reserve FAT
op sector 4 t/m 6.

Bij   een   enkelzijdige   diskette   zijn   twee   sectoren
noodzakelijk, en  staat de  FAT dus  op sector  1 en 2 en de
reserve FAT op sector 3 en 4.


D I R E C T O R Y

Met  alleen de  FAT kom  je er niet. Je kunt een file immers
niet terugvinden zonder te weten waar hij begint (het nummer
van de  eerste cluster). Dus staat er op een diskette direkt
achter  de FAT  (dubbelzijdig sector  7 t/m  13, enkelzijdig
sector  5  t/m  11)  de  directory  (inhoudsopgave).  In  de
directory worden de volgende gegevens opgeslagen:

- de filenaam
- de extensie
- de tijd
- de datum
- de lengte
- de eerste cluster

De  directory is  7 sectoren  lang, dat  is 3584 bytes. Elke
directory entry  vergt 32  bytes, er passen dus 112 files op
een diskette. Maar dat wist u natuurlijk al.


D E   S T R U C T U U R

Hoe  is de  directory opgebouwd? De 112 entry's staan achter
elkaar op  de zeven  sectoren. Elke  entry is 32 bytes lang.
Deze bytes zijn als volgt ingedeeld:

+0      Filenaam
+8      Extensie
+11     Attribuut
+12-21  Niet gebruikt (voor compatibility met MS-DOS)
+22     Tijd
+24     Datum
+26     Eerste cluster
+28     File lengte

Voor  de  filenaam  en de  extensie verwijs  ik u  naar Disk
Cursus (1) op Sunrise Magazine #1. De Filenaam is maximaal 8
karakters lang, de extensie maximaal 3 tekens. De punt staat
uiteraard niet in de directory.

De  attribuut  wordt bij  MSX-DOS1 eigenlijk  niet gebruikt,
normaal staat  daar een  0. Bij  MSX-DOS2 (en MS-DOS) worden
hier  de attributen  (read only,  hidden, etc.)  bewaard. De
enige  attribuut  die door  MSX-DOS1 wordt  "ondersteund" is
"hidden", de  file is  dan helemaal niet meer in te lezen en
komt  niet in  de filelist voor als die wordt opgevraagd met
DIR of  FILES. Onder MSX-DOS1 is de attribuut byte als volgt
opgebouwd:

MSB   7   6   5   4   3   2   1   0   LSB
x   x   x   x   x   x   H   x

x:      niet gebruikt
H:      1=hidden, 0=normaal

De tijd is opgeslagen in twee bytes:

MSB   7   6   5   4   3   2   1   0   LSB
U4  U3  U2  U1  U0  M5  M4  M3    Byte 23
M2  M1  M0  S4  S3  S2  S1  S0    Byte 22

U0-U4:  uren (0-23)
M5-M0:  minuten (0-59)
S4-S0:  secondes (0-29)

Waarbij de  secondes door 2 zijn gedeeld (om het goede getal
te krijgen moet de waarde die in S0-S4 is opgeslagen dus met
2 worden vermenigvuldigd). Let op de volgorde van de bytes!

De datum is op soortgelijke wijze opgeslagen:

MSB   7   6   5   4   3   2   1   0   LSB
J6  J5  J4  J3  J2  J1  J0  M3    Byte 25
M2  M1  M0  D4  D3  D2  D1  D0    Byte 24

J0-J6:  jaar (1980-2079, moet 1980 bij worden opgeteld)
M0-M3:  maand (1-12)
D0-D4:  dag (1-31)

Let wederom op de volgorde van de bytes!

De  eerste  cluster en  de filelengte  staan in  normaal Z80
formaat, dus de bytes in volgorde van low naar high:

cluster = (byte 26) + 256 * (byte 27)
filelengte = (byte 28) + 256 * (byte 29) + 65536 * (byte 30)
+ 16777216 * (byte 31)

Byte 31  van de filelengte zal bij een normale diskette niet
worden  gebruikt, omdat  de maximale lengte van een file (op
een  dubbelzijdige  diskette)  713  * 1024  = 730.112  bytes
bedraagt.  Daarvoor  zijn drie  bytes (0-16.777.215,  16 MB)
ruimschoots voldoende.  De vierde byte (waardoor waardes tot
256^4  mogelijk zijn,  4096 MB),  wordt alleen bij harddisks
gebruikt.

Als een  nieuwe file op een disk wordt gezet, wordt daarvoor
de  eerste de  beste vrije directory entry gebruikt. Als een
file  wordt  gewist, wordt  de eerste  byte van  de filenaam
vervangen door  &HE5. Een  0 als eerste byte van de filenaam
geeft  aan dat  deze en alle volgende entry's nog nooit zijn
gebruikt en dus leeg zijn.

Als de directory vol is, kunnen er geen nieuwe files meer op
de  disk  worden  gezet. Ook  niet als  er nog  bergen vrije
clusters  zijn. Bij  MSX-DOS2 is  dit probleem opgelost door
subdirectories, maar  bij DOS1  zullen we  gewoon een nieuwe
disk moeten pakken of een paar oude files moeten killen.

(Nvdr. Dit  artikel was  langer dan  16 kB,  en moest dus in
twee delen worden gesplitst. Deel twee kunt u in het submenu
vinden.)

                       Stefan Boer
