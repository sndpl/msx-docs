H Y B R I D E   P R O G R A M M E R E N   ( 1  )


I N L E I D I N G

Dit is het eerste deel van een serie artikelen waarin ik wat
dieper in  ga op  het zogenaamde "Hybride programmeren", een
combinatie  van BASIC  en machinetaal.  In de  artikelen zal
vooral aandacht  worden besteed  aan de  hulpmiddelen die de
MSX  standaard  biedt  om  de  samenwerking  beter  te laten
verlopen,  en waar  men bij het hybride programmeren op moet
letten.

Maar  eerst zal  ik uitleggen  waar de  naam hybride vandaan
komt.


H Y B R I D E

Het woord  "hybride" komt  zowel in de Nederlandse als in de
Engelse  taal  voor  en  betekent  hetzelfde,  namelijk  het
resultaat   van  een   kruising  tussen  twee  verschillende
diersoorten.  Bekende  hybriden zijn  muilezels, muildieren,
scheiten en  gapen. Hybriden zijn altijd steriel, en bij het
fokken  moeten  afstotingsverschijnselen  worden onderdrukt.
Maar genoeg biologie, terug naar MSX.

Spreken  we  het  woord op  z'n Engels  uit (haibraid),  dan
bedoelen  we er  in de  computerwereld iets  anders mee: een
kruising tussen twee programmeertalen. Bij MSX is dat altijd
BASIC en machinetaal. Ik ga er in deze artikelen van uit dat
de  lezer  zowel  BASIC  als machinetaal  in redelijke  mate
beheerst.


G R E N Z E N

MSX BASIC  heeft een  werkgebied nodig  voor het opslaan van
een  BASIC programma en de daarbij behorende variabelen. Dit
werkgebied begint  normaal gesproken op &H8000, maar dit kan
worden  veranderd.  Het  adres  waarop  het BASIC  programma
begint wordt opgeslagen op de volgende locatie:

TXTTAB (&HF676, 2 bytes) Begin van "BASIC text area"

Op (TXTTAB)-1  moet altijd een 0 staan. Normaal gesproken is
TXTTAB  gevuld met  de waarde  &H8001, en bevat adres &H8000
dus een 0.

Na het veranderen van TXTTAB moet er een NEW worden gegeven,
zodat  ook  andere  adressen  (zoals  het beginadres  van de
variabelentabel)  kunnen worden  aangepast. Ditzelfde  wordt
gedaan  door  een  RUN"FILENAAM.BAS"  of  LOAD"FILENAAM.BAS"
instructie, er wordt dan door de BASIC interpreter eerst een
NEW uitgevoerd,  en daarna wordt het BASIC programma geladen
en (bij  RUN) ge�xecuteerd. U kunt dus na het veranderen van
TXTTAB   ook  een  RUN"FILENAAM.BAS"  of  LOAD"FILENAAM.BAS"
commando geven.

U  kunt   in  BASIC  het  startadres  van  BASIC  als  volgt
veranderen: (veranderen naar &HC000)

POKE&HF677,&HC0
POKE&HF676,1
POKE&HC000,0
NEW

Het  kan  ook  vanuit  een  programma. Stel  u wilt  dat het
programma  "EXAMPLE.BAS"  op  &HC000 staat.  U kunt  dat als
volgt programmeren:

10 'EXAMPLE.BAS
20 IF PEEK(&HF677)<>&HC0 OR PEEK(&HF676)<>1 THEN
POKE&HF677,&HC0:POKE&HF676,1:POKE&HC000,0:
RUN"EXAMPLE.BAS"
30 .....

Vanaf regel  30 kunt u uw eigen programma invullen, save het
programma  als  "EXAMPLE.BAS".  Wordt dit  programma geladen
terwijl  het adres al goed staat, dan wordt er direct verder
gegaan met  regel 30.  Anders wordt  het adres goed gezet en
laadt  het programma  zichzelf nogmaals in, alleen nu op het
goede adres.

Het gebied  vanaf &H8000 tot (TXTTAB)-1 mag door machinetaal
worden gebruikt, hier kan BASIC nooit komen.

Ook  achter het  BASIC programma  is nog een gebied dat door
machinetaal gebruikt  kan worden.  Dit kan voor BASIC worden
beschermd met de CLEAR instructie.


C L E A R

Dit   is  een   veelgebruikte  (en   meestal  noodzakelijke)
instructie bij  hybride programmeren.  Eerst maar  de syntax
van het CLEAR commando:

CLEAR [<stringruimte>][,<adres>]

Voor  de stringruimte  moet een  grootte in  bytes voor  het
datagebied voor  strings worden  ingevuld. Standaard  is dit
200.  Bij programma's die veel strings gebruiken is dat niet
genoeg, er  volgt dan een "Out of string space" foutmelding.
Zo'n  foutmelding kan  worden voorkomen door de stringruimte
met  het  CLEAR  commando  te  vergroten.  Alleen een  CLEAR
commando, dus zonder parameters, wist alle variabelen.

Voor het  adres moet  het hoogste  adres dat  door BASIC mag
worden  gebruikt worden  ingevuld. Lees  dit goed, het adres
zelf mag  dus nog door BASIC worden gebruikt. Wilt u dus een
werkgeheugen voor machinetaal vanaf adres &HD000 reserveren,
dan moet het commando als volgt luiden:

CLEAR 200,&HCFFF

En niet  CLEAR 200,&HD000,  omdat dan de eerste byte van het
machinetaalgedeelte toch door BASIC kan worden veranderd. In
een  hybride  programma  dat  het  gebied  boven  het  BASIC
programma  gebruikt moet  ALTIJD een  CLEAR commando  worden
gezet!

Met  het CLEAR commando wordt de inhoud van HIMEM veranderd,
maar CLEAR  doet nog  veel meer.  Zoals het  wissen van alle
variabelen en het verplaatsen van een aantal werkgebieden.

HIMEM (&HFC4A, 2 bytes) Bevat het hoogste adres dat door
BASIC mag worden gebruikt.

De  stack verhuist  dan mee  naar het  gebied onder (HIMEM).
(Tussen  haakjes  betekent "de  inhoud van".)  Zoals u  weet
groeit de  stack aan  de onderkant  aan. De top van de stack
wordt ook in het systeem gebied opgeslagen:

STKTOP (&HF674, 2 bytes) Bevat de top van de stack.

Behalve de stack worden ook de File Control Blocks (niet die
van de BDOS, maar van Disk BASIC, 267 bytes per stuk, aantal
in te stellen met MAXFILES) verplaatst.


D E   B O V E N G R E N S

De ondergrens  van het  machinetaalprogramma wordt  dus door
het CLEAR commando bepaald. Maar wat is de bovengrens?

Bij  een MSX computer zonder diskdrive is dat erg eenvoudig.
Het  systeemgebied  van  de  MSX  begint  namelijk op  adres
&HF380,  dus  het  gebied  tot  &HF37F  is  vrij  voor eigen
gebruik.

Bij ��n  of meer  aangesloten diskdrives  wordt de  situatie
anders. De diskROM gebruikt namelijk een stuk geheugen onder
&HF380  als werkgeheugen,  en de  grootte van dat werkgebied
kan  behoorlijk vari�ren.  Dit is afhankelijk van het aantal
diskdrives  en  de  grootte  (enkelzijdig  of  dubbelzijdig)
daarvan.

Meestal  heeft een MSX2 computer 2 softwarematige drives, en
��n  hardwarematige.   Sommige  luxe  modellen  hebben  twee
hardwarematige drives (een hardwarematige drive is een echte
diskdrive,  waar  je  een diskette  in kunt  steken, en  een
softwarematige  drive is een drive die door de diskROM wordt
ondersteund.  Heeft  een  computer  maar ��n  hardwarematige
drive,  dan  zal  de  diskROM  de  softwarematige  B:  drive
simuleren met  een "Insert  diskette for  drive B:" op de A:
drive).   De   tweede   softwarematige   drive   kan  worden
afgekoppeld  door  met  de  [CTRL]  toets  ingedrukt  op  te
starten.  Hierdoor komt  het werkgeheugen  dat eerst voor de
softwarematige B: drive werd gebruikt vrij voor gebruik door
BASIC of machinetaal.

Het  maakt   ook  uit   of  de   diskdrive  enkelzijdig   of
dubbelzijdig  is. Er  kunnen ook  nog meer diskdrives actief
zijn, als  er bijvoorbeeld  een extra losse interface in een
cartridgeslot wordt gestoken.

Tot  slot speelt  ook het actieve disk besturingssysteem een
rol. Bij Disk BASIC 1.0 (DOS1) is een veel groter werkgebied
nodig dan bij Disk BASIC 2.01 (DOS2). Kortom, u mag er nooit
vanuit  gaan  dat het  werkgeheugen van  de diskROM  op elke
computer hetzelfde is.

Waar mag  u dan  wel van uit gaan? Als u de computer opstart
bevat  HIMEM altijd de juiste waarde. Het geheugen onder dat
adres wordt niet door de diskROM gebruikt, en kan dus veilig
voor machinetaal  worden gebruikt.  BASIC zorgt  er zelf wel
voor   dat  het  werkgeheugen  van  de  diskROM  niet  wordt
overschreven.

Lees direct na het opstarten dus HIMEM uit, en controleer of
uw  machinetaal niet  boven dat  adres uit komt. Is dat toch
zo,  geef  dan  een  melding  "Reset  met  [CTRL]"  of  iets
dergelijks, en  stop de  executie van  het programma.  Is er
niets  aan de  hand, dan  kunt u  rustig verder gaan met het
programma. Als  zelfs het  opstarten met  [CTRL] niet helpt,
dan  is  kan  het programma  helemaal niet  op die  computer
worden gedraaid, in ieder geval niet in die configuratie. De
gebruiker kan dan bijvoorbeeld de C: en D: drive afkoppelen.

Het is  belangrijk om HIMEM uit te lezen voordat u een CLEAR
geeft,  omdat CLEAR de waarde van HIMEM verandert. Het CLEAR
commando hoort  trouwens vooraan  het BASIC programma, omdat
het alle variabelen wist.


V E I L I G E   B O V E N G R E N S

Bij bovenstaande  methode moet er toch een bovengrens worden
gekozen.  En het  liefst een die op alle computers werkt, en
het  liefst  een  waarbij  niet met  [CTRL] hoeft  te worden
opgestart.  Houd  de  bovengrens  altijd  zo  laag mogelijk.
&HD7FF  is  een zeer  veilige waarde,  die met  vrijwel geen
enkele configuratie problemen zal geven.


S T A C K

Houd wel  rekening met  de stack. In principe geeft die geen
problemen,  maar als  u eigenwijs  toch geen  CLEAR commando
gebruikt, dan  staat de  stack ergens een paar honderd bytes
onder de waarde van HIMEM bij het opstarten.

Misschien  is het  even handig  om de geheugenindeling onder
BASIC te geven:

- BASIC programma
- Variabelen
- Arrays
- Vrij
- Stack
- Strings
- File Control Block
- Machinetaalprogramma
- Werkgebied Disk BASIC
- Werkgebied BIOS


H I M E M   O P S L A A N

Als  de  besturing  aan  het  eind  van  het programma  weer
volledig aan BASIC wordt overgedragen, zorg dan dat HIMEM en
TXTTAB weer terug worden gezet. Zet TXTTAB daarbij op &H8001
(vergeet  niet de  POKE &H8000,0)  en zet HIMEM op de waarde
die er  bij het opstarten in stond. Om dit te kunnen doen is
het nodig dat de waarde van HIMEM direct na het opstarten op
een veilige plaats wordt bewaard.

We  weten  nu  hoe  we  ervoor kunnen  zorgen dat  het BASIC
werkgebied,   het  machinetaal  werkgebied,  het  BIOS/BASIC
werkgebied en het diskROM werkgebied elkaar niet storen, dus
we kunnen beginnen met het eigenlijke hybride programmeren.


  U S R

Bij  hybride programmeren  wordt het  USR commando van BASIC
het  meeste  gebruikt.  Met  DEFUSR  kunnen  10 machinetaal-
routines worden gedefinieerd. Dit gaat als volgt:

DEFUSR<nummer>=<adres>

Waarbij  voor <nummer>  een getal van 0 t/m 9 wordt ingevuld
(weglaten  =  0)  en  voor  <adres>  het  beginadres  van de
machinetaalroutine. Voorbeeld:

DEFUSR3=&HD000

De  machinetaalroutine  die op  adres &HD000  begint kan  nu
worden opgeroepen met USR3. De DEFUSR adressen worden in het
werkgebied  opgeslagen  in de  tabel USRTAB,  die begint  op
adres &HF39A.  Deze tabel  bevat 10 adressen van 2 bytes per
stuk,   en  is  dus  20  bytes  lang.  POKE  &HF39A,0:  POKE
&HF39B,&HC0 is dus hetzelfde als DEFUSR=&HC000.

Het  aanroepen  van  een  machinetaalroutine  gaat  door het
opvragen van  een waarde!  Het USR commando is dus eigenlijk
een  functie. Aan  het USR commando kan ��n parameter worden
meegegeven. De syntax luidt als volgt:

USR<nummer>(<parameter>)

Voor  het  <nummer>  moet  weer  een  getal  van  0-9 worden
ingevuld. De  parameter kan  niet worden weggelaten. Het USR
commando wordt als functie gebruikt. Een paar voorbeelden:

U=USR(0)
PRINT USR3("HALLO")
A$=USR2(X$)+USR5(3)
PRINT USR6(3)*USR7("D")
P=3+USR(8.8354743)
X=LEN(USR9(5))

Als  de  waarde van  USRx wordt  opgevraagd, wordt  naar het
adres  dat   met  DEFUSRx  is  gedefinieerd  gesprongen.  De
parameter   kan   door   het   machinetaalprogramma   worden
uitgelezen,   en  het   machinetaalprogramma  kan   ook  het
resultaat weer  aan BASIC  doorgeven. Dit  is dus de "waarde
van  USRx". Dit  kan een  getal zijn (INT, SNG of DBL), maar
ook een  string. Hoe gaat dit doorgeven van parameters nu in
zijn werk?

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
