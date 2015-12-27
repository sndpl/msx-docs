        M S X 2 / 2 +   V D P   C U R S U S   ( 1  )
                                                     


                     H E R H A L I N G 

Deze  cursus  is  al eerder  verschenen op  de ClubGuide  en 
Sunrise  Magazine, vanaf  ClubGuide #7.  Omdat ik tientallen 
brieven van  lezers heb  gekregen die de eerste delen hebben 
gemist,  heb ik besloten om de cursus opnieuw te publiceren, 
nu op de Sunrise Special.

Een andere  reden om  de cursus opnieuw te publiceren is dat 
ik  intussen meer  weet over  de VDP, zodat ik de cursus nog 
beter kan  maken. De  tekst zal ik grotendeels herschrijven, 
de oude cursus dient eigenlijk vooral als basismateriaal. Ik 
zal  vooral meer  aandacht besteden aan het programmeren van 
de VDP  in ML.  Hierdoor is  de VDP  cursus op herhaling ook 
interessant  voor de  lezers die  de cursus  al vanaf deel 1 
hebben gevolgd. Veel plezier met de VDP cursus op herhaling!


                     I N L E I D I N G 

Je kunt veel meer uit de VDP halen door hem rechtstreeks aan 
te  sturen.   Zelfs  in   BASIC  zijn  er  al  aanmerkelijke 
snelheidsverbeteringen  mee te realiseren. Alle schitterende 
VDP truuks die u in de demo's op de Sunrise Picturedisk kunt 
bewonderen worden  bereikt door  het rechtstreeks  aansturen 
van de VDP. Het moet in principe mogelijk zijn dat u aan het 
eind  van de  cursus ook  dergelijke truuks  met de VDP kunt 
uithalen,  mits   er  natuurlijk  voldoende  kennis  van  ML 
aanwezig is.

Dit artikel  is zowel  voor MSX2  als voor  MSX2+-gebruikers 
bestemd.  De V9938  (MSX2) en de V9958 (MSX2+) zijn namelijk 
grotendeels hetzelfde.  Alleen waar er verschillen zijn, zal 
het  artikel opsplitsen  in een  MSX2 en  een MSX2+ deel. De 
informatie  in  dit artikel  is zeer  betrouwbaar, want  het 
meeste komt  rechtstreeks uit  de Technical  Data Books  van 
Yamaha. (Inmiddels is er ook de turbo R, die echter dezelfde 
VDP bezit als de MSX2+.)


            L E Z E N   E N   S C H R I J V E N 

             V A N   V D P - R E G I S T E R S 

Voordat  we  iets  met  de  VDP  kunnen  doen  is  het eerst 
noodzakelijk  om te weten hoe de VDP kan worden aangestuurd. 
Het aansturen  van de  VDP registers  kan op  drie manieren, 
namelijk  in BASIC  met het VDP commando, in machinetaal met 
BIOS calls  en in  machinetaal door rechtstreeks I/O poorten 
te  lezen/schrijven. Ik  zal deze  methodes nu  ��n voor ��n 
bespreken.


                         B A S I C 

In BASIC  kun je  VDP-registers uitlezen  of beschrijven met 
het  VDP(N) commando. Voor N moet je meestal NIET het regis- 
ter nummer  invullen, zoals die verder in dit artikel worden 
gebruikt.   Een  register   wordt  aangegeven  met  R#nummer 
(bijvoorbeeld R#23)  en een  statusregister met S#nummer. In 
BASIC moet je de volgende omrekentabel gebruiken:

Register:          N:                  Lezen/Schrijven:
------------------------------------------------------------
R#00-R#07           0- 7               L/S
R#08-R#23           9-24               L/S
R#25-R#27          26-28 (alleen 2+)   L/S
R#32-R#46          33-47               L/S
S#00                 8                 L
S#01-S#09          -1 - -9             L
------------------------------------------------------------

Schrijven  doe je  met VDP(N)=waarde  en lezen met W=VDP(N). 
Bijvoorbeeld VDP(10)=0 of PRINT VDP(1).


                   M A C H I N E T A A L 

In  machinetaal  kun  je  voor  het lezen/schrijven  van VDP 
registers  de routines  gebruiken die  al in  het ROM van je 
MSXje zitten. Dat zijn:

&H0047     WRTVDP Schrijft naar VDP-register.
           Invoer: C = register, B = data
           Verandert: AF,BC
&H013E     RDVDP  Lezen van S#0 van VDP.
           Uitvoer: A = data
&H012D EXT WRTVDP Schrijft naar VDP-register.
           Invoer: C = register, B = data
           Verandert: AF,BC

(EXT betekent  dat de  routine in het Extended ROM staat. Je 
moet zo'n routine aanroepen door het adres in IX te laden en 
daarna CALL &H015F te geven.)

&H0131 EXT VDPSTA Lezen van statusregister van VDP.
           Invoer: A = register (0-9)
           Uitvoer: A = data
           Wijzigt: F

Het  lezen van  niet-statusregisters is  niet mogelijk.  Wel 
wordt er  door de BIOS routines een tabel bijgehouden van de 
VDP  registers  in  het systeem  RAM. Deze  tabel is  alleen 
betrouwbaar als de VDP registers alleen via de BIOS routines 
worden beschreven, of als u er zelf voor zorgt dat u bij het 
schrijven  van een  register de  waarde in het RAM bijwerkt. 
Dit verbetert  vaak de  samenwerking met  andere programma's 
(bijvoorbeeld MemMan TSR's).

De  inhoud van  de VDP  registers wordt  door het BIOS op de 
volgende plaatsen in het systeem RAM bijgehouden:

R#0-R#7         &HF3DF-&HF3E6
R#8-R#23        &HFFE7-&HFFF6
R#25-R#27       &HFFFA-&HFFFC

De  commandoregisters  R#32-R#46  worden niet  (of in  ieder 
geval  niet op  deze manier)  in het systeem RAM opgeslagen. 
Dit is  in principe  ook onzin,  omdat deze registers na het 
uitvoeren  van het commando hun oorsponkelijke waarde hebben 
verloren.  (N.B.  Bij gebruik  van de  VDP door  de SUB  ROM 
routines  voor   COPY  e.d.  worden  de  commando  registers 
uiteraard wel in het systeem RAM gezet, maar dit is voor ons 
verder niet interessant.)


               V I A   I / O   P O O R T E N 

1) Direct

Schrijf eerst de data en daarna het registernummer naar port 
#1, waarbij  bit 7 is gezet. Officieel moet de I/O poort uit 
het  ROM worden  gelezen, maar  onderstaande waardes  gelden 
altijd, en mogen dus gewoon gebruikt worden:

port #0            I/O &H98
port #1            I/O &H99
port #2            I/O &H9A
port #3            I/O &H9B

Het gaat dus volgens het volgende schema: (binair)

                  MSB  7  6  5  4  3  2  1  0  LSB
Port #1 First Byte    D7 D6 D5 D4 D3 D2 D1 D0   DATA
        Second Byte    1  0 R5 R4 R3 R2 R1 R0   REGISTER #


In machinetaal ziet dit er zo uit:

        DI
        LD   A,100
        OUT  (&H99),A
        LD   A,23+128
        OUT  (&H99),A
        EI

Hier wordt  de waarde 100 naar R#23 geschreven. Door 128 bij 
het  registernummer op  te tellen wordt bit 7 gezet, waarmee 
we de  VDP vertellen  dat hij het getal dat hij daarvoor had 
ontvangen  naar  een  register moet  schrijven, waarvan  het 
nummer in de onderste zes bits staat.

De  interrupts  moeten  hierbij  altijd  uitstaan,  omdat er 
anders tussen  de OUTs  een interrupt kan optreden, waardoor 
het misgaat omdat er in de interrupt meestal wel iets met de 
VDP wordt gedaan. Als de interrupts al uit stonden kunt u de 
DI  en EI  natuurlijk weglaten,  de EI kan natuurlijk worden 
weggelaten  als  de  interrupts  uit  moeten  blijven  staan 
(bijvoorbeeld   omdat   er  nog   een  VDP   register  wordt 
beschreven).

Je zou  dit natuurlijk  ook in een subroutine kunnen zetten, 
maar dat heeft weinig nut omdat het zo veel sneller gaat.


2) Indirect

Zet  eerst het  registernummer in  R#17, dat  kan niet op de 
indirekte manier.  Schrijf daarna  de data naar port #3. Dit 
is vooral erg handig als je een reeks opeenlopende registers 
wilt  beschrijven (bijvoorbeeld  de commando  registers R#32 
t/m  R#46).  Als  je  namelijk  AII  (bit 7  van R#17,  Auto 
Increment  Inhibit)   op  0   zet,  wordt  R#17  automatisch 
verhoogd. In schema gaat het als volgt:

                  MSB  7  6  5  4  3  2  1  0  LSB
R#17                 AII  0 R5 R4 R3 R2 R1 R0   REGISTER #
Port #3 First Byte    D7 D6 D5 D4 D3 D2 D1 D0   DATA
Port #3 Second Byte   D7 D6 D5 D4 D3 D2 D1 D0   DATA
...
Port #3 nth Byte      D7 D6 D5 D4 D3 D2 D1 D0   DATA


Dit wordt vooral veel gebruikt bij het sturen van commando's 
naar de  VDP. De commandoregisters worden pas in deel 3 en 4 
behandeld,  maar  ik  zal  nu  vast een  stukje standaarcode 
geven.  We maken  hier gebruik van de instructie OTIR van de 
Z80, dat B bytes vanaf adres HL naar poort C stuurt. Stel we 
willen  de   15  waardes  die  achter  COPY:  staan naar  de 
commandoregisters sturen (R#32 t/m R#46):

        DI
        LD   A,32
        OUT  (&H99),A
        LD   A,17+128
        OUT  (&H99),A

Hiermee zetten we het "beginregister" voor het indirect naar 
de  VDP schrijven  op 32,  omdat het AII bit gelijk is aan 0 
wordt  dit  registernummer automatisch  verhoogd als  we een 
byte naar Port #3 schrijven.

        LD   B,15
        LD   C,&H9B     ; LD BC,&H0F9B kan ook
        LD   HL,COPY
        OTIR
        .
        .
        .
COPY:   DB 0,1,0,1,0,2,1,2,3,4,1,2,1,2,3 ; willekeurige data


Zoals we  al eerder  hebben gezien  is het adres van Port #3 
gelijk  aan &H9B.  Met de  OTIR sturen we de 15 waardes naar 
register 32 t/m 46.


3) Palette Registers

Voor het  palet heeft  de VDP paletteregisters P#0 t/m P#15, 
die  alleen indrect aan te sturen zijn. Het paletnummer moet 
in  R#16  staan, dit  wordt automatisch  verhoogd nadat  het 
paletregister  is  beschreven. Voor  elk paletregister  zijn 
twee bytes  nodig, waarvan slechts 9 bits worden gebruikt. 3 
voor  rood, 3  voor blauw  en 3 voor groen. Totaal geeft dat 
2^9=512 kleuren.  De databytes  moeten naar  Port #2  worden 
geschreven. Wederom een schema:


                  MSB  7  6  5  4  3  2  1  0  LSB
R#16                   0  0  0  0 C3 C2 C1 C0   paletnummer
Port #2 First Byte     0 R2 R1 R0  0 B2 B1 B0   data
                           ROOD       BLAUW
Port #2 Second Byte    0  0  0  0  0 G2 G1 G0   data
                                      GROEN


De  waarde in  R#16 wordt  automatisch verhoogd,  als je het 
hele  palet  wilt invullen  hoef je  dus slechts  eenmaal de 
waarde 0  naar R#16  te schrijven  en kun je daarna 32 bytes 
naar   Port  #2   schrijven.  De  volgende  standaardroutine 
schrijft een willekeurig palet naar de VDP:

        DI
        XOR  A
        OUT  (&H99),A
        LD   A,16+128
        OUT  (&H99),A           ; P#0 selecteren
        LD   BC,&H209A          ; 32 bytes naar &H9A
        LD   HL,PALET           ; palet staat vanaf PALET
        OTIR                    ; stuur palet uit


         S T A T U S R E G I S T E R S   L E Z E N 

Als je  de statusregisters  van de  VDP (S#0-S#9) wilt lezen 
moet  je eerst het registernummer in R#15 zetten. Vervolgens 
moet je port #1 uitlezen. Hier het schema:

                  MSB  7  6  5  4  3  2  1  0  LSB
R#15                   0  0  0  0 S3 S2 S1 S0   STATREG #
Port #1 Read data     D7 D6 D5 D4 D3 D2 D1 D0   DATA

Omdat  de interrupt  routine in het BIOS zo snel mogelijk te 
houden wordt  er vanuit  gegaan dat  statusregister 0 altijd 
geselecteerd  is, er  wordt dus simpelweg IN A,(&H99) gedaan 
om S#0  te lezen (dit is nodig om te kijken of een interrupt 
van  de  VDP  afkomstig is  on niet).  De interrupts  moeten 
daarom  bij het  uitlezen van  een statusregister altijd uit 
staan, en  S#0 moet weer geselecteerd zijn als de interrupts 
weer  aan worden  gezet. In  onderstaand voorbeeld wordt S#2 
uitgelezen in ML:

        DI
        LD   A,2
        OUT  (&H99),A
        LD   A,15+128
        OUT  (&H99),A           ; S#2 selecteren
        NOP
        NOP                     ; wacht even
        IN   A,(&H99)
        EX   AF,AF
        XOR  A
        OUT  (&H99),A
        LD   A,15+128
        OUT  (&H99),A           ; selecteer S#0 voor BIOS
        EI
        EX   AF,AF              ; waarde S#2 staat nu in A


De  twee NOPs  zijn nodig  om de  VDP de tijd te geven om de 
data klaar te zetten op Port #1. Er wordt twee keer EX AF,AF 
gebruikt, zodat  de waarde  van het  statusregister aan  het 
einde van het stukje code nog in A staat.

Eventueel  kan IN  A,(&H99) als volgt worden vervangen om te 
wachten totdat  een bepaald bit (in het voorbeeld bit 0) van 
het statusregister 0 is:

WAIT:   IN   A,(&H99)
        BIT  0,A
        JP   NZ,WAIT

In  dit geval is het natuurlijk niet nodig om het A register 
te bewaren  met EX  AF,AF, eventueel  kan een bepaalde actie 
die  moet worden  uitgevoerd zodra het bit 0 is eerst worden 
uitgevoerd, dus  voordat S#0 weer wordt geselecteerd. Als de 
interrupts maar uit staan!


              A A N S T U U R S N E L H E I D 

Hoewel  dit nergens in het MSX2 Technical Handbook van ASCII 
staat, kunnen er problemen ontstaan als de VDP te snel wordt 
aangestuurd. Dit  is afhankelijk  van de  afstand tussen  de 
processor  en de VDP in de computer. Onderstaande manier van 
de VDP  aansturen is  dan ook af te raden, omdat dit niet op 
alle computers goed zal werken:

        DI
        LD   C,&H99
        LD   D,0
        LD   E,128+9
        OUT  (C),D
        OUT  (C),E

De   OUTs  worden   hier  te  snel  achter  elkaar  gegeven. 
Vuistregel  is  om ongeveer zeven Z80-klokpulsen tussen twee 
OUTs  te  wachten.  Dit  komt  overeen  met  twee  NOPjes of 
bijvoorbeeld   een   LD   A,x  instructie.   In  bovenstaand 
voorbeeld wordt  de waarde  0 naar  R#9 geschreven.  Dit kan 
beter als volgt gebeuren, zodat het op alle computers werkt:

        DI
        XOR  A
        OUT  (&H99),A
        LD   A,128+9
        OUT  (&H99),A

Extra  voordelen hiervan  zijn dat  je veel minder registers 
gebruikt, en  dat het  nog sneller  is ook! Zorg er dus voor 
dat  bij het  schrijven naar Port #0 t/m Port #3 altijd even 
wordt gewacht tussen twee OUTs! De "snelheidswinst" die door 
twee  OUTs   achter  elkaar   wordt  gehaald   is  echt   te 
verwaarlozen,  en bovendien  geeft dit  "troep op het beeld" 
bij sommige computers!


                          R 8 0 0 

Nu verwacht  u misschien  problemen bij  de turbo  R, bij de 
R800  duurt een LD A,x of twee NOPjes immers veel korter dan 
bij de  Z80. Maar  dan onderschat u de mensen bij ASCII, die 
natuurlijk  rekening hebben gehouden met dit probleem. In de 
R800 stand  zorgt de  S1990 er namelijk voor dat de R800 bij 
elke  OUT naar  de VDP  een bepaalde  tijd moet wachten, die 
overeen komt  met de  offici�le aanstuursnelheid  van de VDP 
zoals  die  door  Yamaha wordt  opgegeven. Het  enige nadeel 
hiervan  is dat  deze tijd  iets langer is dan per se nodig. 
Gelukkig is  de R800  zo snel  dat je daar verder weinig van 
merkt. Het maakt in R800 stand dus niet uit hoe snel de data 
naar  de VDP  wordt gestuurd,  maar omdat  de R800 toch moet 
wachten heeft  het weinig zin om de "twee OUTs direct achter 
elkaar methode" toe te passen.

Het is overigens niet zo (zoals in het begin van het turbo R 
tijdperk werd  beweerd) dat de VDP sneller is op de turbo R. 
Hij  is gewoon  precies even  snel. Het aansturen van de VDP 
met  OUTs  gaat  in  R800  mode  zelfs  iets  langzamer! VDP 
routines  (bij  bijvoorbeeld Micro  Cabin spellen  zoals Xak 
III) worden  wel sneller op de R800, maar dat komt omdat het 
rekenwerk  dat aan  vrijwel iedere  VDP opdracht vooraf gaat 
natuurlijk een stuk sneller gaat bij de R800.


                     T E N S L O T T E 

Ik hoop dat ik veel trouwe lezers een plezier heb gedaan met 
deze  herhaling.  Vooral  in  deze  aflevering  heb ik  veel 
veranderd, in  de oorspronkelijke tekst werd er vrijwel niet 
aan ML gedaan.

Hier nog even een overzicht van de onderwerpen die in de VDP 
cursus deel 1 t/m 9 zijn behandeld:

1. VDP registers lezen/schrijven
2. Overzicht VDP registers en VRAM lezen/schrijven
3. Commandoregisters (1)
4. Commandoregisters (2)
5. Statusregisters
6. Sprites
7. Opslag schermen in VRAM (1)
8. Opslag schermen in VRAM (2)
9. Opslag schermen in VRAM (3)

Vanaf deel 10 (dit deel staat op Sunrise Magazine #5) worden 
praktijkvoorbeelden  behandeld.  Er  is verder  nog een  VDP 
cursus  Extra, speciaal  voor MSX2+.  Dit deel zal uiteraard 
ook  worden  herhaald. Ik  zal verder  nog een  deel "Opslag 
schermen in VRAM (4)" schrijven, waarin SCREEN 10 t/m 12 aan 
bod komen. Op deze Sunrise Special ook de herhaling van deel 
2.

Tot de volgende keer!

                                                Stefan Boer