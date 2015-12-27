                    - EERSTE GEDEELTE -

            G E H E U G E N   S C H A K E L E N 
                                                 

Een   tijdje  geleden   vroeg  iemand   aan  mij  waarom  de 
replayroutine die  bij FAC  Soundtracker wordt geleverd niet 
werkt  als je  hem gebruikt  in een machinetaalprogramma dat 
vanuit de bootsector opstart.

Eerst wist  ik het  antwoord niet,  maar toen  ik de routine 
eens bekeek zag ik al snel waar het aan lag: de FAC schakelt 
het geheugen niet op de juiste manier!

Waarom  gaat het dan vanuit BASIC wel goed zult u denken. Om 
dat uit  te leggen, moet ik eerst uitleggen hoe het geheugen 
geschakeld dient te worden.


         P R I M A I R E   E N   S U B S L O T E N 

Elke  MSX computer  heeft vier primaire sloten, die elk weer 
kunnen worden gesplitst in vier subsloten. Zowel de primaire 
als de subsloten worden genummerd van 0 tot 3.

Een voorbeeld van een slotindeling is (FS-A1ST MSX turbo R):

        0-0     0-2     3-0     3-1     3-2     3-3

0000    MAIN    -       RAM     SUB     -       HIRO
3FFF    ROM                     ROM

4000    MAIN    FM      RAM     KANJI   DISK    HIRO
7FFF    ROM     BASIC           BASIC   ROM

8000    -       -       RAM     KANJI   -       HIRO
BFFF                            BASIC

C000    -       -       RAM     -       -       -
FFFF


Slot 1 en 2 zijn de cartridge sloten, de indeling hiervan is 
afhankelijk van  wat men  in het cartridgeslot stopt. Als er 
een  slotexpander in  steekt, wordt  het slot  uitgebreid in 
vier subsloten, bijvoorbeeld slot 1-0, 1-1, 1-2 en 1-3.

Het MAIN  ROM moet volgens de MSX standaard altijd in slot 0 
zitten,  als slot 0 is ge�xpandeerd is dit dus slot 0-0. Het 
MAIN  ROM  vanaf adres  &H0000 bevat  het BIOS  (Basic Input 
Output System), vanaf &H4000 staat de BASIC interpreter.

Slot 0  is bij de turbo R ge�xpandeerd voor de FM BASIC, die 
in  slot 0-2 is geplaatst. In slot 3-0 zit het memory mapper 
RAM,  de  I/O  poorten  &HFC t/m  &HFF bepalen  welke memory 
mapper segmenten hier staan. De SUB ROM is de MSX2 BASIC, de 
KANJI ROM is de MSX2+ en MSX turbo R BASIC.

De diskROM  zit op  adres &H4000 in slot 3-2. Eigenlijk zijn 
er   bij  de   turbo  R   meerdere  diskROMs,  die  met  een 
omschakeladres  worden  geselecteerd.  Welke  diskROM er  is 
geselecteerd  is  o.a.  afhankelijk  van de  mode waarin  de 
computer werd opgestart (Disk BASIC 1.0 of Disk BASIC 2.01).

In   slot   3-3   bevindt   zich  tenslotte   de  ingebouwde 
programmatuur, die  ik hier voor het gemak Hiro noem. Dit is 
het  ingebouwde pakket,  dat met  een CALL HIRO of een reset 
(met  de  keuzeschakelaar  in  de linker  positie) is  op te 
roepen.


                       S L O T   I D 

De BIOS heeft een handige manier om een bepaald slot weer te 
geven, dit  is de  zogenaamde slot ID byte. Deze byte is als 
volgt opgebouwd:

MSB   7   6   5   4   3   2   1   0   LSB

      E   0   0   0   --S--   --P--

E: slot ge�xpandeerd? (1=ja)
S: nummer van secundair slot
P: nummer van primair slot

S  wordt verwaarloosd  als E=0. Enkele voorbeelden maken het 
misschien duidelijker:

Slot:   ID-byte:

2       &B00000010      &H02
3-2     &B10001011      &H8B
3-3     &B10001111      &H8F
0       &B00000000      &H00
0-0     &B10000000      &H80

Merk op  dat er  verschil is  tussen slot 0 en 0-0!!! Dit is 
natuurlijk  alleen  zo  bij  computers  waarbij  slot  0  is 
ge�xpandeerd, maar als je een programma schrijft is het toch 
meestal  de bedoeling  dat het  op alle computers werkt denk 
ik.


                 B I O S   R O U T I N E S 

De  veiligste manier  om met  het geheugen  te schakelen  is 
natuurlijk via  het BIOS, zo hoort het ook via de standaard. 
Het BIOS heeft de volgende routines voor slotschakeling e.d. 
in huis:

&H000C     RDSLT

Werking:   Lees  een  byte  uit een  bepaald slot.  Het slot 
           wordt geselecteerd,  de byte  wordt gelezen en de 
           oude  slot  schakeling  wordt  weer hersteld.  De 
           interrupts  worden uitgezet  bij aanroep van deze 
           routine, en niet meer aangezet.
Input:     A  = slot ID byte
           HL = adres
Output:    A  = data
Registers: AF, BC, DE


&H0014     WRSLT

Werking:   Schrijf een  byte naar een bepaald slot. Het slot 
           wordt  geselecteerd, de  byte wordt geschreven en 
           de oude  slot schakeling  wordt weer hersteld. De 
           interrupts worden  uitgezet bij  aanroep van deze 
           routine, en niet meer aangezet.
Input:     A  = slot ID byte
           HL = adres
           E  = data
Output:    -
Registers: AF, BC, D


&H001C     CALSLT

Werking:   Roept een routine in een bepaald slot aan (inter-
           slot call).
Input:     Bit 9-16 van IY = slot ID byte
           IX = adres
Hangt verder af van de routine die wordt aangeroepen. Let op 
dat hierbij de slot ID byte in de hoogste 8 bits van IY moet 
staan (bij de R800 is dit IYL!).


&H0024     ENASLT

Werking:   Selecteer een slot zodat het kan worden gebruikt. 
           De  interrupts worden  bij het aanroepen van deze 
           routine uitgezet en niet meer aangezet.
Input:     A  = slot ID byte
           H  = bit 7 en 6 bepalen welke page
Output:    -
Registers: allemaal
De waarde  van de  bits 6  en 7  van H bepalen welke page er 
wordt  geschakeld. Het  komt er  dus in feite op neer dat de 
page wordt geschakeld naar de page waarin het adres voorkomt 
dat in HL staat.

Page 0  &H0000-&H3FFF   H tussen &H00 en &H3F
Page 1  &H4000-&H7FFF   H tussen &H40 en &H7F
Page 2  &H8000-&HBFFF   H tussen &H80 en &HBF
Page 3  &HC000-&HFFFF   H tussen &HC0 en &HFF


&H0030     CALLF

Deze  routine kan  behalve met  CALL &H0030 ook met RST &H30 
worden aangeroepen.  Dit kost  maar ��n  byte in  plaats van 
drie, en is iets sneller.

Werking:   roept een routine in een bepaald slot aan (inter- 
           slot call)
Input  en output  zijn afhankelijk  van de routine die wordt 
aangeroepen. De waarde van AF wordt gewijzigd. De invoer van 
adres en slot gaan als volgt:

        RST &H30        ; inter-slot call
        DB  SLOT        ; slot ID byte van slot waarin
                        ; de routine staat
        DW  ADRES       ; adres van aan te roepen routine

Dit is  dus een  zeer snelle en korte methode om een routine 
in een "ver" slot aan te roepen. Dit wordt veel gebruikt bij 
hooks, daar is precies voldoende ruimte (5 bytes) om een RST 
&H30 plus slot, adres en een RET neer te zetten.

Voorbeeld:

        RST  &H30
        DB   &H83
        DW   &H5A00

Hiermee  wordt  de  routine  op  adres  &H5A00  in slot  3-0 
aangeroepen. Dit was het overzicht van de BIOS routines voor 
geheugen schakeling, nu gaan we verder met ons "probleem".


             R O M / R A M   S C H A K E L E N 

Bij ML  programma's die vanuit BASIC worden opgestart is het 
vaak nodig om de MAIN ROM tijdelijk weg te schakelen en daar 
RAM neer te zetten, bij programma's die vanuit MSX-DOS of de 
bootsector  opstarten is het vaak nodig om het RAM tijdelijk 
weg te schakelen en daar RAM neer te zetten. Meestal moet de 
oude situatie later weer worden hersteld.

U kunt  dit het  beste doen  met de  routine ENASLT op adres 
&H0024. Met H geeft u op welke page u wilt schakelen en in A 
staat de slot ID byte.

Waar  haalt  u  nu  de juiste  slot ID  byte vandaan?  U zou 
natuurlijk een  routine kunnen  schrijven die het RAM zoekt, 
maar   gelukkig  heeft  het  besturingssysteem  van  de  MSX 
computer  dit  bij  het opstarten  al gedaan. Per page staat 
er in  het systeem RAM een slot ID byte die aangeeft in welk 
slot  het RAM  voor die page staan. U kunt deze RAM ID bytes 
vinden op de volgende adressen:

Page 0 (&H0000-&H3FFF)  Adres &HF341
Page 1 (&H4000-&H7FFF)  Adres &HF342
Page 2 (&H8000-&HFFFF)  Adres &HF343
Page 3 (&HC000-&HFFFF)  Adres &HF344

Normaal gesproken zijn deze slot ID bytes aan elkaar gelijk, 
maar voor de zekerheid is het natuurlijk beter om altijd het 
juiste  adres te gebruiken. De computer zoekt overigens naar 
de   grootste   mapper,   die   wordt   bij   het  opstarten 
geselecteerd.

Met  deze  gegevens  komen  we  tot  de  volgende  standaard 
routines.

(Nvdr.  De tekst  was langer  dan 16  kB, u  kunt het tweede 
gedeelte in het submenu vinden.)

                                                Stefan Boer



                    - TWEEDE GEDEELTE -

            G E H E U G E N   S C H A K E L E N 
                                                 


(Nvdr.  De tekst  was langer  dan 16  kB, u  kunt het eerste 
gedeelte in het submenu vinden.)


                       R A M   A A N 

Met deze routine selecteert u ROM op &H0000 (page 0):

        LD   A,(&HF341)
        LD   H,&H00
        CALL &H24

Met deze routine selecteert u RAM op &H4000 (page 1):

        LD   A,(&HF342)
        LD   H,&H40
        CALL &H24

Gebruik altijd  deze methode,  dan werkt het altijd. Bij het 
wegschakelen van de BIOS op page 0 moet u er wel voor zorgen 
dat de interrupts uit staan, of dat op adres &H0038 uw eigen 
interrupt  routine staat.  Anders slaat  de computer vrijwel 
zeker vast zodra er een interrupt optreedt.

Het  wegschakelen van  de MAIN  ROM op page 1 (hier staat de 
BASIC  interpreter)  zal in  een ML  programma meestal  geen 
problemen  geven,  er zullen  zelden routines  uit de  BASIC 
interpreter worden  gebruikt. Vergeet niet het MAIN ROM weer 
terug  te schakelen  voordat u  weer naar  BASIC terugkeert, 
anders slaat de computer natuurlijk vast.

Wilt  u toch  de routines in de BASIC interpreter gebruiken, 
dan kunt  u daarvoor  de BIOS routine CALBAS gebruiken. Deze 
routine staat op adres &H159, het adres van de routine die u 
wilt aanroepen moet in IX staan.


                       R O M   A A N 

En nu zijn we dan tot de kern van de zaak doorgedrongen: wat 
is de  juiste manier  om ROM  te selecteren  op page 0 en/of 
page 1.

Omdat  de MAIN  ROM zich  in een MSX computer altijd in slot 
0-0 of  slot 0  bevindt, is  er eigenlijk  geen adres in het 
systeem  RAM dat  u kunt uitlezen om de slot ID byte van het 
MAIN ROM te vinden.

Toch  kunt u hem wel uit het MAIN ROM halen. Op adres &HFCC1 
staat namelijk  een tabel  van 4 bytes, die per primair slot 
aangeeft  of het  slot is ge�xpandeerd of niet. Slot 0 staat 
op &HFCC1, slot 1 op &HFCC2, enz.

Deze  tabel heet  EXPTBL (EXPansion  TaBLe). Er zijn slechts 
twee waarden mogelijk: &H80 (wel ge�xpandeerd) en &H00 (niet 
ge�xpandeerd). Het  leuke is  nu dat  deze byte  voor slot 0 
precies het slot van de MAIN ROM aangeeft!

Is  slot  0  namelijk  ge�xpandeerd, dan  staat er  op adres 
&HFCC1 een  &H80, de  slot ID  byte van  slot 0-0. Is slot 0 
niet ge�xpandeerd, dan staat er op adres &HFCC1 een &H00, de 
slot ID byte voor slot 0!

De juiste routines om ROM te selecteren zijn dus:

ROM selecteren op &H0000 (page 0):

        LD   A,(&HFCC1)
        LD   H,&H00
        CALL &H24

ROM selecteren op &H4000 (page 1):

        LD   A,(&HFCC1)
        LD   H,&H40
        CALL &H24

Gebruik  altijd deze  methode, dan  werkt het altijd! Helaas 
zijn er  veel programmeurs  die het  niet zo  doen, hierover 
gaat de volgende paragraaf.


             H O E   H E T   N I E T   M O E T 

Een aantal  (Nederlandse) programmeurs  denken dat  op adres 
&HFCC0  de slot ID byte staat voor het MAIN ROM in page 0 en 
op adres &HFCC1 de slot ID byte voor het MAIN ROM in page 1. 
Dit is  klinkklare onzin!!! Op adres &HFCC1 staat de slot ID 
byte voor page 1 EN page 0!!!

Helaas zijn er daarom veel programmeurs die als volgt ROM op 
&H0000 selecteren:

        LD   A,(&HFCC0)
        LD   H,&H00
        CALL &H24

Dit  is dus echt helemaal fout!!! Doe dit dus nooit zo!!! Ik 
snap wel  hoe ze  hierbij komen,  het komt door CALSLT. Deze 
BIOS  routine wordt  vaak onder MSX-DOS gebruikt om een BIOS 
routine aan  te roepen.  Voor CALSLT moet de slot ID byte in 
de  HOOGSTE ACHT  BITS van  register IY  staan. Dit  kan als 
volgt worden gedaan:

        LD   IY,(&HFCC0)

Op  deze manier  worden de  laagste acht bits geladen met de 
waarde op &HFCC0 (die waarde doet er verder niet toe), en de 
hoogste acht  bits (waar  het om  draait) met  de waarde  op 
&HFCC1, precies  zoals de  bedoeling is. Hier komt misschien 
de verwarring met &HFCC0 vandaan.


                         B S A V E 

Dat het  echt onzin  is om &HFCC0 te gebruiken om de slot ID 
byte  van het MAIN ROM te achterhalen blijkt wel als we gaan 
kijken wat  er nu  eigenlijk wel  op &HFCC0  staat. Op adres 
&HFCBF  staat een 2 bytes waarde die SAVENT heet. Dit is een 
beetje vreemde  naam, want  dit adres  is het startadres bij 
BSAVE en BLOAD:

BSAVE"NAAM.BIN",beginadres,eindadres,startadres

Dit adres staat dus op &HFCBF en &HFCC0, &HFCC0 bevat dus de 
high byte  van het  BSAVE start adres!!! Misschien helpt een 
voorbeeldje hier:

BLOAD"NAAM.BIN",R

is precies hetzelfde als

BLOAD"NAAM.BIN"
DEFUSR=PEEK(&HFCBF)+256*PEEK(&HFCC0)
U=USR(0)

U begrijpt  dus wel dat elke programmeur die &HFCC0 gebruikt 
om  de slot  ID byte  te achterhalen  niet helemaal goed bij 
zijn hoofd  is, want  de waarde  die daar  staat is volkomen 
onvoorspelbaar!!!


         H O E   H E T   O O K   N I E T   M O E T 

Een andere methode die ik vaak zie is de volgende, het maakt 
hierbij niet uit of het om page 0 of page 1 gaat:

        XOR  A
        LD   H,&H40     ; &H00 voor page 0
        CALL &H24

Zoals  u weet is XOR A hetzelfde als LD A,0, voor de slot ID 
byte wordt  dus 0  genomen. Bij  computers waar  slot 0 niet 
ge�xpandeerd  is geeft  dit natuurlijk  geen problemen, maar 
als slot 0 wel ge�xpandeerd kan dit problemen geven.

Dit  komt omdat  de routine  op &H24  naar bit 7 kijkt om te 
kijken of het secundaire slot moet worden veranderd of niet.

Stel  u heeft  een computer  waarvan slot  0 is ge�xpandeerd 
(bijvoorbeeld een turbo R), en u heeft slot 3-3 geselecteerd 
op &H4000 (page 1). Nu doet u

        XOR  A
        LD   H,&H40
        CALL &H24
 
om  het ROM  te selecteren  op &H4000.  Dit gaat  nu mis! De 
routine op  &H24 kijkt  immers naar  bit 7, en die is gelijk 
aan  0. Het  gevolg is  dat slot 0-3 wordt geselecteerd, dit 
uiteraard met onbekende gevolgen (meestal een vastloper).


              H E T   F S T   P R O B L E E M 

U raadt  het al: de replay routine van FAC Soundtracker doet 
het  niet  altijd  goed  omdat  hij  het  geheugen  verkeerd 
schakelt. Om precies te zijn schakelt FAC Soundtracker op de 
volgende manier ROM op &H0000:

        LD   A,(&HFCC0)
        LD   H,&H40
        CALL &H24

Zoals  u  weet  bevat  &HFCC0  de  high-byte  van  het BSAVE 
startadres, de waarde op dit adres is dus onvoorspelbaar. Er 
wordt  op   deze  manier   dus  naar  een  willekeurig  slot 
geschakeld, met onbekende gevolgen.


 W A A R O M   W E R K T   H E T   M E E S T A L   W E L ? 

De  oplettende lezertjes vragen zich nu natuurlijk af waarom 
FST.BIN onder  BASIC wel altijd werkt. Dit wordt veroorzaakt 
door toeval, oftewel ontzettende mazzel voor de FAC.

Zoals  u weet  staat de  MAIN ROM  altijd in  slot 0 of 0-0, 
waarvan de  slot ID  bytes resp.  &H00 en &H80 zijn. &H80 is 
echter  altijd goed,  want als  slot 0  niet ge�xpandeerd is 
maakt het  tenslotte niet uit of het subslot wordt veranderd 
of niet.

Blijkbaar staat  er dus bij gebruik van FST.BIN vanuit BASIC 
altijd  &H80 op adres &HFCC0, anders zou de routine dus niet 
op alle computers werken! Hoe komt die &H80 daar?

Zoals u weet zijn zowel de drumkits als de muziekjes van FAC 
Soundtracker binaire files van 16 kB. Dit zijn files van het 
volgende type:

BSAVE"FILENAAM.EXT",&H8000,&HBFFF,&H8000

Het  startadres is  dus &H8000,  de high  byte daarvan &H80. 
Door  het  BLOADen  van  Soundtracker muziekjes  of drumkits 
wordt dus  automatisch de  waarde &H80 op adres &HFCC0 gezet 
(en  de waarde  &H00 op  &HFCBF, maar  dat doet er hier niet 
toe). En  dat is  nu precies  de waarde die altijd werkt als 
slot  ID  byte  voor het  MAIN ROM!!!  Kortom, de  FAC heeft 
ontzettende mazzel gehad.


                     D E   M O R A A L 

Ik  hoop  dat  alle  Nederlandse  programmeurs hier  wat van 
hebben   geleerd,  en  voortaan  de  enige  echte  standaard 
routines gebruiken  om het  geheugen te schakelen. Ik zal ze 
hier voor de zekerheid nog maar een keer geven:

RAM000: LD   A,(&HF341)
        LD   H,&H00
        CALL &H24

RAM400: LD   A,(&HF342)
        LD   H,&H40
        CALL &H24

ROM000: LD   A,(&HFCC1)
        LD   H,&H00
        CALL &H24

ROM400: LD   A,(&HFCC1)
        LD   H,&H40
        CALL &H24

De  FAC is  echt niet  de enige  die dit  fout doet, er zijn 
helaas  veel  meer  Nederlandse  MSX  programmeurs  die  dit 
verkeerd doen.  Ik hoop  dat dit artikel daar verandering in 
gaat brengen...

                                                Stefan Boer
