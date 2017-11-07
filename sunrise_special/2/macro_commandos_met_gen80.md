# M A C R O   C O M M A N D O - S   M E T   G E N 8 0 
                                                         

## W A T   I S   E E N   M A C R O ? 

Je  kunt  macro's  gebruiken  om  je assemblercode  een stuk 
flexibeler  en overzichtelijker  te maken.  Een macro kun je 
zien als  een extra  commando dat  door de  assembler in een 
aantal andere instructies wordt omgezet.

Nu zeg  je: Dat  is toch onzin. Het is toch veel duidelijker 
om  gewoon de  instructies te gebruiken. Ja, dat kan ook wel 
zo  zijn,  maar  als  je  10  keer dezelfde  instructie moet 
gebruiken en  een CALL  is niet  handig of  mogelijk, is het 
gebruik  van macro's  een mooi  alternatief. En  het is  nog 
makkelijk aan te passen ook.

Noot:  Alleen GEN80  en Microsoft  M80 werken, voor zover ik 
weet, met  macro's. WBASS2-gebruikers  kunnen of  deze tekst 
niet lezen of een andere assembler gaan aanschaffen.


## H E T   G E B R U I K 

Met macro's is het omzetten van de source van een BLOAD-file 
naar een  .COM-file een  koud kunstje. Het label BDOS had je 
natuurlijk al aan het begin gedefinieerd met
```
        BDOS:   EQU  #F37D
```
en  dan hoef je #F37D alleen maar in het begin te veranderen 
in 5 (of andersom als je van MSX-DOS naar BASIC wil).

Maar dan moet je ook de BIOS-commando's omzetten. Meestal is 
dat  dan een  kwestie van zoeken en vervangen (in TED). Maar 
als je  macro's gebruikt  had was  het een karweitje van ��n 
twaalfhonderdste uur (oftewel 3 seconde) geweest.

Ik  gebruik in mijn eigen sources altijd ��n van de volgende 
macro's:
```
        BIOS:   MACRO   @FNC            ;Voor MSX-DOS
                LD      IX,@FNC
                LD      IY,(#FCC0)
                CALL    #001C
                ENDM

        BIOS:   MACRO   @FNC            ;Voor BASIC
                CALL    @FNC
                ENDM
```
Het  is  dan een  koud kunstje  om een  BIOS-funktie aan  te 
roepen  vanuit  een  .COM-file. En  last but  not least, het 
wordt een heel stuk duidelijker:
```
                LD      IX,#0180
                LD      IY,(#FCC0)
                CALL    #001C
```
of
```
                BIOS    #0180
```

## C O M M A N D O - S 

Ik  werk zelf  alleen nog  maar met GEN80. (Ik heb eerst met 
WBASS2  gewerkt,  maar  toen  ik  GEN80  door  had,  was  ik 
verkocht.)  Ik  bespreek dit  dan ook  met de  commando's en 
voorbeelden  die  in  de  handleiding van  GEN80 staan.  M80 
werkt,  denk  ik,  precies  hetzelfde,  maar  je  weet  maar 
nooit...

Uit  het  bovenstaande voorbeeld  zal grotendeels  duidelijk 
zijn hoe  een macro  moet worden  gedefinieerd. Maar voor de 
zekerheid staat het hieronder iets beter.

MACRO:  De  naam van  het macro geef je op aan het begin van 
        de  regel.  Dan  zet  je  daar  MACRO  achter en  de 
        eventuele te gebruiken variabelen:

        [macro] MACRO   @var1(,@var2,...,@varX)

        De  variabelen met  een apestaartje ("@") erin, zijn 
        lokale variabelen.  Die komen  verder ook niet in de 
        .SYM-file  terecht  en  worden  alleen  gebruikt als 
        input.

ENDM:   Je be�indigt  een macro  met het  commando ENDM, dat 
        (natuurlijk) staat voor END of Macro.

DEFL:   Met  DEFL kan een label een waarde worden gegeven IN 
        een macro.

IF:     Met  IF kun  je voorwaardelijk  assembleren. Als  de 
        waarde van  de uitdrukking  ongelijk aan 0 is worden 
        de instructies op de regels na IF uitgevoerd tot aan 
        ELSE of ENDC.
        IFs zijn nestbaar tot een diepte van 8.
COND:   Gelijk aan IF.

ELSE:   Dit  commando  komt  voor  ENDIF  en  na IF.  Als de 
        uitdrukking achter IF wel 0 is, worden de commando's 
        op de regels na ELSE uitgevoerd tot aan ENDIF.

ENDC:   Dit geeft  het einde van een voorwaardelijk gedeelte 
        aan. 
ENDIF:  Gelijk aan ENDC.


## V O O R B E E L D E N 

Alle voorbeelden staan ook in de file MACRO.LIB. Je kunt dan 
de macro's die je nodig hebt daaruit halen.
De volgende voorbeelden komen uit de handleiding van GEN80:
```
        BC_DE:  MACRO   @PARAM1, @PARAM2
                LD      BC,@PARAM1
                LD      DE,@PARAM2
                ENDM
```
De instructie
```
                BC_DE   #4444,-5
```
zal door GEN80 worden omgezet in de assembler-instructies
```
                LD      BC,#4444
                LD      DE,-5
```

## E X C H A N G E 
```
        EXCH:   MACRO   @REG1,@REG2
                IF      @REG1=HL .AND. @REG2=DE
                EX      DE,HL
                ELSE
                IF      @REG1=DE .AND. @REG2=HL
                EX      DE,HL
                ELSE
                PUSH    @REG1
                PUSH    @REG2
                POP     @REG1
                POP     @REG2
                ENDIF           ;Niet vergeten!
                ENDM
```
Er wordt  eerst gekeken  of @REG1  HL en @REG2 DE is. In dat 
geval  is  EX  DE,HL  een  kortere  en  snellere  oplossing. 
Natuurlijk  moet  ook  worden gecontroleerd  of @REG1  DE en 
@REG2 HL is.
Is dat niet het geval dan worden de registerparen volgens de 
bekende (ja toch?) manier verwisseld.

Met dit  macro is het verwisselen van twee registerparen een 
koud  kunstje en  hoeft er  niet meer  worden gedacht aan EX 
DE,HL.  Alleen   SP  kun   je  niet   gebruiken.  Om   nu  2 
registerparen  te  verwisselen  hoef  je  alleen  maar  EXCH 
reg1,reg2  in te  geven en  bovenstaand macro  in je  source 
zetten.


##  A B S O L U T E   W A A R D E 
```
        ABS     MACRO
                OR      A
                JP      P,ABS@SYM        
                NEG
        ABS@SYM:
                ENDM
```
Dit  macro maakt A altijd positief. Als al A positief is dan 
wordt er  doorgeJumPt naar  het einde  van het macro (dat is 
dan de instructie na het macro).

@SYM wordt  omgezet in een hexadecimaal getal van 4 cijfers. 
Dit  macro was  anders niet  te maken. Het zorgt er ook voor 
dat de  naam van  het label verandert met de recursiediepte. 
Ook   was  anders   recursie  in   ��n  macro  zo  goed  als 
onmogelijk...


##  F A C U L T E I T 
```
        FACT:   MACRO   @RESULT,@N
                IF      @N=1
        @RESULT DEFL    1
                ELSE
                FACT    T@SYM,@N-1
        @RESULT DEFL    T@SYM*(@N)
                ENDIF
                ENDM
```
Dit  macro geeft  een label een faculteit van een getal mee. 
Helaas  werkt  het  maar  tot  6  faculteit. Het  werkt vrij 
ingewikkeld en  je kunt  beter gewoon 120 invullen in plaats 
van  dit  macro  te  gebruiken,  maar  het  is wel  een mooi 
voorbeeld.
```
        FACT:   MACRO   @RESULT,@N
```
Dit  definieert  het  macro  FACT  en  de  lokale variabelen 
@RESULT en @N zijn de lokale variabelen.
```
                IF      @N=1
       @RESULT: DEFL    1
```
Als er  naar 1  faculteit gevraagd wordt geef dan gewoon ��n 
terug.
```
                ELSE
                FACT    T@SYM,@N-1
```
Indien  niet  aan @N=1  voldaan wordt,  wordt FACT  nog eens 
aangeroepen met  als faculteit ��n minder en het label hangt 
af van de recursiediepte (@SYM, zie boven).
```
       @RESULT: DEFL    T@SYM*(@N)
                ENDIF
                ENDM
```
Het label  dat achter FACT bij het aanroepen staat krijgt de 
waarde  van  de  vorige  variabele  vermenigvuldigd met  het 
faculteitsgetal.


## B D O S 
```
        BDOS:   MACRO   @FN1,@FN2
                IF      "@FN2">""
                LD      DE,@FCB
                ENDC
                LD      C,@FN1
                CALL    5
                ENDM
```
Dit macro maakt het aanroepen van de BDOS iets makkelijker:
```
                BDOS    9,Tekst
```
is bijv.  genoeg om  de tekst  achter het label Tekst op het 
scherm te laten komen.
Maar  mensen die  MSX-DOS 2  gebruiken hebben eigenlijk niks 
aan dit  macro. MSX-DOS  2 heeft  meestal wel  meer dan  ��n 
input register(paar) nodig.


# V R A G E N ,   O P M E R K I N G E N ? 

Schrijf naar:
                     Stichting Sunrise
                    t.a.v. Kasper Souren
                        Postbus 2146
                2400 CC  Alphen aan den Rijn

Of bel Sunrise BBS Nuth.

Kasper Souren
