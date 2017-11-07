# B L I N K M O D E 
                                        

Ik  ga ervan  uit dat  de blinkmode in Text Mode 2 (SCREEN 0 
WIDTH 80)  bij de  lezer bekend is. Voor elke positie op het 
scherm  is er  een bit  in de  blinktabel die aangeeft of de 
positie "blinkt" of normaal is.

Door deze  binaire opbouw is het aansturen van de blink mode 
behoorlijk  gecompliceerd. Ik  heb daarom  een standaard  ML 
routine  geschreven  waarmee van  een bepaalde  rechthoek de 
blinkmode aan of uit kan worden gezet.


## G E B R U I K   I N   B A S I C 

Ik heb  de routine  meteen maar  in een  CMD jasje gestoken, 
zodat  hij vanuit  BASIC te  gebruiken is.  Dit heeft  extra 
voordelen,  de  routine  bespaart  nu niet  alleen een  hoop 
rekenwerk, maar ook veel tijd.

Met    een   BLOAD"CMDBLINK.BIN",R   initialiseert   u  vier 
nieuwe BASIC commando's:
```
CMD BFIL (x1,y1)-(x2,y2)
CMD BRES (x1,y1)-(x2,y2)
CMD BCOL (voorgrondkleur,achtergrondkleur)
CMD BTIM (normaaltijd,blinktijd)
```

Met (x1,y1)-(x2,y2)  geeft u  een rechthoek  op, net als bij 
LINE(x1,y1)-(x2,y2),,BF.  De  x  co�rdinaten  moeten  liggen 
tussen 0 en 79, de y co�rdinaten tussen 0 en 26 (de onderste 
2.5  regel wordt  alleen getoond als bit 7 van R#9 gelijk is 
aan 1). Met CMD BFIL zet u de blinkmode aan in die rechthoek 
en met CMD BRES zet u de blinkmode uit.

De  voorgrondkleur  en  achtergrondkleur  moeten  natuurlijk 
tussen 0  en 15  liggen, ook  de normaaltijd en de blinktijd 
moeten  tussen 0  en 15  liggen. De  tijd wordt opgegeven in 
eenheden van ca. 1/6 seconde.


## C M D 

Voordat we  met de  assemblerlisting gaan  beginnen moet  ik 
natuurlijk  eerst nog  even uitleggen  hoe CMD werkt. CMD is 
een  afkorting  van  CoMmanD,  en is  speciaal bedoeld  voor 
toekomstige uitbreidingen  van BASIC.  In feite is er alleen 
een  hook voor  gereserveerd op adres &HFE0D, zodra de BASIC 
interpreter het  commando CMD  tegenkomt wordt  er naar deze 
hook gesprongen.

Normaal  gesproken  staat  hier  een  RET,  en  zal  er  een 
foutmelding   worden  gegeven.  Wordt  de  CMD  hook  echter 
afgebogen,  dan   kan  er  een  eigen  commando  aan  worden 
gehangen. Eventueel kunnen er meerdere CMD commando's worden 
gecre�erd  door te  kijken naar  de karakters die na het CMD 
commando staan. Dit wordt hier ook gedaan.

Als u met een gewone RET terugspringt zal er een foutmelding 
volgen,  zo  hebben we  net gezien.  We moeten  er dus  voor 
zorgen dat  het terugspringadres van de stack wordt gehaald, 
dit doen me met een extra POP AF.

HL wijst naar de plaats in het BASIC programma, in het geval 
van aanroep van de CMD hook dus naar het adres direct achter 
CMD in het BASIC programma. Hier kunnen eventueel parameters 
worden  doorgegeven, wat  bij deze  routine dan ook gebeurt. 
Bij  het  terugspringen naar  BASIC moet  HL wijzen  naar de 
positie direct achter het complete CMD commando.

Nu is  het tijd  voor de  assemblerlisting. Zoals gewoonlijk 
zal  ik  hem  regelmatig  onderbreken  voor  commentaar.  De 
listing is aanwezig op de disk onder de naam "CMDBLINK.ASC".

```
; C M D B L I N K . A S M 
; Gebruikt BLINK
; Door Stefan Boer 21/10/92
; Sunrise Special #2 - (c) Stichting Sunrise 1992

; Maakt nieuwe BASIC commando's voor blink mode
; (SCREEN 0 WIDTH 80)
; BFIL, BRES, BCOL en BTIM

; Syntax: CMD BFIL (X1,Y1)-(X2,Y2)
;         CMD BRES (X1,Y1)-(X2,Y2)
; Zet blinkmode aan (BFIL) of uit (BRES) in een bepaalde
; rechthoek
; 0 <= X1,X2 <=79
; 0 <= Y1,Y2 <=26

; Syntax: CMD BCOL (V,A)
; Stel blinkkleuren in
; 0 <= V,A <= 15
; V=voorgrondkleur, A=achtergrondkleur

; Syntax: CMD BTIM (N,B)
; Stel blinktijden in
; 0 <= N,B <= 15
; N=tijd normale kleuren, B=tijd blinkkleuren
; tijd in eenheden van ca. 1/6 seconde

H_CMD:  EQU   &HFE0D          ; hook
GETBYT: EQU   &H521C          ; BASIC haal byte -> E en A
SYNTAX: EQU   &H4055          ; Syntax error

        ORG   &HD000

; buig de hook af

INIT:   LD    HL,HOOK
        LD    DE,H_CMD
        LD    BC,5
        LDIR
        RET

; de nieuwe hook

HOOK:   POP   AF              ; wis terugspringadres
        CALL  PROG
        RET
```

Met een CALL INIT wordt de nieuwe hook over de oude CMD hook 
gezet.  De POP  AF zorgt  ervoor dat er geen foutmelding zal 
volgen  na  het  uitvoeren  van  een  van  de  nieuwe  BASIC 
commando's. De nieuwe hook roept daarna PROG aan.

```
; dit wordt aangeroepen bij een CMD

PROG:   RST   8
        DB    "B"             ; eerste letter altijd "B"
```

HL wijst naar de huidige positie in het BASIC programma. Bij 
aanroep van PROG is dit dus de positie direct achter de code 
voor  CMD.  Hierachter  moet  een "B"  staan, omdat  al onze 
nieuwe commando's met een B beginnen.

Met  de BIOS routine SYNCHR (adres &H0008, aanroep RST 8) is 
het zeer  eenvoudig om  te controleren  of een bepaalde code 
(hier dus "B") op de huidige positie (die wordt gegeven door 
HL)  in het  BASIC programma  staat. Na de RST 8 geef je met 
een DB  de code op. Wordt deze code gevonden, dan springt de 
computer  naar CHRGTR  (adres &H0010,  aanroep RST  &H10) en 
haalt  de  code  op,  dit  is  voor  ons  hier  verder  niet 
belanrijk. Wordt  de code echter niet gevonden, dan volgt er 
een   foutmelding.  HL  wordt  automatisch  verzet  naar  de 
volgende   positie,   waarbij  spaties   automatisch  worden 
overgeslagen.

```
        LD    A,(HL)
        INC   HL
        CP    "F"             ; bFil
        JP    Z,BFIL
        CP    "R"             ; bRes
        JP    Z,BRES
        CP    "C"             ; bCol
        JP    Z,BCOL
        CP    "T"             ; bTim
        JP    Z,BTIM
        JP    SYNTAX          ; Syntax error
```

Hier wordt de volgende code opgehaald, HL wordt daarna op de 
volgende positie gezet met een INC HL. Vervolgens wordt naar 
de  letter  direct achter  de B  gekeken om  te kijken  welk 
commando het  is. Dit wordt met CP's en voorwaardelijke JP's 
gedaan. Wordt geen van de tweede letters gevonden, dan staat 
er  blijkbaar een  verkeerd woord  achter CMD  en wordt  een 
foutmelding gegeven.

```
BFIL:   RST   8
        DB    "I"
        RST   8
        DB    "L"             ; bfIL
        LD    A,1             ; blink aan
        LD    (MODE),A
        JP    FILRES
```

Hier wordt naartoe gesprongen als er "BF" is gevonden. Eerst 
moet  er dus  nog worden  gecontroleerd of  er ook inderdaad 
BFIL stond,  door met RST 8 te controleren of er "IL" volgt. 
Zo nee, dan zorgt RST 8 zelf voor de foutmelding. Zo ja, dan 
wordt  (MODE) op  1 gezet. Met (MODE) wordt bepaald of in de 
rechthoek de  blinkmode aan-  of uitgezet  wordt. Vervolgens 
wordt  er naar  FILRES gesprongen,  vanaf daar is de routine 
voor BFIL en BRES hetzelfde.

```
BRES:   RST   8
        DB    "E"
        RST   8
        DB    "S"             ; brES
        XOR   A               ; blink uit
        LD    (MODE),A
```

Dit is ongeveer hetzelfde, alleen nu voor BRES. (MODE) wordt 
nu op 0 gezet, een JP FILRES is niet nodig omdat FILRES hier 
direct achter staat.

```
; routine voor BFIL en BRES (MODE is al ingevuld)

FILRES: RST   8
        DB    "("
        CALL  GETBYT          ; lees X1
        LD    (BEGIN+1),A
        RST   8
        DB    ","
```

Eerst wordt  op het  haakje openen gecontroleerd, vervolgens 
wordt  met de  routine GETBYT  van de  BASIC interpreter  de 
eerste x  co�rdinaat opgehaald.  GETBYT verhoogt  zelf HL en 
zet  het resultaat  in A  en in  E. We  bewaren de  eerste x 
co�rdinaat en controleren vervolgens op de komma.

```
        CALL  GETBYT          ; lees Y1
        LD    (BEGIN),A
        RST   8
        DB    ")"
        RST   8
        DB    &HF2            ; is "-"
```
Hier  wordt  de  eerste y  co�rdinaat opgehaald.  Vervolgens 
wordt  het haakje sluiten gecontroleerd en het streepje. Let 
op: dit kan niet met RST 8, DB "-", omdat het token voor het 
streepje niet gelijk is aan de ASCII code, maar &HF2!

```
        RST   8
        DB    "("
        CALL  GETBYT          ; lees x2
        LD    (EIND+1),A
        RST   8
        DB    ","
        CALL  GETBYT          ; lees y2
        LD    (EIND),A
        RST   8
        DB    ")"
        PUSH  HL              ; save BASIC pointer
```

Hier worden  ook de  tweede x-  en y  co�rdinaten opgehaald. 
Vervolgens   wordt  HL   bewaard,  want  die  moet  bij  het 
terugkeren naar  BASIC wijzen  naar de positie direct achter 
het commando.

```
        LD    BC,(BEGIN)      ; aanroep standaardroutine
        LD    DE,(EIND)       ; met juiste waarden
        LD    A,(MODE)        ; in registers
        CALL  BLINK           ; (B,C)-(D,E), A=aan/uit
```

Hier worden  de registers  met de juiste waardes gevuld voor 
de  standaardroutine  BLINK.  De  routine  wordt  vervolgens 
aangeroepen.  De standaardroutine  BLINK staat aan het einde 
van de assemblerlisting.

```
        POP   HL              ; BASIC pointer terug
        RET                   ; terug naar BASIC
```

Het commando  is uitgevoerd,  dus we  gaan terug naar BASIC. 
Eerst   wordt  natuurlijk  de  juiste  waarde  van  HL  weer 
teruggehaald. Nu de routines voor BCOL en BTIM.

```
BCOL:   RST   8
        DB    "O"
        RST   8
        DB    "L"             ; bcOL
        RST   8
```
Eerst controleren we wel of er echt BCOL stond.
```
        DB    "("
        CALL  GETBYT          ; lees voorgrondkleur
        AND   &H0F
        ADD   A,A
        ADD   A,A
        ADD   A,A
        ADD   A,A
        LD    (BUF1),A
```
Vervolgens  controleren  we  op het  haakje en  halen we  de 
voorgrondkleur  op. Deze  verplaatsen we  naar de  4 hoogste 
bits, en we slaan deze waarde zolang op in (BUF1).

```
        RST   8
        DB    ","
        CALL  GETBYT          ; lees achtergrondkleur
        AND   &H0F
        LD    (BUF2),A
        RST   8
        DB    ")"
```

Ook de  achtergrondkleur wordt  opgehaald. De AND instructie 
is nodig omdat alleen de onderste vier bits van belang zijn. 
Deze   waarde  wordt  in  (BUF2)  opgeslagen,  en  er  wordt 
gecontroleerd of het laatste haakje er wel staat.

```
        LD    A,(BUF1)
        LD    C,A
        LD    A,(BUF2)
        OR    C
        LD    (&HFFEB),A      ; voor compatibiliteit BASIC
        DI
        OUT   (&H99),A
        LD    A,12+128        ; blinkkleuren naar R#12
        OUT   (&H99),A
        EI
        RET
```

De  juiste waarde  wordt berekend  en naar  VDP register  12 
gestuurd, dit  is hetzelfde als VDP(13). Om te zorgen dat in 
BASIC de waarde van VDP(13) nog kan worden opgevraagd zetten 
we  de waarde ook op de juiste plaats in het systeem RAM. HL 
wordt hier  niet veranderd,  daarom hoeft hij niet te worden 
gePUSHd en gePOPt.

```
BTIM:   RST   8
        DB    "I"
        RST   8
        DB    "M"             ; btIM
        RST   8
        DB    "("
        CALL  GETBYT          ; lees normaaltijd
        AND   &H0F
        LD    (BUF1),A
        RST   8
        DB    ","
        CALL  GETBYT          ; lees blinktijd
        AND   &H0F
        ADD   A,A
        ADD   A,A
        ADD   A,A
        ADD   A,A
        LD    (BUF2),A
        RST   8
        DB    ")"
        LD    A,(BUF1)
        LD    C,A
        LD    A,(BUF2)
        OR    C
        LD    (&HFFEC),A      ; voor compatibiliteit BASIC
        DI
        OUT   (&H99),A
        LD    A,13+128        ; blinktijden naar R#13
        OUT   (&H99),A
        EI
        RET

BUF1:   DS    1
BUF2:   DS    1
```

De  routine voor BTIM lijkt zo sterk op de routine voor BCOL 
dat  ik   hier  verder  niets  aan  heb  toe  te voegen. Dit 
gedeelte  van de  routine is  nu klaar,  alleen de standaard 
blink routine moet nog worden toegevoegd.

```
; BLINK
; Standaardroutine voor blinkmode SCREEN0:WIDTH80 (T2)
; Door Stefan Boer 21/10/92
; Sunrise Special #2 - (c) Stichting Sunrise 1992

; Invoer: (B,C)-(D,E) rechthoek
;         A=0: wissen / A<>0: zetten

LDIRVM: EQU   &H5C
LDIRMV: EQU   &H59

; check of B<=D en C<=E, verwissel indien nodig

BLINK:  LD    (MODE),A        ; bewaar mode
        LD    A,D
        CP    B
        JR    NC,CHECK2
        LD    L,D
        LD    D,B
        LD    B,L             ; verwissel B en D
```

Voor  het  juist functioneren  van de  BLINK routine  moet B 
kleiner of gelijk zijn aan D en C kleiner of gelijk zijn aan 
E.  Dit  wordt gecontroleerd,  de waardes  van de  registers 
worden  indien  nodig  verwisseld.  Omdat  er  geen  EX  B,D 
instructie  is  wordt dit  met een  klein truukje  gedaan, L 
wordt daarbij als hulpregister gebruikt.

```
CHECK2: LD    A,E
        CP    C
        JR    NC,SAVE
        LD    L,E
        LD    E,C
        LD    C,L             ; verwissel C en E

SAVE:   LD    (BEGIN),BC
        LD    (EIND),DE
```

Ook C  en E  zijn nu  gecheckt en  indien nodig  verwisseld, 
vervolgens  worden  de  co�rdinaten  opgeslagen  voor  later 
gebruik.

```
; bereken hoogte

        LD    A,E
        SUB   C
        INC   A               ; A=aantal regels
        LD    (HOOGTE),A
```

Dit  stukje spreekt  eigenlijk voor  zich, de hoogte van het 
blok waarvan  de blink  mode aan  of uit  gezet gaat  worden 
wordt hier uitgerekend en opgeslagen.

```
; bereken beginadres

        LD    HL,0
        XOR   A
        CP    C
        JP    Z,ZETWEG
        LD    DE,10
        LD    B,C
TELOP:  ADD   HL,DE
        DJNZ  TELOP
ZETWEG: LD    DE,2048         ; beginadres blinktabel VRAM
        ADD   HL,DE
        LD    (BEGADR),HL
```

C  bevat  de  bovenste  regel  van  het  blok waarvan  we de 
"blinkstand"  gaan wijzigen,  het beginadres  van deze regel 
wordt  berekend.  Hiervoor wordt  C vermenigvuldigd  met 10, 
want elke regel van 80 karakters neemt 80 bits = 10 bytes in 
beslag.  Voor  het  vermenigvuldigen  wordt  de  methode van 
herhaald optellen gebruikt. Het zo berekende adres wordt bij 
het startadres  van de  blinktabel in het VRAM opgeteld. Een 
speciaal  geval  hebben  we  als  C=0,  als  we hierop  niet 
speciaal zouden checken zou bij C=0 het verkeerde beginadres 
(namelijk 2048 + 10) worden genomen. Het gevonden beginadres 
wordt opgeslagen voor later gebruik.

```
; wis masker

        LD    HL,MASKER
        LD    B,10
LEEG:   LD    (HL),0
        INC   HL
        DJNZ  LEEG
```

Ik  heb voor  deze routine  een systeem  bedacht waarbij  er 
eerst een  masker wordt  aangemaakt, dat  vervolgens over de 
juiste regels van de blink tabel wordt geAND of geORd. Eerst 
wordt dit masker leeg gemaakt, d.w.z. gevuld met nullen. Het 
belangrijkste  en ingewikkeldste  gedeelte van de routine is 
het maken van het masker.

```
; maak masker

; bepaal eerste byte in masker die moet worden aangepast

        LD    A,(BEGIN+1)     ; linkse X coordinaat
        LD    HL,MASKER
        SRL   A
        SRL   A
        SRL   A               ; A=A/8
        LD    C,A
        LD    B,0             ; LD BC,A
        ADD   HL,BC           ; sla bytes over
```

Hier wordt  aan de  hand van  de linker x coordinaat bepaald 
welke  bytes aan  de linkerkant van het masker kunnen worden 
overgeslagen. Bijvoorbeeld:  linker x  coordinaat =  10, dan 
hoeft  met de  linker 8 posities niets te gebeuren en is het 
masker daar dus 0.

```
; bepaal eerste bit in die byte die moet worden aangepast

        LD    A,(BEGIN+1)
        AND   7               ; A=bitnummer
        OR    A
        JP    Z,SPEC2         ; nog een speciaal geval!
        LD    B,A
        LD    A,255           ; alle bits 1
MAAK0:  SRL   A               ; maak bit 0 en schuif
        DJNZ  MAAK0
        LD    (HL),A          ; zet byte weg
BACK:   LD    (VLGND),HL
```

Hier wordt het al wat moeilijker. We halen wederom de linker 
x co�rdinaat  op en  bekijken daarvan alleen de drie laagste 
bits,  dit het bit nummer aangeven. Hiermee wordt het masker 
aan  de  linkerkant  gemaakt.  We  maken  hiervoor een  byte 
11111111 en  schuiven met  het juiste aantal SRL instructies 
de  juiste hoeveelheid nullen erin (aan de linkerkant). Stel 
de linker x co�rdinaat is 3, dan moet het masker dus worden

00011111 masker

01234567 x co�rdinaat

Dat  zijn precies  drie nullen. Een uitzondering moet worden 
gemaakt als er helemaal niet geschoven moet worden, daarvoor 
wordt  er  naar  SPEC2  gesprongen. SPEC2  maakt het  masker 
11111111,  en  springt  dan  naar  BACK.  Op (HL)  wordt het 
zojuist  berekende  masker  neergeset,  in (VLGND)  wordt de 
waarde van HL bewaard.

```
; bepaal laatste byte in masker die moet worden aangepast

ACHTER: LD    A,(EIND+1)      ; rechtse x coordinaat
        LD    B,A
        LD    A,79
        SUB   B
        LD    HL,MASKER+9
        SRL   A
        SRL   A
        SRL   A               ; a=a/8
        LD    C,A
        LD    B,0
        OR    A               ; wis carry (voor SBC)
        SBC   HL,BC           ; laatste byte
        LD    (LAATS),HL
```

Hier wordt  gekeken welke bytes we aan de rechterkant kunnen 
overslaan   omdat  daar   toch  niets   verandert.  Met  SRL 
instructies wordt het aantal bytes uitgerekend, het gevonden 
adres wordt in (LAATS) bewaard.

```
; maak gebied na byte links t/m byte rechts FF

        LD    DE,(VLGND)
        RST   &H20            ; vergelijk DE met HL
        JP    Z,SPECIA        ; speciaal geval!
MAAKFF: LD    (HL),255
        DEC   HL
        RST   &H20
        JP    NZ,MAAKFF
```

Bovenaan staat  al precies wat dit stukje doet. De bytes van 
(VLGND)+1  t/m  (LAATS)  worden  255  gemaakt, het  gedeelte 
tussen  de linker-  en de  rechtergrens moet immers allemaal 
worden "geblinkt".  Ook hier  weer een  speciaal geval:  als 
alles  zich in een byte afspeelt, dus (VLGND) = (LAATS), dan 
moet deze  byte op een aparte manier worden bepaald (zie bij 
het label SPECIA) en rest van de bytes blijft 0.

```
; bepaal laatste bit in die byte die moet worden aangepast

        LD    A,(EIND+1)
        AND   7               ; a=bitnummer
        XOR   7               ; a=7-a
        JP    Z,INVERT        ; masker klaar
        LD    B,A
        LD    A,255           ; alle bits 1
MAAK_0: SLA   A               ; maak bit 0 en schuif
        DJNZ  MAAK_0
        LD    HL,(LAATS)
        LD    (HL),A          ; zet laatste byte weg
```

Hier  wordt aan  de rechterkant de juiste hoeveelheid nullen 
toegevoegd. Eerst  wordt weer  het bitnummer berekend. Omdat 
we  het nu  van de rechterkant bekijken is de XOR instructie 
nodig. Het masker is nu klaar.

```
; inverteer masker als MODE=0 (wissen)

INVERT: LD    A,(MODE)
        OR    A
        JP    NZ,POKE
        LD    HL,MASKER
        LD    B,10
INVER2: LD    A,(HL)
        XOR   255
        LD    (HL),A
        INC   HL
        DJNZ  INVER2
```

Het masker dat we hebben gemaakt bevat een 0 op posities die 
hetzelfde  moeten blijven  en een  1 op  posities die moeten 
veranderen.  Voor  het  aanzetten  van de  blinkmode is  dit 
precies goed, we kunnen het masker dan gewoon over de juiste 
regels  in  de  blink  tabel  ORren.  Het  uitzetten  van de 
blinkmode gaat  echter met AND, en daar moeten de bits juist 
1  zijn om  de blinkstand  niet te laten veranderen. Vandaar 
dat hele masker geXORd wordt als (MODE)=0.

```
; masker is klaar, zet het over de blinktabel

POKE:   LD    A,(HOOGTE)
        LD    B,A
POKE1:  PUSH  BC
        LD    HL,(BEGADR)
        LD    DE,BUFFER
        LD    BC,10
        CALL  LDIRMV          ; kopieer regel uit VRAM
                              ; naar BUFFER
```

Het B  register bevat  het aantal regels waarover het masker 
moet  worden gezet.  We maken namelijk een lus met DJNZ. Met 
een CALL  LDIRMV wordt  een regel  uit de blink tabel in het 
VRAM naar een buffer in het RAM gekopieerd.

```
        CALL  CALCUL          ; bereken nieuwe data
        LD    DE,(BEGADR)
        LD    HL,BUFFER
        LD    BC,10
        CALL  LDIRVM
```

Met   CALL  CALCUL  wordt  het  masker  er  overheen  gezet, 
vervolgens wordt  de nieuwe  regel van de blinktabel met een 
LDIRVM in de blinktabel in het VRAM gezet.

```
        LD    HL,(BEGADR)
        LD    BC,10
        ADD   HL,BC
        LD    (BEGADR),HL
        POP   BC
        DJNZ  POKE1

        RET                   ; einde hoofdroutine
```

Het nieuwe adres in de blinktabel in het VRAM wordt berekend 
en weggezet, einde van de DJNZ lus. Met een RET besluiten we 
de hoofdroutine.

```
; zet het masker over de buffer

CALCUL: LD    A,(MODE)
        OR    A
        JP    Z,WIS
```

Eerst  wordt gekeken  of het  masker moeten  ORren of ANDen. 
Vervolgens wordt naar de juiste routine gesprongen.

```
ZET:    LD    HL,MASKER
        LD    DE,BUFFER
        LD    B,10
ZET1:   LD    A,(DE)
        OR    (HL)
        LD    (DE),A
        INC   DE
        INC   HL
        DJNZ  ZET1
        RET
```

Hier wordt  het masker  over de buffer geORd. Verdere uitleg 
overbodig.

```
WIS:    LD    HL,MASKER
        LD    DE,BUFFER
        LD    B,10
WIS2:   LD    A,(DE)
        AND   (HL)
        LD    (DE),A
        INC   DE
        INC   HL
        DJNZ  WIS2
        RET
```

Hier  wordt het  masker over de buffer geAND. Verdere uitleg 
overbodig.

```
; speciaal geval: zowel begin als eind van breedte
; ligt in 1 byte

SPECIA: LD    HL,(VLGND)      ; adres in masker
        LD    A,(EIND+1)
        AND   7               ; a=bitnummer
        XOR   7               ; a=7-a
        JP    Z,SPEC3         ; NOG een speciaal geval!
        LD    B,A
        LD    A,255           ; alle bits 1
MAK__0: SLA   A               ; maak bit 0 en schuif
        DJNZ  MAK__0
        LD    C,A             ; mini-masker
BACK2:  LD    A,(HL)
        AND   C               ; bereken juiste byte
        LD    (HL),A
        JP    INVERT          ; ga verder met
                              ; normale routine
```

Dit is  de speciale  routine waar ik het al eerder over had. 
Dit komt voor als de beide x co�rdinaten in een byte liggen. 
Stel  de linker  x is  3 en de rechter x 4. Dan was de eerst 
byte  van  het masker  al 00011111,  zoals we  eerder hadden 
gezien. De  9 andere  bytes van  het masker zijn gewoon 0 en 
dus  al goed.  We moeten  nu alleen  nog drie  nullen aan de 
linkerkant van  de eerst  byte toevoegen.  Dit kan  niet met 
schuiven,   omdat  dan   de  linkerkant  verloren  gaat.  We 
berekenen daarom  dezelfde byte  die we  nodig zouden hebben 
als  de x  co�rdinaten niet  in dezelfde byte zouden zitten, 
dit is  dus 11111000.  Vervolgens ANDen  we die over de oude 
byte, en klaar is Kees!! Het masker is nu ook klaar.

```
; nog twee speciale gevallen!

SPEC2:  LD    (HL),255
        JP    BACK

SPEC3:  LD    C,255
        JP    BACK2
```

Hier  worden de  gevallen opgelost waarbij helemaal geen nul 
moet worden  ingeschoven. Door  de constructie  met een DJNZ 
lus  wordt deze lus namelijk ook doorlopen als het aantal te 
verschuiven  bits  gelijk is  aan 0.  Dit vangen  we af  met 
voorwaardelijke sprongen en deze twee kleine routinetjes.

```
; data gebied

BEGIN:  DS    2
EIND:   DS    2
MODE:   DS    1
HOOGTE: DS    1
BEGADR: DS    2
VLGND:  DS    2
LAATS:  DS    2
MASKER: DS    10
BUFFER: DS    10
```

Tot  slot wordt  het datagebied nog gedefinieerd. De routine 
is nu helemaal klaar!!!


## B L I N K B A L 

In het softwaremenu vindt u onder de naam "BLINKBAL.BAS" een 
simpel spelletje  computertennis, dat  gebruikt maakt van de 
vier nieuwe BASIC commando's.


## T E N S L O T T E 

Best een  ingewikkelde routine  om te  schrijven, maar  zeer 
handig in het gebruik. In machinetaal nooit meer ingewikkeld 
rekenen  als de blink mode gebruikt wordt, en in BASIC wordt 
het bovendien een heel stuk sneller.

Veel plezier met deze vier nieuwe BASIC commando's!

Stefan Boer