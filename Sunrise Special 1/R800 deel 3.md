- DEEL 3 -

D E   R 8 0 0


(Nvdr. Deel 1 en 2 kunt u in het submenu vinden.)

S C H A K E L E N

Nu  zit ik  wel een  lang artikel over de R800 te schrijven,
maar  als  u  niet  weet  hoe  u  deze  wonderprocessor moet
selecteren heeft u daar nog niet veel aan.

In het  MAIN ROM  van de MSX turbo R zitten vier nieuwe BIOS
routines,  waarvan er  twee voor het schakelen tussen de Z80
en de R800 zijn bedoeld. Dat zijn:

CHGCPU  &H0180  Selecteer CPU
Invoer: A register
MSB   7   6   5   4   3   2   1   0   LSB
LED  0   0   0   0   0   X   X
LED=1: led wordt bestuurd
LED=0: led wordt niet bestuurd
XX=00: Z80
XX=01: R800 ROM
XX=10: R800 DRAM
Gebruikt: AF

GETCPU  &H0183  Bepaal huidige CPU
Invoer: -
Uitvoer: A register
0: Z80
1: R800 ROM
2: R800 DRAM

Over   LED   is  wegens   foutieve  publikaties   in  andere
Nederlandse  MSX  bladen een  misverstand ontstaan.  Dit bit
bestuurt niet  het led  (1=aan, 0=uit),  maar bepaalt  of de
status  van het  led (aan  of uit)  door de gekozen CPU moet
worden be�nvloed!

Stel u  zit in  R800 mode en het lampje brandt. Doet u nu LD
A,0  en daarna CALL CHGCPU, dan zal de Z80 uitgaan, maar het
lampje blijft  branden. Bit 7 van register A (LED) is immers
0, en dus zal het led niet door de gekozen CPU (Z80, normaal
gesproken moet het lampje dus uit!) worden be�nvloed.

Andersom  kan ook,  als u  in Z80 mode zit (lampje uit) en u
selecteert de R800 mode door een CALL &H180 te geven terwijl
het A register de waarde 1 bevat (R800), dan zal de R800 wel
aangaan, maar het lampje niet!

Bij  "normaal"  gebruik van  deze BIOS  routine, waarbij  de
stand  van  het  lampje  in overeenkomst  is met  de actieve
processor  (aan=R800,  uit=Z80),  dient  u  dus  ��n van  de
volgende drie waardes te gebruiken:

&H80    Z80
&H81    R800 ROM
&H82    R800 DRAM

U  kunt  met  GETCPU  opvragen  welke  processor  er  op dat
ogenblik actief  is. U  kunt op  de volgende  manier dus het
lampje in de goede stand zetten:

CALL    &H183           ; welke CPU actief?
SET     7,A             ; LED besturen aan
CALL    &H180           ; CPU zelfde, LED besturen


 D R A M

Ik heb  het in  bovenstaande tekst vrolijk over DRAM, zonder
uit te leggen wat dat nou eigenlijk inhoudt. De DRAM mode is
een  slimme  truuk,  die ook  door andere  moderne computers
wordt  gebruikt. In  feite is  het niet  de R800 die voor de
DRAM mode zorgt, maar de S1990.

Bij  de  Z80  maakt  het  niet uit  of er  RAM of  ROM wordt
gelezen,  dit gaat  beiden even snel. Omdat er in de turbo R
dezelfde ROMs  worden gebruikt  als in een MSX1/2/2+, moeten
die  op een langzame snelheid worden gelezen. Telkens als de
R800  ROM  moet  lezen,  schakelt  hij  even terug  naar een
langzamere snelheid.  Vandaar dat  op een R800 het lezen van
RAM  sneller is dan ROM, want in de turbo R wordt "snel" RAM
gebruikt, zodat terugschakelen niet nodig is.

Vooral in  BASIC wordt  er continu  uit de  ROM gelezen.  De
BASIC  zelf staat  immers op ROM! De R800 ROM mode is daarom
in  BASIC  helemaal  niet  zo snel,  omdat er  telkens wordt
teruggeschakeld naar een lagere snelheid. De oplossing is de
DRAM mode.


R O M   I N   R A M

Bij  het  opstarten  van de  computer wordt  de 64  kB meest
gebruikte ROM naar de bovenste 64 kB van het RAM gekopieerd.
De  turbo R heeft dan dus 64 kB minder werkgeheugen over. De
ROM die gekopieerd wordt is:

MAXPGE-3:       MAIN ROM 0000-3FFF
MAXPGE-2:       MAIN ROM 4000-7FFF
MAXPGE-1:       SUB ROM
MAXPGE  :       KANJI ROM

Voor  MAXPGE   moet  u   het  hoogste  memorymapper  segment
invullen.  De FS-A1ST heeft bijvoorbeeld 256 kB, dat zijn 16
memorymapper segmenten  van 16  kB. Daar  het nummeren bij 0
begint, is MAXPGE dus gelijk aan 15. De SUB ROM staat dus in
memorymapper segment 14.

Als de  DRAM mode wordt geselecteerd komt de S1990 in actie.
Wordt  nu ��n van de hierboven genoemde ROMs gelezen door de
R800, dan  zorgt de S1990 ervoor dat niet de ROM maar de RAM
(waarin  het ROM is gekopieerd) wordt gelezen. De R800 hoeft
dan niet  te wachten  op het langzame ROM, en werkt een stuk
sneller.  De S1990  zorgt ervoor  dat er niet in de DRAM kan
worden geschreven  (zo heet  de tot  ROM verklaarde RAM), en
zorgt  er tevens  voor dat  het RAM  dat voor  de DRAM wordt
gebruikt niet  meer te gebruiken is. Probeert u er toch iets
uit te lezen, dan zal dat altijd de waarde &HFF opleveren.


R O M   M O D E   E N   D R A M   M O D E

Er zijn dus twee verschillende R800 standen. Bij de R800 ROM
mode  gaat het  net als bij de Z80 mode, er wordt gewoon ROM
gelezen. Vandaar de naam ROM mode. Deze mode is vooral onder
BASIC langzaam,  omdat daar  veel ROM  wordt gelezen.  Onder
machinetaal springt de computer ook 60 keer per seconde naar
ROM,  voor  de  interrupt  routine  op  &H38.  Ook  als  het
machinetaalprogramma  de BIOS  gebruikt zal  er naar het ROM
worden gesprongen.  Daarom heeft  het selecteren van de DRAM
mode  ook bij machinetaal zin, al is heeft het minder effect
dan  bij  BASIC. De  R800 ROM  mode heeft  als belangrijkste
voordeel  dat   het  volledige  RAM  geheugen  vrij  is  als
werkgeheugen.

De  DRAM mode  is sneller,  maar kost  wel 64 kB geheugen. U
kunt dus  zelf per situatie beslissen of u de ROM mode of de
DRAM mode wilt gebruiken.


I N   R O M   P O K E N !

U  kunt bij  de turbo  R als  het ware in het ROM POKEn. Dit
gaat als volgt:

- Selecteer de R800 ROM mode (of Z80 mode)

In de  bovenste 64 kB van de memorymapper staat nu een kopie
van  de ROM,  waar u  net zoveel in kunt knoeien als u wilt.
Als u klaar bent

- Selecteert u de R800 DRAM mode

Nu wordt  het RAM  waar u  zojuist in hebt geknoeid gebruikt
voor de DRAM, en dus hebt u als het ware in de ROM gePOKEt!


P A S   O P !

Je  zou misschien  denken dat  het ROM elke keer dat de DRAM
wordt geselecteerd  in het RAM wordt gekopieerd. Dat is niet
zo,  anders zou bovenstaande truuk ook niet werken. De reden
hiervoor is  dat het  kopi�ren van  64 kB toch wel even tijd
kost,  en  op  die  manier  zou  de  CHGCPU routine  veel te
langzaam worden.

Het ROM wordt alleen bij een reset in het RAM gekopieerd, en
hiermee dient  terdege rekening gehouden te worden! Als u de
DRAM  mode selecteert  terwijl het RAM dat voor de DRAM mode
wordt  gebruikt  verminkt is,  kan dit  onverwachte gevolgen
hebben!


D I S K G E B R U I K   E N   R 8 0 0

Het aansturen  van de  diskdrive gaat  niet goed  onder R800
stand.  De  diskcontroller  kan  bij  zo'n hoge  snelheid de
processor  blijkbaar  niet  bijhouden.  Tenminste,  bij  het
saven. Het laden onder R800 stand gaat prima, maar bij saven
kan er  van alles  mis gaan. Het gaat niet altijd fout, maar
vaak  treedt er  een Disk  I/O error  op of  er wordt gewoon
niets gesaved.

Bij  MSX-DOS2.31/MSX Disk  BASIC 2.01  is daar  rekening mee
gehouden. Bij elke diskactie wordt de R800 stiekem uitgezet.
Ik schrijf  "stiekem", omdat het lampje niet wordt uitgezet.
Dat blijft branden, omdat de R800 wordt uitgezet door

LD      A,0     
CALL    &H180           ; R800 uit, lampje niet

En  u hebt  aan het begin van het artikel al geleerd dat het
lampje dan  niet uitgaat.  Hierdoor wordt  behalve het saven
ook het laden vertraagd, wat eigenlijk niet nodig is.

De  ontwikkelaars  van  de  turbo  R hebben  dit in  de MSX-
DOS2.31/Disk  BASIC 2.01  ROMs ingebouwd,  omdat dat normaal
gesproken onder R800 stand wordt gedraaid.


D O S 1   M O D E

Onder DOS1  mode (MSX-DOS1.11/Disk BASIC 1.0) wordt dit niet
gedaan.  Die stand  is tenslotte  bedoeld voor het uitvoeren
van  MSX1/2/2+  programma's, en  die werken  toch onder  Z80
stand.

Toch zullen programmeurs vaak MSX turbo R software voor DOS1
mode  ontwikkelen,  omdat  de  geheugenhuishouding van  DOS2
nogal irritant is. De R800 wordt dan dus aangezet onder DOS1
mode, terwijl de diskROM daar geen rekening mee houdt.

In het  technical databook  van de turbo R staat dan ook dat
als  een programmeur  toch de combinatie DOS1/R800 gebruikt,
hij  bij   elke  diskactie  de  R800  uit  moet  zetten.  De
programmeur  van Seed  of Dragon  heeft zich daar netjes aan
gehouden; elke  keer als  het disklampje gaat branden zie je
bij dat spel het R800 lampje uitgaan.

Hoewel dit niet in het technical databook staat, kan de R800
bij het  laden rustig  aan blijven  staan. Alleen  bij saven
zijn  er problemen.  Onder R800  stand gaat  het laden zelfs
aanmerkelijk  sneller!  MicroCabin wist  dit blijkbaar  ook,
want bij bijvoorbeeld Fray wordt de R800 alleen uitgezet als
er wordt gesaved.

Conclusie:

Z E T   D E   R 8 0 0   A L T I J D   U I T   A L S

E R   I N   D O S 1   M O D E   N A A R   D I S K

W O R D T   G E S C H R E V E N ! ! !


Nogmaals: bij laden kunt u de R800 rustig aan laten staan.


P A G E   M O D E   A C C E S S

Bij  het bekijken van een artikel over de R800 in een Japans
blad kwam  ik de  term "DRAM  page access"  tegen, en ik had
eerst  geen flauw  idee wat  dat was.  Later kwam  ik er wel
achter.

Wanneer  van het  adres in  de externe  databus de high-byte
niet verandert,  zal de R800 de high-byte van het adres niet
opnieuw op de adresbus plaatsen (lijkt heel logisch, maar de
Z80 kent deze truuk niet!)

Een  "page" is in de processorwereld een stukje geheugen van
256 bytes,  dat op  een 256-byte  grens begint, dus &H??00 -
&H??FF.  Verwar dit niet met de bij MSX bekende term "page",
een stuk geheugen van 16 kB.

Het hierboven  beschreven truukje  dat de R800 gebruikt heet
"Page  mode access"  en verhoogt  de snelheid  nog eens  met
ongeveer een factor twee.

De programmeur kan met "paged DRAM access" maximaal van deze
truuk  gebruik maken.  Paged DRAM  access betekent  dat alle
geheugentoegang van een routine (dus ook de routine zelf) in
dezelfde page staat (bijvoorbeeld &HD000-&HD0FF). Dit is wel
zeer beperkt,  want met  alle geheugentoegang  bedoel ik dan
dus  echt ALLE  geheugentoegang, inclusief stack, variabelen
en constanten. Het uitzetten van de interrupts is in verband
met die  stack dan  ook ten  zeerste aan te raden. Maar deze
moeite wordt wel beloond met een ultra-snelle routine!

We  begrijpen  nu  ook beter  hoe het  komt dat  een LD  A,B
instruktie  op de  R800 tien  keer zo snel is dan op de Z80,
terwijl een LD A,(HL) maar 5,3 keer zo snel is.

Bij het  uitvoeren van een instructie hoort namelijk ook het
ophalen van de opcode uit het geheugen. Omdat bij een LD A,B
instructie  de externe  adresbus niet wordt gewijzigd, hoeft
bij het  ophalen van  de volgende opcode (meestal) alleen de
low-byte van de externe adresbus te worden veranderd. Bij de
LD  A,(HL) instructie  is de externe adresbus wel veranderd,
zodat zowel  de low-  als de  high-byte van  het PC register
(Program  Counter) naar  de externe  adresbus moeten  worden
verplaatst.


T O T   S L O T

Tot  slot   wil  ik   Alex  Wulms  nog  bedanken  voor  zijn
medewerking,  een gedeelte  van de informatie in dit artikel
is  van  hem afkomstig.  De overige  informatie heb  ik zelf
uitgevonden of komt uit Japanse bladen.

De R800  is (inclusief  toetsenbord, diskdrive,  kast en een
zooitje  chips onder  de naam  FS-A1GT) te koop bij MSX Club
Gouda voor  � 1795,-.  Verplicht voor elke MSX'er die bij de
tijd wil blijven (maar nu nog een antieke MSX2 heeft)!

                       Stefan Boer
