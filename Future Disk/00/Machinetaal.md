Machinetaal

Hoe  werkt  een  computer  ?  De
basis van de computer  ligt  bij
stroom. De computer herkent maar
twee situaties: Wel spanning  of
geen spanning. Hiervan is de bit
afgeleid. Een bit  is  de  basis
van alle computers. In	een  bit
kan  de  waarde  1  of	0  staan
respectivelijk	 wel   of   geen
spanning.  Acht   bits	 tezamen
vormen een byte.  Een  byte  kan
bijvoorbeeld zijn:  00000001
Om de waarde van een byte te
bepalen heeft men de machten van
2 nodig. In  de  voorbeeld  byte
staat namelijk:
1*2  tot   de  macht  0  =   1
0*2  tot   de  macht  1  =   0
0*2  tot   de  macht  2  =   0
etc.
Zou er nu in deze byte 10110011
hebben gestaan wordt dit:
1*2 tot de macht 0 = 1
1*2 tot de macht 1 = 2
0*2 tot de macht 2 = 0
0*2 tot de macht 3 = 0
1*2 tot de macht 4 = 16
1*2 tot de macht 5 = 32
0*2 tot de macht 6 = 0
1*2 tot de macht 7 = 128
Alles opgeteld wordt dit
1+2+0+0+16+32+0+128=179.
De hoogste waarde die een byte
kan bevatten  is  dus:
1*2  tot  de macht 8 min 1 = 255
Waarom min 1 ? Simpel, de
computer ziet  de nul ook als
een heel  getal. De Z80 is  een
8-bits	processor, d.w.z. dat
hij  8-bits  tegelijk  aan kan
sturen. De Z80	begrijpt alleen
maar nulletjes en eentjes, dus
worden alle commando's terug-
gezet naar bytes en bits.
Ook de BASIC commando's !!
Men noemt deze	nulletjes  en
eentjes ook wel  de  MCODE  (wat
staat voor  machinecode).  Omdat
nulletjes en  eentjes  lezen  en
programmeren  heel  moeilijk  is
hebben	 de    ontwerpers    een
speciale  taal	ontwikkeld,   de
machinetaal. Machinetaal bestaat
uit MNEMONICS  (geen  drukvaut).
Dit zijn commando's die door ons
mensen een stuk gemakkelijker te
begrijpen zijn, maar de computer
snapt er nog steeds  niets  van.
Pas  als  deze	mnemonics   zijn
vertaald  naar	Mcode	kan   de
computer er iets mee  doen.  Dit
vertalen  doet	een   zogenaamde
assembler.   Wijzelf   gebruiken
Wbass 2, maar  er  zijn  er  nog
vele andere  assemblers.  Om  de
rest van  de  cursus  te  kunnen
volgen	  is	een    assembler
onmisbaar. We  beginnen  nu  met
een eerste machinetaalprogramma.
De   begrippen	 die   we   niet
behandeld hebben  zullen  in  de
loop van de  volgende  cursussen
worden uitgelegd.

ORG &HC000
LD A,10
LD B,20
ADD A,B
RET

Wat doet dit programma nu ?
ORG &HC000: Dit is een
assemblercommando dat aangeeft
op   welk   geheugenadres    het
programma moet	beginnen.    In
dit geval dus op  adres  &hc000.
LD  A,10  :  Om  dit   goed   te
begrijpen eerst wat  algemene
informatie over registers:

Een register is  te  vergelijken
met een variable in  basic.  Het
register  A  is   een	8   bits
register. De Z80 kent een aantal
verschillende	registers.   (We
behandelen  in	deze   cursussen
alleen	   de	   belangrijkste
registers, voor meer  informatie
verwijzen   we	  jullie    naar
specifiekere documentatie)

HET A - REGISTER.

Het   A    register    is    het
belangrjkste  register	van   de
Z80.  Het  wordt  ook	wel   de
Accumulator genoemd. Het is  een
8-bits register, waarmee vrijwel
alle rekeninstructies uitgevoerd
kunnen worden.

DE B-C,D-E,H-L REGISTERS.

Deze registers worden meestal in
paren	    gebruikt.	    Door
samenvoeging van bijv.	B  en  C
wordt het  16-bits  register  BC
verkregen. Men kan  ze	ook  los
van   elkaar	gebruiken.    De
registers B en C worden vaak als
tellers  gebruikt.  Het  16-bits
register HL is het  een  van  de
weinige      registers	     dat
geheugenadressen kan aansturen.

HET F (of VLAG) - REGISTER.

Dit  is  een   zeer   belangrijk
register  voor	het  testen  van
voorwaarden. Het bestaat uit  de
volgende vlaggen :

bit   7- 6- 5- 4- 3- 2- 1- 0
vlag  S  Z     H    P/V N  C

De betekenissen van deze vlaggen
zal in de komende cursussen  nog
aan de orde komen. We  vermelden
ze alleen voor de volledigheid.

Er bestaan ook nog de  registers
IX,IY,R,I,PC en   nog	enkele
ontoegankelijke registers die de
Z80 zelf gebruikt.

Nu terug naar ons voorbeeldje.

ORG &HC000
LD A,10
LD B,20
ADD A,B
RET

LD r,n : Deze instructie zet  in
het 8 / 16 register r	(waarbij
r alle registers kan aangeven)
de waarde n.
Deze waarde  kan  voor een
8-bits	 register   tussen   de
0 en de 255  liggen en	voor een
16-bits register tussen de 0  en
65535. De instructie is nog  het
beste te vergelijken met: A = 10
in BASIC.

ADD A,r : Deze	instructie  telt
register A met register r op  en
zet de de uitkomst  in	register
A. Deze uitkomst mag niet groter
zijn dan 255, A is namelijk  een
8-bits register. De term ADD  is
afkomstig uit  het  Engels  waar
het	"optellen"      betekent
(eigenlijk   Amerikaans,    want
Zilog is een Amerikaanse firma).

RET : Deze instructie geeft  aan
dat het machinetaalprogramma, of
een deel daarvan, beeindigd  is.
Men  kan  dit  vergelijken   met
RETURN in BASIC.  In  dit  geval
springt het programma terug naar
BASIC.

Voor zover het	eerste	gedeelte
van de machinetaalcursus.  Zoals
je   wel    gezien    hebt    is
machinetaal niet zo moeilijk als
het  lijkt.  Als   je	ongeveer
begrijpt  wat  in  deze   cursus
besproken  is,	 dan   zijn   de
volgende cursussen ook	niet  zo
moeilijk.

Veel succes en programmeer-
plezier van:

Ruud  Gelissen
Jan-Willem  van Helden

Tel. 04746-1655 (Ruud)

Voor verdere documentatie  raden
wij je aan:
MACHINETAAL   Z80
van  Kluwer Techinische  boeken
van J.B. Vonk en E. Doppenberg.
MSX    MACHINETAAL     HANDBOEK
van Stark-Texel door:
H.  Klopper  en M. le Belle.
