
                    - EERSTE GEDEELTE -

    I N T E R R U P T   S E R V I C E   R O U T I N E S 
                                                         

Soms  zit het  mee en soms zit het tegen. Deze eerste regels 
zijn als  laatste aan het artikel toegevoegd omdat ik tot de 
ontdekking  ben gekomen  dat een dergelijk artikel al op MCM 
nummer 51 heeft gestaan. Ik ben zelf geen lid van MCM en zie 
het blad  vrij weinig.  Ik heb  de informatie  nota bene van 
Sunrise  Magazine #1  af moeten  halen waar een beschrijving 
van MCM  51 op  staat. Het is nooit de bedoeling van Sunrise 
geweest om oud nieuws te gaan vertellen, maar ik wist gewoon 
niet  dat MCM  al iets  dergelijks gedaan  heeft. Affijn, ik 
weet niet precies wat er allemaal in dat artikel staat, maar 
ik hoop  dat ik wat informatie toe kan voegen zodat dit niet 
een nutteloos artikel is.

(Nvdr. Maak  je maar  niet ongerust,  Alex. Jouw  artikel is 
veel beter en uitgebreider dan dat in MCM.)


         H E T   M A K E N   V A N   E E N   I S R 
                                                    

ISR  is  een  algemeen  gebruikte  afkorting voor  Interrupt 
Service  Routine, oftewel  een routine die wordt aangeroepen 
als er  een interrupt  optreedt. Interrupts  bestaan al lang 
als  we het over de computerhistorie hebben, want ze zijn al 
terug  te   vinden  in   de  eerste   commercieel  verkochte 
computers.  Dat  is  ook  niet  zo  gek  als je  bedenkt dat 
interrupts  heel  handig  kunnen zijn.  De populariteit  van 
interrupts  is bijna  grenzeloos en  er is tegenwoordig geen 
processor meer  in de handel die geen interrupt faciliteiten 
biedt.   Natuurlijk   zijn   er  altijd   situaties  waarbij 
interrupts  alleen maar  aan de complexiteit bijdragen, maar 
omdat  wij  allen  maar  een simpele  MSX bezitten  komt die 
situatie bij ons nauwelijks voor.


         I N T E R R U P T S   V S   P O L L I N G 

Polling is  de tegenhanger van interrupts. Bij polling wordt 
er   geen  interrupt  gegenereerd,  maar  zal  de  CPU  zelf 
periodiek naar  een randapparaat  kijken of deze iets aan te 
bieden  heeft (om  maar iets te noemen). Nu zal je misschien 
denken dat  polling de computer erg traag maakt, maar dat is 
helemaal  niet  waar.  Polling wordt  nl. o.a.  gebruikt bij 
dingen waarvan je in zekere mate kunt voorspellen dat de CPU 
er aandacht aan moet besteden zoals bij een screensplit.

We  kunnen dus rustig stellen dat polling zo gek nog niet is 
en  dat  wordt  nog  een  bevestigd  door  het  feit dat  de 
reaktietijd van  polling wel  10x zo  hoog kan  zijn als  de 
reaktietijd  van een  interrupt. Dit komt omdat de ISR eerst 
de oude  waarde van alle registers moet bewaren om daarna de 
bron van de interrupt te achterhalen. Pas na deze testen kan 
naar  de routine  gesprongen worden  die de feitelijke aktie 
moet uitvoeren.

Met polling  heb je  dit alles niet, omdat de registers niet 
bewaard  hoeven te  worden en je meteen na de test weet waar 
de interrupt vandaan komt. Vooral bij tijdkritische signalen 
is polling  dus beter  dan interrupts  omdat je veel sneller 
kunt  reageren  op  een signaal.  Natuurlijk blijft  het bij 
polling  wel  belangerijk  dat je  ongeveer weet  wanneer er 
getest  moet worden  omdat de  reaktietijd ook heel laag kan 
zijn indien  je niet  konstant aan  het pollen bent maar ook 
andere dingen doet.

Het  laatste voordeel  van polling  boven interrupts  is het 
feit  dat   je  precies  weet  waar  het  lopende  programma 
onderbroken  wordt  als  de  polling  positief  blijkt.  Met 
interrupts   moet  je   altijd  nog  oppassen  bij  bv.  het 
beschrijven van  de VDP omdat daar geen interrupt tussendoor 
mag  komen. Met  polling heb  je dit  probleem niet omdat je 
immers weet waar het optreed.


          W A N N E E R   I N T E R R U P T S ? ? 

ALs we het even over een algemene  moderne  computer  hebben 
kunnen we stellen dat interrupts  voor  twee  hoofdsituaties 
gebruikt worden en wel:
- bij het verwerken van binnenkomende data 'bursts'.
- bij het snel moeten kunnen reageren  op  een  binnenkomend 
  tijdkritisch signaal.

Een  burst is  een situatie  waarin in  korte tijd een grote 
hoeveelheid  data  binnen  kan  komen.  Een  disk-controller 
levert bv.  complete clusters  aan de  CPU die  dat dan maar 
snel  mag verwerken  omdat het volgende blok er weer aan kan 
komen (bij  MSX zit dit echter niet op een interrupt, dat is 
ook  de reden  waarom je  geen muziek af kunt spelen tijdens 
het laden).

Het  is maar  moeilijk te voorspellen wanneer de data binnen 
komt en  daarom zijn interrupts eigenlijk verplicht bij deze 
situatie omdat je anders in een polling lus moet gaan zitten 
om toch snel te kunnen reageren en niet de kans te lopen een 
complete  cluster aan data mis te lopen. Op MSX moet het dus 
met  zo'n  lus en  men is  daarin zelfs  zo ver  gaan om  de 
interrupts tijdens  het lezen  van disk stil te leggen omdat 
de  CPU anders  wel eens  tijd tekort  kan komen. Polling is 
hier dus niet de beste oplossing omdat die methode feitelijk 
90% van de tijd test op iets dat er (nog) niet is.

Bij een  screensplit is  dat echter  een heel ander verhaal. 
Daar  is elke microseconde winst een grote vooruitgang omdat 
je de  beeldopbouw nu  eenmaal niet  even stil  kunt leggen. 
Screensplits  zijn ook  heel voorspelbaar omdat ze immers op 
een vast  tijdsinterval optreden  en daarom  is polling hier 
een betere oplossing (ook vanwege de reaktiesnelheid).

Kijk  maar eens naar de bovenste screensplit van uw geliefde 
magazine. U  zult hier  twee zwarte lijnen zien die o.a. het 
gevolg  zijn van de relatief trage reaktie van de interrupt. 
Ik zal  in de  toekomst eens  proberen om die screensplit te 
vervangen  door polling  om op  die manier  ��n van  de twee 
lijnen weg te halen (je zult het dus vanzelf wel merken).

Ik heb eigenlijk niet een uitgesproken mening over de manier 
om een  screensplit te detecteren omdat het praktisch blijkt 
dat  het niet  zo gek  veel uit  maakt of  je nu  polling of 
interrupts gebruikt  om een  screensplit mee af te handelen. 
De  regel dat  je interrupts kunt gebruiken om tijdkritische 
signalen op te vangen werkt met polling net zo goed. Je moet 
er bij  polling echter  wel even  op letten waar je de tests 
uitvoert  en dat  is bij interrupts niet zo. Daar hoef je er 
alleen maar voor te zorgen dat de interrupts zoveel mogelijk 
aan staan.


      I S R   M O G E L I J K H E D E N   O P   M S X 

Laten we  maar eens  wat methoden en programmaatjes bekijken 
die  interrupts  afhandelen  en  polling  uitvoeren.  Om  te 
beginnen zullen we de interrupts behandelen.

Om een interrupt goed te kunnen begrijpen zullen we toch met 
de  hardware moeten beginnen omdat de in overvloed aanwezige 
zwarte klodders immers interrupts genereren. Daarom volgt nu 
een standaard protocol waar ALLE interrupts mee werken:

- Het randapparaat (een chip) genereert een interrupt en zet 
  deze op de INT lijn.

- Op een gegeven moment honoreert de CPU deze  interrupt  en 
  gaat afhankelijk van de processor  en  de  instelling  van 
  deze processor naar een ISR.

- Hier worden meestal eerst enkele  registers  op  de  stack 
  gezet.  Op   sommige   processoren   gebeurt   dit   zelfs 
  hardwarematig (zoals de IRQ lijn op een MC6809). Nu  wordt 
  meestal eerst de bron van de interrupt  gewist,  omdat  de 
  INT lijn van dat apparaat nog steeds aktief is. Wordt  dit 
  niet gedaan dan zal na afronding van de  ISR  meteen  weer 
  een interrupt optreden die naar dezelfde ISR springt.  Dit 
  weghalen van de oorzaak van een interrupt  wordt  nog  wel 
  eens vergeten waardoor het net lijkt of je programma  vast 
  loopt terwijl het feitelijk gewoon eeuwig in de ISR blijft 
  hangen.

- Nu wordt de feitelijke interrupt aktie uitgevoerd en nadat 
  alle registers weer van de  stack  gehaald  zijn  zal  het 
  programma dat liep voor de interrupt hervat worden.

Dan volgt nu een lijst van dingen die gebeuren  als  op  een 
MSX een interrupt optreed op de INT-lijn.

- De CPU accepteert altijd de interrupt maar hoeft hem  miet 
  noodzakelijk onmiddelijk te honoreren (dit  is  het  geval 
  als een DI instruktie gedaan is). Als de CPU de  interrupt 
  uiteindelijk toch honoreerd zal naar adres 38H  gesprongen 
  worden. Hier staat normaal gesproken een ROM met op  adres 
  38H een sprong naar de eigenlijke ISR

- In de ISR aangekomen worden eerst  alle  registers  op  de 
  stack gezet (ook schaduwregisters). Nu  wordt  naar  adres 
  FD9AH gesprongen. Zodra dit gedaan is wordt gekeken of  de 
  interrupt van het 50/60Hz signaal van de VDP afkomstig  is 
  en wordt naar FD9FH gesprongen indien dit zo is.

- Nu wordt het  toetsenbord  afgetast  en  ook  worden  alle 
  vuurknoppen gelezen en bewaard. Bij een MSX2+ en de  turbo 
  R wordt ook naar de  PAUZE  toets  gekeken  wordt  en  wel 
  nadat naar FD9AH gesprongen is.


Omdat  dit  artikel 'iets'  groter is  geworden dan  ik zelf 
gedacht had  staat het nu uitgesmeerd over 3 submenu-opties. 
De  volgende optie van het submenu bevat deel 2 en de daarop 
volgende optie deel 3.

                                           Alex van der Wal



                    - TWEEDE GEDEELTE -

    I N T E R R U P T   S E R V I C E   R O U T I N E S 
                                                         

Wat nu  volgt is  een listing  van het  voor ons belangrijke 
deel  van de  MSX2 ISR.  Nogmaals, de  ISR's van de 2+ en de 
turbo R  zijn een beetje anders (dat verklaart voor een deel 
waarom screensplits op de turbo R vaak fout gaan indien deze 
voor MSX2 geschreven zijn).

&H0038  C33C0C    JP    &H0C3C

&H0C3C  E5        PUSH  HL         ; Push registers
&H0C3D  D5        PUSH  DE
&H0C3E  C5        PUSH  BC
&H0C3F  F5        PUSH  AF
&H0C40  D9        EXX
&H0C41  08        EX    AF,AF
&H0C42  E5        PUSH  HL         ; Push schaduwregisters
&H0C43  D5        PUSH  DE
&H0C44  C5        PUSH  BC
&H0C45  F5        PUSH  AF
&H0C46  FDE5      PUSH  IY         ; Push indirektie reg.
&H0C48  DDE5      PUSH  IX
&H0C4A  CD9AFD    CALL  &HFD9A     ; Algemene INT.hook
&H0C4D  CD7914    CALL  &H1479     ; 50/60Hz VDP int. ?
&H0C50  F2020D    JP    P,&H0D02   ; Nee
&H0C53  CD9FFD    CALL  &HFD9F     ; 50/60Hz timing hook
&H0C56  FB        EI               ; Zie: Opm. 1
&H0C57  32E7F3    LD    (&HF3E7),A ; Bewaar S#0 voor BASIC
&H0C5A  E620      AND   &H20       ; Spritebotsing ?
&H0C5C  216DFC    LD    HL,&HFC6D  ; Zie: Opm. 2
&H0C5F  C4F10E    CALL  NZ,&H0EF1
&H0C62  2AA2FC    LD    HL,(&HFCA2); Voor: ON INTERVAL=
&H0C65  2B        DEC   HL
&H0C66  7C        LD    A,H
&H0C67  B5        OR    L
&H0C68  2009      JR    NZ,&H0C73
&H0C6A  217FFC    LD    HL,&HFC7F
&H0C6D  CDF10E    CALL  &H0EF1     ; Zie: Opm. 2
&H0C70  2AA0FC    LD    HL,(&HFCA0)
&H0C73  22A2FC    LD    (&HFCA2),HL; Herstart ON INTERVAL=
&H0C76  2A9EFC    LD    HL,(&HFC9E)
&H0C79  23        INC   HL
&H0C7A  229EFC    LD    (&HFC9E),HL; Softwareklok verhogen
&H0C7D  3A3FFB    LD    A,(&HFB3F) ; Doe PSG PLAY statement
&H0C80  4F        LD    C,A
&H0C81  AF        XOR   A
&H0C82  CB19      RR    C
&H0C84  F5        PUSH  AF
&H0C85  C5        PUSH  BC
&H0C86  DC3111    CALL  C,&H1131
&H0C89  C1        POP   BC
&H0C8A  F1        POP   AF
&H0C8B  3C        INC   A
&H0C8C  FE03      CP    &H03
&H0C8E  38F2      JR    C,&H0C82   ; Doe volgende PSG stem
&H0C90  21F6F3    LD    HL,&HF3F6  ; Zie: Opm. 3
&H0C93  35        DEC   (HL)
&H0C94  206C      JR    NZ,&H0D02  ; Einde ISR
&H0C96  3601      LD    (HL),&H01
&H0C98  AF        XOR   A
&H0C99  CD0212    CALL  &H1202     ; Lees ALLE vuurknoppen
&H0C9C  E630      AND   &H30
&H0C9E  F5        PUSH  AF
&H0C9F  3E01      LD    A,&H01
&H0CA1  CD0212    CALL  &H1202     ; idem.
&H0CA4  E630      AND   &H30
&H0CA6  07        RLCA
&H0CA7  07        RLCA
&H0CA8  C1        POP   BC
&H0CA9  B0        OR    B
&H0CAA  F5        PUSH  AF
&H0CAB  CD1C12    CALL  &H121C     ; idem.
&H0CAE  E601      AND   &H01
&H0CB0  C1        POP   BC
&H0CB1  B0        OR    B
&H0CB2  4F        LD    C,A
&H0CB3  21E8F3    LD    HL,&HF3E8  ; Bewaaradres vuurknoppen 
&H0CB6  AE        XOR   (HL)
&H0CB7  A6        AND   (HL)
&H0CB8  71        LD    (HL),C
&H0CB9  4F        LD    C,A
&H0CBA  0F        RRCA
&H0CBB  2170FC    LD    HL,&HFC70  ; Spatiebalk bewaaradres
&H0CBE  DCF10E    CALL  C,&H0EF1
&H0CC1  CB11      RL    C
&H0CC3  217CFC    LD    HL,&HFC7C  ; Joy 2 knop 2
&H0CC6  DCF10E    CALL  C,&H0EF1
&H0CC9  CB11      RL    C
&H0CCB  2176FC    LD    HL,&HFC76  ; Joy 2 knop 1
&H0CCE  DCF10E    CALL  C,&H0EF1
&H0CD1  CB11      RL    C
&H0CD3  2179FC    LD    HL,&HFC79  ; Joy 1 knop 2
&H0CD6  DCF10E    CALL  C,&H0EF1
&H0CD9  CB11      RL    C
&H0CDB  2173FC    LD    HL,&HFC73  ; Joy 1 knop 1
&H0CDE  DCF10E    CALL  C,&H0EF1
&H0CE1  AF        XOR   A
&H0CE2  32D9FB    LD    (&HFBD9),A ; Zie: Opm. 4
&H0CE5  CD120D    CALL  &H0D12
&H0CE8  2018      JR    NZ,&H0D02
&H0CEA  21F7F3    LD    HL,&HF3F7  ; Zie: Opm. 5
&H0CED  35        DEC   (HL)
&H0CEE  2012      JR    NZ,&H0D02
&H0CF0  3602      LD    (HL),&H02
&H0CF2  21DAFB    LD    HL,&HFBDA
&H0CF5  11DBFB    LD    DE,&HFBDB
&H0CF8  010A00    LD    BC,&H0A
&H0CFB  36FF      LD    (HL),&HFF  ; Oude toestand toetsbrd. 
&H0CFD  EDB0      LDIR             ; matrix wissen
&H0CFF  CD4E0D    CALL  &H0D4E
&H0D02  DDE1      POP   IX
&H0D04  FDE1      POP   IY
&H0D06  F1        POP   AF
&H0D07  C1        POP   BC
&H0D08  D1        POP   DE
&H0D09  E1        POP   HL
&H0D0A  08        EX    AF,AF
&H0D0B  D9        EXX
&H0D0C  F1        POP   AF
&H0D0D  C1        POP   BC
&H0D0E  D1        POP   DE
&H0D0F  E1        POP   HL
&H0D10  FB        EI
&H0D11  C9        RET

&H0EF1  7E        LD    A,(HL)
&H0EF2  E601      AND   &H01
&H0EF4  C8        RET   Z
   .    (Voor ons onbelangrijke code)
   .
   .

&H1479  DBBA      IN    A,(&HBA)   ; Lichtpen poort
&H147B  E610      AND   &H10
&H147D  200D      JR    NZ,&H148C
&H147F  21FFFA    LD    HL,&HFAFF  ; (Lightpen int request)
&H1482  CBFE      SET   7,(HL)
&H1484  3E20      LD    A,&H20
&H1486  D3BB      OUT   (&HBB),A   ; Ook lichtpen
&H1488  F608      OR    &H08
&H148A  D3BB      OUT   (&HBB),A
&H148C  DB99      IN    A,(&H99)   ; Lees statreg. 0 van VDP 
&H148E  A7        AND   A          ; Zie ook: Opm 5
&H148F  C9        RET


Opm. 1
Nadat  naar &HFD9F  is gesprongen  worden de interrupts weer 
aangezet en  dit doet  men om  snel op  bv. een  nog komende 
screensplit  te kunnen  reageren. Deze screensplit wordt dan 
dus afgehandeld  terwijl de  vorige int  ook nog 'bezig' is. 
Meestal zal dit goed gaan, maar als het fout gaat dan is het 
een  lastig te  vinden fout  omdat hij alles behalve voor de 
hand ligt.  Ik raad het vroegtijdig aanzetten van interrupts 
af  omdat  het  een  extra  onzekere faktor  aan programma's 
toevoegt.

Indien blijkt  dat de  afhandeling van een interrupt te lang 
duurt  moet je in de ISR gewoon een byte 'hoog' zetten zodat 
een aparte routine buiten de ISR zich hier op kan timen. Dit 
heeft als  gevolg dat  je een hele snelle ISR krijgt terwijl 
de  responstijd  wel  iets lager  wordt. De  methode is  dus 
alleen  bruikbaar  bij  routines  die  geen  extreem  snelle 
responstijd  nodig hebben  zoals een  muziek replay  routine 
(alhoewel  je  wel  een  volkomen  leek  moet  zijn  om  een 
replayroutine zo traag te maken dat hij niet op de interrupt 
kan!!).

Indien er  een interrupt optreedt als de INT lijn nog steeds 
aktief  is (oftewel  als de  bron van  de interrupt nog niet 
weggehaald is)  zal een andere eventueel optredene interrupt 
van  dezelfde  bron  (bv.  de  VDP) verloren  gaan en  nooit 
afgehandeld  worden. Dit  is gewoon  een hardwareprobleem en 
kan verder niet opgelost worden. Het is dus handig om liefst 
snel na het optreden van een interrupt de bron te 'wissen'.

Iets wat  veel mensen  vergeten te  doen is  het bewaren van 
register A tijdens de &HFD9F hook omdat die na deze CALL nog 
bewaard  wordt  en  de  verdere  afhandeling van  een sprite 
botsing  hangt ook nog van deze waarde af. Het is echter wel 
handig dat  je A  zelf moet  bewaren omdat  je de ISR zo een 
beetje  kunt manipuleren. Als laatste instruktie voor de RET 
van &HFD9F  zou je  A op een voor jou gunstige waarde kunnen 
zetten.  Ikzelf heb  echter nog nooit een zinvolle invulling 
aan deze mogelijkheid kunnen geven.


Opm. 2
In het systeem RAM van  de  BIOS  staat  een  tabel,  TRPTBL 
geheten met daarin een aantal  plaatsen  om  alle  mogelijke 
BASIC  interrupts  in  kwijt  te  kunnen.  Zo   heeft   elke 
funktietoets hier een byte in en ook de stop toets, een byte 
voor  de  spritebotsing,  de  spatiebalk  en  alle  joystick 
vuurknoppen.  Als  laatste  staat  hier  een  byte  voor  de 
INTERVAL instruktie  in.  Buiten  BASIC  wordt  hier  echter 
weinig gebruik van  gemaakt,  dus  ik  zal  er  verder  geen 
woorden aan vuil maken.


Opm. 3
Deze instrukiereeks  zorgt  er  voor  dat  om  de  twee  VDP 
interrupts  zowel  het  toetsenbord  als  alle   vuurknoppen 
gescanned worden. De preciese werking van dit blok  code  is 
niet belangerijk omdat  deze  code  toch  alleen  maar  data 
aanmaakt voor de eenvoudig te gebruiken BIOS calls.


Opm. 4
Om te voorkomen dat bij het printen van een ASCII dubbelcode 
twee  toetsenbordklikken  gegenereerd  worden  is  dit  byte 
ingevoerd. Ik vind dit een behoorlijk klungelig iets  om  in 
een ISR te zetten, maar de Microsoft  jongens  wisten  zeker 
niets beters.


Opm. 5
Dit  is  het  mechanisme dat  de toetsenbord  toetsherhaling 
bijhoudt.  Als dit  er niet  in had gezeten dan hadden we nu 
allemaal  heel  houterig op  ons toestenbord  moeten drukken 
omdat je anders om de haverklap een dubbel 'e' aan zou slaan 
om maar  een voorbeeld  te geven. Deze routine zorgt er voor 
dat  nadat ��n  toets ingedrukt is het even duurt voordat de 
herhaling begint.


    D I T   K A N   B E T E R   O F   N I E T   S O M S 

Natuurlijk kunnen  wij zo'n ISR veel beter schrijven dan die 
oenen  van Microsoft (geen wonder dat IBM met Apple samen is 
gaan  werken).  De  MSX  ISR is  allemaal wel  heel leuk  en 
aardig, maar  de routine bevat eigenlijk geen nuttige dingen 
voor  de gemiddelde  machinetaal programmeur.  De ISR is dan 
feitelijk ook  geschreven om BASIC wat meer mogelijkheden te 
geven  maar daar  hebben wij  weinig aan. De routine is niet 
alleen onhandig  voor ons,  maar ook  nog eens onnodig traag 
(we  gebruiken immers  bijna niets  van de informatie die de 
ISR aanmaakt).  Wat we  dus nodig hebben is een manier om al 
die  zinloze  ellende  over  te  slaan en  daar bestaat  een 
buitengewoon leuk geintje voor.


           D E   I N T E R R U P T   B Y P A S S 

Wat  we gaan  doen is  ervoor zorgen dat de interrupt alleen 
nog maar naar &HFD9A springt en de rest overslaat. Ik zal nu 
even een voorbeeld geven hoe je zoiets aanpakt.

V_BLNK:  EQU  &HFD9F

         ORG  &HFD9A     ; Ints staan uit !!!!!!!!
         JP   VDPISR

; De ISR waar alle interrupts naar toe springen
VDPISR:  IN   A,(&H99)   ; Lees S#0 en wis bron van int.
         PUSH AF
         CALL LNEISR     ; = Wat eerst op &HFD9A stond
         POP  AF
         RLCA            ; Bit 7 in Cy
         CALL C,V_BLNK   ; &HFD9F hook aanroepen (=VDP int.) 
         POP  AF         ; Haal terugkeeradres van stack
         POP  IX
         POP  IY
         POP  AF
         POP  BC
         POP  DE
         POP  HL
         EX   AF,AF
         EXX
         POP  AF
         POP  BC
         POP  DE
         POP  HL
         EI
         RET

V_BLNK is  een afkorting  voor Vertical  Blank en het is een 
standaars  benaming (op  zo'n beetje  ALLE computers behalve 
MSX) voor  de standaard 50 of 60Hz interrupt die de computer 
genereert.

Het  leuke van  deze routine  is dat je feitelijk een beetje 
met de  stack gaat  knoeien om de routine naar je eigen hand 
te  zetten. De  POP AF die na de &HFD9F call staat haalt het 
terugkeeradres van  de stack  dat er door de CALL &HFD9A van 
de  ISR op  is gezet. Nu hoef je alleen nog maar de gePUSHte 
registers weer  van de stack af te halen om de stack weer zo 
te  maken als  hij was  voordat de  interrupt optrad. De RET 
waar de  routine mee  eindigt springt  terug naar de routine 
waar de computer mee bezig was voordat de interrupt optrad.

Op deze manier sla je dus zo'n beetje alles van de standaard 
ISR over en heb de feitelijk het hele besturingssysteem plat 
gelegt.  Het voordeel  van deze methode is dat je nog steeds 
de beschikking  hebt over  de BIOS  en dat je (op een aantal 
na)  nog steeds van deze funkties gebruik kunt maken. Dingen 
als de toetsenbordaftasting werken echter niet meer, dus als 
je nu  iets van  toetsenbord wilt lezen dan zal je de matrix 
moeten scannen met bv. BIOS call &H0141.

Een ander voordeel van deze methode is het feit dat de PAUZE 
toets niet meer uitgelezen wordt op een MSX2+ en een turbo R 
computer. De  PAUZE toets  wordt dus gewoon uitgeschakeld en 
dat is bijzonder prettig.

(Nvdr.  Alleen  softwarematige  pauzetoetsen,  zoals bij  de 
turbo R, kunnen op deze manier worden uitgeschakeld. De Sony 
MSX2+ computers  hebben een  hardwarematige PAUZE  toets, en 
die is op deze manier natuurlijk niet te omzeilen.)

Deze  routine simuleert feitelijk de standaard ISR omdat bij 
elke interrupt naar LNEISR gesprongen zal en ook naar V_BLNK 
indien het  een VDP  int. is. Dit is echter niet zo'n handig 
systeem. Je kunt beter naar LENISR springen als het geen VDP 
int.  is en  anders naar V_BLNK Je springt dan dus niet meer 
naar beide  als het  een VDP  int. is.  Op deze manier is de 
reaktiesnelheid  op een  VDP int.  hoger als  bij de gegeven 
routine staat.

Het ziet er dan ongeveer zo uit:

DOINT:   IN   A,(&H99)
         RLCA
         PUSH AF
         CALL C,V_BLNK   ; VDP int eerst voor snelle reaktie 
         POP AF
         CALL NC,LNEISR  ; Alleen bij screensplit
         POP AF
         POP IX
          .
          .
         etc.

Overigens  kan  alleen de  VDP interrupts  doorgeven aan  de 
processor via  de CPU interrupt pin. Geen enkel ander IC kan 
een  interrupt aan  de CPU  doorgeven (als dit niet waar is, 
dan  hoor  ik dat  nog wel)  ook al  heeft dat  IC daar  wel 
capaciteiten voor (zoals de klokchip dat een alarm kent).

Voordat  ik  verder ga  met het  schrijven van  een kompleet 
eigen ISR  wil ik  toch eerst  even wat kwijt over de andere 
interrupts en de interrupt faciliteiten van de Z80, maar dat 
kan je allemaal op de volgende submenu-optie lezen.

                                           Alex van der Wal



                     - DERDE GEDEELTE -

    I N T E R R U P T   S E R V I C E   R O U T I N E S 
                                                         


                         M O D E S 

De Z80 kent 3 verschillende 'modes' van interruptafhandeling 
Deze  modes  kunnen  met  een  simpele  instruktie  aangezet 
worden. Ze heten:

IM 0   <= Op MSX niet gedefinieerd!!
IM 1   <= Op MSX gebruikt
IM 2   <= Op MSX niet gedefinieerd!!

Mode  0 kan  alleen worden gebruikt als de interruptbron via 
een I/O-module met de Z80 verbonden is. Bij een int. in mode 
0 moet de I/O-module nl. ��n van de 8 ReSTart instrukties op 
de databus  zetten waarna  de CPU automatisch naar dat adres 
zal  springen. Je  kunt op  deze manier  dus 8  snelle ISR's 
maken zodat de CPU niet hoeft te testen waar de int. vandaan 
komt omdat de bron dat zelf al verteld heeft. Het nadeel van 
deze methode  is dat  er een extra stuk hardware nodig is om 
alles te kunnen realiseren (de I/O-module). Ik meen te weten 
dat  het  op  MSX  wel  werkt,  maar  het  is  absoluut niet 
gedefinieerd  en het  kan dus  in principe op elk nieuw type 
MSX fout gaan. Dit geldt ook voor IM 2!

Mode 1  is de  mode die op MSX gebruikt wordt en daar hebben 
we het straks weer over.

Mode  2 heeft  hetzelfde probleem  als mode  0 want ook hier 
moet er  een I/O-module  aanwezig zijn.  Deze module  levert 
echter een 'vektor' aan de CPU en geen adres. Deze vektor is 
8 bits groot en is het LOWBYTE van een 16 bits adres waarvan 
het  HIGHBYTE in  het I-register  staat. Dit  register en de 
vektor vormen  samen een adrespointer. Op dit adres moet dan 
feitelijk  het  adres  staan  waar  de  computer  heen  moet 
springen.   Op   deze   manier  is   het  mogelijk   om  128 
verschillende  ISR's te  maken (bit  0 van  het gegenereerde 
getal is  nl. 0)  zodat zonder  ook maar 1 test te doen over 
waar  de interrupt vandaan komt naar de juiste routine wordt 
gesprongen. Ook  hier geldt  dat met  de huidige  IC's extra 
hardware nodig zal zijn om het te realiseren, maar het is in 
het  verleden toch  een veel  gebruikte methode geweest. Als 
iemand het  dus over vektor interrupts heeft, dan weet je nu 
waar hij het over heeft.


        E V E N   E E N   A A N T A L   F E I T E N 

Laten we nu eerst maar eens opsommen wat we precies weten.

Een  interrupt wordt  altijd geaccepteerd,  maar niet  nood- 
zakelijk gehonoreerd. Dit is het geval als de interrupts uit 
staan (DI).

Indien er twee interrupts optreden van dezelfde bron terwijl 
de eerste nog niet gewist is door de CPU, dan zal de  tweede 
verloren gaan. Deze situatie komt echter ERG weinig voor.

Als de interrupt gehonoreerd wordt, zal hardwarematig een DI 
gegeven worden en het is aan de schrijver van de ISR om weer 
een EI te geven (aan het eind van de  routine  bv.)  Het  is 
mogelijk dat een interrupt  geaccepteerd  wordt  terwijl  de 
interrupts uit staan.

Bij een normale INT zal er naar adres 38H gesprogen worden.

De bron van de int. zal  zo  snel  mogelijk  verteld  moeten 
worden dat de int. geaccepteerd is. De manier van de VDP die 
het INT bit zelf wist als S#0 gelezen wordt is trouwens heel 
standaard. De meeste chips die interrupts  kumnen  genereren 
werken op deze manier omdat het makkelijk en snel is.

Als een  EI instruktie  gegeven wordt  zullen de  interrupts 
niet   meteen  aktief   worden.  Ze   worden  feitelijk  pas 
ingeschakeld na  de instruktie  die op  de EI volgt. Op deze 
manier  kan je  dus eerst nog een RET doen na een EI voordat 
een geaccepteerde int. gehonoreerd wordt.


                           N M I 

De Z80  heeft echter  nog een  interrupt die  bij alle modes 
aanwezig  is, maar die op de MSX niet aangesloten is. Het is 
de Non  Maskable Interrupt  die op  bijna elke  processor te 
vinden is en ook meestal dezelfde naam heeft. Deze interrupt 
is  niet  uit  te  zetten  en  heeft  de op  ��n na  hoogste 
prioriteit  van  alle  interrupts. Als  een NMI  en een  INT 
tegelijkertijd  optreden, dan  zal eerst  de NMI afgehandeld 
worden.  Omdat  de NMI  niet uit  te zetten  is zal  een NMI 
altijd meteen  gehonoreerd worden  tenzij er  een busrequest 
optreedt  (dit is  nl. ook een soort int. maar deze heeft de 
allerhoogste prioriteit).

Als een  NMI optreedt, dan zal de Z80 zelf een DI uitvoeren, 
maar  hij bewaart  de stand  van de  ints op  het moment van 
acceptatie. Na  afhandeling van  de NMI  (die naar adres 66H 
springt)  zal de  Z80 zelf  de interrupt status terugzetten. 
Als  de  ints dus  uit stonden  voordat de  NMI optrad,  dan 
blijven ze ook uit na de afhandeling van de NMI en andersom. 
De NMI moet afgesloten worden met de RETN instruktie en niet 
met een  RET omdat  een RETN  die oude int. status hersteld. 
Bij  een normale int. mag je geen RETN geven, want dat heeft 
geen zin en kan alleen tot fouten leiden.

Een vriend  van me  heeft al  eens een  schakelaar op de NMI 
aangesloten  en dat werkt prima. Hij kan dus met een druk op 
de knop  een routine  starten die  hij zelf  eerst op de NMI 
HOOK  heeft gezet.  Dit is  erg leuk,  maar het  kan bij het 
laden van disk problemen veroorzaken. Voor de hardwaremannen 
onder ons  is het toch een leuk en simpel projekt om eens te 
doen.


                          R E T I 

Veel mensen weten van het bestaan van deze instruktie,  maar 
er zijn er maar weinig die weten waar hij voor dient. Welnu, 
deze instruktie is  helemaal  gelijk  aan  een  normale  RET 
instruktie met het verschil dat hij een andere OPCODE heeft. 
Zo zou een randapparaat dus kunnen  kijken  wanneer  de  CPU 
klaar is met het afhandelen van een ISR door de  databus  te 
scannen. Deze methode is bij mijn weten nog nooit  gebruikt. 
Het gebruik ervan is echter best handig  omdat  je  zo  heel 
snel een ISR routine kunt herkennen.


                      E I   O F   D I 

Heb je dat nu ook wel eens dat  je  wilt  weten  of  op  een 
bepaald moment de ints aan of uit  staan  ??  Lang  werd  er 
gedacht dat dit niet te testen is, maar dat  is  niet  waar. 
Het volgende programma kan dit testen:

         LD   A,R       ; Het werkt ook met LD   A,I
         PUSH AF
         POP  BC
         BIT  2,C
         JR   Z,INTUIT
INTAAN:     .
            .

In bit 2 van het F-register staat na een LD  A,R  de  inhoud 
van de IFF flip-flop. Dit bit wordt 0  gemaakt  bij  een  DI 
instruktie en 1 bij een EI.


                  D E   E I G E N   I S R 

Om  een kompleet  eigen ISR te kunnen schrijven is het nodig 
om de  BIOS weg  te halen  en deze door RAM te vervangen. Zo 
kan  je zelf  bepalen wat  er vanaf adres 38H komt te staan, 
maar voor  de rest is het precies hetzelfde principe als bij 
de  normale MSX-ISR.  Deze methode heeft alleen maar zin als 
je  HEEEL  snel op  een int.  wilt reageren  (bv. als  je 40 
screensplits  op  je  scherm  wilt  toveren).  Je  kunt  nl. 
beslissen  om  alleen  de  registers te  PUSHen die  je gaat 
veranderen  en  niet de  hele meute  zoals de  standaard ISR 
doet.


                   S L I M M E   T R U C 

Bij screensplits is het te voorspellen welke screensplit aan 
de beurt  is door een soort tellertje bij te houden. Stel je 
nu  eens voor  dat je  meerdere ISR's  hebt die  aangeroepen 
moeten worden  voor de verschillende screensplits. Als er nu 
eens  een manier  was om  voordat een  int. optreedt vast te 
stellen waar  deze heen moet springen (als deze uiteindelijk 
optreedt) dan zou dat een behoorlijke respons snelheidswinst 
betekenen.  Het enige  probleem dat  hierbij om de hoek komt 
kijken is  de ISR  zelf. Het adres waar heen gesprongen moet 
worden  staat in  het RAM.  Dus deze  waarde moet  ingelezen 
worden,  maar  hoe  doe  je dat  nu zonder  uiteindelijk een 
register te veranderen (iets dat immers niet mag in een ISR) 
Welnu, laat  ik eerst  maar de oplossing geven en daarna nog 
wat verdere uitleg.

0038H         PUSH HL
0039H         LD   HL,(ISRPNT)   ; De ISR pointer
003CH         EX   (SP),HL
003DH         RET

00F0H ISRPNT: DW &HFD9A          ; Het pointeradres

HL  moet op de stack gezet worden omdat je anders de pointer 
niet uit  kan lezen.  De EX  (SP),HL draait de pointer en de 
oude  waarde van  HL echter  om zodat  het programma  ook te 
beschrijven is als:

              PUSH (ISRPNT)
              RET

of:

              JP   (ISRPNT)

Deze   laatste   twee   oplossingen   kunnen   echter   niet 
gerealiseerd worden omdat de Z80 deze instrukties niet kent. 
In principe doen ze echter hetzelfde. Het resultaat van  dit 
alles is dus dat je voordat een int. optreedt  kunt  bepalen 
waar deze heen moet springen en deze sprong uit kunt  voeren 
zonder  een  register  van  waarde  te  veranderen.  De  RET 
instruktie van de echte oplossing verhoogt de SP immers  ook 
weer naar zijn oude waarde, dus het enige dat  verandert  is 
de PC (Program Counter) en dat was nu juist de bedoeling.

De responstijd is zo goed omdat je weet welke  en  wat  voor 
interrupt op zal gaan treden zodat  je  ook  het  feitelijke 
pollen kunt doen  voordat  de  int.  optreedt.  Als  er  dan 
uiteindelijk een int optreed hoef je alleen nog maar naar de 
juiste ISR te springen en is alle informatie al  voorbereid. 
Je moet echter goed oppassen dat je de tel niet kwijt  raakt 
omdat het dan dus echt een puinhoop wordt. Zorg er dus  voor 
dat  de  ints.  zoveel  mogelijk  aan  staan  (ook   om   de 
responstijd hoog te houden).


             D E   F E I T E L I J K E   I S R 

Hier is nu nog maar weinig over te vertellen. Het enige  dat 
je hier extra moet doen  t.o.v.  de  standaard  ISR  is  het 
PUSHen van registers die je gaat veranderen oftewel:

0038H        JP   ISR

0100H ISR:   PUSH AF
             PUSH xx
                .
             IN   A,(&H99) ; Is het een 50/60 Hz signaal??
                .
                .
             POP  xx
             POP  AF
             RET           ; Einde van int. afhandeling


                   B I O S   W E G   ? ? 

Dat is heel simpel met de volgende routine:

             LD   A,(&HF341)
             LD   H,0
             CALL &H24       ; Zet RAM over BIOS
             LD   A,RAMPGE
             OUT (&HFC),A
         [ Initialiseer de ISR ]
             EI

Nu staat  mapper page RAMPGE vanaf adres 0000H tot 3FFFH. Je 
hoeft  nu alleen  nog maar  de ISR  af te  buigen en je bent 
klaar  voor  aktie.  Na  een  CALL  op adres  &H24 staan  de 
interupts overigens  uit, en het is aan de gebruiker om deze 
daarna weer aan te zetten.

Goed,  de eigen ISR is ge�nstalleerd, maar hoe krijgen we nu 
de BIOS  weer terug??  Met CALL &H24 gaat dat niet, want die 
is  immers weggehaald!! Het zelf schakelen van sloten is een 
mogelijkheid, maar  er is  een simpeler manier. Op deze disk 
staat de assembly listing "ENASLT.ASM" waar de complete CALL 
&H24  in nagemaakt  is. Voeg  deze routine  simpelweg aan je 
eigen programma  toe en  gebruik hem inplaats van CALL &H24. 
Alle schakelproblemen zouden dan opgelost moeten zijn. Om de 
BIOS weer terug te krijgen moet je het volgende intikken.

             LD   A,(&HFCC1)
             LD   H,0
             CALL ENASLT     ; Dus niet CALL &H24


Hopelijk  zijn jullie  iets nieuws te weten gekomen uit deze 
informatie en moet het mogelijk zijn om eens lekker ISR's te 
kunnen gaan  maken. Het  resultaat zie  ik dan wel op bv. de 
Sunrise Picturedisk!!

                                           Alex van der Wal
