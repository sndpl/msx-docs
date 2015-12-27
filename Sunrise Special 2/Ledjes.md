                        L E D J E S 
                                     

Zoals  u weet  heeft de turbo R een schitterende rij ledjes. 
Van deze  zes ledjes zijn vier eenvoudig met I/O poorten aan 
te  sturen. Het power ledje is volgens mij helemaal niet aan 
te sturen,  en de  aansturing van  het "FDD IN USE" ledje is 
bij  mij onbekend.  Als er  lezers zijn  die dit  wel weten, 
schrijf dan even een briefje naar de Sunrise postbus.

Met de  ledjes kun  je leuke  dingen doen. Ze zijn voorbeeld 
geschikt  voor een  simpele output indicator voor muziek, of 
een simpele input indicator voor samples.


                   L E D J E S   S H O W 

Hier doen  we er  iets anders  mee, we laten de ledjes op de 
interrupt een bepaald patroontje uitvoeren, wat er heel leuk 
uitziet.

In  het  softwaremenu  vindt  u  het programma  LED.BAS, dat 
eigenlijk  weinig anders doet dan een BLOAD"LED.BIN",R. Deze 
routine hangt  zichzelf aan de interrupt en geeft vervolgens 
de besturing weer terug aan BASIC.

We gaan  nu de  assemblerlisting van LED.BIN bespreken. Deze 
assemblerlisting  staat ook  als LED.ASC  op de  disk. Zoals 
gebruikelijk  onderbreek   ik  de  listing  regelmatig  voor 
uitgebreide uitleg.


; L E D . A S M 
; Show met de ledjes van de MSX turbo R
; Alleen turbo R!!!
; Door Stefan Boer 21/04/92 - 21/05/92 - 19/10/92 - 25/10/92
; Sunrise Special #2 - (c) Stichting Sunrise 1992
; Eventueel op &HD006-&HD007 beginadres tabel POKEn
; &HD000 aanroepen om te starten
; &HD003 aanroepen om te stoppen
; Commando struktuur: (binair)
; 0 - - - C K P R --> lampjes besturen
;                     C=caps, K=kana, P=pause, R=R800
; 1 T T T T T T T --> wachttijd instellen (dus 128+tijd)
; 1 1 1 1 1 1 1 1 --> einde tabel (&HFF)

HOOK:   EQU   &HFD9F          ; H.TIMI
WRTPSG: EQU   &H93            ; BIOS PSG register schrijven
                              ; (voor kana led)
RDPSG:  EQU   &H96            ; BIOS PSG register lezen
                              ; (voor kana led)
WACHT:  EQU   5               ; default wachttijd

        ORG   &HD000

        JP    INIT            ; initialiseer en buig int af
        JP    EXIT            ; herstel interrupt

ST_TAB: DW    TABEL           ; beginadres aan routine
                              ; doorgeven
AD_TAB: DS    2               ; ruimte om huidige plaats
                              ; in tabel op te slaan
W_A7:   DS    1               ; ruimte opslag poort &HA7
TIME:   DB    WACHT           ; wachttijd
TELLER: DS    1               ; ruimte om tijd bij te houden


De  ledjes  show  wordt  gestart  door  adres &HD000  aan te 
roepen, en weer gestopt door &HD003 aan te roepen. Op ST_TAB 
(adres  &HD006  en &HD007)  kan ook  een ander  adres worden 
gezet, zodat  het startadres  van de  tabel met  de data met 
twee   simpele  POKEs  kan  worden  verwijderd.  De  default 
wachttijd is  5, dit  betekent dat  om de vijf interrupts de 
"stand" van de ledjes wordt gewijzigd.


INIT:   LD    HL,HOOK
        LD    DE,OLDHOK
        LD    BC,5
        LDIR                  ; bewaar oude hook
        DI
        LD    A,&HC3
        LD    (HOOK),A
        LD    HL,LEDPRG
        LD    (HOOK+1),HL     ; zet JP HOOK op &HFD9F
        LD    HL,(ST_TAB)     ; startadres tabel
        LD    (AD_TAB),HL     ; init pointer
        LD    A,WACHT
        LD    (TIME),A        ; zet default wachttijd
        XOR   A
        LD    (TELLER),A      ; teller op 0 zetten
        EI
        RET


Dit is  de initialisatieroutine.  Eerst wordt  de oude  hook 
bewaard,  dit is nodig voor het terugzetten van de oude hook 
en bovendien  zijn zo  geneste hooks  mogelijk. De oude hook 
wordt   daarvoor   aangeroepen   nadat   onze   routine   is 
afgehandeld.

De  adrespointer wordt  ge�nitialiseerd door  het startadres 
van  de  tabel  in  te  vullen.  Bovendien wordt  de default 
wachttijd ingesteld, en wordt de teller die bijhoudt hoeveel 
interrupts er al zijn geweest op 0 gezet.


EXIT:   DI
        LD    HL,OLDHOK
        LD    DE,HOOK
        LD    BC,5
        LDIR                  ; herstel oude hook
        EI
        RET


Hiermee wordt  de ledjes  show stopgezet, de oude hook wordt 
weer hersteld.


LEDPRG: PUSH  AF              ; verplicht bij gebruik &HFD9F
        LD    A,(TIME)
        LD    HL,TELLER
        INC   (HL)            ; teller verhogen
        CP    (HL)            ; klaar met wachten?
        JP    NZ,ENDINT       ; nee, dan einde interrupt
        XOR   A
        LD    (HL),A          ; teller op 0 zetten


Bij binnenkomst wordt eerst AF bewaard. Dit is verplicht als 
je  de routine  onder BASIC wilt gebruiken!! Anders werkt ON 
SPRITE GOSUB  en het opvragen van statusregister 0 niet meer 
goed.  AF bevat namelijk de waarde van statusregister 0, die 
wordt pas NA het aanroepen van &HFD9F opgeslagen.

Dit stukje  code zorgt  ervoor dat  steeds het juiste aantal 
interrupts  wordt overgeslagen.  A wordt geladen met (TIME), 
het  aantal  interrupts  dat  moet  worden  overgeslagen, en 
(TELLER) wordt  met ��n  verhoogd. Is  de juiste  waarde nog 
niet  bereikt, dan  wordt de  interrupt be�indigd  door naar 
ENDINT te  springen. Anders  wordt de  teller op  0 gezet en 
gaan we verder met de rest van de routine.


        LD    HL,(AD_TAB)     ; adres commando inlezen
GETCMD: LD    A,(HL)          ; haal commando op
        CP    255             ; einde tabel?
        JP    Z,EINDE
        BIT   7,A             ; chtime of normaal commando?
        JP    NZ,CHTIME       ; CHange TIME
        LD    C,A
        INC   HL
        LD    (AD_TAB),HL     ; nieuwe pointer wegzetten


Hier  wordt eerst  het commando opgehaald. Is dit gelijk aan 
255, dan  is het  einde van  de tabel  bereikt en wordt naar 
EINDE  gesprongen (er  wordt weer aan het begin van de tabel 
opnieuw begonnen).  Is bit  7 gezet, dan betekent dit dat de 
wachttijd  veranderd  wordt.  Ook hiervoor  is een  speciale 
routine.

Anders is  het een  getal tussen  0-15 dat de gewenste stand 
van  de ledjes weergeeft. Dit getal wordt in C bewaard en de 
pointer  wordt   naar  de   volgende  positie  in  de  tabel 
verplaatst.


; Commando uitvoeren
; A-register: 0 - - - C K P R
; C=caps, K=kana, P=pause, R=R800
; 0=uit, 1=aan

        IN    A,(&HAA)
        RES   6,A
        BIT   3,C             ; caps aan/uit?
        JR    NZ,CAPS
        SET   6,A             ; caps uit
CAPS:   OUT   (&HAA),A


Dit stukje code bestuurd het caps lampje, dat wordt bestuurd 
door bit 6 van poort &HAA. Bit 6 heeft overigens een inverse 
werking, als  het bit  aan is, is het ledje uit! Eerst wordt 
poort  &HAA uitgelezen. Bit 6 wordt alvast gewist, zodat het 
lampje aangaat.  Dan wordt  bit 3  van C gelezen. Is dit bit 
aan,  dan is  de waarde  van het A register al goed en wordt 
die bij  het label  CAPS: naar  poort &HAA  gestuurd. Anders 
wordt bit 6 eerst gezet (ledje uit).


        LD    A,15
        CALL  RDPSG
        RES   7,A
        BIT   2,C             ; kana aan/uit?
        JR    NZ,KANA
        SET   7,A             ; kana uit
KANA:   LD    E,A
        LD    A,15
        CALL  WRTPSG


Het  kana  lampje  zit  op  register 15  van de  PSG! Bit  7 
bestuurd  het led,  ook dit  bit heeft  een inverse werking. 
Eerst wordt  register 15  van de  PSG gelezen  via de  BIOS. 
Vervolgens  wordt  via  dezelfde  methode  als bij  het caps 
lampje  bit 7  op de  juiste wijze  veranderd en  vervolgens 
wordt  register   15  via  de  BIOS  met  de  nieuwe  waarde 
beschreven.


        XOR   A               ; pause en R800 led uit
        BIT   1,C             ; pause aan/uit?
        JR    Z,R800
        SET   0,A             ; pause led aan
R800:   BIT   0,C             ; R800 aan/uit?
        JR    Z,WR_A7
        SET   7,A             ; R800 led aan
WR_A7:  OUT   (&HA7),A


Het R800  ledje en  het PAUSE  ledje zitten  allebei op  I/O 
poort  &HA7. Het R800 led zit op bit 0, het PAUSE led op bit 
7. Omdat  beide ledjes  op dezelfde  poort zitten, sturen we 
deze  uiteraard ook  maar ��n  keer uit.  Eerst wordt  met A 
gelijk gemaakt  aan 0  met een  XOR A.  Vervolgens worden de 
bits  0 en  7 van  A aan de hand van bit 0 en 1 van C indien 
nodig aangezet.  Tenslotte wordt  de waarde  naar poort &HA7 
gestuurd.


ENDINT: POP   AF              ; einde introutine, herstel AF
                              ; (=S#0 voor BASIC)
OLDHOK: RET                   ; hier komt oude interrupt
        RET
        RET
        RET
        RET


Einde  van de hoofdroutine. Aan het begin van de routine heb 
ik  al  uitgelegd  waarom  AF moet  worden bewaard.  Over de 
RETjes wordt  door de initialisatieroutine de oude interrupt 
hook  neergezet. Hierdoor  zijn geneste interrupts mogelijk, 
en kan  er bijvoorbeeld MoonBlaster muziek worden afgespeeld 
tijdens de ledjes show.


; Commando 128+tijd (1-127) verandert wachttijd

CHTIME: RES   7,A             ; wis bit 7
        LD    (TIME),A        ; nieuwe wachttijd
        INC   HL
        JP    GETCMD          ; ga verder met volgende cmd


Hier  wordt een  "change time"  commando afgehandeld. Na het 
instellen van  de nieuwe  wachttijd wordt weer direct verder 
gegaan met het volgende commando.


; Commando &HFF: begin opnieuw in tabel

EINDE:  LD    HL,(ST_TAB)
        LD    (AD_TAB),HL     ; zet adres op begin tabel
        JP    GETCMD          ; ga verder met eerste cmd


Hier wordt het "einde van de tabel" commando afgehandeld. De 
adrespointer  wordt weer  op het begin van de tabel gezet en 
er wordt direct verdergegaan met het volgende commando.


; tabel met commando's, vergeet niet 255 laatste data

TABEL:  DB    128+10,15,0,15,0,15,0,15,0,15,0,15,0
        DB    5,10,5,10,5,10,5,10,5,10,5,10,5,10,0
        DB    15,0,15,0,15,0,15,0
        DB    1,3,7,15,14,12,8,0,8,12,14,15,7,3,1,0
        DB    3,0,3,0,12,0,12,0,3,0,3,0,12,0,12,0
        DB    128+5,1,2,4,8,4,2,1,2,4,8,4,2,1,2,4,8,4,2,1
        DB    2,4,8,4,2,1,2,4,8
        DB    4,2,1,2,4,8,4,2,1,2,4,8,4,2,1,2,4,8,4,2,1,255


Deze  tabel  bevat  voorbeelddata.  Deze data  is als  volgt 
opgebouwd:

MSB   7     6     5     4     3     2     1     0   LSB

      0     -     -     -    CAPS  KANA PAUSE  R800

of    1     --------------wachttijd--------------


En natuurlijk 255 voor het einde van de tabel. Het komt erop 
neer  dat  128+tijd  (0-127)  voor  het  veranderen  van  de 
wachttijd staat  en de  waardes 1 voor pause, 2 voor R800, 4 
voor  kana  en  8 voor  caps moeten  worden opgeteld  om een 
commando  te krijgen  dat de  stand van de ledjes verandert. 
Veel plezier met het zelf maken van leuke patroontjes!


                      T O T   S L O T 

Ik  hoop  dat  dit  een  leerzaam  artikel  was. Behalve  de 
aansturing  van de ledjes zijn ook andere technieken aan bod 
gekomen  zoals  het  ophalen  van  een  commando,  interrupt 
afbuigen,  een  interruptroutine  om  de  zoveel  interrupts 
uitvoeren, het behandelen van een commando, etc.

Nog  iets grappigs:  omdat het natuurlijk erg stom staat als 
het ren sha autofire led gewoon door blijft knipperen als de 
computer in  de pause  stand staat, hebben de ontwerpers van 
de turbo R dit led aan het pause led geschakeld. Het ren sha 
led gaat daardoor uit als het pause led aan is.

Dit  geeft  een leuk  effect in  combinatie met  mijn ledjes 
routine. Zet  de ren  sha autofire  maar eens  aan en  start 
vervolgens  de ledjes routine. Iedere keer als het pause led 
aangaat stopt het rensha led!

                                                Stefan Boer
