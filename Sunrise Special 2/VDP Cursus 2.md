                    - EERSTE GEDEELTE -

        M S X 2 / 2 +   V D P   C U R S U S   ( 2  )
                                                     

                     I N L E I D I N G 

Dit  is het tweede deel van de VDP cursus op herhaling. Deze 
keer zal  ik een  overzicht geven  van een  groot aantal VDP 
registers.  Ze worden niet op volgorde van nummer behandeld, 
maar in groepjes bij elkaar.

De belangrijkste  wijziging ten  opzichte van de oude versie 
is  dat er nu bij het lezen/schrijven van VRAM meer aandacht 
wordt besteed aan ML.


                M O D E   R E G I S T E R S 

    MSB  7   6   5   4   3   2   1   0  LSB
R#0      0   DG  IE2 IE1 M5  M4  M3  0      Mode Register 0
R#1      0   BL  IE0 M1  M2  0   SI  MAG    Mode Register 1
R#8      MS  LP  TP  CB  VR  0   SPD BW     Mode Register 2
R#9      LN  0   S1  S0  IL  E0  NT  DC     Mode Register 3

R#0:     DG :   digitaliseren
         IE2:   1 = interrupt van lichtpen of muis mogelijk
         IE1:   1 = interrupt mogelijk bij bepaalde beeld-
                lijn (zie R#19)
R#1:     BL :   scherm aan/uit (0=uit)
         IE0:   1 = normale interrupt mogelijk
         SI :   1 = sprite grootte  16x16, 0 = 8x8
         MA :   1 = vergrote sprites, 0 = normale sprites

De schermmodi kunnen worden ingesteld met M1-M5:

                 M5  M4  M3  M2  M1
SCREEN 1 (G1)    0   0   0   0   0
SCREEN 0 (T1)    0   0   0   0   1   (WIDTH 40)
SCREEN 3 (MC)    0   0   0   1   0
SCREEN 2 (G2)    0   0   1   0   0
SCREEN 4 (G3)    0   1   0   0   0
SCREEN 0 (T2)    0   1   0   0   1   (WIDTH 80)
SCREEN 5 (G4)    0   1   1   0   0
SCREEN 6 (G5)    1   0   0   0   0
SCREEN 7 (G6)    1   0   1   0   0
SCREEN 8 (G7)    1   1   1   0   0

De tekens  tussen  haakjes  achter  de  schermmodi  zijn  de 
offici�le benamingen. (T=TEXT, G=GRAPHIC, MC=MULTI COLOR)

R#8:     MS :   1 = color bus input  / muis mogelijk
                0 = color bus output / geen muis mogelijk
         LP :   1 = lichtpen mogelijk, 0 = geen lichtpen
         TP :   1 = kleur 0 niet meer transparant
         CB :   1 = color bus input, 0 = color bus output
         VR :   kies soort VRAM
                1 = 64K x 1 bit of 64K x 4 bits
                0 = 16K x 1 bit of 16K x 4 bits
         SPD:   1 = sprites uit, 0 = sprites aan
         BW :   1 = zwart/wit in 32 grijstinten, 0 = kleur
R#9:     LN :   Bepaal hoogte van scherm. 1=212, 0=192
         S0/1:  Synchronisatiemode:
                S1:   S0:
                0     0         intern
                0     1         mix
                1     0         extern (digitize)
                1     1         niet
         IL :   1 = interlace
         EO :   1 = afwisselen even/oneven PAGE
         NT :   0 = NTSC 60 Hz, 1 = PAL 50 Hz
         DC :   1 = *DLCLK input, 0 = *DLCLK output


  T A B L E   B A S E   A D D R E S S   R E G I S T E R S 

Deze adressen geven aan waar de diverse tabellen in het VRAM 
beginnen. Dit komt overeen met BASE(n) in BASIC.

    MSB  7   6   5   4   3   2   1   0  LSB
R#2      0   A16 A15 A14 A13 A12 A11 A10    scherminfotabel
R#3      A13 A12 A11 A10 A9  A8  A7  A6     kleurtabel low
R#10     0   0   0   0   0   A16 A15 A14    kleurtabel high
R#4      0   0   A16 A15 A14 A13 A12 A11    patroontabel
R#5      A14 A13 A12 A11 A10 A9  A8  A7     sprite-info low
R#11     0   0   0   0   0   0   A16 A15    sprite-info high 
R#6      0   0   A16 A15 A14 A13 A12 A11    sprite-patroon

In BASIC kun je deze adressen opvragen met BASE(n).  n  moet 
dan gelijk zijn aan vijf maal de screenmode plus:
0:  scherminfo       : SCREEN 0-4: patroonnummers
                       SCREEN 5-8: bitmapped
1:  kleur            : alleen SCREEN 0-4 (beh. WIDTH 40)
2:  patroon          : alleen SCREEN 0-4
3:  sprite-info      : coordinaten e.d.
4:  sprite-patroon   : sprite$()="..."

Voorbeeld: de  sprite-info tabel  van SCREEN 7 begint dus op 
de waarde die in BASE(5*7+3) staat.

Je  kunt met  behulp van  deze registers  in SCREEN 0-4 zeer 
veel  schermen  tegelijk  in  het  VRAM  zetten,  omdat  die 
schermen maximaal  16 kB  per page  nodig hebben, en je hebt 
128  kB. Uitleg over hoe de beeldinformatie wordt opgeslagen 
in deel 7 t/m 9.

Je kunt  aan de gegevens ook zien waarvan de BASE(n) waarden 
een  veelvoud moeten  zijn. Als  je je  daaraan niet  houdt, 
volgt  er  een foutmelding.  Bijvoorbeeld de  spritepatroon- 
tabel. Het  laagste bit  dat door  R#6 wordt bepaald is A11. 
Het moet dus een veelvoud zijn van 2^11 = 2048.

Zie verder deel 7 t/m 9.


                K L E U R R E G I S T E R S 

    MSB  7   6   5   4   3   2   1   0  LSB
R#7      TC3 TC2 TC1 TC0 BD3 BD2 BD1 BD0    Tekst/Achtergr
R#12     T23 T22 T21 T20 BC3 BC2 BC1 BC0    Blinkkleuren
R#13     ON3 ON2 ON1 ON0 OF3 OF2 OF1 OF0    Blinktijden

TC3-TC0      : tekstkleur in T1 en T2
BD3-BD0      : achtergrondkleur in alle schermmodi
T23-T20      : tekstkleur voor blink in T2
BC3-BC0      : achtergrondkleur voor blink in T2
ON3-ON0      : tijd voor blink-kleur
OF3-OF0      : tijd voor normale kleur

Je  kunt  R#13  ook  gebruiken  om de  PAGEs van  SCREEN 5-8 
afwisseld  weer te  geven. Stel  met SET PAGE de weergegeven 
PAGE op  1 of 3, en zet dan de gewenste tijden in R#13. PAGE 
1 en 0 (resp. 3 en 2) zullen dan beurtelings worden getoond. 
Voorbeeld:  SET PAGE 1 : VDP(14)=&H66 'page 0 en 1 worden nu 
beurtelings getoond met een tussenpauze van ca. 1 seconde.


          U I T L E G   B I J   B L I N K M O D E 

Je kunt in SCREEN 0, WIDTH 80 (T2 mode) alternatieve scherm- 
kleuren  gebruiken.   Er  is  hiervoor  een  speciale  tabel 
gereserveerd  in het  VRAM. Het  beginadres daarvan staat in 
BASE(1). In  die tabel  staat voor elke schermpositie 1 bit. 
Is  dat bit  gelijk aan  1, dan  wordt de alternatieve kleur 
getoond. Is het bit gelijk aan 0, dan wordt de normale kleur 
getoond. Eventueel kun je de alternatief gekleurde delen van 
het scherm  ook nog  laten knipperen. De knippertijd moet in 
R#13   worden  gezet.  Dat  gaat  in  ongeveer  1/6  seconde 
nauwkeurig. De  hoogste vier  bits bepalen  de tijd  voor de 
alternatieve  kleur, de  laagste vier  bits bepalen  de tijd 
voor  de   normale  kleur.   In  R#12   (VDP(13))  staan  de 
alternatieve kleuren. De bovenste vier bits de voorgrond- en 
de  onderste vier  bits de  achtergrondkleur. Zet  de waarde 
&H10 in  R#13 (VDP(14))  als je niet wilt knipperen, maar de 
alternatieve kleur constant wilt tonen.

De blinkertabel is als volgt ingedeeld:
BASE(1)+  0: posities ( 0, 0)...( 7, 0)
BASE(1)+  1: posities ( 8, 0)...(15, 0)
BASE(1)+  9: posities (72, 0)...(79, 0)
BASE(1)+ 10: posities ( 0, 1)...( 7, 1)
BASE(1)+239: posities (72,23)...(79,23)

Blinktijden (60 Hz, VDP(10)=0, NTSC, milliseconde):
Data: (hex)   Tijd: (ms)     Data: (hex)    Tijd: (ms)
------------------------------------------------------------ 
0             0              8              1335.1
1             166.9          9              1501.9
2             333.8          A              1668.8
3             500.6          B              1835.7
4             667.5          C              2002.6
5             834.4          D              2169.5
6             1001.3         E              2336.3
7             1168.2         F              2503.2
------------------------------------------------------------ 

Voorbeeld:  je wilt  de posities  (12,3), (14,3)  en (50,12) 
laten knipperen  met voorgrondkleur  magenta (13) en achter- 
grondkleur  donkerblauw (4).  De normale kleur moet 500.6 ms 
getoond worden, de alternatieve kleur 166.9 ms.

Eerst berekenen we de adressen:
14 \ 8 = 1     > Het adres wordt dus: BASE(1)+30+1
14 MOD 8 = 6   > Het ene bitnummer wordt 7 - 6 = 1 en
12 MOD 8 = 4   > het andere bitnummer wordt 7 - 4 = 3, dus
3 * 10 = 30    > de te VPOKEn waarde is &B00001010 = 10

50 \ 8 = 6     > Het adres wordt dus: BASE(1)+120+6
50 MOD 8 = 2   > Het bitnummer wordt 7 - 2 = 5, dus de te
12 * 10 = 120  > VPOKEn waarde is &B00100000 = 32

In de tabel vinden we voor de blinktijden de waardes 3 en 1. 
13 is gelijk aan &HD. Dat lever totaal op:
VPOKE  BASE(1)+31,10:  VPOKE  BASE(1)+126,32:  VDP(13)=&HD4: 
VDP(14)=&H13

N.B. Op Sunrise Special #2 staat een uitgebreid artikel over 
een  standaard ML  routine voor  de blinkmode en vier nieuwe 
BASIC commando's (CMD BCOL, CMD BFIL, CMD BTIM en CMD BRES), 
waarmee de blink mode eenvoudig vanuit BASIC te besturen is.


         C O L O R   B U R S T   R E G I S T E R S 

    MSB  7   6   5   4   3   2   1   0  LSB
R#20     0   0   0   0   0   0   0   0      Color burst 1
R#21     0   0   1   1   1   0   1   1      Color burst 2
R#22     0   0   0   0   0   1   0   1      Color burst 3

Ik weet nog steeds niet waar deze registers voor dienen. Als 
iemand  het  weet, graag  even een  briefje naar  de Sunrise 
postbus t.a.v.  ondergetekende. Ik  denk overigens  dat deze 
registers in het geheel geen nut hebben voor de programmeur, 
het  schrijven van  willekeurige waardes naar deze registers 
heeft namelijk geen enkel merkbaar effect.

(Nvdr. De tekst was langer dan 16 kB, u kunt het tweede deel 
in het submenu vinden.)

                                                Stefan Boer



                    - TWEEDE GEDEELTE -

        M S X 2 / 2 +   V D P   C U R S U S   ( 2  )
                                                     


(Nvdr. De tekst was langer dan 16 kB, u kunt het eerste deel 
in het submenu vinden.)


             D I S P L A Y   R E G I S T E R S 

    MSB  7   6   5   4   3   2   1   0  LSB
R#18     V3  V2  V1  V0  H3  H2  H1  H0     Display adjust
R#23     DO7 DO6 DO5 DO4 DO3 DO2 DO1 DO0    Display offset
R#19     IL7 IL6 IL5 IL4 IL3 IL2 IL1 IL0    Interrupt line

R#18 werkt ongeveer net zo als SET ADJUST(dx,dy)  in  Basic. 
Het bepaalt de positie van het beeld op het scherm.
LINKS: H=7...H=1    MIDDEN: H=0    RECHTS: H=15...H=8
ONDER: V=8...V=15   MIDDEN: V=0    BOVEN:  V=1...V=7
Dit register wordt vaak op de MSX2 gebruikt om een  horizon- 
tale  scroll  te  maken.  Denk  hierbij  aan  Space  Manbow, 
Golvellius 2, Super Cooks, Pennant Race 2. Je ziet  dan  een 
happende rand. Op de  MSX2+  wordt  de  scroll  dan  meestal 
beter.

Met R#23  (beter bekend  als VDP(24))  kun je zeer eenvoudig 
een  verticale scroll  maken op de MSX2. Een van de volgende 
keren zal  ik een  programmavoorbeeld hiervan laten zien. De 
waarde in VDP(24) geeft aan op welke hoogte de bovenste lijn 
van het scherm zich bevindt.

Met  R#19 kun  je aangeven  bij welke  beeldlijn de  VDP een 
interrupt moet  geven. (Zie  ook R#0).  Je hebt dit register 
nodig  voor  screensplits.  Bij een  screensplit kun  je een 
bepaalde  actie (bijvoorbeeld  een pagewissel, schermwissel, 
kleurwissel, scrollen,  etc.) laten uitvoeren, op het moment 
dat  de  VDP  een  bepaalde  beeldlijn naar  het beeldscherm 
stuurt.  Ik  zal  hier  nu  verder  niet  op  in  gaan,  een 
screensplit  zal  bij  de  praktijkvoorbeelden  (deel 10  en 
verder van de VDP cursus) zeker aan bod komen.


               A C C E S   R E G I S T E R S 

Deze  registers  gebruik  je  bij  lezen/schrijven van  VDP- 
registers en VRAM.

    MSB  7   6   5   4   3   2   1   0  LSB
R#14     0   0   0   0   0   A16 A15 A14    VRAM Base
R#15     0   0   0   0   S3  S2  S1  S0     S# pointer
R#16     0   0   0   0   C3  C2  C1  C0     Color palette
R#17     AII 0   RS5 RS4 RS3 RS2 RS1 RS0    R# pointer

Over het gebruik van R#15-17 heb ik in deel 1 al het een  en 
ander uitgelegd. Daarom hier alleen kort de werking:
R#15  Statusregister pointer.  Schrijf het nummer van het te 
      lezen statusregister  naar R#15.  De data komt in Port 
      #1.
R#16  Color  palette  pointer.  Schrijf  het  color  palette 
      nummer  naar  R#16. Schrijf  daarna de  kleurdata naar 
      Port #2. Doe dat als volgt (binair):
      1e byte: 0  R1 R2 R0 0  B2 B1 B0
      2e byte: 0  0  0  0  0  G2 G1 G0
      De waarde  van R#16  wordt automatisch verhoogd, zodat 
      meerdere palet  entries kunnen worden veranderd zonder 
      eerst het nummer naar R#16 te moeten schrijven.
R#17  Indirekte VDP-beschrijving. Schrijf het registernummer 
      naar  R#17.  Schrijf  daarna  de  data  die  naar  dat 
      register moet naar Port #3. Als je wilt dat het regis- 
      ternummer automatisch verhoogd wordt, moet je het  AII 
      bit op 0 zetten.

(Het  schrijven  naar  Port  #0  t/m  Port  #3  gebeurt  met 
OUT-instrukties. De  I/O adressen  voor Port  #0 t/m Port #3 
zijn altijd &H98 t/m &H9B.)


          L E Z E N / S C H R I J V E N   V R A M 

Om het VRAM te lezen/schrijven moet je de volgende procedure 
volgen:

1)  Bankswitching.  In  principe  is dit  niet nodig,  omdat 
normaal gesproken  altijd het  normale VRAM is geselecteerd. 
Sommige  computers  hebben  64  kB  expansion VRAM,  dit kan 
gebruikt  worden door  bankswitching toe  te passen. in R#45 
aan  of  je  het  VRAM  (0)  of het  Expansion RAM  (1) wilt 
gebruiken. Plaats de overeenkomstige waarde in bit 6.
    MSB  7   6   5   4   3   2   1   0  LSB
R#45     0   MXC MXD MXS DIY DIX EQ  MAJ    Argument Reg.
MXD = 0 betekent VRAM, 1 betekent Exp RAM

2) Het VRAM adres kan vari�ren van 0-1FFFFH (128 kB). Plaats 
de drie hoogste bits in R#14:
    MSB  7   6   5   4   3   2   1   0  LSB
R#14     0   0   0   0   0   A16 A15 A14    VRAM Base
(Vanaf hier is het MSX1 compatible.)

3) Schrijf de acht laagste bits naar Port #1.
    MSB  7   6   5   4   3   2   1   0  LSB
Port #1  A7  A6  A5  A4  A3  A2  A1  A0     First Byte

4)  Schrijf  de  bits  8-13  naar  Port  #1.  Bepaal  lezen/ 
schrijven met bit X. X=1: Schrijven, X=0: Lezen.

    MSB  7   6   5   4   3   2   1   0  LSB
Port #1  0   X   A13 A12 A11 A10 A9  A8     Second Byte

N.B. Bit  7 moet  0 zijn,  hieraan kan  de VDP  zien dat  de 
waarde  die hij  zojuist ontvangen  heeft niet  naar een VDP 
register moet  (dan is  bit 7  namelijk 1),  maar dat het de 
bits  0-7 van  het adres zijn. Voor bit 6 en 7 geldt dus het 
volgende:

b7 b6
1  0    VDP register schrijven
0  0    VRAM lezen
0  1    VRAM schrijven

5)  Lees  of  schrijf de  data van/naar  Port #0  (I/O poort 
&H98).  Het adres  wordt automatisch  verhoogd, dus  je kunt 
continu doorgaan met het lezen of schrijven van data.

Het  lezen/schrijven  van  VRAM kan  weer op  drie manieren, 
namelijk in BASIC, in machinetaal via BIOS en in machinetaal 
door rechtstreeks de VDP aan te sturen met IN en OUT.

In  BASIC  gaat  het  uiteraard gewoon  met VPOKE,  de BASIC 
interpreter zorgt voor de rest. In het BIOS zijn de volgende 
routines aanwezig voor het lezen/schrijven van VRAM:

                            MSX2:

&H016E    NSETRD  Stel VDP  in om  VRAM te lezen vanaf adres 
                  in  HL. Na  het aanroepen van deze routine 
                  kan de data uit Port #0 worden gelezen.
&H0171    NSTWRT  Stel  VDP  in om  VRAM te  schrijven vanaf 
                  adres  in HL.  Na het  aanroepen van  deze 
                  routine kan  de data  naar Port  #0 worden 
                  geschreven.
&H0174    NRDVRM  Lees VRAM. HL=adres, A=data, wijzigt F.
&H0177    NWRVRM  Schrijf VRAM. HL=adres, A=data,
                  wijzigt AF.
N.B. NRDVRM en NWRVRM gebruiken NSETRD en NSTWRT.

                           MSX1/2:
&H004A    RDVRM   Zie NRDVRM, alleen SCREEN 0-4!
&H004D    WRTVRM  Zie NWRVRM, alleen SCREEN 0-4!
&H0050    SETRD   Zie NSETRD, alleen SCREEN 0-4!
&H0053    SETWRT  Zie NSTWRT, alleen SCREEN 0-4!

(De hierboven  genoemde routines  zijn gemaakt voor de MSX1, 
en  kunnen daarom alleen adressen tussen 0 en 16383 aan. Dit 
is in  SCREEN 0-4  geen bezwaar, maar in SCREEN 5-12 wel. Op 
een  MSX2 programma  gewoon altijd NRDVRM, NWRVRM, NSETRD en 
NSTWRT  gebruiken,  ook  in  SCREEN  0-4.  RDVRM  en  WRTVRM 
gebruiken SETRD en SETWRT.)

0056H    FILVRM  Vult blok VRAM vanaf HL met lengte BC met
                 waarde A. Wijzigt: AF,BC.
0059H    LDIRMV  Kopieert blok VRAM->RAM. HL=bron, DE=doel,
                 BC=lengte. Wijzigt: alle.
005CH    LDIRVM  Kopieert blok RAM->VRAM. Zie LDIRMV
016BH    BIGFIL  Doet hetzelfde  als FILVRM. Het verschil is 
                 dat FILVRM bij SCREEN 0-3 net doet alsof er 
                 maar 16 kB, en BIGFIL niet.

Soortgelijke routines  zijn ook in het SUBROM aanwezig, maar 
aangezien  het aanroepen  daarvan alleen  maar extra tijd in 
beslag neemt laat ik dat hier achterwege.

Nu rechtstreeks  in ML.  Ik geef  hier de routines NSTWRT en 
NSETRD.  Deze routines zijn praktisch gelijk aan de routines 
in  het  MAIN ROM,  die kunt  u dus  rustig gebruiken.  Deze 
routines zijn dan ook vooral leerzaam!


; VRAM routines door Stefan Boer
; VDP cursus op herhaling deel 2
; Sunrise Special #2
; Door Stefan Boer
; (c) Sunrise 1992

; SETWRT
; Zet VDP klaar om naar VRAM te schrijven
; In: HL: bit 0-15 van adres
;     C : bit 16 van adres

SETWRT: LD    A,H
        RES   7,A
        SET   6,A             ; 0 1 is VRAM schrijven
        JP    RDWRT

; SETRD
; Zet VDP klaar om uit VRAM te lezen
; In: HL: bit 0-15 van adres
;      C: bit 16 van adres

SETRD:  LD    A,H
        AND   &B00111111      ; 0 0 is VRAM lezen

; Dit gedeelte is voor beide routines hetzelfde

RDWRT:  PUSH  AF              ; deze byte moet pas als
                              ; laatste naar Port #1
        LD    A,C             ; bit 16 van adres
        AND   1               ; alleen bit 0
        LD    C,A
        LD    A,H
        AND   &HC0            ; alleen bit 6 en 7
                              ; (bit 14 en 15 van adres)
        OR    C               ; bit 16 van adres erbij
        RLCA
        RLCA                  ; schuif bit 0, 6 en 7 naar
                              ; bit 0, 1 en 2
        DI
        OUT   (&H99),A
        LD    A,14+128
        OUT   (&H99),A        ; schrijf bit 14-16 van adres
                              ; naar R#14
        LD    A,L             ; bit 0-7 van adres
        OUT   (&H99),A        ; naar Port #1
        POP   AF              ; bit 8-13 van adres, plus VDP
                              ; instructie
        OUT   (&H99),A        ; naar Port #1
        EI
        RET

; data kan nu worden gelezen/geschreven via Port #0 (&H98)

Deze  routine  spreekt  voor  zichzelf,  u  kunt  de  eerder 
omschreven standaardprocedure hierin herkennen.


                     T E N S L O T T E 

Met vragen  over de  VDP cursus kunt u natuurlijk altijd bij 
mij  terecht,  net  als  met  alle andere  programmeervragen 
overigens.  Schrijf  een  briefje naar  de Sunrise  postbus, 
t.a.v. ondergetekende.

Op de volgende Sunrise Special zullen deel 3 en 4 van de VDP 
cursus  worden herhaald, daarin worden de commando registers 
van de  VDP behandeld.  Hiermee kunt  u zelf  COPY's, LINEs, 
etc. in machinetaal programmeren!

Tot de volgende keer!

                                                Stefan Boer