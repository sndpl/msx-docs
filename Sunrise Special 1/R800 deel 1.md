- DEEL 1 -

D E   R 8 0 0


I N L E I D I N G

Het hart  van de  MSX turbo  R wordt gevormd door een nieuwe
processor:  de R800.  In dit  artikel wordt  alles uitgelegd
over het  selecteren van de processor, de DRAM, kloksnelheid
en nog  veel meer.  In de  Sunrise Times vindt u de complete
instructietabellen  van  de  R800, die  nog nooit  eerder in
Nederland werden gepubliceerd!


T W E E   P R O C E S S O R S

De  turbo R bezit twee microprocessors, een Z80 (een Z80A om
precies te zijn) en een R800. Het is dus niet zo dat tijdens
de  Z80   mode  gewoon   de  R800  wordt  vertraagd,  er  is
daadwerkelijk een andere processor actief.

De   R800  is  upwards  compatible  met de  Z80, en  kan dus
alle Z80  instructies verwerken.  Maar de R800 kan meer. Ten
eerste  kan hij  de instructies  gemiddeld acht keer sneller
uitvoeren,  en ten  tweede heeft  de R800  een aantal nieuwe
instructies. Om  het verschil  in snelheid  beter te  kunnen
uitleggen ga ik u eerst uitleggen wat de term "kloksnelheid"
nu eigenlijk precies betekent.


K L O K S N E L H E I D

Dit  zal u wel een beetje vreemd in de oren klinken, maar de
kloksnelheid  zegt   eigenlijk  weinig  over  de  werkelijke
snelheid van de microprocessor, en nog minder over de totale
snelheid van de computer.

De  Z80A heeft  een kloksnelheid  van 3.579.545  Hz (meestal
afgekort tot 3.58 MHz). Dit betekent dat de Z80A per seconde
3.579.545 (ruim  3,5 miljoen!) klokpulsen genereert. Tijdens
zo'n  klokpuls  kan  de processor  een bepaalde  hoeveelheid
taken  verrichten. En  de truuk  is nu, dat deze hoeveelheid
taken per processor niet gelijk is!

Daarom kan  een processor  met een  lagere kloksnelheid toch
sneller  zijn. Een  voorbeeld: een R800 die draait op 2 MHz,
is  sneller  dan  een  Z80 op  3.58 MHz!  Zo ziet  u dat  de
kloksnelheid  slechts  weinig zegt.  Alleen bij  processoren
waarvan  alleen   de  kloksnelheid  verschilt,  is  dat  een
duidelijke  maatstaf voor  de snelheid. Een Z80B die op 7.16
MHz draait  is uiteraard  wel twee keer zo snel als een Z80A
die  op 3.58  MHz draait,  omdat het  hier om dezelfde soort
processor gaat.

Voor de snelheid van de totale computer zegt de kloksnelheid
zoals gezegd  nog minder.  Daar is  namelijk ook de snelheid
van  bijvoorbeeld de  videochip, en het geheugen van belang.
We kunnen  stellen dat  een gemiddeld  programma op  een MSX
turbo R vijf � zes keer sneller is dan op een MSX2.


D E   K L O K S N E L H E I D   V A N   D E   R 8 0 0

De kloksnelheid  van de  R800 (dus het aantal klokpulsen van
de  R800 per seconde), is 7.159.090 Hz, oftewel precies twee
maal de kloksnelheid van de Z80A. Dit wordt meestal 7.16 MHz
genoemd. Dit  betekent overigens dat een klokpuls 1 seconde/
7159090 = 140 ns (nanoseconde, miljardste seconde) duurt.

Waarom  lees je  dan overal  (zelfs in  de offici�le Japanse
handboeken van Panasonic!) dat de klokfrequentie van de R800
28.636.360 Hz  (28.6 MHz) bedraagt? Dit lijkt onzin, maar is
toch ook weer niet zo heel vreemd.

Als  men zou  schrijven dat de R800 een klokfrequentie heeft
van 7.16  MHz (de  waarheid!), dan  zouden de meeste MSX'ers
waarschijnlijk  denken dat  deze CPU  dus net zo snel is als
een 7.16 MHz Z80. En dat is niet zo, met zuiver rekenwerk is
de R800  gemiddeld (!)  acht keer  sneller dan  een 3.58 MHz
Z80. En zo komt men aan 28.636.360 Hz, dat is namelijk exact
8 maal 3.579.545 Hz!


H O E   K A N   D A T ?

Hoe  kan het  dat de  R800 acht  maal zo snel is als de Z80,
maar toch  maar een twee keer zo hoge kloksnelheid heeft? De
oplettende lezertjes weten het antwoord al: de R800 kan meer
doen in ��n klokpuls dan de Z80! Een voorbeeld:

De  instructie LD A,B neemt op de zowel de Z80 als de R800 1
zogenaamde "machine  cycle" (M-cycle) in beslag. Bij de R800
duurt een M-cycle ook ��n T-State, maar bij de Z80 duurt die
M-cycle maar liefst vier T-States.

Het aantal klokpulsen is gelijk aan het aantal T-States plus
eventuele  extra klokpulsen.  Bij de  Z80 is  dat er  altijd
minimaal  1  (voor  de  refresh), bij  de R800  zijn er  een
heleboel  instructies  waarbij geen  extra klokpulsen  nodig
zijn (de refresh vindt daar niet bij elke instructie plaats,
maar ongeveer   32 keer   per seconde).

Bij  deze instructie  (LD A,B)  is er  bij de  Z80 een extra
klokpuls nodig,  waardoor het  totaal op  vijf komt,  bij de
R800 is het gewoon ��n klokpuls. Hierdoor is de R800 (bij de
instructie LD A,B) dus vijf keer zo snel als de Z80.

Bovendien is de klokfrequentie van de R800 twee keer zo hoog
(zoals we al eerder hebben vastgesteld). De instructie wordt
op  de  R800  dus  twee  maal  vijf  is  TIEN  keer zo  snel
uitgevoerd!  Even narekenen:  een klokpuls duurt bij de R800
140 ns, en bij de Z80 280 ns. We berekenen nu hoe lang beide
processoren doen  over het  uitvoeren van  de instructie  LD
A,B:

Z80:    5 klokpulsen * 280 ns = 1397   ns
R800:   1 klokpuls   * 140 ns =  139,7 ns

Dit is dus inderdaad precies tien maal zo snel!

Een overzicht van een aantala andere instructies:

Instructie:     Z80:            R800:           Versnelling:
------------------------------------------------------------
LD A,B          5       (1,1)   1       (1,0)   10
LD A,(HL)       8       (2,1)   3       (2,1)   5,3
LD A,(IX+0)     21      (5,2)   5       (5,0)   8,4
PUSH HL         12      (3,1)   4       (4,0)   6
LDIR (BC<>0)    23      (5,2)   7       (4,3)   6,6
ADD A,B         5       (1,1)   1       (1,0)   10
INC A           5       (1,1)   1       (1,0)   10
ADD HL,DE       12      (3,1)   1       (1,0)   24 (!!!)
INC HL          7       (1,1)   1       (1,0)   14
JP              11      (3,1)   3       (3,0)   7,3
JR              13      (3,1)   3       (3,0)   8,7
DJNZ (B<>0)     14      (3,1)   3       (2,1)   9,3
CALL            18      (5,1)   6       (5,1)   6
RET             11      (3,1)   4       (3,1)   5,5
------------------------------------------------------------
Gemiddeld                                       9,36

Zoals  u ziet is de gemiddelde versnelling in deze tabel dus
9,36  maal,   maar  u  moet  niet  vergeten  dat  niet  elke
instructie   evenveel  voorkomt   in  een  routine,  dus  de
versnelling zal  van routine tot routine verschillen. Het is
in   ieder  geval  zeer  onwaarschijnlijk  dat  bovenstaande
instructies (en  soortgelijke, want  LD D,E duurt natuurlijk
net  zo lang  als LD  A,B) allemaal  evenveel voorkomen.  De
totale versnelling  van een programma is ook nog afhankelijk
van  andere  zaken,  zoals bijvoorbeeld  de snelheid  van de
videochip (die overigens ongewijzigd is t.o.v. MSX2/2+!).

Hoe  moet  u  bovenstaande tabel  nu lezen?  Links staat  de
mnemonic, in Z80 formaat. Daarnaast het aantal klokpulsen op
de  Z80. De tijd die zo'n instructie vergt kunt u uitrekenen
door dit  getal te  delen door  de kloksnelheid  van de Z80.
Daarnaast  staan twee  getallen tussen  haakjes. Het  eerste
getal is  het aantal  M-cycles, het  tweede getal het aantal
extra klokpulsen. U kunt het aantal T-states dus vinden door
van het opgegeven aantal klokpulsen dat getal af te trekken.

De  volgende kolom  geeft het aantal klokpulsen van dezelfde
instructie op  de R800.  De tijd kunt u weer uitrekenen door
dit  getal te delen door de kloksnelheid van de R800 (wel de
goede nemen  natuurlijk). Het eerste getal tussen haakjes is
het  aantal M-cycles.  Het aantal  T-states is  bij de  R800
altijd  gelijk  aan het  aantal M-cycles!  Het tweede  getal
geeft aan hoeveel extra klokpulsen er zijn. Een voorbeeld:

DJNZ (B<>0)     14      (3,1)   3       (2,1)   9,3

Dit moet als volgt worden gelezen:

Z80:                    R800:
-----------------------------------------------------------
Klokpulsen:     14                      3
Tijd:           3.91 microseconde       0.42 microseconde
M-cycles:       3                       2
T-states:       13 (14-1)               2 (3-1)
Extra pulsen:   1                       1
Versnelling:                    9,3
------------------------------------------------------------

Over  het algemeen  is het  aantal M-cycles  op de Z80 en de
R800 gelijk,  en komt  de versnelling vooral doordat de R800
maar  ��n T-state  nodig heeft per M-cycle, en dit er bij de
Z80 meestal  3 �  4 zijn.  Bovendien wordt  er soms  ook nog
"bezuiningd" op de extra klokpulsen.


1 6   B I T

De meest  opvallende versnelling  is toch  wel de  ADD HL,DE
instruktie,  die  op  de R800  maar liefst  24 keer  sneller
wordt.  Deze instructie is vooral sneller doordat het aantal
M-cycles drastisch  is verminderd van 3 naar 1. Hoe kan dat?
Bij de meeste instructies is het aantal M-cycles bij de R800
en de Z80 gelijk, zoals eerder opgemerkt.

Dit  komt  door  de  R800  zoveel  mogelijk (!)  16 bits  is
gemaakt. In  Japan wordt  er ook geadverteerd dat de R800 16
bit  is. Toch  is de  R800 niet  helemaal 16 bit, de externe
adresbus  is  namelijk  nog  steeds acht  bits. De  hele MSX
computer is  acht bits,  en als  je een processor met een 16
bits externe adresbus in een MSX zou plaatsen, dan lijkt het
niet  veel  meer  op  een  MSX!  Er  zouden dan  veel (dure)
aanpassingen nodig zijn.

Vandaar dat men de externe adresbus ongemoeid heeft gelaten.
Maar intern  is de  R800 wel 16 bits, en daardoor gaan de 16
bits instructies (zoals ADD HL,DE) zo ontzettend snel!


D E   I N S T R U C T I E T A B E L L E N

In de Sunrise Times staan de complete instructietabellen van
de  R800.  Dit  is  de  eerste  keer  dat  deze  tabellen in
Nederland worden gepubliceerd!!

Als  u  de instructietabellen  bekijkt, zult  u zien  dat de
meeste  mnemonics   zijn  veranderd.   De  instructies  zijn
hetzelfde  gebleven, maar  de namen zijn gemoderniseerd. Een
overzicht van de instructies die een naamsverandering hebben
ondergaan:

R800: (nieuwe naam)             Z80: (oude naam)
------------------------------------------------------------
XCH                             EX
XCHX                            EXX
MOVE (HL++),(DE++)              LDI
MOVE (HL--),(DE--)              LDD
MOVEM (HL++),(DE++)             LDIR
MOVEM (HL--),(DE--)             LDDR
CMP A,(HL++)                    CPI
CMP A,(HL--)                    CPD
CMPM A,(HL++)                   CPIR
CMPM A,(HL--)                   CPDR
ADDC                            ADC
SUBC                            SBC
AND A,                          AND
OR A,                           OR
XOR A,                          XOR
CMP A,                          CP
CLR                             RES
ROLA                            RLCA
RORA                            RRCA
ROLCA                           RLA
RORCA                           RRA
ROL                             RLC
ROR                             RRC
ROLC                            RL
RORC                            RR
ROL4 (HL)                       RLD
ROR4 (HL)                       RRD
SHL                             SLA
SHR                             SRL
SHRA                            SRA
BR                              JP
BNZ                             JP NZ,
BZ                              JP Z,
BNC                             JP NC,
BC                              JP C,
BPO                             JP PO,
BPE                             JP PE,
BP                              JP P,
BM                              JP M,
SHORT BR                        JR
SHORT BNZ                       JR NZ,
SHORT BZ                        JR Z,
SHORT BNC                       JR NC,
SHORT BC                        JR C,
DBNZ                            DJNZ
BRK                             RST
IN (HL++),(C)                   INI
IN (HL--),(C)                   IND
INM (HL++),(C)                  INIR
INM (HL--),(C)                  INDR
OUT (C),(HL++)                  OUTI
OUT (C),(HL--)                  OUTD
OUTM (C),(HL++)                 OTIR
OUTM (C),(HL--)                 OTDR
ADJ A                           DAA
NOT A                           CPL
NEG A                           NEG
NOTC                            CCF
SETC                            SCF
------------------------------------------------------------

(Nvdr. Deze tekst was langer dan 32 kB, en moest dus in drie
gedeelten  worden  gesplitst.  Deel  2  en 3  kunt u  in het
submenu vinden.)

                       Stefan Boer
