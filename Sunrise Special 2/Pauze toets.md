                   P A U Z E   T O E T S 
                                          

Zoals naar  ik aanneem  algemeen bekend  is heeft de turbo R 
een  PAUSE toets,  waarmee de computer kan worden stilgezet. 
Deze PAUSE toets werkt softwarematig, en kan daardoor worden 
omzeild. Dit in tegenstelling tot de PAUSE toets bij de Sony 
MSX2+ computers,  die hardwarematig  werkt. Hierbij wordt de 
computer  ECHT stilgezet, waardoor er ook problemen optreden 
bij externe  geheugenuitbreidingen die dan geen refresh meer 
krijgen.

Deze  problemen zijn  er bij de turbo R niet, omdat de PAUSE 
toets gewoon  in de interrupt routine wordt uitgelezen. Voor 
verdere  uitleg over  de interrupt routine verwijs ik u naar 
de tekst  over Interrupt  Service Routines  (ISR's) van Alex 
van der Wal op deze Special.


                   T U R B O   R   I S R 

Hieronder  een disassembly  van een  deel van  de ISR van de 
turbo R. Ik zet hier alleen het deel dat voor de PAUSE toets 
interessant is,  voor de  rest verwijs  ik u dus naar de ISR 
tekst van Alex.

Bij elke interrupt springt de computer naar &H0038, waar een 
JP naar de eigenlijke ISR staat:

&H0038  C33C0C    JP    &H0C3C

&H0C3C  E5        PUSH  HL
&H0C3D  D5        PUSH  DE
&H0C3E  C5        PUSH  BC
&H0C3F  F5        PUSH  AF
&H0C40  D9        EXX
&H0C41  08        EX    AF,AF
&H0C42  E5        PUSH  HL
&H0C43  D5        PUSH  DE
&H0C44  C5        PUSH  BC
&H0C45  F5        PUSH  AF
&H0C46  FDE5      PUSH  IY
&H0C48  DDE5      PUSH  IX
&H0C4A  CD9AFD    CALL  &HFD9A
&H0C4D  C30B1A    JP    &H1A0B
&H0C50  F2020D    JP    P,&H0D02
&H0C53  CD9FFD    CALL  &HFD9F
&H0C56  FB        EI

Zoals  u ziet  wordt er NA het aanroepen van &HFD9A naar een 
routine gesprongen op adres &H1A0B:

&H1A0B  DB99      IN    A,(&H99)        ; S#0 lezen
&H1A0D  A7        AND   A               ; vlaggen wissen
&H1A0E  08        EX    AF,AF           ; bewaren voor ISR
&H1A0F  DBA7      IN    A,(&HA7)        ; PAUSE ingedrukt?
&H1A11  0F        RRCA
&H1A12  3019      JR    NC,&H1A2D       ; nee, normale ISR
&H1A14  3AB1FC    LD    A,(&HFCB1)      ; R800 en PAUSE led
&H1A17  F601      OR    &H01            ; PAUSE led aan
&H1A19  D3A7      OUT   (&HA7),A
&H1A1B  3E01      LD    A,&H01
&H1A1D  D3A5      OUT   (&HA5),A        ; geluid uit
&H1A1F  DBA7      IN    A,(&HA7)        ; wacht tot PAUSE
&H1A21  0F        RRCA                  ; weer wordt
&H1A22  38FB      JR    C,&H1A1F        ; ingedrukt
&H1A24  3AB1FC    LD    A,(&HFCB1)      ; leds weer in
&H1A27  D3A7      OUT   (&HA7),A        ; oude stand
&H1A29  3E03      LD    A,&H03
&H1A2B  D3A5      OUT   (&HA5),A        ; geluid aan
&H1A2D  08        EX    AF,AF           ; A=S#0
&H1A2E  C3500C    JP    &H0C50          ; verder met normale
                                        ; ISR

Het  eerste gedeelte  zit ook bij de MSX2 ISR, dit is gewoon 
het  uitlezen  van  S#0  om te  kijken of  het een  50/60 Hz 
interrupt is of niet. De waarde van het A register wordt met 
een EX  AF,AF bewaard, zodat die straks weer aan de "normale 
ISR" kan worden doorgegeven.

Vervolgens  wordt  I/O poort  &HA7 gelezen.  Bit 0  van deze 
poort geeft aan of PAUSE aan is of niet. Dit bit wordt gezet 
zodra de PAUSE toets wordt ingedrukt, en blijft gezet totdat 
de PAUSE  toets nogmaals  wordt ingedrukt. Is het bit 0, dan 
is  er geen  PAUSE en  wordt er verder gegaan met de normale 
ISR.

Is er wel PAUSE, dan wordt eerst het PAUSE led aangezet. Het 
PAUSE led en het R800 led zitten ook op poort &HA7, en zoals 
we zojuist  al hebben  gezien heeft poort &HA7 bij lezen een 
andere functie, zodat de BIOS de status van de ledjes in het 
systeem  RAM  bewaard  (adres  &HFCB1).  Op  de  "oude"  MSX 
generaties  met  een  datarecorder aansluiting  was dit  het 
adres   CASPRV,  het  werd  gebruikt  voor  de  datarecorder 
routines. Aangezien de turbo R geen datarecorder aansluiting 
meer heeft wordt dit adres nu voor dit nieuwe doel gebruikt.

Enfin,  &HFCB1 wordt gelezen en bit 0 wordt gezet (het PAUSE 
led zit op bit 0, zie voor verdere uitleg over het aansturen 
van de  ledjes de speciale tekst daarover). Vervolgens wordt 
het geluid uitgezet met een OUT (&HA5),1. Dit kun je normaal 
ook gebruiken!

Hierna  wordt  poort  &HA7 constant  uitgelezen, en  gewacht 
totdat  de PAUSE toets weer wordt ingedrukt. Is dit gebeurt, 
dan wordt  eerst het  led weer in de oude stand hersteld. Ik 
zeg  expres  niet  "uitgezet",  omdat  het PAUSE  led blijft 
branden  als het  al aanstond.  Zoals u in de code kunt zien 
wordt de  waarde van  &HFCB1 gewoon uitgelezen en naar poort 
&HA7 gestuurd.

Tot slot wordt het geluid aangezet en wordt er verder gegaan 
met de normale ISR, u kunt de rest vinden in de ISR tekst.


               P A U S E   U I T Z E T T E N 

De werking van de PAUSE toets kan op een zeer simpele manier 
worden  geblokkeerd: gewoon  de interrupts uitzetten. Dit is 
echter wel  een zeer drastische oplossing, meestal heb je de 
interrupts wel nodig en kan dit niet.

Een andere methode wordt ook al in de ISR tekst aangedragen: 
in   de  routine  die  je  aan  de  hook  &HFD9A  hangt  het 
terugspringadres POPpen  evenals alle  registers die door de 
ISR  op de  stack werden  gezet. Ik verwijs hiervoor wederom 
naar de ISR tekst.

In machinetaal  is dit  een prima  oplossing, in BASIC niet. 
Alle  dingen die normaal gesproken door de ISR worden gedaan 
(toetsenbord uitlezen,  ON SPRITE  GOSUB, ON INTERVAL GOSUB, 
ON  KEY GOSUB,  ON STRIG  GOSUB) vinden nu niet meer plaats. 
Dit  geldt  uiteraard ook  voor de  oplossing waarbij  we de 
interrupts helemaal uitzetten.


                 D E   O P L O S S I N G ? 

Er  is nog een andere methode om dit probleem aan te pakken, 
waarbij de rest van de ISR helemaal intact blijft. Het grote 
nadeel is dat deze methode verre van standaard is. Ik heb de 
routine op  zowel FS-A1ST  als FS-A1GT  getest, maar het zou 
heel  goed kunnen  dat het  al niet  meer werkt zodra er een 
externe mapper wordt aangesloten. Het is zelfs vrijwel zeker 
dat dit niet zal werken bij eventuele nieuwe MSX computers.

Bij  deze routine ga ik namelijk in de ISR in het ROM zitten 
knoeien. In  ROM knoeien kan toch niet, denkt u. Daar hebt u 
gelijk  in, maar  zoals u weet heeft de turbo R de zgn. DRAM 
mode, waarin RAM wordt gebruikt als ROM. In de tekst over de 
R800  op  Sunrise Special  #1 heb  ik al  uitgelegd dat  het 
mogelijk  is  om bij  wijze van  spreken iets  in de  ROM te 
veranderen door  de ROM  mode aan  te schakelen, iets in het 
RAM   te  veranderen   dat  voor  DRAM  wordt  gebruikt,  en 
vervolgens weer  de DRAM mode aan te schakelen. Deze methode 
werkt dus alleen in samenwerking met de DRAM mode.


             I N   D E   I S R   K N O E I E N 

We hoeven  maar twee  bytes in  de ISR  te veranderen  om te 
zorgen  dat hij de PAUSE toets niet meer kan detecteren. Als 
u nog even "terugbladert" kunt u zien dat er op adres &H1A0F 
een  IN   A,(&HA7)  instructie  staat  (opcode  DB  A7).  We 
veranderen  dit gewoon  in LD  A,0 (opcode 3E 00), en de ISR 
zal het niet meer merken als de PAUSE toets wordt ingedrukt.

Dit  wordt gedaan door "PAUSEOFF.BIN", een ML routine die op 
deze   diskette   staat.  De   source  staat   er  ook   op, 
"PAUSEOFF.ASC". Hier  volgt deze  source, voorzien van extra 
commentaar.


; P A U S E O F F . A S C 
; Alleen turbo R, zet PAUSE toets uit
; Door Stefan Boer, (c) Ectry 1992
; Sunrise Special #2, (c) Sunrise 1992

        ORG   &HD000

        LD    A,&H81
        CALL  &H0180          ; R800 ROM mode
        IN    A,(&HFE)
        PUSH  AF              ; bewaar oude &HFE page
        LD    A,&HFC
        OUT   (&HFE),A        ; DRAM page met MAIN ROM


Als  de computer  opstart worden de 32 kB MAIN ROM, de 16 kB 
SUB ROM  en de  16 kB  Kanji ROM  naar de bovenste 64 kB RAM 
gekopieerd.  Dit RAM  wordt als DRAM gebruikt indien de R800 
DRAM mode  wordt aangeschakeld.  Hierbij komt de onderste 16 
kB  van het  MAIN ROM  (waar wij  in willen gaan knoeien) in 
mapper  page  &HFC terecht.  (Zie de  R800 tekst  op Sunrise 
Special  #1.)   Wij  zetten   dit  RAM  op  &H8000  met  OUT 
(&HFE),&HFC.


        LD    HL,(&H9A0F)     ; &H1A0F+&H8000
        LD    DE,&HA7DB       ; DB A7 = IN A,(&HA7)
        RST   &H20            ; controle
        JP    NZ,ERROR        ; foutmelding als niet Ok


Hier  wordt  gecontroleerd  of  op  adres &H1A0F  (dit wordt 
&H9A0F omdat het op &H8000 staat) wel een IN A,(&HA7) staat. 
Dit  als een  beperkte controle,  op deze manier merk je het 
bijvoorbeeld als het DRAM is gewist.


        LD    HL,&H3E         ; 3E 00 = LD A,0
        LD    (&H9A0F),HL     ; knoeien
        POP   AF
        OUT   (&HFE),A        ; &HFE page herstellen
        LD    A,&H82
        CALL  &H0180          ; R800 DRAM mode
        LD    HL,TEKST
        CALL  PRTXT
        RET


Hier  wordt het "patchen" gedaan, en wordt de oude &HFE page 
weer hersteld.  Tot slot wordt de DRAM mode aangeschakeld en 
melden  we met  een tekstje op het scherm dat de PAUSE toets 
nu uit staat.


ERROR:  LD    HL,ERRTXT
        CALL  PRTXT
        POP   AF
        OUT   (&HFE),A
        RET

PRTXT:  LD    A,(HL)
        AND   A
        RET   Z
        CALL  &HA2
        INC   HL
        JP    PRTXT

TEKST:  DM    13,10,"PAUSE inhibited",13,10
        DM    "(c) Ectry 1992",13,10,10,0

ERRTXT: DB    13,10,7,"No correct DRAM found",13,10,0


                     T E N S L O T T E 

Nogmaals,  dit is  zeker geen standaardroutine! Maar hij zal 
normaal  gesproken  wel  werken  en  het  voorkomt ongewenst 
gebruik van  de PAUSE  toets, terwijl  de ISR  verder gewoon 
zijn werk blijft doen.

In  machinetaal  kun  je veel  beter de  "POPjes aan  &HFD9A 
hangen" methode gebruiken, dat werkt altijd en kapt de (toch 
onnodige) ISR af.

                                                Stefan Boer
