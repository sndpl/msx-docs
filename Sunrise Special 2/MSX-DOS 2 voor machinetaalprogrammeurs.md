         C U R S U S   M S X - D O S   2   V O O R 

       M A C H I N E T A A L P R O G R A M M E U R S 
                                                      

Op deze  Sunrise Special  start ik  een cursus  waarin ik de 
vele  mogelijkheden van  MSX-DOS 2  zal beschrijven.  Ik zal 
proberen door  middel van  voorbeelden de  nieuwe DOS-CALL's 
duidelijk  te maken.  De cursus  zal uit  ongeveer zes delen 
bestaan met op deze disk het eerste deel. In dit eerste deel 
zal ik  de programma-omgeving beschrijven die MSX-DOS 2 voor 
programma's biedt.


              H E T   T P A - G E H E U G E N 

TPA staat voor Transient Program Area. Een Transient Program 
(TP) is  een moeilijk  woord voor een normale COM-file. Deze 
COM-files  worden door  COMMAND2 ingeladen op adres #0100 en 
vervolgens geCALLed  met de  stackpointer (SP) aan het einde 
van  het TPA.  Al het geheugen tot aan de stack mag gebruikt 
worden door  het TP.  De inhoud van de Z80-registers is niet 
gedefinieerd.  De eerste  256 bytes  van het  RAM hebben een 
speciale betekenis.  Deze bytes  zal ik  dadelijk verklaren. 
Interrupts  staan  aan  als  een  TP aangeroepen  wordt door 
COMMAND2  en  moeten  in  het  algemeen  aan  blijven staan. 
MSX-DOS   function-calls   zullen   meestal  de   interrupts 
aanzetten als ze uitstonden.


        T E R U G K E E R   N A A R   M S X - D O S 

Een  TP kan  zichzelf be�indigen  door middel van een van de 
onderstaande manieren:

1 Terugkeren d.m.v. RET, met de originele SP.
2 JumP naar adres #0000.
3 MSX-DOS "Program Terminate" function call.
4 MSX-DOS 2 "Terminate with Error Code" function call.

De  eerste  drie zijn  identiek met  CP/M en  MSX-DOS 1.  De 
laatste  is  een  nieuwe  functie  van  MSX-DOS 2.  Met deze 
functie  kan  een programma  een foutmelding  teruggeven aan 
COMMAND2 die  deze zal afdrukken op het standaard device CON 
(NvdR:  Hoeft niet  per se  CON te  zijn, kan ook PRN of NUL 
zijn). Deze  functie zal  geen foutmelding  afdrukken als de 
error  code gelijk is aan nul. Vele andere acties kunnen het 
programma dwingen  af te  breken, waaronder onder andere het 
drukken  van CTRL-C,  CTRL-STOP of  het kiezen  van de optie 
Abort als  er een  diskerror optreedt. Een TP kan een "abort 
routine" definieren, die deze gevallen kan opvangen.


         A D R E S   # 0 0 0 0   T O T   # 0 1 0 0 

In de eerste 256 bytes van het geheugen worden door COMMAND2 
enige  parameters gezet  voor het  TP. De  indeling van deze 
eerste bytes is als volgt.

      +0    +1    +2    +3    +4    +5    +6    +7
      +-----+-----+-----+-----+-----+-----+-----+-----+
#0000 |   Reboot Entry  | Reserved  |  MSX-DOS Entry  |
      +-----+-----+-----+-----+-----+-----+-----+-----+
#0008 | RST #08: ongebruikt   |  RDSLT routine Entry  |
      +-----+-----+-----+-----+-----+-----+-----+-----+
#0010 | RST #10: ongebruikt   |  WRSLT routine Entry  |
      +-----+-----+-----+-----+-----+-----+-----+-----+
#0018 | RST #18: ongebruikt   |  CALSLT routine Entry |
      +-----+-----+-----+-----+-----+-----+-----+-----+
#0020 | RST #20: ongebruikt   |  ENASLT routine Entry |
      +-----+-----+-----+-----+-----+-----+-----+-----+
#0028 | RST #28: ongebruikt   |      ongebruikt       |
      +-----+-----+-----+-----+-----+-----+-----+-----+
#0030 | CALLF routine entry   |      ongebruikt       |
      +-----+-----+-----+-----+-----+-----+-----+-----+
#0038 | Interrupt vector      |                       |
      +-----+-----+-----+-----+                       +
#0040 |                                               |
      +                                               +
#0048 |    Gebruikt door slot switching code          |
      +                                               +
#0050 |                                               |
      +                       +-----+-----+-----+-----+
#0058 |                       |                       |
      +-----+-----+-----+-----+                       +
#0060 |     Ongeopend FCB van de eerste parameter     |
      +                       +-----+-----+-----+-----+
#0068 |                       |                       |
      +-----+-----+-----+-----+                       |
#0070 |     Ongeopend FCB van de tweede parameter     |
      +                       +-----+-----+-----+-----+
#0078 |                       | Ruimte voor FCB-data  |
      +-----+-----+-----+-----+-----+-----+-----+-----+
#0080 |                                               |
      .                                               .
      .            Default DMA-adres                  .
      .    Hier staat de originele commando line      .
      .                                               .
      .                                               .
#00F8 |                                               |
      +-----+-----+-----+-----+-----+-----+-----+-----+

Op  adres #0000  staat de  jump, die gebruikt wordt voor het 
be�indigen van  een programma.  Deze jump  kan ook  gebruikt 
worden  om de BIOS jump vector te vinden. (Zie verderop). De 
lage  byte  van  dit  adres  zal  altijd  #03  zijn voor  de 
compatibiliteit met CP/M.

De twee  gereserveerde bytes op adres #0003 en #0004 zijn de 
IO-byte  en  de  Current  Drive/User  byte  in  CP/M. Hoewel 
MSX-DOS  2 deze byte altijd up-to-date houdt, wordt het niet 
aanbevolen om  in nieuwe programma's deze byte te gebruiken. 
Deze  kunnen beter  de MSX-DOS  2 function call "Get current 
drive" gebruiken.  Het IO-byte wordt niet ondersteund, omdat 
MSX-DOS 2 device-IO anders geregeld heeft dan CP/M.

Op  adres  #0005  staat  een  jump  naar  de  start  van  de 
BDOS-verwerking van  MSX-DOS 2. Dit jumpadres bepaalt de top 
van  het  TPA.  De  grootte  van  het  TPA hangt  af van  de 
uitbreidingen  die u  gebruikt, maar  is standaard 53 kB. De 
lage  byte  van  dit  adres  zal  altijd  #06  zijn voor  de 
compatibiliteit  met  CP/M. De  zes bytes  direct hierachter 
bevatten het CP/M-versienummer en een serienummer.

Er zijn  vier bytes  gereserveerd voor  de gebruiker  op elk 
Z80-restart  adres. De bytes tussen de restart adressen zijn 
entry's voor enkele MSX slot switching routines.

Het  hele gebied  van #0038 tot #005C wordt gebruikt voor de 
interruptafhandeling in slot switching code.

De twee  FCB's op  adres #005C  en #006C zijn de eerste twee 
parameters achter de commandonaam, gezien als filenamen. Als 
beide  FCB's gebruikt  worden moet de tweede naar een andere 
locatie gekopieerd worden, want als de eerste geopend wordt, 
zal die de tweede overschrijven.

De  gehele commandoline,  zonder commandonaam, wordt bewaard 
in het  standaard DMA  gebied (vanaf  #0080) met  als eerste 
byte  een lengtebyte  en als  laatste byte een nul. (Noch de 
lengtebyte noch  de nul  worden meegeteld  in de lengte). De 
commandoline  zal geheel  uit hoofdletters  bestaan als  het 
environment item  "UPPER" op  "ON" gezet is. De commandoline 
zal niet ontdaan worden van inleidende spaties.

Nieuwe  programma's  kunnen  de oude  FCB's beter  niet meer 
gebruiken.  Er zijn  in MSX-DOS 2 andere, simpelere functies 
beschikbaar.  Deze  functies  maken  het  ook  mogelijk  met 
subdirectory's te werken.

Er  is  nog  een  manier  om de  commandoline uit  te lezen. 
COMMAND2  zet  namelijk  een environment  item, "PARAMETERS" 
genaamd,  op  die  de commandoline  bevat in  zijn originele 
staat.  Het tweede environment item dat COMMAND2 op zet heet 
"PROGRAM". Dit bevat het Drive\Path\Filenaam string dat gold 
voor het geladen programma.


            D E   B I O S   J U M P   T A B L E 

De jump  op adres  #0000 zal springen naar een adres dat ook 
een  jump bevat.  Deze jump  is de  tweede entry in een jump 
table  van  zeventien entry's.  Dit komt  exact overeen  met 
CP/M.

De eerste  acht entry's  zijn voor reboot en karakter-IO. De 
andere  negen  doen  niets  en  geven,  waar  mogelijk,  een 
errorcode terug.

MSX-DOS  2 zal  bij een  CALL overschakelen naar een interne 
stack,  zodat  de userstack  niet groter  hoeft te  zijn dan 
ongeveer acht bytes.

Overzicht BIOS jumptable:

#xx00  Warm boot
#xx03  Idem
#xx06  Console status
#xx09  Console input
#xx0C  Console output
#xx0F  List output
#xx12  Punch output
#xx15  Reader input
#xx18  Geen functie
#xx1B  Geen functie
#xx1E  Geen functie
#xx21  Geen functie
#xx24  Geen functie
#xx27  Geen functie
#xx2A  Geen functie
#xx2D  List status
#xx30  Geen functie


               G E H E U G E N G E B R U I K 

Een  TP mag  zoveel slotswitching  doen als het wil, als het 
maar rekening  houdt met  de interrupts e.d. als het TP page 
nul  of drie  wegschakelt. TP's  moeten erg voorzichtig zijn 
met  het   veranderen  van   page  0  en  TP's  mogen  nooit 
veranderingen  aanbrengen in page 3. Page 0, 1 en 2 mogen in 
elk  slot  zitten  als  het  TP  MSX-DOS  2  aanroept.  Deze 
instellingen zullen  bewaard blijven.  Een parameter kan via 
elk  slot aan  MSX-DOS 2  worden doorgegeven. Dit geldt niet 
voor de  environmentstrings en  de diskbuffers.  Deze moeten 
zich in de main mapper bevinden. Het is mogelijk om gegevens 
in andere segmenten dan de originele TPA segmenten te laden. 
Als  het TP de memory mapper wil schakelen moet het daarvoor 
gebruik maken  van de  mappersupportroutines in page 3. Deze 
routines  zal ik de volgende keer beschrijven. Een TP dat de 
mapper schakelt  moet er  zelf voor  zorgen dat de originele 
TPA  segmenten weer  ingeschakelt worden,  als het programma 
eindigt.


                  F I L E   H A N D L E S 

File handles zijn de nieuwe verbeterde methoden om files aan 
te  spreken en  te manipuleren.  Een file handle is een byte 
die  betrekking  heeft op  een geopende  file. Er  wordt een 
nieuwe file  handle aangemaakt bij de aanroep van de MSX-DOS 
2  functie "Open  file handle" (#43) of "Create file handle" 
(#44). De  file handle  kan gebruikt  worden om  data van de 
file  te lezen  of data  naar de file te schrijven. Een file 
handle blijft  bestaan totdat  een van de volgende MSX-DOS 2 
functies  wordt  gebruikt:  "Close  file  handle"  (#45)  of 
"delete  file handle" (#46). MSX-DOS 2 opent als het opstart 
al enige  file handles  voor de  standaard IO-kanalen.  Deze 
default file handles zijn:

#00  Standaard input (CON)
#01  Standaard output (CON)
#02  Standaard error in/output (CON)
#03  Standaard auxiliary in/output (AUX)
#04  Standaard printer output (PRN)

Een  TP mag deze file handles sluiten, verwijderen of op een 
andere manier  beschadigen, omdat COMMAND2 deze file handles 
automatisch  zal herstellen. Ook als een TP zich niet netjes 
gedraagt  en  niet  alle  file handles  sluit, zal  dit geen 
probleem opleveren,  omdat COMMAND2 dit zal doen. (Netjes is 
anders).


        F I L E   I N F O   B L O C K S   ( F I B  )

Alle  nieuwe MSX-DOS  2 functies die op files werken, kunnen 
aangeroepen worden  met een simpele pointer naar een string. 
Deze  string bevat  dan de  benodigde parameters  afgesloten 
door een nul. Dit soort strings noemt men ASCIIZ-strings. In 
plaats van  een ASCIIZ-string  mag men ook een FIB doorgeven 
aan MSX-DOS 2. FIB's zien er als volgt uit:

     0 - Altijd #FF
 1..13 - Filename als ASCIIZ-string
    14 - File atributen
15..16 - Tijd
17..18 - Datum
19..20 - Eerste datacluster
21..24 - Grootte van de file
    25 - Logische drive (A: = 1)
26..63 - Interne informatie

De  #FF aan het begin van een FIB is als herkenning voor het 
systeem  dat  het  om  een FIB  gaat en  niet om  een platte 
ASCIIZ-string. De interne informatie vanaf byte 26 mag nooit 
verandert worden.  De byte  van de file atributen ziet er zo 
uit:

Bit 0  Read only
Bit 1  Hidden
Bit 2  System
Bit 3  Volume naam
Bit 4  Directory
Bit 5  Archive
Bit 6  Gereserveerd
Bit 7  Device

De tijd en datum moet men zo coderen:
Tijd:  Bits 15..11  Uren (0..23)
       Bits 10...5  Minuten (0..59)
       Bits  4...0  Seconden (0..58, even getallen)
Datum: Bits 15...9  Jaar (0..99, 1980 tot 2080)
       Bits  8...5  Maand (1..12)
       Bits  4...0  Dag (0..31)

De filegrootte  wordt opgeslagen als een 32-bit getal met de 
lowbyte eerst. Deze byte is nul voor directory's. Deze FIB's 
worden  door de functie calls "Find first entry", "Find next 
entry" en "Find new entry" ingevuld.


     F I L E   C O N T R O L   B L O C K S   ( F C B  )

Een FCB is 32 bytes lang en bevat de volgende informatie:

     #00  Drive nummer (A:=1, Default=0)
#01..#08  Filename
#09..#0B  Filename extensie
     #0C  Bloknummer
     #0D  Attributen
#0E..#0F  Record-lengte
#10..#13  Filegrootte
#14..#17  Volume-ID (DOS 2) Time/Date (DOS 1)
#18..#1F  Interne informatie
     #20  Current record
#21..#24  Random recordnummer

Het  wordt  niet aanbevolen  om deze  verouderde manier  van 
fileacces nog te gebruiken. Men kan onder MSX-DOS 2 beter de 
file  handles   gebruiken.  Deze   laatste  kunnen  ook  met 
directory's werken en dat kunnen FCB's niet.


                 F O U T M E L D I N G E N 

MSX-DOS  2  kan vele  foutmeldingen teruggeven.  Deze hebben 
alle een nummer tussen #FF en #00. De complete lijst (?) van 
foutmeldingen volgt nu:

Nummer Melding

#FF    Incompatible disk.
#FE    Write error
#FD    Disk error
#FC    Not ready
#FB    Verify error
#FA    Data error
#F9    Sector not found
#F8    Write prtected disk
#F7    Unformatted disk
#F6    Not a DOS disk
#F5    Wrong disk
#F4    Wrong disk for file
#F3    Seek error
#F2    Bad file allocation table
#F1    -
#F0    Cannot format this drive
#EF
 .
 .     Niet gebruikt
 .
#E0
#DF    Internal error
#DE    Not enough memory
#DD    -
#DC    Invalid MSX-DOS call
#DB    Invalid drive
#DA    Invalid filename
#D9    Invalid pathname
#D8    Pathname too long
#D7    File not found
#D6    Directory not found
#D5    Root directory full
#D4    Disk full
#D3    Duplicate filename
#D2    Invalid directory move
#D1    Read only file
#D0    Directory not empty
#CF    Invalid attributes
#CE    Invalid . or .. operation
#CD    System file exists
#CC    Directory exists
#CB    File exists
#CA    File already in use
#C9    Cannot tranfer above 64K
#C8    File alloccation error
#C7    End of file
#C6    File acces violation
#C5    Invalid process ID
#C4    No spare file handles
#C3    Invalid file handle
#C2    File handle not open
#C1    Invalid device operation
#C0    Invalid environment string
#BF    Environment string too long
#BE    Invalid date
#BD    Invalid time
#BC    RAM disk already exists
#BB    RAM disk does not exist
#BA    File handle has been deleted
#B9    -
#B8    Invalid sub-function number
#B7
 .
 .     Niet gebruikt
 .
#A0
#9F    Ctrl-STOP pressed
#9E    Ctrl-C pressed
#9D    Disk operation aborted
#9C    Error on standard output
#9B    Error on standard input
#9A
 .
 .     Niet gebruikt
 .
#90
#8F    Wrong version of COMMAND
#8E    Unrecognized command
#8D    Command too long
#8C    -
#8B    Invalid parameter
#8A    Too many parameters
#89    Missing parameter
#88    Invalid option
#87    Invalid number
#86    File for HELP not found
#85    Wrong version of MSX-DOS
#84    Cannot concatenate destination file
#83    Cannot create destination file
#82    File cannot be copied onto itself
#81    Cannot overwrite previous destination file
#80
 .
 .     Niet gebruikt
 .
#00

De  hierboven  met  "niet  gebruikt" aangegeven  codes geven 
meestal een system of user error xx.

Zo dat  was het  voor deze keer. Allemaal een beetje saai en 
droog. De volgende keer zal ik de mapper support routines en 
een  gedeelte van de function-calls bespreken. Ik zal er dan 
enige voorbeelden  bij doen  zodat u  niet helemaal in slaap 
valt.

                                            Daniel Wiermans