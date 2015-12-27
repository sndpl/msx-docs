
               G E H E U G E N   T E L L E N


Niet  elke  MSX  computer  heeft  dezelfde  hoeveelheid  RAM
geheugen.  Vandaar dat  het soms handig is om even te kijken
hoeveel geheugen er aanwezig is, zodat het lopende programma
daar rekening mee kan houden.

Dit  wordt bijvoorbeeld ook gedaan door de layoutroutine van
de Sunrise Special. Er wordt gekeken hoeveel RAM geheugen er
aanwezig is.  Is dat  64 kB  (het minimum op MSX2 en hoger),
dan   moet  elk  MoonBlaster  muziekje  afzonderlijk  worden
ingeladen.  Is  het  meer,  dan  worden  er zoveel  mogelijk
MoonBlaster muziekjes  in het geheugen gezet. Met 128 kB zal
er meestal al niet meer hoeven worden bijgeladen.


                      A D D E R T J E

Er zit echter een addertje onder het gras. Veel programmeurs
vinden  het  blijkbaar  nogal  moeilijk  om zo'n  routine te
schrijven.  In  demo's  zit soms  een hardwaretest,  die ook
aangeeft hoeveel geheugen er aanwezig is. Ik heb het al heel
vaak  meegemaakt  dat  zo'n  routine  veel te  veel geheugen
opgeeft.  Sommige demo's  zeggen dat  ik 2048 kB heb! Dat is
natuurlijk leuk, maar ik heb maar 256 kB dus heb ik er niets
aan als zo'n demo dat zegt.


                  M E M O R Y M A P P E R

De routine  die ik  geschreven heb, telt het RAM geheugen in
de  huidige memorymapper  dat op  page 2  (&H8000-&HBFFF) is
geschakeld.  De  routine  vindt  dus niet  AL het  geheugen,
normaal  gesproken  alleen  het  geheugen  van  de  grootste
mapper.  Maar   aangezien  het  gelijktijdig  aansturen  van
verschillende  mappers een  vervelende klus  is, is het toch
niet zo  erg. Ik  raad u  aan MemMan  te gebruiken als u dat
toch van plan bent.

Toch kan  de routine wel gebruikt worden voor het tellen van
geheugen  in verschillende  mappers, er  moet dan alleen een
routine  omheen  worden geschreven  die alle  sloten afgaat,
kijkt  of   er  RAM   in  zit,   ze  op   page  2   schakelt
(&H8000-&HBFFF) en vervolgens de telroutine aanroept.

Dat  schakelen van  geheugen kan  rechtstreeks gebeuren (met
I/O poort  &HA8 en  adres &HFFFF),  of via  de BIOS  routine
ENASLT (&H0024). Misschien op de volgende Special de routine
die alle sloten afgaat.

Bij het  opstarten selecteert de MSX computer automatisch de
grootste  mapper. Als in een Japanse MSX2+ met 64 kB dus een
externe mapper  met 512 kB wordt gestoken, start de computer
vanuit  die externe  mapper op. Als u bijvoorbeeld een BASIC
programma laadt,  dan komt dat in de externe mapper terecht.
Als  u dus  zonder eerst met het geheugen geknoeid te hebben
mijn routine gebruikt, zal altijd de grootte van de grootste
mapper worden bepaald.


                        S L O T E N

De  plaats  van  het  RAM  in  de computer  is niet  op alle
computers hetzelfde. Op de adressen &HF341 t/m &HF344 is per
page te vinden in welk slot het RAM zit:

&HF341  page 0  &H0000-&H3FFF
&HF342  page 1  &H4000-&H7FFF
&HF343  page 2  &H8000-&HBFFF
&HF344  page 3  &HC000-&HFFFF

De  byte die op die adressen staat is een zogenaamde slot-ID
byte. Zo'n byte is als volgt opgebouwd:

bit  7   6   5   4   3   2   1   0
     E   0   0   0   S1  S0  P1  P0

Bit 7 geeft aan of het slot geexpandeerd is of niet (1=ja).
Bits 2-3 geven het secundaire slotnummer (mits E=1).
Bits 0-1 geven het primaire slotnummer.

De  telroutine  werkt  vanuit  page  2,  dus  daar  moet  de
memorymapper   worden   geschakeld.   Normaal   staat   daar
natuurlijk al  RAM, maar  als dat  niet zo is kan er op deze
manier RAM worden geschakeld:

        LD A,(&HF343)           ; A=ID byte RAM page 2
        LD H,&H80               ; page 2
        CALL &H0024             ; ENASLT

Voor  de routine  ENASLT moet de slot-ID byte in A staan, en
moet in HL een adres staan in de gewenste page. (Het is niet
nodig om  LD HL,&H8000 te doen, want alleen de bovenste twee
bits  van H  worden hier  gebruikt. Alle  getallen tussen LD
H,&H80 en LD H,&HBF zijn dus geschikt.) U kan dit natuurlijk
ook zelf doen door met I/O poort &HA8 en adres &HFFFF aan de
gang te gaan, maar dat is veel ingewikkelder. Ik zal er hier
dan ook niet over uitweiden hoe dat in zijn werk gaat.


                     S E G M E N T E N

Er staat nu dus RAM op page 2. Dit RAM zit in een zogenaamde
memorymapper.  Het RAM  in een  memorymapper is  verdeeld in
blokken (soms  wel 'maps'  genoemd) van  16 kB. Ik noem deze
blokken  verder  segmenten.  Per page  is er  een I/O  poort
waarmee  het segment  van de  memorymapper dat  op die  page
staat kan worden geselecteerd:

page 0  &H0000-&H3FFF   poort &HFC      (3)
page 1  &H4000-&H7FFF   poort &HFD      (2)
page 2  &H8000-&HBFFF   poort &HFE      (1)
page 3  &HC000-&HFFFF   poort &HFF      (0)

Tussen  haakjes staan  de standaardwaardes vermeld, zo is de
situatie als  de computer  is opgestart. Wees overigens niet
verbaasd  als u andere waardes vindt bij het uitlezen van de
poorten, het gaat alleen om de geldige bits!

Stel  de computer  heeft 64  kB, er zijn dan dus 4 segmenten
van 16  kB. Alleen de bits 0 en 1 van de vier poorten worden
dan  gebruikt  (zijn  geldig),  de  overige  6  bits  worden
genegeerd.  (In twee bits kunnen namelijk vier verschillende
waardes worden gezet; 00, 01, 10 en 11.)

Selecteer je  bijvoorbeeld segment 9 op een computer met 128
kB  RAM  (segmenten  0 t/m  7), dan  wordt gewoon  segment 1
geselecteerd!  (9 is  &B1001, alleen  de onderste  drie bits
zijn geldig, &B001 is 1!)

Een poort  kan de  waarde 0-255  bevatten, er kan totaal dus
256*16=4096 kB RAM in een memorymapper, oftewel 4 MB!

Pas  op met het gebruiken van segment 0, zoals je ziet staat
dit standaard  op page 3, waar het systeem RAM staat. Zou je
dit  segment ook op &HFE schakelen en het dan gebruiken, dan
zou  het  systeem  RAM  kunnen worden  verminkt en  slaat de
computer meestal vast.

Voor poort  &HFF geldt  een zelfde waarschuwing, als je daar
een  andere waarde  naar schrijft  moet je wel zorgen dat in
dat  segment  ook  het  systeem  RAM  staat,  anders  zal de
computer zeker hangen.


                        T E L L E N

Het bepalen  van de  grootte van de memorymapper is dus heel
simpel;   gewoon  het   aantal  segmenten   tellen  en   dat
vermenigvuldigen  met  16 kB.  We gaan  daarvoor alsvolgt te
werk:

- We  kiezen  een adres  uit wat  zowel de  routine als  het
  systeem RAM niet aantast
- We zetten in alle segmenten op dat adres een 0
- We gaan nu alle segmenten langs en zetten daar een 255 als
  we hem gehad hebben, zodat dubbel tellen uitgesloten is

Deze procedure  heeft een  nadeel: na  het doorlopen  van de
routine  is  de  inhoud  van  de memorymapper  verminkt. Dit
kunnen we voorkomen door:

- Aan het begin van de routine de waardes die op het gekozen
  adres staan op te slaan in een buffer
- Aan het  eind van de routine de waardes uit de buffer weer
  terug te zetten


                     D E   S O U R C E

Bovenstaande procedure is terug te vinden in de machinetaal-
routine  die ik heb geschreven. Hier is de source, rijkelijk
voorzien van commentaar.


; M E M C O U N T . A S C
; Door Stefan Boer
; Deze routine telt het aantal aanwezige memorymaps
; op &H8000-&HBFFF
; Routine kan eventueel worden uitgebreid, zodat al het RAM
; wordt gevonden.
; 19/03/92, (c) SteveSoft 1992
; Uitvoer:
; vanuit ML:    A=hoogste memorymapper segment
; vanuit BASIC: U=USR(0) --> U=hoogste segment
; N.B.  niet aantal  segmenten, omdat 4 MB (256 segmenten)
; dan niet kan!

; Enkele waardes

ADRES:  EQU   &H81FF          ; schrijft routine noch
                              ; systeem RAM over
MEMPRT: EQU   &HFE            ; I/O poort memorymapper
                              ; schakeling page 2

        ORG   &HC200          ; beginadres routine

; Initialisatie

MEMCNT: LD    HL,ADRES        ; HL check-adres
        LD    DE,BUFFER       ; DE beginadres buffer
        LD    C,MEMPRT        ; C  I/O poort memorymapper
                              ; page 2
        IN    A,(C)           ; lees huidig segmentnummer
        PUSH  AF              ; bewaar dit, wordt aan het
                              ; eind weer hersteld

; Bewaar de huidige informatie die op +1FF staan
; N.B. Ik zeg +1FF in plaats van &H81FF omdat de segmenten
;      geen vast beginadres hebben. Ze kunnen ook op 0000,
;      4000 of C000 staan.

        LD    B,0             ; voer de lus 256 keer uit
                              ; (0 t/m 255)
SAVE:   OUT   (C),B           ; schakel memorymapper
        LD    A,(HL)          ; lees waarde +1FF
        LD    (DE),A          ; zet waarde in buffer
        INC   DE              ; volgende plaats in buffer
        DJNZ  SAVE            ; doe dit met alle segmenten
                              ; (0 t/m 255)

; Zet 0 op +1FF in alle segmenten

        LD    B,0             ; voer de lus 256 keer uit
                              ; (0 t/m 255)
        XOR   A               ; A=0
ZERO:   OUT   (C),B           ; schakel memorymapper
        LD    (HL),A          ; zet 0 op +1FF
        DJNZ  ZERO            ; doe dit met alle segmenten
                              ; (0 t/m 255)

; Tel aantal werkelijk aanwezige segmenten

        LD    D,0             ; D aantal gevonden segmenten
        LD    B,0             ; voer de lus 256 keer uit
                              ; (0 t/m 255)
CHECK:  OUT   (C),B           ; schakel memorymapper
        LD    A,(HL)          ; lees +1FF
        CP    &HFF            ; al geteld?
        JR    Z,SLAOVR        ; Ja, dan volgende segment
        INC   D               ; aantal segmenten verhogen
        LD    (HL),&HFF       ; segment gehad
SLAOVR: DJNZ  CHECK           ; doe dit met alle segmenten
                              ; (0 t/m 255)
        PUSH  DE              ; bewaar aantal segmenten

; Herstel de inhoud van +1FF van alle segmenten

        LD    DE,BUFFER       ; beginadres waar waardes
                              ; zijn opgeslagen
        LD    B,0             ; 256 segmenten (0 t/m 255)
RESTOR: OUT   (C),B           ; schakel memorymapper
        LD    A,(DE)          ; lees waarde uit buffer
        LD    (HL),A          ; zet waarde terug op oude
                              ; plaats
        INC   DE              ; volgende plaats in buffer
        DJNZ  RESTOR          ; doe dit met alle segmenten
                              ; (0 t/m 255)

; Geef gevonden waarde door aan BASIC en ML en herstel
; originele stand van de memorymapper.

        POP   DE              ; D bevat aantal segmenten
        POP   AF              ; haal oude waarde &HFE terug
        OUT   (C),A           ; memorymapper page 2 weer
                              ; zoals bij begin
        LD    A,2             ; integer
        LD    (&HF663),A      ; geef type variabele aan
                              ; BASIC door
        LD    A,D             ; A bevat aantal segmenten
                              ; van 16 kB --> ML
        DEC   A               ; met een verminderen
                              ; niet aantal maar hoogste!
        LD    L,A
        LD    H,0             ; LD HL,A
        LD    (&HF7F8),HL     ; geef waarde aan BASIC door
        RET                   ; einde routine

BUFFER: DS    256             ; ruimte voor opslag +1FF
                              ; inhoud per segment


                    O P   D E   D I S K

Deze source is geschreven in WB-ASS2 en staat in ASCII op de
disk  onder  de naam  "MEMCOUNT.ASC". Om  de routine  uit te
testen  staat  ook de  file "MEMCOUNT.BIN"  op deze  Sunrise
Special #1. Deze file wordt geladen door "MEMCOUNT.BAS", dat
u in  het softwaremenu  kunt vinden. Dit programma laat zien
hoe u de routine vanuit BASIC kunt gebruiken.


                        L E T   O P

U  kunt  deze  routine normaal  gesproken altijd  gebruiken,
omdat  hij het  RAM niet blijvend aantast. Tijdens het lopen
van  de   routine  wordt   het  RAM  echter  wel  tijdelijke
aangetast.  Het zou  dus kunnen  dat u  een muziekje over de
interrupt laat  lopen en  dat dit programma in de driver zit
te   POKEn  en   dan gaat   het mis. De oplossing is simpel:
gewoon de interrupts uitzetten. Dus:

DI
CALL MEMCNT
EI

Misschien  de  volgende  keer  een  uitbreiding  waardoor de
routine meer  dan ��n  memorymapper aankan.  Voor deze  keer
laat ik het hierbij.

                                                Stefan Boer
