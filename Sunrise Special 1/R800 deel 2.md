- DEEL 2 -

D E   R 8 0 0


(Nvdr. Deel 1 en 3 kunt u in het submenu vinden.)


U I T L E G   B I J   T A B E L L E N

Voor degenen die nog nooit een instructietabel hebben gezien
is een  beetje uitleg  wel op z'n plaats. In de eerste kolom
staat  de mnemonic, de "syntax" van de instructie. Daarnaast
een symbolische  notatie van de werking van deze instruktie.
In  de volgende kolom wordt aangegeven hoe de vlaggen worden
be�nvloedt. Daarvoor worden de volgende tekens gebruikt:

stip    vlag wordt niet be�nvloedt
pijltje stand van vlag wordt bepaald door de uitkomst van
de instructie
0       vlag staat op 0 na instructie
1       vlag staat op 1 na instructie
?       vlag onbekend
P       P/V vlag geeft pariteit
V       P/V vlag geeft overflow

In de  volgende kolommen  wordt de opcode gegeven, in binair
en  in hexadecimaal. De kolom "B" geeft aan hoeveel bytes de
instructie in beslag neemt, de kolom "C" het aantal M-cycles
(C is  dus een afkorting van Cycles). Zoals we eerder hebben
vastgesteld  is dit  altijd gelijk  aan het aantal T-states,
maar het aantal klokpulsen kan een paar meer zijn.

Verder  worden  nog  een  heleboel  symbolen  gebruikt.  Een
overzicht:

a{7}            bit 7 van register a
a{4..7}         bit 4 t/m 7 van register a
de:hl           een 32 bits waarde, waarbij DE de hoogste
16 bits bevat
(ix+d)          d is de 8 bits offset
C               carry vlag
Z               zero vlag
P/V             parity/overflow vlag
S               sign vlag
N               subtract vlag
H               half-carry vlag
IFF             interrupt flipflop
r,r'            A,B,C,D,E,H,L
u,u'            A,B,C,D,E,IXH,IXL
v,v'            A,B,C,D,E,IYH,IYL
p               IXH,IYL
q               IYH,IYL
ss              BC,DE,HL,SP
pp              BC,DE,IX,SP
rr              BC,DE,IY,SP
qq              BC,DE,HL,AF
e               offset bij SHORT BR (JR bij Z80)
k               adres bij BRK (RST bij Z80)
nn              16 bits constante
n               8 bits constante
b               bitnummer (0-7)
NOT             logische bewerking
tmp             tijdelijke opslag van een bit
B               aantal bytes
C               aantal (M) cycles

Voor  de  logische  bewerkingen  OR,  XOR  en AND  worden de
wiskundige  notaties  gebruikt  (OR  is  een  "V",  XOR  een
"omgekeerde A" en AND een "omgekeerde V").


O P C O D E S   S A M E N S T E L L E N

De  instructietabellen zouden  wel erg  lang worden  als per
mogelijke instructie  de opcode  zou zijn vermeld. Bovendien
is  dan de logica in de opcodes niet meer uit de tabel op te
maken. Vaak  moet de  opcode dan ook (op binaire wijze) zelf
worden  samengesteld.  Met  een voorbeeld  kan ik  het beste
laten zien hoe dat gaat.

U  wilt  bijvoorbeeld  de  opcode van  de instructie  LD C,H
weten. Als u de instructietabellen in de Sunrise Times erbij
pakt, kunt u daar LD r,r' vinden. In de kolom opcode staat:

7   6   5   4   3   2   1   0    <-- bitnummers
0   1       r           r'       <-- opcode

Onderaan ziet  u de  tabel voor r en r'. U ziet daar dat 001
staat  voor C  en 100  voor H.  U vervang  nu r  en r' in de
binaire opcode door de gevonden codes voor C en H. De opcode
wordt dan:

7   6   5   4   3   2   1   0    <-- bitnummers
0   1   0   0   1   1   0   0    <-- opcode

U heeft de opcode gevonden, het is &B01001100.


N I E U W E   I N S T R U C T I E S

De R800  kent ten  opzichte van  de Z80  drie geheel  nieuwe
instructies,  bovendien zijn de indexregisters (IX en IY) nu
ook   per    8   bits    aanspreekbaar.   U   kunt   in   de
instructietabellen   opzoeken  bij   welke  instructies  dat
mogelijk is, te herkennen aan u, v, p of q.

De andere nieuwe instructies zijn:

IN F,(C)
MULUB A,r
MULUW HL,ss

De eerste  instructie is  erg handig. Routines waarbij (een)
bepaald(e)  bit(s) in  de gaten  wordt gehouden, worden door
deze  instructies   korter,  sneller   en  gebruiken  minder
registers.  Een voorbeeld: stel u moet bit 0 van I/O poort C
in de  gaten houden.  De routine moet net zolang wachten tot
bit 0 op 0 staat. Op de Z80 zou dat er als volgt uit zien:

WACHT:  IN      A,(C)
BIT     0,A
JR      NZ,WACHT

Dit zijn 6 bytes en het A-register wordt gebruikt.

Op de R800 kan het ook zo:

WACHT:  IN      F,(C)
JR      C,WACHT

Dit  zijn  4 bytes  en het  A-register wordt  niet gebruikt.
Bovendien is deze manier sneller. (N.B. Het carry bit is bit
0 van het F-register.)


V E R M E N I G V U L D I G E N !

Echt  super  zijn  de  vermenigvuldigingsinstructies van  de
R800. De instructie MULUB (MULtiply Bytes) vermenigvuldigt A
met B, C, D of E en zet het 16 bits antwoord in HL. Dit gaat
razendsnel, deze  instructie gebruikt  slechts 14 klokpulsen
(1.96 micro- seconde).

De instructie  MULUW (MULtiply Words) vermenigvuldigt HL met
BC  of SP  (zal in  de praktijk meestal BC zijn), en zet het
antwoord in  DE en HL, waarbij de hoogste bits in DE terecht
komen.  Deze instructie  duurt slechts 5.03 microseconde (36
klokpulsen)!

Deze twee instructies zijn ontzettend krachtig en maken zeer
snelle    "realtime    calculated"    animaties    mogelijk,
bijvoorbeeld vectorgraphics.  Ook voor  fractals is  dit erg
handig, die kunnen nu veel en veel sneller worden berekend.

Een leuke  vergelijking met  de Z80  is dat  de MULUW  HL,BC
instructie  op  de  R800  net  zolang  duurt  als  een  CALL
instructie op de Z80!! Bij de Z80 moet voor vermenigvuldigen
een  aparte routine worden geschreven, en als je die met een
CALL aanroept  is de  R800 in  de tussentijd al klaar met de
complete  vermenigvuldiging. U  begrijpt wel dat de R800 als
het om vermenigvuldigen gaat veel meer dan acht maal sneller
is, ik  denk wel  honderd maal sneller. Bovendien spaart het
een  hoop programmeertijd, want berekeningen kunnen een stuk
makkelijker worden geprogrammeerd.

Onderstaand  voorbeeld is  een routine die HL=A*B uitrekent.
Op de  R800 is ��n instructie daarvoor voldoende: MULUB A,B.
Deze  instructie is  sneller dan  een CALL instructie bij de
Z80! Bij de Z80 zou je dan ongeveer zo'n routine krijgen:

; MULUB A,B voor de Z80
; Door Stefan Boer
; Voor Sunrise Special #1
; Werking:  HL=A*B
; In:       A,B
; Uit:      HL
; Gebruikt: DE
; Methode:  Herhaald optellen

MULUB:  LD    HL,0
LD    E,A
LD    D,0             ; LD DE,A
XOR   A
CP    B
RET   Z               ; B=0, dan antwoord 0
MULTI:  ADD   HL,DE
DJNZ  MULTI           ; tel B maal DE bij HL op
RET

U  kunt zien  dat dit  heel wat meer bytes en tijd in beslag
neemt, de  CALL MULUB duurt op de Z80 al langer dan de MULUB
A,B instructie op de R800.


R 8 0 0   A S S E M B L E R ?

Nu  denkt u misschien dat u een nieuwe assembler nodig heeft
als u  software voor  de R800 wilt ontwikkelen. Dit is zeker
niet  het geval! Alle bestaande Z80 assemblers kunnen gewoon
worden gebruikt.  Dit is  ook de  reden waarom  er nog  geen
speciale R800 assembler op de markt is gebracht.

Het  zou trouwens  ook verwarrend  zijn voor een programmeur
die de Z80 assemblertaal al beheerst, om de nieuwe mnemonics
voor de R800 te leren.

"En de  nieuwe instructies  dan?" hoor  ik u denken. Nu, die
kunt  u  gewoon  met DEFB  instructies simuleren!  Alleen de
disassembler  zal hier  een beetje van in de war raken, maar
dat  is  toch niet  zo'n ramp?  Ik zal  nu voor  alle nieuwe
instructies uitleggen  hoe ze  met een  gewone Z80 assembler
(zoals WB-ASS2) kunnen worden gebruikt.


I N D E X R E G I S T E R S

Bij  de indexregisters IX en IY is het ontzettend makkelijk.
Het  is  u  vast  al  wel  eens opgevallen,  dat de  opcodes
hetzelfde zijn  als die voor de instructies voor HL, behalve
dan  dat er een extra code voor staat. Die code is &HDD voor
IX en &HFD voor IY. Een voorbeeld:

Mnemonic:       Opcode:

LD HL,&H1BBF    21 BF 1B
LD IX,&H1BBF    DD 21 BF 1B
LD IY,&H1BBF    FD 21 BF 1B

U kunt  voor alle  instructies met  IXL, IXH,  IYL en IYH de
assembler  op een heel eenvoudige manier voor de gek houden.
Voor  IXL en  IYL zet u een L en voor IYH en IXH een H. Door
er een  DEFB &HDD  voor te zetten wordt de L resp. H voor de
R800  IXL  resp.  IXH,  met  een  &HFD  hetzelfde  voor  IY.
Voorbeelden:

DEFB    &HDD
LD      A,L             ; LD A,IXL

DEFB    &HFD
LD      L,H             ; LD IYL,IYH

DEFB    &HDD
LD      H,B             ; LD IXH,B

Zoals  u ziet  is de  instructie LD IXH,IYL niet mogelijk, u
kunt immers niet de code voor IX �n de code voor IY voor een
instructie plaatsen.  Maar verder  is alles mogelijk, zie de
instructietabellen in de Sunrise Times.


I N   F , ( C  )

Voor  deze instructie kunt u gewoon DEFB &HED,&H70 invullen.
U kunt dat het beste zo in de assemblerlisting zetten:

DEFB    &HED,&H70       ; IN F,(C)

Zo  kunt  u  goed  zien dat  het geen  data bytes  zijn maar
eigenlijk  gewoon   een  opcode.  Door  een  foutje  in  die
assemblers   zullen   sommige   assemblers   dit   overigens
disassembleren  als IN  (HL),(C). Dit  is een niet bestaande
instructie, maar het komt omdat IN F,(C) voor de R800 gewoon
een speciaal  geval is  van IN r,(C). Bij andere instructies
(bijvoorbeeld   LD  A,(HL))   wordt  (HL)  weergegeven  door
dezelfde bits  als F  bij de IN F,(C) instructie, vandaar de
fout.


M U L U B   E N   M U L U W

Voor  deze instructies  kunt u  de instructietabellen van de
R800 die  in de  Sunrise Times  staan afgedrukt gebruiken. U
kunt  de opcodes  aan de  hand daarvan samenstellen (zie het
stukje   over   opcodes  samenstellen),   en  ze   met  DEFB
instructies in de code opnemen. Bijvoorbeeld:

DEFB    &HED,&B11000001 ; MULUB A,B

Ook hier  is het  weer aan te raden om bij het commentaar de
R800  mnemonic in te vullen, zo houdt u uw assemblerlistings
leesbaar.

(Nvdr.  Dit artikel  was langer dan 32 kB, en is dus in drie
delen gesplitst. Deel 3 kunt u in het submenu vinden.)

                        Stefan Boer
