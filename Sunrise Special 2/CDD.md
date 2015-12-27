# C D D 
                                  

CDD  is een  afkorting voor  Change Drive and Directory. Dit 
programma heb  ik geschreven  omdat ik  anders altijd  met 2 
commando's  in een batchfile moest werken. De interne CD kan 
niet de  default drive  veranderen: als  je een  driveletter 
opgeeft wordt de directory van die drive veranderd.

Het  programma heeft  alleen nut  op een  harddisk. Het kost 
altijd een cluster en het moet van disk worden ingeladen. En 
boven alles:  een diskette  van 720  kB is  veel te klein en 
langzaam  om  met  subdirectory's  te  werken.  Meer  dan  2 
diskdrives heeft toch bijna niemand.

        C:
        CD \MUSIC\SME3

wordt met CDD.COM

        CDD C:\MUSIC\SME3


## W E R K I N G 

Op  deze disk  staan de files CDD.GEN en de al kant en klare 
versie CDD.COM.
Hier  volgt  een  bespreking  en  uitleg  van  de  gebruikte 
methodes.

```
LOCATE:      MACRO  @X,@Y
               DB   27,"Y",32+@Y,32+@X
             ENDM
```

Zie de tekst over macro's, ook op deze Special.


## T E S T E N   O P   M S X - D O S   1 

```
             LD     A,(#F313)           ;DOS-versie
             OR     A                   ;CP 0
             JR     Z,DOS1              ;0 -> DOS1
```

Op  adres #F313  staat een getal dat door MSX-DOS 2 gebruikt 
wordt. Hier  zet hij  het versienummer  in BCD neer. MSX-DOS 
2.30 en 2.31 gebruiken hetzelfde getal: #23.

MSX-DOS 1 laat er gewoon 0 staan. OR A verandert niks aan A, 
maar be�nvloed  het flag-register  aan de hand van de waarde 
van A. AND A heeft hetzelfde effect, maar wordt, volgens mij 
door gewenning, bijna nooit gebruikt.


## I S   E R   W E L   I N P U T ? 

```
             LD     HL,#0080            ;DMA-adres
             LD     A,(HL)              ;Lengte
             OR     A                   ;CP 0
             JR     Z,Expl              ;Geen input? Uitleg
```

Er  wordt even  gecontroleerd of  er wel een input is achter 
CDD.
Ik  gebruik de  plaats (adres  #0080) die  door ASCII  wordt 
aanbevolen. Op  adres #005C staan ook de tekens die ik nodig 
heb, en die zijn makkelijker te gebruiken.

Ik  zet het  adres in  HL omdat dat korter is als ik het nog 
eens nodig heb (H blijft toch #00):

```
             INC    L                   ;L <- #82
             INC    L
             LD     C,A                 ;Lo-byte (A: lengte)
             LD     B,0                 ;Hi-byte
             LD     A,":"
             CPIR                       ;Zoek dubbele punt
```
CPIR doet het volgende:
             A - (HL)
             HL <- HL + 1
             BC <- BC - 1
             Herhaal tot A=(HL) of BC=0

Er  wordt in  dit geval  gezocht naar  ":", het  teken na de 
driveletter. BC  krijgt de  lengte van  de invoer. En HL had 
\#0082. Door het gebruik van CPIR mag er ook iets als

             CDD        C:\MUSIC\SME3

ingevoerd worden.
```
             LD     A,B                 ;BC=0?
             OR     C
             JR     Z,Expl              ;Ja? Uitleg
```
Als BC  0 is,  is er niks bij CPIR gevonden en wordt er naar 
de uitleg gesprongen.
Deze constructie is een stuk korter en sneller dan
```
             LD     A,B
             OR     A
             JR     Z,Expl
             LD     A,C
             OR     A
             JR     Z,Expl
```
De werking  zal meteen  duidelijk zijn, maar het je moet het 
wel  weten. (Een  maand geleden  deed ik  het zelf nog op de 
tweede manier.)


## G E B R U I K   I N P U T 
```
             DEC    L                   ;(HL)=Driveletter
             DEC    L
             LD     A,(HL)
             AND    255-32
             SUB    "A"                 ;A=Drivenummer
             JR     C,Error
```
HL  bevatte nog  het adres  van de dubbele punt plus ��n. Er 
moet dus  2 vanaf  getrokken worden  om tot het adres van de 
driveletter te komen. H blijft toch #00.
Dan  moet er  nog een hoofdletter van gemaakt worden met AND 
255-32. (Met  dank aan  Hans Peeters  voor de  attentie!) En 
wordt  er  "A"  vanaf getrokken  om tot  een drivenummer  te 
komen.  Als die waarde dan kleiner dan 0 (JR C) is dan is er 
iets fout.
```
             INC    HL                  ;HL+=2 (in C)
             INC    HL                  ;HL=HL+2 (in BASIC)
             PUSH   HL                  ;SP1
             PUSH   AF                  ;SP2
             LD     C,#0E               ;_SELDSK
             LD     E,A                 ;0=A: 1=B: usw.
             CALL   5
```
Stel  eerst  HL  en  A  veilig  voor  het  verdere programma 
(directory veranderen).  En dan  de drive veranderen met een 
routine uit het CP/M-tijdperk.

```
             LD     A,(#0004)           ;Default drive CP/M
             LD     B,A
             POP    AF                  ;SP2
             CP     B
             JR     NZ,DriveErr
```
Op (#0004) staat het default drivenummer (ook van CP/M). Als 
het default drivenummer na het veranderen ongelijk is aan de 
input,  bestaat  de drive  niet en  wordt er  een error-code 
gegeven door MSX-DOS 2.


## C H A N G E   D I R E C T O R Y 
```
             POP    DE                  ;SP1
             LD     C,#5A               ;_CHDIR
             CALL   5
             JR     NZ,Error
             RST    0                   ;Einde
```
Haal HL  van de stapel in DE. Dit is korter dan POP HL en EX 
DE,HL.  Verander van  directory en controleer of er nog iets 
fout is gegaan. De BDOS van MSX-DOS 2 (van MSX-DOS 1 weet ik 
het niet)  heeft OR  A (CP 0) als laatste instructie voor de 
RET.  Je kunt  dus meteen met een JP NZ,Error of JR NZ,Error 
beginnen. De  string die BDOS-funktie #5A gebruikt is ASCIIZ 
(afgesloten  met Zero,  0) en  de input  op #0080  is ook al 
ASCIIZ. (Meer  over de BDOS-funkties bij de MSX-DOS 2 cursus 
op deze en volgende Specials.)

RST 0  is tenslotte  het einde. RST 0 is maar een byte lang. 
Er  kan  ook  met  RET  teruggekeerd  worden,  maar  dit  is 
makkelijker bij het debuggen (bij MSX-Debug dan).

```
Error:       LD     B,A
Error2:      LD     C,#62               ;Return with error
             JP     5
```
"Return  with error-code"  keert terug naar MSX-DOS en geeft 
een foutmelding  als A  ongelijk aan 0 is. De fout stond nog 
in A en moet in B staan voor "Return with error-code".
Error2 is als label erbij gezet voor:
```
DriveErr:    LD     B,#DB
             JR     Error2
```
\#DB is de fout-code voor "Drive does not exist".

```
Expl:        LD     DE,T_Expl
Expl1:       LD     C,9                 ;String output
             JP     5
```
Geeft  uitleg d.m.v.  de standaard BDOS-output zodat het nog 
te pipen  en redirectioneren  (?!) is.  JP kan  ook gebruikt 
worden  in plaats van CALL en RET. MSX-DOS heeft altijd 0 op 
de stapel  staan en een RET als einde heeft hetzelfde effect 
als RST 0.

```
DOS1:        LD     DE,T_DOS1
             LD     C,9                 ;String output
             CALL   5
             LD     C,1                 ;Console input
             CALL   5
             JR     Expl
```
Even  een  tekstje  voor  MSX-DOS  1 gebruikers  laten zien, 
wachten op een toets en terug naar uitleg.

```
T_Expl:      DB     12
             DB     "Change Drive and Directory",13,10
             DB     "--------------------------",13,10,10
             DB     "Usage: CDD d:[dir]",13,10,10
             DB     "d:   drive",13,10
             DB     "dir  subdirectory",13,10,10
             LOCATE 0,56
             DB     "(f) WSL - Kasper Souren"
             LOCATE 0,10
             DB     "$"

T_DOS1:      DB     12,"Jammer, dit programma werkt alleen "
             DB     "met MSX-DOS 2 of hoger.",13,10,10
             DB     "Druk op een toets. $"
```
De teksten.  Code 12  is hetzelfde  als CLS,  LOCATE is  een 
macro  dat werkt  met de  VT-52 code voor LOCATE. Code 10 is 
linefeed en  als je  een regel wilt overslaan moet je 2 keer 
10  gebruiken. De  streepjes die  ik in  het echte programma 
gebruik worden  niet geaccepteerd  door de  tekstroutine van 
Sunrise  Special dus  heb ik  die maar vervangen voor andere 
streepjes. De "(f)" staat tenslotte voor Freeware.

Kasper Souren
