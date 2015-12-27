                   P C M   S A M P L E R 

Een  van de leuke dingen van de turbo R is de ingebouwde PCM 
sampler. Voor het opnemen en afspelen met deze PCM chip zijn 
twee  nieuwe  BIOS  calls  aanwezig, die  we in  dit artikel 
zullen gaan bespreken.

Bovendien  zal  het  programma  LONG  PCM worden  besproken, 
waarmee  je zeer  lange samples kunt opnemen en afspelen. De 
lengte van  deze samples  is het RAM geheugen plus 32 kB, op 
een  standaard FS-A1ST is dit dus 288 kB en op een standaard 
FS-A1GT  544  kB. Dit  programma maakt  gebruik van  de BIOS 
calls.

De  volgende  keer  zal  ik  uitleggen  hoe je  de PCM  chip 
rechtstreeks kunt  aansturen. Hiervoor wordt onder andere de 
systeem  counter van  de turbo  R gebruikt,  die dan ook zal 
worden besproken.

Maar nu eerst de twee nieuwe BIOS calls.


             N I E U W E   B I O S   C A L L S 

Naam:    PCMPLY
Adres:   &H0186 (MAIN ROM)
Functie: sample afspelen
Invoer:  EHL = startadres
         DBC = lengte
         A   = mode
         bit 0 en 1: 00  15.75  kHz
                     01  7.875  kHz
                     10  5.25   kHz
                     11  3.9375 kHz
         bit 2-6:    0
         bit 7:      0   RAM
                     1   VRAM

Bij  gebruik van  alleen RAM zou een 16 bits adres voldoende 
zijn, maar  omdat we ook met VRAM kunnen werken zijn 17 bits 
noodzakelijk.  Daar wordt het DE register voor gebruikt, het 
17de bit  van het  startadres staat in E en het 17de bit van 
de  lengte  staat  in  D.  Voor  de  duidelijkheid een  paar 
voorbeelden:

1) Sample op adres &H9000-&HBFFF (RAM) afspelen op 5.25 kHz:

        LD   HL,&H9000
        LD   BC,&H3000  ; &HBFFF-&H9000+1 = &H3000
        LD   DE,0
        LD   A,&B00000010
        CALL PCMPLY

2) Sample op  adres &H04000-&H1BFFF (VRAM) afspelen op 15.75 
   kHz:

        LD   HL,&H4000
        LD   BC,&H8000  ; &H1BFFF-&H04000+1 = &H18000
        LD   DE,&H0100
        LD   A,&B10000000
        CALL PCMPLY


Naam:    PCMREC
Adres:   &H0189 (MAIN ROM)
Functie: sample opnemen
Invoer:  EHL = startadres
         DBC = lengte
         A   = mode        
         bit 0 en 1: zie PCMPLY
         bit 2     : 0 crunch mode uit
                     1 crunch mode uit
         bit 3-6   : trigger
         bit 7     : zie PCMPLY

Voor de  uitleg bij  EHL en DBC verwijs ik naar PCMPLY. Voor 
bit  2 heb  ik zelf maar de naam "crunch mode" verzonnen, ik 
weet niet  wat de offici�le naam hiervoor is. Als dit bit is 
gezet  neemt "stilte"  veel minder geheugenruimte in beslag. 
Een  extra  voordeel  is  dat  je  geen ruis  hoort bij  een 
"stilte". Deze  mode wordt  bijvoorbeeld gebruikt  bij zowel 
het   ingebouwde  programma  van  de  turbo  R  als  bij  de 
meegeleverde sample  tool. Je  kunt duidelijk merken dat het 
geheugen veel minder snel vol is als er weinig geluid is.

De trigger kennen we al van de MSX-AUDIO sampler. Als PCMREC 
wordt  aangeroepen wordt  gewacht met  opnemen totdat er een 
signaal binnenkomt  dat de  triggerwaarde evenaart. Als er 0 
wordt  ingevuld  voor  de  trigger  wordt  de  opname meteen 
gestart. Ook hier weer twee voorbeelden:

1) Sample met 7.875 kHz opnemen van &H8300 tot &HD4FF (RAM), 
   trigger is uit en crunch mode is aan:

        LD   HL,&H8300
        LD   BC,&H5200  ; &H8300-&HD4FF+1 = &H5200
        LD   DE,0
        LD   A,&B00000101
        CALL PCMREC

2) Sample met  3.9375 kHz  opnemen van  &H13000 tot  &H13FFF 
   (VRAM), trigger is 10 en crunch mode is uit:

        LD   HL,&H3000
        LD   BC,&H1000  ; &H13000-&H13FFF+1 = &H1000
        LD   DE,&H0001
        LD   A,&B11010011
        CALL PCMREC


Het is  nu tijd  voor het  voorbeeldprogramma: LONG PCM. Dit 
programma  is vanzelfsprekend ook onder dezelfde naam in het 
softwaremenu te  vinden en zoals u van mij gewend bent staat 
de  assemblerlisting in  ASCII formaat  op de  disk onder de 
naam "LONGPCM.ASC". We gaan deze source nu bespreken.

(Ik denk  dat het  wel verstandig  is om het programma eerst 
eens  uit te  proberen, en  dan pas  de assemblerlisting  te 
bestuderen.)


; L O N G P C M . A S M 
; Door Stefan Boer 25/10/92
; Lange samples opnemen en afspelen
; Gebruikt 120 kB VRAM en RAM - 80 kB (voor DRAM en
; systeem RAM + routine)
; Alleen MSX turbo R
; Geen DOS2!!


Om  het simpel  te houden  wordt DOS2  niet ondersteund, het 
aansturen van  het geheugen  zou dan namelijk anders worden. 
Bovendien  moet er dan rekening worden gehouden met de 32 kB 
RAM die DOS2 in beslag neemt, en dat doen we ook niet.


CHGMOD: EQU   &H5F
ERAFNK: EQU   &HCC
CHGET:  EQU   &H9F
CHPUT:  EQU   &HA2
CHGCPU: EQU   &H0180
PCMREC: EQU   &H0189
PCMPLY: EQU   &H0186
LINL80: EQU   &HF3AE


De  in  de  routine  gebruikte  BIOS  calls  en  systeem RAM 
adressen worden hier gedefinieerd.


        ORG   &HC800
        IN    A,(&HFE)
        PUSH  AF
        LD    A,&H82
        CALL  CHGCPU          ; R800 DRAM mode
        CALL  ERAFNK          ; KEY OFF
        LD    A,80
        LD    (LINL80),A      ; WIDTH 80
        XOR   A
        CALL  CHGMOD          ; SCREEN 0
        DI
        LD    A,&HF1          ; COLOR 15,1,1
        OUT   (&H99),A
        LD    A,7+128
        OUT   (&H99),A
        EI


Hier wordt eerst de inhoud van I/O poort &HFE bewaard, zodat 
we  die  straks  weer  kunnen  herstellen.  Verder  wordt de 
computer  in R800  DRAM mode  gezet, en initialiseren we het 
scherm.  De  R800  DRAM  mode is  noodzakelijk omdat  anders 
PCMREC alleen werkt bij lage frequenties.


        CALL  MEMCNT          ; geheugen tellen
        LD    (MAXPGE),A
MAIN:   LD    HL,WELKOM
        CALL  PRINTS
        LD    A,(MAXPGE)
        LD    B,16
        DEFB  &HED,&B11000001 ; MULUB A,B (HL=16*A)
        LD    BC,112
        ADD   HL,BC           ; VRAM erbij
        CALL  PRTDEC          ; getal in decimaal naar scherm


Hier  wordt  eerste  MEMCOUNT  aangeroepen, mijn  standaard- 
routine  om  de  grootte  van  de  actuele memory  mapper te 
bepalen die we op Sunrise Special #1 al hebben behandeld. Ik 
maak er  hier dus verder geen woorden aan vuil. Deze routine 
geeft  in het A register de hoogste mapper pagina terug. Bij 
256 kB is dit bijvoorbeeld 15. Deze waarde wordt opgeslagen. 
Vervolgens  worden  de titel  e.d. op  het scherm  gezet, de 
routine PRINTS drukt een string af die op adres HL begint en 
eindigt met een 0.

Vervolgens  wordt  de  hoeveelheid samplegeheugen  berekend. 
MEMCNT telt automatisch het RAM dat voor DRAM wordt gebruikt 
al  niet mee,  op een  turbo R  met 256  kB is  (MAXPGE) dus 
gelijk aan 15-4 = 11. Er zijn dus 12 mapperpagina's, waarvan 
er  een  (nr.  0)  nodig  is  voor  het  systeem RAM  en het 
programma (die  mapper pagina staat dus op &HC000). (MAXPGE) 
geeft  dus zonder verdere correctie het aantal memory mapper 
pagina's (van 16 kB) dat voor samples vrij is.

Dit vermenigvuldigen  we via MULUB met 16, het antwoord komt 
in  HL. Vervolgens  tellen we er nog 112 bij op, omdat we de 
onderste 16  kB van  het 128  kB grote  VRAM niet gebruiken. 
Deze  waarde wordt  met PRTDEC  in decimaal  naar het scherm 
gestuurd (met  voorloopnullen), deze standaardroutine zal ik 
een andere keer nog wel eens bespreken.


        LD    HL,WELKM2
        CALL  PRINTS
        LD    HL,MENU
        CALL  PRINTS


Met WELKM2 wordt er nog een "kB" achter het zojuist geprinte 
getal gezet, achter MENU gaat het complete menu schuil. (Het 
is  misschien  handig  om  even  vooruit  te kijken  naar de 
definites van WELKOM, WELKM2, MENU, etc.)


        LD    A,(FREQUE)      ; current frequency
        LD    HL,FREQ0
        AND   A
        JR    Z,FQ0
        LD    HL,FREQ1
FQ0:    CALL  PRINTS


Hier wordt de actuele frequentie (15.75 kHz of 7.875 kHz) op 
het  scherm  gezet.  Vanaf  FREQ0 en  FREQ1 staan  de juiste 
teksten,  via  een voorwaardelijke  sprong wordt  HL met  de 
juiste  waarde  geladen. Vervolgens  wordt de  juiste string 
op het scherm gezet.


        LD    HL,MENU2
        CALL  PRINTS
        LD    A,(CRUNCH)      ; crunch mode
        LD    HL,CRU0
        AND   A
        JR    Z,CR0
        LD    HL,CRU1
CR0:    CALL  PRINTS


Hier hetzelfde, alleen nu voor de crunch mode.


        LD    HL,MENU3        ; trigger
        CALL  PRINTS
        LD    A,(TRIGGR)
        LD    L,A
        LD    H,0
        CALL  PRTDC2


Bij de  trigger gaat het om een getal. Dit wordt in HL gezet 
en vervolgens wordt PRTDC2 aangeroepen, een ingekorte versie 
van  PRTDEC die  een decimaal  getal op  twee cijfers  print 
(eventueel met voorloopnul).


        LD    HL,MENU4
        CALL  PRINTS
        LD    A,(LIST_F)
        OUT   (&HA5),A        ; listen aan/uit
        LD    HL,CRU0
        AND   A
        JR    Z,LI0
        LD    HL,CRU1
LI0:    CALL  PRINTS          ; listen


Tot slot van ons overzicht van de instellingen de instelling 
"listen".  Hiermee  kan  het  binnenkomende  signaal  worden 
beluisterd, handig  om het volume af te stellen. Let op: zet 
dit alleen aan als er een extrne geluidsbron (bv. CD speler) 
of  microfoon  op  de  externe  microfoonaansluiting  op  de 
achterzijde  is aangesloten,  anders gaat  het geluid zingen 
wat een afschuwelijke herrie tot resultaat heeft!

Het  doorvoeren   van  het  binnenkomende  signaal  naar  de 
luidspreker  gaat met  een OUT  &HA5,10, u  kunt dit  ook in 
BASIC gebruiken! Met een OUT &HA5,0 wordt het weer uitgezet. 
De  waarde  wordt  elke  keer  opnieuw  naar I/O  poort &HA5 
gestuurd, omdat het wordt uitgezet bij aanroep van PCMPLY of 
PCMREC.


        LD    HL,PROMPT
        CALL  PRINTS
GETKEY: CALL  CHGET
        CP    "1"
        JR    C,GETKEY
        CP    "7"+1
        JR    NC,GETKEY       ; controle tussen 1 en 7
        PUSH  AF
        CALL  &HA2
        LD    A,13
        CALL  &HA2
        LD    A,10
        CALL  &HA2


Hier  wordt de  prompt ("Your choice please?") op het scherm 
gezet. Vervolgens  wordt CHGET aangeroepen, die op een toets 
wacht  en de  ASCII van die toets in A teruggeeft. We kijken 
met voorwaardelijke  sprongen of het een toets tussen 1 en 7 
is.  De carry  wordt bij  CP "1"  gezet als A kleiner is dan 
&H31, de ASCII van "1". Bij een carry wordt het dus nog eens 
geprobeerd. Bij  de tweede CP wordt er vergeleken met "7"+1, 
als  A inderdaad  kleiner of  gelijk is aan 7 wordt de carry 
niet gezet.  Anders moeten we het nog eens proberen. Bij een 
goede  toets  wordt  AF  bewaard  en wordt  er een  linefeed 
(13,10) gegeven.


        POP   AF
        SUB   "1"             ; omzetten in getal 0-7
        ADD   A,A
        LD    E,A
        LD    D,0
        LD    HL,JPTAB
        ADD   HL,DE           ; bereken adres in sprongtabel
        LD    E,(HL)
        INC   HL
        LD    D,(HL)
        EX    DE,HL
        JP    (HL)            ; spring naar juiste routine


Hier  wordt er aan de hand van het ingetoetse cijfer naar de 
juiste routine gesprongen. Eerst halen we de eerder bewaarde 
waarde van  A terug met een POP. Vervolgens trekken we er de 
ASCII  van "1"  vanaf, zodat we een getal tussen 0 en 6. Met 
ADD  A,A  wordt  dit  met  twee  vermenigvuldigd, omdat  een 
sprongadres twee bytes in beslag neemt. Deze waarde wordt in 
DE gezet  en vervolgens  bij het beginadres van de tabel met 
sprongadressen  (dat in  HL staat)  opgeteld. Het  adres dat 
hier staat wordt in DE gezet, en daarna met een EX DE,HL aan 
HL  overgedragen.  De  JP  (HL) springt  tenslotte naar  het 
juiste adres. Onthoud deze constructie goed, het is de beste 
manier om een dergelijk menu te programmeren.

(Nvdr.  De tekst  was langer  dan 16  kB, u  kunt het tweede 
gedeelte in het submenu vinden.)

                                                Stefan Boer



                    - TWEEDE GEDEELTE -

                   P C M   S A M P L E R 
                                          


(Nvdr.  Dit  is  het  tweede  gedeelte.  U  kunt  het eerste 
gedeelte in het submenu vinden.)


Nu komen  de afzonderlijke  routines, die door het maken van 
een  keuze  uit  het  menu  worden aangeroepen.  De routines 
eindigen  met een  JP MAIN, waardoor het scherm wordt gewist 
en het  menu opnieuw  opgebouwd. Vervolgens kan de gebruiker 
zijn (of haar?) volgende keuze maken.


; 1) Record sample

RECORD: LD    HL,RECTXT
        CALL  PRINTS
        CALL  CHGET
        LD    HL,RECTX2
        CALL  PRINTS


Eerst  wordt er  gevraagd om  op een toets te drukken om met 
het  opnemen   te  beginnen.   Vervolgens  wordt   de  tekst 
"Recording..." op het scherm gezet.


        LD    A,(TRIGGR)
        ADD   A,A
        ADD   A,A
        ADD   A,A             ; * 8 ==> naar bit 3-6
        OR    128             ; VRAM
        LD    C,A
        LD    A,(FREQUE)
        AND   A
        JR    Z,CRNCH         ; 15.75 kHz
        SET   0,C             ; 7.875 kHz
CRNCH:  LD    A,(CRUNCH)
        AND   A
        JR    Z,REC           ; crunch mode uit
        SET   2,C             ; crunch mode aan
REC:    LD    A,C
        PUSH  AF

Eerst  samplen we  het VRAM vol. Hier wordt de juiste waarde 
voor het  A register berekend. De trigger wordt met drie ADD 
A,A  instrukties naar  bit 3-6  verplaatst. Met  een OR  128 
wordt het  VRAM bit  gezet. Tot  slot worden  nog de  juiste 
crunch en frequntie bits toegevoegd.


        LD    HL,&H4000       ; startadres in VRAM = &H04000
        LD    BC,&HC000       ; lengte in VRAM = &H1C000
        LD    DE,&H0100
        CALL  PCMREC


Het  eigenlijke  aanroepen van  de BIOS  routine wordt  hier 
gedaan. De  eerste 16 kB VRAM wordt overgeslagen, hier staat 
immmers de schermdata.


        POP   AF
        AND   &B00000111      ; wis VRAM en trigger
        LD    (PARAM),A


We  hadden de berekende waarde voor A zojuist bewaard, zodat 
we die  niet nog  een keer  hoeven te  berekenen. De trigger 
moet nu echter worden uitgezet, omdat er anders een "gat" in 
de opnamen komt bij de overgang van VRAM naar RAM. Uiteraard 
wordt ook bit 7 (het VRAM bit) gewist.


        LD    A,(MAXPGE)
        LD    B,A
RECLUS: LD    A,B
        OUT   (&HFE),A
        PUSH  BC
        LD    HL,&H8000
        LD    BC,&H4000
        LD    DE,0
        LD    A,(PARAM)
        CALL  PCMREC
        POP   BC
        DJNZ  RECLUS
        JP    MAIN


Nu is het RAM aan de beurt. Bovenstaande lus wordt voor alle 
mapper pagina's die beschikbaar zijn voor samplen doorlopen. 
De  juiste  waarde  wordt  naar  I/O  poort  &HFE  gestuurd, 
vervolgens   wordt   PCMREC   met   de   juiste   parameters 
aangeroepen.


; 2) Play sample

PLAY:   LD    HL,PLYTXT
        CALL  PRINTS
        LD    A,(FREQUE)
        OR    128             ; VRAM
        LD    HL,&H4000
        LD    BC,&HC000
        LD    DE,&H0100
        CALL  PCMPLY


Eerst   worde  tekst   "Playing..."  op  het  scherm  gezet. 
Vervolgens wordt de waarde voor het A register berekend. Bij 
afspelen  is  dit veel  simpeler dan  bij opnemen,  het gaat 
immers  alleen   om  de  frequentie  en  het  RAM/VRAM  bit. 
Vervolgens wordt de sample in het VRAM afgespeeld.


        LD    A,(MAXPGE)
        LD    B,A
PLYLUS: LD    A,B
        OUT   (&HFE),A
        PUSH  BC
        LD    HL,&H8000
        LD    BC,&H4000
        LD    DE,0
        LD    A,(FREQUE)
        CALL  PCMPLY
        POP   BC
        DJNZ  PLYLUS
        JP    MAIN


Deze lus  lijkt sprekend  op die  voor het  opnemen, verdere 
uitleg is hier dan ook overbodig. Merk op dat het A register 
nu  alleen de  frequentie bits  bevat, het  RAM/VRAM bit  is 
immers 0.


; 3) Frequency 15.75 kHz/7.875 kHz

CHFREQ: LD    A,(FREQUE)
        XOR   1
        LD    (FREQUE),A
        JP    MAIN


Hier  wordt  er  geswitchd  tussen  15.75 en  7.875 kHz.  De 
frequentie wordt aangegeven door bit 0 van (FREQUE), dit bit 
wordt  met een  XOR 1  omgeklapt. Door  de JP  MAIN wordt de 
wijziging vanzelf op het scherm zichtbaar gemaakt.


; 4) Crunch mode ON/OFF

CHCRUN: LD    A,(CRUNCH)
        XOR   1
        LD    (CRUNCH),A
        JP    MAIN


Analoog  aan   de  frequentie.  De  trigger  is  echter  wat 
moeilijker,  omdat hier een getal tussen 0 en 15 moet worden 
ingesteld.  We   zetten  daarom  eerst  een  tekstje  op het 
scherm.  Vervolgens wordt de tekst "Trigger: " op het scherm 
gezet met  daarachter de  actuele waarde. Hiervoor gebruiken 
we PRTDC2.


; 5) Change trigger

CHTRIG: LD    HL,TRGTXT
        CALL  PRINTS
CHTRG2: LD    HL,TRGTX2
        CALL  PRINTS
        LD    A,(TRIGGR)
        LD    L,A
        LD    H,0
        CALL  PRTDC2
CURSOR: CALL  CHGET
        CP    " "             ; spatie
        JP    Z,MAIN
        CP    30              ; cursor omhoog
        JR    Z,TRGUP
        CP    31              ; cursor omlaag
        JR    NZ,CURSOR
        LD    A,(TRIGGR)
        AND   A
        JR    Z,CURSOR
        DEC   A
        LD    (TRIGGR),A
        JR    CHTRG2


Bij CURSOR wordt er op een toets gewacht. Is dit een spatie, 
dan  is de  juiste waarde  blijkbaar bereikt  en kan  worden 
teruggekeerd naar  het hoofdmenu.  Voor cursor  omhoog wordt 
naar  de juiste  routine gesprongen.  Is het ook niet cursor 
omlaag, dan  moet de  gebruiker het  nog maar  eens met  een 
andere  toets  proberen  (we  springen  terug naar  cursor). 
Cursor  omlaag was  nu dus  ingedrukt, en dus controleren we 
eerst met  een AND  A of  A niet  toevallig gelijk is aan 0, 
want  dan valt  er niets te verlagen. Is dit niet het geval, 
dan wordt  A met  een verlaagd  en wordt  de nieuwe  trigger 
waarde  opgeslagen. Door  naar CHTRG2  te springen  wordt de 
nieuwe waarde op het scherm gezet.


TRGUP:  LD    A,(TRIGGR)
        CP    15
        JR    Z,CURSOR
        INC   A
        LD    (TRIGGR),A
        JR    CHTRG2


De routine  voor trigger up is praktisch gelijk aan die voor 
trigger down. Listen gaat weer net als frequentie en crunch, 
met het verschil dat er hier wordt geswitchd tussen 10 en 0. 
De  OUT (&HA5),A is eigenlijk overbodig, dit wordt immers in 
MAIN al gedaan.


; 6) Listen ON/OFF

LISTEN: LD    A,(LIST_F)
        XOR   10
        LD    (LIST_F),A
        OUT   (&HA5),A
        JP    MAIN

; 7) Quit

QUIT:   LD    HL,QUITTX
        CALL  PRINTS
        POP   AF
        OUT   (&HFE),A
        RET


Bij   het   verlaten   van  het   programma  wordt   er  een 
afscheidstekst  op  het  scherm gezet.  Vervolgens wordt  de 
inhoud  van I/O poort &HFE hersteld en wordt er teruggekeerd 
naar BASIC.


PRINTS: LD    A,(HL)
        AND   A
        RET   Z
        CALL  CHPUT
        INC   HL
        JR    PRINTS


Deze standaardroutine zet een string op het scherm. HL wijst 
naar het  eerste teken  van de  string, de  string moet zijn 
afgesloten   met  een   0.  Dit  is  een  van  de  bekendste 
standaardroutines.


MAXPGE: DS    1
PARAM:  DS    1
FREQUE: DB    0
TRIGGR: DB    0
CRUNCH: DB    0
LIST_F: DB    0
JPTAB:  DW    RECORD,PLAY,CHFREQ,CHCRUN,CHTRIG,LISTEN,QUIT


Hierboven   het   numerieke   datagebied.  Hier   worden  de 
variabelen  opgeslagen, bovendien  staat hier de sprongtabel 
voor het  menu. Hieronder  volgt de  tekstdata, alle strings 
die  in het programma met PRINTS op het scherm kunnen worden 
gezet. (Nvdr.  Omdat de layout van de Special in 60 kolommen 
is, past het hier en daar niet echt lekker.)


WELKOM: DM    12,"LONG PCM v1.0                           "
        DM    "                By Stefan Boer 25/10/92"
              ,13,10
        DM    "MSX turbo R only, no MSX-DOS 2 allowed!"
              ,13,10
        DM    "Uses 112 kB VRAM and all your available RAM"
              ,13,10,10
        DM    "Total available memory: ",0
WELKM2: DM    " kB",13,10,13,10,0
MENU:   DM    "1) Record sample",13,10
        DM    "2) Play sample",13,10
        DM    "3) Frequency 15.75 kHz/7.875 kHz",13,10
        DM    "4) Crunch mode ON/OFF",13,10
        DM    "5) Change trigger",13,10
        DM    "6) Listen ON/OFF",13,10
        DM    "7) Quit",13,10,10
        DM    "Current frequency: ",0
FREQ0:  DM    "15.75 kHz",13,10,0
FREQ1:  DM    "7.875 kHz",13,10,0
MENU2:  DM    "Crunch mode      : O",0
CRU0:   DM    "FF",13,10,0
CRU1:   DM    "N",13,10,0
MENU3:  DM    "Trigger          : ",0
MENU4:  DM    13,10,"Listen           : O",0
PROMPT: DM    13,10,"Your choice please? ",0
TRGTXT: DM    13,10,"Use cursor keys up and down, space bar
              when ready",13,10,0
TRGTX2: DM    13,"Trigger: ",0
QUITTX: DM    13,10,"Good bye!",13,10,0
RECTXT: DM    13,10,"Press any key to start recording",0
RECTX2: DM    13,10,"Recording...",0
PLYTXT: DM    13,10,"Playing...",0


Onderstaande standaardroutine  zet het  getal in  HL op  het 
scherm,  met voorloopnullen.  Deze routine  wordt een andere 
keer misschien nog eens behandeld.


; PRTDEC
; Zet getal in HL in decimaal op scherm
; Door Stefan Boer, juli 1992

PRTDEC: LD    DE,1000         ; 0-9999
        CALL  PRINT
        LD    DE,100
        CALL  PRINT
PRTDC2: LD    DE,10           ; 0-99
        CALL  PRINT
        LD    DE,1
        CALL  PRINT
        RET

PRINT:  XOR   A               ; A=0
NEXT:   LD    B,H
        LD    C,L             ; LD BC,HL
        OR    A               ; wis carry, A blijft gelijk
        SBC   HL,DE
        JP    C,EINDE
        INC   A
        JP    NEXT
EINDE:  LD    H,B
        LD    L,C             ; LD HL,BC
        ADD   "0"
        CALL  CHPUT           ; cijfer naar scherm
        RET


De  volgende  standaardroutine  is al  op de  vorige Sunrise 
Special  besproken, voor  uitleg verwijs  ik je dus naar die 
Special.


; MEMCNT
; Door Stefan Boer 19/03/92
; Uitvoer: A=aantal vrije memory mapper segmenten van 16 kB

ADRES:  EQU   &H81FF
MEMPRT: EQU   &HFE

MEMCNT: LD    HL,ADRES
        LD    DE,BUFFER
        LD    C,MEMPRT
        IN    A,(C)
        PUSH  AF
        LD    B,0
SAVE:   OUT   (C),B
        LD    A,(HL)
        LD    (DE),A
        INC   DE
        DJNZ  SAVE
        LD    B,0
        XOR   A
ZERO:   OUT   (C),B
        LD    (HL),A
        DJNZ  ZERO
        LD    D,0
        LD    B,0
CHECK:  OUT   (C),B
        LD    A,(HL)
        CP    &HFF
        JR    Z,SLAOVR
        INC   D
        LD    (HL),&HFF
SLAOVR: DJNZ  CHECK
        PUSH  DE
        LD    DE,BUFFER
        LD    B,0
RESTOR: OUT   (C),B
        LD    A,(DE)
        LD    (HL),A
        INC   DE
        DJNZ  RESTOR
        POP   DE
        POP   AF
        OUT   (C),A
        LD    A,D
        DEC   A
        RET

BUFFER: DEFS  256


                      T O T   S L O T 

In het  softwaremenu vindt  u het programmaatje LONGPCM.BAS, 
dat  de routine van disk laadt en start. Bovendien zorgt het 
voor de terugkeer naar het softwaremenu.

Ik wil er nog even op wijzen dat ik dit programma vooral heb 
geschreven  om de  leerzaamheid, en  niet om  het nut. In de 
routine komen  behalve het  samplen ook  andere dingen voor, 
zoals  het maken  van een menu. Het ging mij er uiteindelijk 
om om te laten zien hoe de BIOS routines PCMREC en PCMPLY in 
de praktijk kunnen worden gebruikt.

De volgende  keer zal  ik zoals beloofd het direct aansturen 
van   de  samplechip   bespreken,  de   frequentie  kan  dan 
bijvoorbeeld veel nauwkeuriger worden gekozen. Maar daarover 
de volgende keer meer.

                                                Stefan Boer