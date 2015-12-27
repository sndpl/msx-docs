 R E C H T S T R E E K S   V I A   P C M   A F S P E L E N 
                                                            

De PCM  afspeelroutines in de turbo R zijn via een BIOS call 
te  gebruiken, maar  kunnen dan  maar op  een beperkt aantal 
snelheden ingesteld  worden. De  maximale sample-snelheid is 
16  kHz. Dat  is goed  voor een redelijk geluid. PCM samples 
voor de  Amiga, PC  (Soundblaster) en Apple Macintosh worden 
echter  regelmatig op snelheden van 22 tot 44 kHz opgenomen, 
en klinken  op een  turbo R  dan te langzaam als hiervoor de 
BIOS  wordt gebruikt.  Sample-data rechtstreeks  naar de PCM 
sturen via  de D/A-poorten  biedt de  oplossing. Het  is ook 
mogelijk   samples  met   een  hoge   snelheid  (dus  hogere 
kwaliteit) op  te nemen  door de  BIOS te  passeren, maar in 
deze tekst beperk ik me tot het afspelen van samples. Ik heb 
dit  uitgezocht voor toepassing in het programma PLAYSMP, de 
universele sample-speler  voor iedere MSX2, MSX2+ en turbo R 
(shareware  t.g.v. Jos-Tel,  staat ook  op Sunrise  Magazine 
#6).

Bij  het afspelen van een sample via de BIOS doet de turbo R 
het volgende:

1. Kijk welke processor actief is
   Is dit de Z80?
       Dan bewaar het resultaat en schakel om naar R800-ROM
2. Reset de PCM (stel hem in op schrijven)
3. Voor TELLER=1 tot LENGTE (sample)
       Lees een databyte
       Stuur hem naar de PCM
LUS1:  PCM klaar?
       Als niet klaar, dan ga naar LUS1
LUS2:  Wachttijd verstreken (i.v.m. afspeelsnelheid)?
       Als niet verstreken dan ga naar LUS2
   Verlaag TELLER, volgende byte
4. Reset de PCM
5. Was de oorspronkelijke processor Z80?
       Dan schakel die weer in
6. Klaar


Om  nu een  hogere afspeelsnelheid te bereiken, kunnen we de 
wachtlussen in  de PCM  routine simpelweg  overslaan. Het is 
ook   niet  noodzakelijk   de  processor  om  te  schakelen, 
aangezien een  Z80A snel  genoeg is voor samples van 44 kHz. 
Wil  je echter  de zekerheid  dat samples altijd op dezelfde 
snelheid worden  afgespeeld, dan  moet je er wel voor zorgen 
dat de processor altijd in dezelfde 'mode' staat.


                     P R O G R A M M A 

Hieronder vind je een voorbeeld van een ML-subroutine waarin 
een   PCM  sample  op  adres  HL  en  met  lengte  BC  wordt 
afgespeeld. Het  voorbeeld staat  ook op  disk onder de naam 
PCMPLAY.ASC.


;SpeelPCM

PCM:    CALL    RESPCM          ;reset de PCM
        CALL    PLAY            ;speel de sample af
RESPCM: DI
        LD      A,80H           ;reset de PCM
        OUT     (0A4H),A
        LD      A,3
        OUT     (0A5H),A
        EI
        RET                     ;klaar

PLAY:   LD      A,(HL)          ;pak byte op adres in HL
        OUT     (0A4H),A        ;stuur naar PCM datapoort
        INC     HL              ;verhoog pointer HL
        DEC     BC              ;verlaag teller BC
        LD      A,B
        OR      C               ;is BC al 0?
        JR      NZ,PLAY         ;nee, volgende byte
        RET                     ;ja, klaar


Bovenstaande afspeelroutine zal onder normale omstandigheden 
veel  te snel  zijn, ook  als de Z80 ingeschakeld is. Daarom 
moet in de PLAY routine een vertragingslus worden ingebouwd:


WACHT:  PUSH    BC              ;bewaar teller
        LD      B,WACHTTIJD     ;wachten (0..255)
LUS:    DJNZ    LUS             ;verlaag B, als niet 0 ga
                                ; naar LUS
        POP     BC              ;herstel teller


Deze wachtlus  is bijvoorbeeld in te bouwen tussen de INC HL 
en  DEC BC  instructies. De  BIOS van  de turbo R bevat geen 
wachtlus  zoals   hierboven  aangegeven,   maar  bepaalt  de 
afspeelsnelheid  aan de  hand van terugmeldingen van de PCM, 
wat een  stuk nauwkeuriger  is maar  de al  genoemde nadelen 
heeft. Onderstaande routine is een stukje disassembly uit de 
turbo  R BIOS,  dat thuishoort  op de  plaats waar hierboven 
alleen de instructie OUT (0A4H),A staat:


        OUT     (0A4H),A        ;stuur byte naar PCM
PAKPCM: IN      (0A4H),A        ;lees byte uit PCM
        OR      A               ;nul?
        JR      Z,PAKPCM        ;ja, wacht tot niet 0
        EXX                     ;alternatieve registers
PAKPC2: IN      A,(0A4H)        ;lees byte uit PCM
        CP      E               ;gelijk aan E'?
        JR      NZ,PAKPC2       ;nee, herhaal
        EXX                     ;terug naar normale
                                ; registers


Het  moge duidelijk  zijn dat de snelheid die de aanroepende 
routine aan de BIOS-CALL PLAYPCM heeft doorgegeven inmiddels 
in register E' is gezet (EXX; LD E,A; EXX). De turbo R scant 
tijdens   het   afspelen   van   een  sample   ook  nog   de 
toetsenbord-matrix  om  te  kijken  of  de  STOP-toets wordt 
ingedrukt. Ik  laat het aan de fantasie van de lezer over om 
daar zelf een routine voor te schrijven.

                                              Pierre Gielen