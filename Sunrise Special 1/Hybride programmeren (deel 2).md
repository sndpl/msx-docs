

                         - DEEL 2 -

      H Y B R I D E   P R O G R A M M E R E N   ( 1  )


(Nvdr.  Het eerste  gedeelte van  dit artikel  kunt u in het
submenu vinden.)


             D A T A   U I T W I S S E L I N G

Als  een USR  wordt aangeroepen springt de BASIC interpreter
niet meteen  naar de  machinetaalroutine, maar zal eerst een
aantal  dingen  doen.  Een  daarvan  is het  opslaan van  de
parameter die tussen haakjes staat op een vaste plaats.

Op  VALTYP (&HF663) wordt het type variabele gezet. Hiervoor
gebruikt BASIC  altijd dezelfde  getallen, die  overeenkomen
met  de ruimte  die de  opslag van  zo'n variabele in beslag
neemt (bij  de stringvariabele  is dat  exclusief de  string
zelf!). Een tabel:

(VALTYP)        Soort variabele:        Voorbeelden:
------------------------------------------------------------
2               integer (INT)           A%, 32767
3               string (STR)            A$, "Sunrise"
4               enkele precisie (SNG)   A!, 1.23456
8               dubbele precisie (DBL)  A#, 1.2345678901234
------------------------------------------------------------

Deze  waarde wordt bovendien in het A register geladen. Wilt
u  bijvoorbeeld  voordat  u  verdergaat met  de rest  van de
routine controleren  of de parameter wel een integer is, dan
ziet dat er in ML zo uit:

        CP      2               ; parameter INT?
        RET     NZ              ; nee, dan einde

Het  HL   register  bevat   altijd  de  waarde  &HF7F6,  het
beginadres  van DAC. Bij parameters van de typen INT, DBL en
SNG wordt de inhoud in DAC opgeslagen. Dit gaat als volgt:

INT:    DAC+2 bevat low byte
        DAC+3 bevat high byte

SNG:    DAC+0 t/m DAC+3 bevatten getal in BCD formaat

DBL:    DAC+0 t/m DAC+7 bevatten getal in BCD formaat

De behandeling  van het BCD formaat valt buiten dit artikel.
Dit  zal uitgebreid  aan bod komen bij de behandeling van de
Math Pack  (de rekenroutines  van de BASIC interpreter). Dit
zal  zeker op  een komende Sunrise Special worden behandeld,
het is nu nog niet bekend op welke.

Bij  een  string staat  op adres  &HF7F8 het  adres waar  de
zogenaamde "string descriptor" begint. Ditzelfde adres staat
ook in  het DE  register. De  string descriptor  ziet er als
volgt uit:

+0      lengte van de string (0-255)
+1      low byte van beginadres van string
+2      high byte van beginadres van string

Verderop in  het artikel zullen een aantal voorbeeldlistings
volgen.  Maar  nu  eerst  het  doorgeven van  variabelen aan
BASIC.

Als  de  machinetaalroutine geen  variabele aan  BASIC hoeft
terug te  geven, dan kunt u de machinetaalroutine gewoon met
een  RET be�ndigen.  Anders moet  voor de  RET eerst  DAC en
VALTYP met de juiste waarde worden gevuld.

Zet  de  juiste  waarde  in  VALTYP.  Is het  dezelfde soort
variabele als  de parameter die werd doorgegeven, en heeft u
VALTYP  in  de  routine  niet  veranderd,  dan  hoeft u  dit
natuurlijk  niet  te  doen.  Door  VALTYP  te  veranderen is
bijvoorbeeld zoiets mogelijk:

A!=USR(B$)

De waarden van de registers mogen allemaal worden veranderd.
A  hoeft  dus  niet  het  type  te bevatten  en HL  niet het
beginadres van  DAC (en  bij strings  DE niet het beginadres
van  de string  descriptor), zoals  bij de  aanroep van  een
routine door  USR. BASIC  kijkt daar  namelijk helemaal niet
naar.  Het vullen van HL en A (bij strings DE) met de juiste
waarden is alleen maar extra service van BASIC.

Waar BASIC  w�l naar  kijkt is  de inhoud  van VALTYP  en de
inhoud van DAC. Bij INT, DBL en SNG variabelen is het vullen
van  DAC met  de juiste  waarde voldoende. Bij strings is er
echter een probleem.


                       S T R I N G S

Het  doorgeven van een string aan een machinetaalroutine kan
erg handig  zijn en geeft geen enkel probleem. Het doorgeven
van  een string  door een  machinetaalroutine aan  BASIC kan
echter wel grote problemen opleveren.

De  vraag daarbij  is namelijk:  waar moet die string in het
geheugen worden  gezet? We  gaan nu even uit van de situatie
A$=USR(B$).

Als  de resultaat-string  even lang  of korter is dan B$, en
als het  niet erg is als de waarde van B$ verloren gaat, dan
is er geen probleem. Schrijf in de machinetaalroutine gewoon
het resultaat  over de  oude string  heen, en klaar is Kees.
Indien  nodig kan  in de  stringdescriptor de  lengte worden
aangepast (alleen verkort!).

Mag de  string echter niet langer worden, of als B$ niet mag
worden overschreven, waar moet de resultaat-string (die door
BASIC aan A$ wordt doorgegeven) dan naartoe?

Boven de  CLEAR grens  kan niet,  omdat BASIC  dan in de war
raakt. Strings mogen immers niet boven de CLEAR grens staan!
Ik heb deze manier zelf wel eens gebruikt, en bij mij werkte
het  prima. Tot  ik trots  mijn programma ergens anders liet
zien, en de vreemdste teksten op het scherm werden getoverd.

Het  geheugen  onder  de  CLEAR  grens wordt  door de  BASIC
interpreter  beheerd, je kan daar dus niet zomaar ergens een
string  neer gaan  zetten. De enige oplossing zou zijn om de
BASIC  interpreter  dit  werk  voor je  te laten  doen, maar
daarover heb  ik onvoldoende documentatie. Gelukkig komt het
toch   zelden  voor   dat  je  een  machinetaalroutine  wilt
schrijven waarbij het resultaat een string is.


                  H O O F D L E T T E R S

De eerste  voorbeeld assemblerlisting zet van de string alle
kleine    letters   om   in   hoofdletters.   Hier   is   de
assemblerlisting, regelmatig onderbroken voor uitleg.

; C A P S . A S M
; Zet alle kleine letters in een string om in hoofdletters
; Door Stefan Boer, 2 juli 1992
; (c) Stichting Sunrise 1992, Sunrise Special #1

FCERR:  EQU   &H475A          ; BIOS error "Illegal
                              ; function call"

        ORG   &HD000

        CP    3
        JP    NZ,FCERR        ; error als geen string

Register A  bevat de  soort variabele.  Dit moet  een string
zijn,  met code 3. CP 3 vergelijkt A met de waarde 3. Is het
niet gelijk (NZ) dan volgt er een Illegal function call.

        LD    A,(DE)
        AND   A
        RET Z                 ; controle op lege string
        LD    B,A             ; B=lengte
        INC   DE
        LD    A,(DE)
        LD    L,A
        INC   DE
        LD    A,(DE)
        LD    H,A             ; HL=beginadres

Hier  wordt  de stringdescriptor  gelezen. De  descriptor (=
"beschrijver") begint  op het  adres waar  DE naar wijst. De
lengte  van de string wordt in B gezet en het beginadres van
de string  in HL. Als het een lege string is wordt er meteen
teruggesprongen naar BASIC.

LOOP:   LD    A,(HL)
        CP    "a"
        JR    C,NEXT
        CP    "z"+1
        JR    NC,NEXT
        RES   5,A             ; maak hoofdletter van
                              ; kleine letter
        LD    (HL),A
NEXT:   INC   HL
        DJNZ  LOOP            ; herhaal voor hele string

        RET                   ; VALTYP en DAC hoeven niet
                              ; te worden veranderd

Dit gedeelte zet de kleine letters om in hoofdletters. Eerst
wordt er  gecontroleerd of het teken wel tussen a en z ligt.
Zo  ja, dan  wordt bit  5 gereset.  Met een voorbeeld kunt u
zien hoe dat werkt. Een paar binaire ASCII codes:

A       &B01000001
a       &B01100001
S       &B01010011
s       &B01110011
            ^
            bit 5

Zoals  u ziet  bepaalt bit  5 of  het een  hoofd- of  kleine
letter is (0=hoofd, 1=klein).

De string wordt in dit voorbeeld gewoon overschreven. Stel u
heeft   bovenstaande   assemblerlisting   geassembleerd,  de
machinetaal  staat  dus  op  adres  &HD000. Dan  kunt u  het
volgende BASIC programma uitvoeren:

10 DEFUSR=&HD000
20 A$="Sunrise"
30 B$=USR(A$)

In  dit geval  bevatten zowel A$ als B$ na afloop "SUNRISE".
Mag de  waarde van  A$ niet  verloren gaan,  dan kunt  u het
voorbeeld zo doen:

10 DEFUSR=&HD000
20 A$="Sunrise"
30 Q$=A$:B$=USR(Q$)

Er gebeurt iets grappigs als u met een constante werkt:

10 DEFUSR=&HD000
20 U$=USR("Sunrise")

Na  het  RUNnen  van  dit  programma bevat  de variabele  U$
uiteraard "SUNRISE". Typt u nu maar eens LIST in. Dit is wat
u dan te zien krijgt:

10 DEFUSR=&HD000
20 U$=USR("SUNRISE")

Dus  ook in  het BASIC programma is de tekst in hoofdletters
omgezet.  Dit  komt  omdat  de BASIC  interpreter de  string
descriptor in  dit geval  gewoon naar de constante "Sunrise"
in het BASIC programma laat wijzen.

U snapt nu wel dat het doorgeven van strings van machinetaal
aan BASIC nogal problematisch gaat. Andersom gaat het prima.


               T E K S T J E   P R I N T E N

Dit  is  een  goed voorbeeld  van een  string doorgeven  aan
machinetaal. Onderstaande assemblerlisting zet de string die
als parameter aan USR wordt doorgegeven op het scherm.

; P R I N T S T R . A S M
; Print een string
; Door Stefan Boer, 2 juli 1992
; (c) Stichting Sunrise 1992, Sunrise Special #1

FCERR:  EQU   &H475A          ; BIOS error "Illegal
                              ; function call"

        ORG   &HD000

        CP    3
        JP    NZ,FCERR        ; error als geen string

        LD    A,(DE)
        AND   A
        RET   Z               ; lege string
        LD    B,A             ; B=lengte
        INC   DE
        LD    A,(DE)
        LD    L,A
        INC   DE
        LD    A,(DE)
        LD    H,A             ; HL=beginadres

Dit  gedeelte  is bijna  gelijk aan  de vorige  routine, het
haalt de stringdescriptor op.

PRINT:  LD    A,(HL)          ; haal teken
        CALL  &HA2            ; teken naar scherm
        INC   HL              ; volgende teken
        DJNZ  PRINT           ; herhaal B maal

        RET

Dit  gedeelte zet  de string  op het  scherm, en gebruikt de
BIOS routine  CHPUT (&HA2).  De volgende routine bewijst dat
het  doorgeven van  getallen in  beide richtingen geen enkel
probleem oplevert.


                       R E K E N E N

Onderstaande  routine  doet iets  ontzettend moeilijks,  hij
telt 1  op bij  het getal  dat als  parameter aan  USR wordt
meegegeven!!! Geweldig he?!

; P L U S 1 . A S M
; Telt 1 op bij een getal
; Door Stefan Boer, 2 juli 1992
; (c) Stichting Sunrise 1992, Sunrise Special #1

DAC:    EQU   &HF7F6
ILLEGA: EQU   &H475A

        ORG   &HD000

        CP    2
        JP    NZ,ILLEGA       ; Illegal function call als
                              ; geen integer
        LD    HL,(DAC+2)      ; lees waarde
        INC   HL              ; plus 1
        LD    (DAC+2),HL      ; geef waarde door aan BASIC
        RET

Deze routine  behoeft geen  uitleg meer,  alle uitleg  is al
eerder  in dit  artikel gegeven.  Toch zit  er hier  wel een
addertje onder het gras. Typt u bijvoorbeeld:

PRINT USR(3)

Dan  zet  de computer  netjes een  4 op  het scherm.  Typt u
echter:

A=3
PRINT USR(A)

Dan  volgt er  een Illegal  function call.  Hoe kan dat? Dat
komt omdat  alle variabelen  (tenzij anders  bepaald met een
DEF commando) automatisch van het DBL type zijn. Overal waar
A  staat moet u dus A# lezen. En natuurlijk krijg je dan een
illegal  function  call,  omdat  VALTYP dan  een 8  bevat in
plaats van een 2. Deze fout kan worden voorkomen door DEFINT
A te typen, of door achter elke A een % te plaatsen.

DEFINT A
A=3
PRINT USR(A)

Dit  werkt wel.  Nog een  laatste opmerking:  alle registers
mogen worden  veranderd door  een routine  die met USR wordt
aangeroepen  (behalve  natuurlijk  SP,  want  dan kan  er na
afloop  niet  meer  worden  teruggesprongen  naar  de  BASIC
interpreter).


                      T O T   S L O T

Dit lijkt  me wel voldoende voor deze keer, de eerste manier
van  samenwerking tussen  BASIC en  ML (USR)  is nu volledig
besproken. De volgende keer komt het volgende BASIC commando
voor deze  samenwerking aan bod: CMD. Ik heb al een leuke en
vooral handige voorbeeldroutine klaarliggen.

Wie  heeft er  meer ervaring met het stringprobleem, en weet
wel  een  goede oplossing?  Reacties graag  naar de  Sunrise
postbus, ter  attentie van  ondergetekende. Ik  ben zelf  al
bezig  geweest met  een oplossing, dus misschien kom ik daar
de volgende keer zelf mee op de proppen.

                                                Stefan Boer
