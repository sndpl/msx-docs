- DEEL 2 -

D I S K   C U R S U S   S P E C I A L   ( 1  )


(Nvdr. Deel 1 kunt u in het submenu vinden.)


E E N   V O O R B E E L D

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


D I R . B A S

1000 'DIR.BAS
1010 'Door Stefan Boer
1020 'Geeft dir met filenaam, tijd, datum, lengte, aantal
clusters en clusterverdeling van file
1030 'Voor Sunrise Special #1 - Bij Disk Cursus Special (1)
1040 '(c) SteveSoft/Stichting Sunrise 1992
1050 '
1060 CLEAR200,&HBFFF:DEFINTA-Z:DEFUSR=&H156

In  regel 1060  wordt het  gebied vanaf  &HC000 tegen  BASIC
beschremd,  hier  komen  de FAT en de directory te staan. De
BIOS routine  op adres  &H156 wist de toetsenbordbuffer, met
DEFINTA-Z wordt bepaald dat alle variabelen integers zijn.

1070 DEFFNA$(X)=RIGHT$("0"+MID$(STR$(X),2),2)
1080 KEYOFF:SCREEN0:WIDTH80:COLOR=(4,0,0,4):COLOR15,4,4
1090 PRINT"Uitgebreide directory"
1100 PRINT"Door Stefan Boer"
1110 PRINT"Gepubliceerd op Sunrise Special #1"
1120 PRINT"(c) Stichting Sunrise 1992"

In  regel 170  wordt een  functie gedefinieerd, die nodig is
voor een  nette datum  en tijd.  De functie voegt waar nodig
"verloopnullen"   toe,  zodat   je  bijvoorbeeld  01/06/1992
krijgt.

1130 PRINT
1140 PRINT"Plaats een diskette...";:U=USR(0):I$=INPUT$(1)
1150 PRINT:PRINT:PRINT"Inlezen van FAT en directory..."
1160 POKE&HF351,0:POKE&HF352,&HC0:A$=DSKI$(0,1):
POKE&HF352,&HC2:A$=DSKI$(0,2)

In regel  1160 worden  de eerste  twee sectoren  van de  FAT
gelezen  en vanaf &HC000 in het RAM gezet. Door het commando
A$=DSKI$(drive,sector)  komt  er  niets  in de  variabele A$
terecht (dat  kan ook  niet, want  een string  kan maar  255
tekens  lang zijn,  en geen  512). De  inhoud van  de sector
wordt in  het geheugen geplaatst, en wel vanaf het adres dat
op  locatie &HF351/&HF352 staat. Drive 0 betekent de default
drive.

1170 IFPEEK(&HC000)=&HF9THENS=7:POKE&HF352,&HC4:
A$=DSKI$(0,3)ELSES=5
1180 FORI=0TO6:POKE&HF352,&HC6+2*I:A$=DSKI$(0,S+I):NEXT

In regel  1170 wordt  de FAT  ID byte gelezen. Hieraan is te
zien  of de  diskette enkelzijdig (&HF8) is, of dubbelzijdig
(&HF9). Is  de diskette dubbelzijdig, dan bestaat de FAT uit
drie sectoren (de derde sector wordt ingeladen) en begint de
directory  op sector  7 (S=7), bij een enkelzijdige diskette
bestaat de FAT uit twee sectoren (die reeds zijn ingelezen),
en begint de directory op sector 5 (S=5).

In regel  1180 wordt  de directory ingelezen, die 7 sectoren
lang is. De directory wordt vanaf &HC600 in het RAM gezet.

1190 PRINT:PRINT"Filenaam:    Datum:     Tijd:    Bytes:
Cls: Clusters:":PRINT

"Cls"  staat  voor de  lengte in  clusters, "Bytes"  voor de
lengte in  bytes. Onder "Clusters:" komen de clusters waarop
de file staat in de juiste volgorde te staan.

1200 FORAD=&HC600TO&HD3FFSTEP32
1210 IFPEEK(AD)=0THENENDELSEIFPEEK(AD)=&HE5THEN1360

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

1240 PRINTFNA$(PEEK(AD+24)AND31);"/";FNA$((PEEK(AD+24)
AND224)\32+8*(PEEK(AD+25)AND1));"/";:PRINTUSING
"#### ";PEEK(AD+25)\2+1980;

Regel 1240 zet de datum op het scherm, die in de bytes 24 en
25  van  de  directory  entry  staat.  Met  de  ingewikkelde
formules worden  de juiste bits uit de twee bytes gefilterd.
De  dag staat  bijvoorbeeld in  bit 0 t/m 4 van byte 24. Met
een AND  31 (=&B11111)  instruktie wordt  de dag uit de byte
gehaald. Ter herinnering:

MSB   7   6   5   4   3   2   1   0   LSB
J6  J5  J4  J3  J2  J1  J0  M3    Byte 25
M2  M1  M0  D4  D3  D2  D1  D0    Byte 24

1250 PRINTFNA$(PEEK(AD+23)\8);":";FNA$((PEEK(AD+23)AND7)*8+
(PEEK(AD+22)AND224)\32);":";FNA$(2*(PEEK(AD+22)AND31));
" ";

Met  de tijd  gaat het net zo. Het schema van de tijd opslag
nog een keer:

MSB   7   6   5   4   3   2   1   0   LSB
U4  U3  U2  U1  U0  M5  M4  M3    Byte 23
M2  M1  M0  S4  S3  S2  S1  S0    Byte 22

1260 L#=PEEK(AD+28)+256*PEEK(AD+29)+65536!*PEEK(AD+30):
PRINTUSING"###### ";L#;

Hier wordt de lengte in bytes uit de directory gelezen en op
het   scherm  gezet.   Eigenlijk  moet   ook  AD+31   worden
uitgelezen,  maar  die  wordt  bij  een  diskette toch  niet
gebruikt. Er  moet een  # achter de L, omdat een file groter
kan  zijn  dan  32767  bytes,  en dan  zou je  zonder #  een
"Overflow" foutmelding krijgen (vanwege de DEFINTA-Z).

1270 PRINTUSING"###  ";INT((L#+1023)/1024);:X=45

Hier  wordt het  aantal clusters dat de file in beslag neemt
berekend  en  op  het  scherm gezet.  De berekening  zou ook
als volgt kunnen worden gedaan:

CL=INT(L#/1024):IFCL<>L#/1024THENCL=CL+1

Het  resultaat  is  precies  hetzelfde  (CL  is  het  aantal
clusters),  maar  ik  vind  de  eerste  manier veel  mooier.
INT(L#/1024)  is  niet  goed,  omdat je  dan bijna  altijd 1
cluster te  weinig hebt  (INT(1000/1024) is 0, maar een file
van  1000 bytes neemt ��n cluster in beslag). INT(L#/1024)+1
is  meestal  goed, maar niet als L# een veelvoud van 1024 is
(zoals in de tweede oplossing duidelijk is te zien).

1280 C=PEEK(AD+26)+256*PEEK(AD+27)

Lees het  nummer van de eerste cluster uit de directory. Dit
staat op byte 26 en 27.

1290 FA=(CAND1022)*1.5+&HC000:PRINTTAB(X);:
PRINTUSING"###";C;:X=X+4:IFX=81THENX=45

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

1300 IFC<>(CAND1022)THEN1340

In  deze   regel  wordt   gekeken  of  we  de  eerste  (even
clusternummer) of de tweede (oneven clusternummer) FAT entry
uit  het groepje  van 3  bytes moeten  hebben. In regel 1310
wordt de even gelezen, in regel 1340 de oneven.

1310 V=C:C=(PEEK(FA+1)AND15)*256+PEEK(FA)
1320 IFC=&HFFFTHEN1370ELSEIFV<>C-1THENPRINTTAB(X-1);">";
1330 GOTO1290

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

1340 V=C:C=((PEEK(FA+1)AND240)\16)+PEEK(FA+2)*16
1350 IFC=&HFFFTHEN1370ELSEIFV<>C-1THENPRINTTAB(X-1);">";
1360 GOTO1290

In  regel 1340 t/m 1360 precies hetzelfde, alleen nu voor de
FAT entry  met een  oneven nummer.  Hier staan  de 4 laagste
bits op FA+1 en de 8 hoogste bits op FA+2.

1370 IFPOS(0)>0THENPRINT
1380 NEXT

Regel 1370  zorgt ervoor  dat er  op een  nieuwe regel wordt
begonnen. Als er alleen PRINT zou hebben gestaan, zou er een
regel  over worden  geslagen als  de cursor al op een nieuwe
regel  stond.  Regel  1380  zorgt  dat  er  met  de volgende
directory entry wordt verdergegaan.


V O O R B E E L D   U I T V O E R

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


 B D O S

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


W A A R S C H U W I N G ! !

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

D O E   D I T   N O O I T ! ! !

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


T O T   S L O T

Ik denk dat ik er na deze afdwaling maar een eind aan draai.
Ik  weet nu  nog niet  wat ik de volgende keer ga doen, maar
dat zou  best wel eens de BDOS kunnen zijn. Dat zal een hele
lange  tekst  worden,  want er  zijn 42  BDOS calls!  Tot de
volgende keer!

                       Stefan Boer
