# D I S K   C U R S U S   S P E C I A L   ( 1  )


## I N L E I D I N G

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
- Opbouw en werking     bootsector
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

## F A T

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


## D E   S T R U C T U U R

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


## P L A A T S   V A N   D E   F A T

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


## D I R E C T O R Y

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


## D E   S T R U C T U U R

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
```
MSB   7   6   5   4   3   2   1   0   LSB
      x   x   x   x   x   x   H   x
```
x:      niet gebruikt
H:      1=hidden, 0=normaal

De tijd is opgeslagen in twee bytes:
```
MSB   7   6   5   4   3   2   1   0   LSB
U4  U3  U2  U1  U0  M5  M4  M3    Byte 23
M2  M1  M0  S4  S3  S2  S1  S0    Byte 22
```
U0-U4:  uren (0-23)
M5-M0:  minuten (0-59)
S4-S0:  secondes (0-29)

Waarbij de  secondes door 2 zijn gedeeld (om het goede getal
te krijgen moet de waarde die in S0-S4 is opgeslagen dus met
2 worden vermenigvuldigd). Let op de volgorde van de bytes!

De datum is op soortgelijke wijze opgeslagen:
```
MSB   7   6   5   4   3   2   1   0   LSB
J6  J5  J4  J3  J2  J1  J0  M3    Byte 25
M2  M1  M0  D4  D3  D2  D1  D0    Byte 24
```
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

## E E N   V O O R B E E L D

Op  deze  Sunrise  Special  #1  staat  het  BASIC  programma
"DIR.BAS".  Dit  programma past  de eerder  verworven kennis
toe. Dit programma geeft van een diskette een zeer volledige
directory, met de volgende gegevens:

- de filenaam
- de tijd
- de datum
- de filelengte in bytes
- de filelengte in clusters (cls)
- een  lijstje met  alle clusters waarop de file staat in de
juiste volgorde

De eerste  vier worden ook door het DIR commando van MSX-DOS
gegeven,  en  daarvoor  hoeft  alleen  de  directory  worden
uitgelezen.  Maar  voor  het  laatste  moet  de  FAT  worden
uitgeplozen.

Ik  heb  het  programma in  BASIC geschreven  omdat het  dan
makkelijker  is te  volgen, bovendien zou ik bij machinetaal
dingen moeten  gebruiken die  we nu  nog niet  gehad hebben.
Hieronder  volgt de  listing, die ook in het softwaremenu te
vinden  is.  De  listing  wordt regelmatig  onderbroken voor
uitleg.


## D I R . B A S
```
1000 'DIR.BAS
1010 'Door Stefan Boer
1020 'Geeft dir met filenaam, tijd, datum, lengte, aantal
clusters en clusterverdeling van file
1030 'Voor Sunrise Special #1 - Bij Disk Cursus Special (1)
1040 '(c) SteveSoft/Stichting Sunrise 1992
1050 '
1060 CLEAR200,&HBFFF:DEFINTA-Z:DEFUSR=&H156
```
In  regel 1060  wordt het  gebied vanaf  &HC000 tegen  BASIC
beschremd,  hier  komen  de FAT en de directory te staan. De
BIOS routine  op adres  &H156 wist de toetsenbordbuffer, met
DEFINTA-Z wordt bepaald dat alle variabelen integers zijn.
```
1070 DEFFNA$(X)=RIGHT$("0"+MID$(STR$(X),2),2)
1080 KEYOFF:SCREEN0:WIDTH80:COLOR=(4,0,0,4):COLOR15,4,4
1090 PRINT"Uitgebreide directory"
1100 PRINT"Door Stefan Boer"
1110 PRINT"Gepubliceerd op Sunrise Special #1"
1120 PRINT"(c) Stichting Sunrise 1992"
```
In  regel 170  wordt een  functie gedefinieerd, die nodig is
voor een  nette datum  en tijd.  De functie voegt waar nodig
"verloopnullen"   toe,  zodat   je  bijvoorbeeld  01/06/1992
krijgt.
```
1130 PRINT
1140 PRINT"Plaats een diskette...";:U=USR(0):I$=INPUT$(1)
1150 PRINT:PRINT:PRINT"Inlezen van FAT en directory..."
1160 POKE&HF351,0:POKE&HF352,&HC0:A$=DSKI$(0,1):
POKE&HF352,&HC2:A$=DSKI$(0,2)
```
In regel  1160 worden  de eerste  twee sectoren  van de  FAT
gelezen  en vanaf &HC000 in het RAM gezet. Door het commando
A$=DSKI$(drive,sector)  komt  er  niets  in de  variabele A$
terecht (dat  kan ook  niet, want  een string  kan maar  255
tekens  lang zijn,  en geen  512). De  inhoud van  de sector
wordt in  het geheugen geplaatst, en wel vanaf het adres dat
op  locatie &HF351/&HF352 staat. Drive 0 betekent de default
drive.
```
1170 IFPEEK(&HC000)=&HF9THENS=7:POKE&HF352,&HC4:
A$=DSKI$(0,3)ELSES=5
1180 FORI=0TO6:POKE&HF352,&HC6+2*I:A$=DSKI$(0,S+I):NEXT
```
In regel  1170 wordt  de FAT  ID byte gelezen. Hieraan is te
zien  of de  diskette enkelzijdig (&HF8) is, of dubbelzijdig
(&HF9). Is  de diskette dubbelzijdig, dan bestaat de FAT uit
drie sectoren (de derde sector wordt ingeladen) en begint de
directory  op sector  7 (S=7), bij een enkelzijdige diskette
bestaat de FAT uit twee sectoren (die reeds zijn ingelezen),
en begint de directory op sector 5 (S=5).

In regel  1180 wordt  de directory ingelezen, die 7 sectoren
lang is. De directory wordt vanaf &HC600 in het RAM gezet.
```
1190 PRINT:PRINT"Filenaam:    Datum:     Tijd:    Bytes:
Cls: Clusters:":PRINT
```
"Cls"  staat  voor de  lengte in  clusters, "Bytes"  voor de
lengte in  bytes. Onder "Clusters:" komen de clusters waarop
de file staat in de juiste volgorde te staan.
```
1200 FORAD=&HC600TO&HD3FFSTEP32
1210 IFPEEK(AD)=0THENENDELSEIFPEEK(AD)=&HE5THEN1360
```
Regel  1200 doorloopt  de complete directory in het RAM. Die
directory begint  op &HC600 en is 3.5 kB lang. Een directory
entry  is 32 bytes groot. In regel 1210 wordt gekeken of het
einde van  de directory is bereikt (eerste byte van filenaam
is  0), zo ja, dan wordt het programma met END be�indigt. Is
de  eerste  byte &HE5,  dan is  de file  gewist en  wordt er
verder gegaan  met de volgende file (op regel 1360 staat een
NEXT commando).

1220 FORI=0TO7:PRINTCHR$(PEEK(AD+I));:NEXT
1230 IFPEEK(AD+8)=32THENPRINT"     ";ELSEPRINT".";:
FORI=8TO10:PRINTCHR$(PEEK(AD+I));:NEXT:PRINT" ";

Deze twee  regels zetten  de filenaam  op het  scherm. Regel
1230  zet  alleen  een  punt  op  het  scherm indien  er een
extensie is.
```
1240 PRINTFNA$(PEEK(AD+24)AND31);"/";FNA$((PEEK(AD+24)
AND224)\32+8*(PEEK(AD+25)AND1));"/";:PRINTUSING
"#### ";PEEK(AD+25)\2+1980;
```
Regel 1240 zet de datum op het scherm, die in de bytes 24 en
25  van  de  directory  entry  staat.  Met  de  ingewikkelde
formules worden  de juiste bits uit de twee bytes gefilterd.
De  dag staat  bijvoorbeeld in  bit 0 t/m 4 van byte 24. Met
een AND  31 (=&B11111)  instruktie wordt  de dag uit de byte
gehaald. Ter herinnering:
```
MSB   7   6   5   4   3   2   1   0   LSB
J6  J5  J4  J3  J2  J1  J0  M3    Byte 25
M2  M1  M0  D4  D3  D2  D1  D0    Byte 24
```
```
1250 PRINTFNA$(PEEK(AD+23)\8);":";FNA$((PEEK(AD+23)AND7)*8+
(PEEK(AD+22)AND224)\32);":";FNA$(2*(PEEK(AD+22)AND31));
" ";
```
Met  de tijd  gaat het net zo. Het schema van de tijd opslag
nog een keer:
```
MSB   7   6   5   4   3   2   1   0   LSB
U4  U3  U2  U1  U0  M5  M4  M3    Byte 23
M2  M1  M0  S4  S3  S2  S1  S0    Byte 22
```
```
1260 L#=PEEK(AD+28)+256*PEEK(AD+29)+65536!*PEEK(AD+30):
PRINTUSING"###### ";L#;
```
Hier wordt de lengte in bytes uit de directory gelezen en op
het   scherm  gezet.   Eigenlijk  moet   ook  AD+31   worden
uitgelezen,  maar  die  wordt  bij  een  diskette toch  niet
gebruikt. Er  moet een  # achter de L, omdat een file groter
kan  zijn  dan  32767  bytes,  en dan  zou je  zonder #  een
"Overflow" foutmelding krijgen (vanwege de DEFINTA-Z).
```
1270 PRINTUSING"###  ";INT((L#+1023)/1024);:X=45
```
Hier  wordt het  aantal clusters dat de file in beslag neemt
berekend  en  op  het  scherm gezet.  De berekening  zou ook
als volgt kunnen worden gedaan:
```
CL=INT(L#/1024):IFCL<>L#/1024THENCL=CL+1
```
Het  resultaat  is  precies  hetzelfde  (CL  is  het  aantal
clusters),  maar  ik  vind  de  eerste  manier veel  mooier.
INT(L#/1024)  is  niet  goed,  omdat je  dan bijna  altijd 1
cluster te  weinig hebt  (INT(1000/1024) is 0, maar een file
van  1000 bytes neemt ��n cluster in beslag). INT(L#/1024)+1
is  meestal  goed, maar niet als L# een veelvoud van 1024 is
(zoals in de tweede oplossing duidelijk is te zien).
```
1280 C=PEEK(AD+26)+256*PEEK(AD+27)
```
Lees het  nummer van de eerste cluster uit de directory. Dit
staat op byte 26 en 27.
```
1290 FA=(CAND1022)*1.5+&HC000:PRINTTAB(X);:
PRINTUSING"###";C;:X=X+4:IFX=81THENX=45
```
In  regel 1290 wordt het FAT adres (FA) berekend. C AND 1022
zorgt dat C een even getal wordt, door er 1 vanaf te trekken
als het  oneven was. Omdat twee FAT entry's samen 3 bytes in
beslag  nemen, wordt  dit vermenigvuldigd met 1.5. &HC000 is
het startadres van de FAT in het RAM.

In  X   wordt  de   actuele  X-coordinaat   van  de   cursor
bijgehouden,  zodat de  clusternummers netjes  rechts van de
overige informatie  komen te  staan. Het clusternummer wordt
op  het scherm  gezet met  een PRINT  USING, en  de nieuwe X
coordinaat wordt berekend.
```
1300 IFC<>(CAND1022)THEN1340
```
In  deze   regel  wordt   gekeken  of  we  de  eerste  (even
clusternummer) of de tweede (oneven clusternummer) FAT entry
uit  het groepje  van 3  bytes moeten  hebben. In regel 1310
wordt de even gelezen, in regel 1340 de oneven.
```
1310 V=C:C=(PEEK(FA+1)AND15)*256+PEEK(FA)
1320 IFC=&HFFFTHEN1370ELSEIFV<>C-1THENPRINTTAB(X-1);">";
1330 GOTO1290
```
Hier wordt  de even  FAT entry  gelezen. De  4 hoogste  bits
staan  op FA+1 en de 8 laagste bits op FA. Deze waarde wijst
naar de  volgende cluster. Als de waarde gelijk is aan &HFFF
dan is het einde van de file bereikt, en wordt verder gegaan
met   de  volgende   file  (regel  1370).  Anders  wordt  er
teruggegaan naar regel 1290, waar het nieuwe FAT adres wordt
berekend,  het  clusternummer wordt  geprint en  weer verder
wordt  gegaan met het uitlezen van die FAT entry. In V wordt
het  Vorige  clusternummer  bewaard,  in  regel  1320  wordt
gekeken  of  dat  ��n  lager  is  dan het  zojuist berekende
clusternummer  (als  dat  zo  is,  dan  zijn het  twee opeen
volgende clusters).  Er wordt  een > op het scherm gezet als
er een "gat" is.
```
1340 V=C:C=((PEEK(FA+1)AND240)\16)+PEEK(FA+2)*16
1350 IFC=&HFFFTHEN1370ELSEIFV<>C-1THENPRINTTAB(X-1);">";
1360 GOTO1290
```
In  regel 1340 t/m 1360 precies hetzelfde, alleen nu voor de
FAT entry  met een  oneven nummer.  Hier staan  de 4 laagste
bits op FA+1 en de 8 hoogste bits op FA+2.
```
1370 IFPOS(0)>0THENPRINT
1380 NEXT
```
Regel 1370  zorgt ervoor  dat er  op een  nieuwe regel wordt
begonnen. Als er alleen PRINT zou hebben gestaan, zou er een
regel  over worden  geslagen als  de cursor al op een nieuwe
regel  stond.  Regel  1380  zorgt  dat  er  met  de volgende
directory entry wordt verdergegaan.


## V O O R B E E L D   U I T V O E R

Laten we  een eerder  gebruikt voorbeeld  nog eens nemen. Op
een disk staan de files:

SUNRISE.001     cluster 2 t/m 4
SUNRISE.004     cluster 5 t/m 6 en 11 t/m 16
SUNRISE.003     cluster 7 t/m 10

DIR.BAS geeft dan de volgende uitvoer:

Filenaam:    Datum:     Tijd:    Bytes: Cls: Clusters:

SUNRISE .001 22/06/1992 16:03:42   2404   3    2   3   4
SUNRISE .004 24/06/1992 10:12:02   7169   8    5   6> 11
                     12  13  14
                     15  16
SUNRISE .003 22/06/1992 16:10:16   3092   4    7   8   9
                     10

Het  ">"  tekentje geeft  dus aan  dat er  een sprong  wordt
gemaakt. Het kan soms voorkomen dat het programma de soep in
loopt. Dit  ligt dan niet aan DIR.BAS (hoop ik), maar dan is
er waarschijnlijk iets mis met de FAT van die diskette.

Gebruikt u  DIR.BAS en  ziet u  veel > tekentjes, dan is het
verstandig  om  die  diskette  met  een kopieerprogramma  op
bestandsniveau   (bijvoorbeeld  BK   of  gewoon  COPY  onder
MSX-DOS) naar  een andere  (lege!) diskette  te kopi�ren. De
bestanden  worden dan  weer netjes aangesloten achter elkaar
gezet. Bij  het laden  van een  file betekent elk > tekentje
namelijk   dat  er   een  stuk   van  de  disk  moet  worden
overgeslagen,  en  dit  kost  meestal  extra  tijd. Let  op:
kopi�ren met  een kopieerprogramma  op sectorniveau  (XCopy,
FastCopy,  WorkMate, TurboCopy,  etc.) heeft  hier geen zin,
omdat de diskette dan letterlijk wordt overgenomen.


## B D O S

Dit  is  behoorlijk  ingewikkeld,  maar  gelukkig  hoeft  de
machinetaalprogrammeur  zich normaal  gesproken niet druk te
maken over FAT en directory. Als alle diskacties via de BDOS
worden afgehandeld  (die in  een volgend  deel van de cursus
zeer  uitgebreid zal  worden besproken),  dan zorgt  de BDOS
ervoor dat de FAT en directory keurig worden bijgehouden.

Toch is  het handig  om van  deze informatie op de hoogte te
zijn.  Het is  bijvoorbeeld best  te doen  om een  supersnel
laadsysteem te  schrijven, waarbij de directory en de FAT in
het  geheugen staan.  Die hoeven  dan niet  steeds te worden
ingeladen, zodat  de kop  van de diskdrive minder op en neer
hoeft te gaan. Dit versnelt het laden van files aanzienlijk.
Dit  systeem wordt  toegepast bij  BK, de BestandsKopieerder
voor MemMan van MCM's MST.


## W A A R S C H U W I N G ! !

Dit is  een zeer belangrijke waarschuwing die ik ook nog wel
eens  op Sunrise  Magazine zal  zetten. Stel  er gebeurt het
volgende:

U bent met een origineel programma bezig waarbij de diskette
waarop  dat  originele  programma  staat uiteraard  op write
protected staat. Nu wilt u iets saven op een werkdisk, dus u
kiest de save optie. Maar per ongeluk laat u de disk van het
originele  programma  in de  drive zitten,  waardoor er  een
write protected  foutmelding volgt.  U kunt kiezen uit Retry
of Abort.

De logische oplossing lijkt nu om de werkdisk in de drive te
doen (write enable) en voor retry te kiezen.

## D O E   D I T   N O O I T ! ! !

U  kunt uw  werkdisk daarna  wel formatteren,  want in ieder
geval  de  FAT  en  meestal  ook  de directory  zal compleet
vernield zijn!!!

Dit  is een foutje van de BDOS, die in zo'n geval niet eerst
de FAT  en directory  van de  nieuwe disk  inlaadt, maar  er
vanuit  gaat dat  alleen de write protect eraf is gehaald en
dat het  nog steeds  dezelfde diskette is. De BDOS maakt dus
de  nodige wijzigingen in de FAT en directory die hij nog in
het RAM  had staan  en schrijft  die dan  naar de  disk. Dat
waren  dus de FAT en directory van de disk met het originele
programma, en  die worden  nu op  uw werkdisk  geschreven. U
begrijpt  wel dat  de informatie  die op  die werkdisk stond
alleen met  zeer veel  moeite of  helemaal niet  is terug te
halen. Kortom, kies in zo'n geval altijd voor abort en begin
opnieuw met het saven. Dan zal het wel goed gaan.

Ik denk  dat veel MSX'ers al eens een dergelijke ramp hebben
meegemaakt  (bij mij  is m'n disk met teksten al een stuk of
vijf keer  op deze  manier naar  de haaien gegaan voordat ik
doorhad  hoe het  kwam), en niet wisten waar het aan lag. Nu
weet u  het wel,  en het  kan u  een hoop  werk en  ergernis
besparen!

Oproep   aan  iedereen   die  software  uitgeeft:  waarschuw
hiervoor in de handleiding of maak een retry onmogelijk. (Ik
zal hier  bij alle software die Sunrise uitgeeft persoonlijk
op letten!)


## T O T   S L O T

Ik denk dat ik er na deze afdwaling maar een eind aan draai.
Ik  weet nu  nog niet  wat ik de volgende keer ga doen, maar
dat zou  best wel eens de BDOS kunnen zijn. Dat zal een hele
lange  tekst  worden,  want er  zijn 42  BDOS calls!  Tot de
volgende keer!

Stefan Boer
