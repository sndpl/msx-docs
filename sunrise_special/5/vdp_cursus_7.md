                            V D P   C U R S U S   7 
                                                     
          
          
                               I N L E I D I N G 
          
          In het  alweer zevende  deel van  deze zeer populaire cursus 
          wordt een begin gemaakt met het behandelen van de schermmodi 
          van  de V9938  en de  opslag in het VRAM. In deel 8 volgt de 
          rest van  dit overzicht.  Als we  dat achter  de rug hebben, 
          hebben  we alle  theorie gehad en kunnen we beginnen met het 
          interessantste   gedeelte:   de  praktijkvoorbeelden.   Vele 
          nuttige, leerzame  of gewoon  leuke routines zullen de revue 
          passeren.
          
          Maar  eerst moeten  we nog  even het  laatste stukje theorie 
          behandelen.  De  MSX2+ schermen  worden (helaas)  erg weinig 
          gebruikt en  ik heb  ze al  vele malen eerder besproken, dus 
          die zal ik hier buiten beschouwing laten.
          
          
                              S C H E R M M O D I 
          
          Hoeveel schermmodi (schermsoorten) heeft een MSX2 volgens u? 
          Een  heleboel  MSX'ers  zullen  op  deze vraag  een verkeerd 
          antwoord  geven, namelijk  9 of zelfs 8! Het juiste antwoord 
          is 10! Een overzicht:
          
          BASIC:                  VDP:            Afkorting:
          --------------------------------------------------
          SCREEN 0: WIDTH 1-40    TEXT 1          T1
          SCREEN 0: WIDTH 41-80   TEXT 2          T2
          SCREEN 3                MULTI-COLOR     MC
          SCREEN 1                GRAPHIC 1       G1
          SCREEN 2                GRAPHIC 2       G2
          SCREEN 4                GRAPHIC 3       G3
          SCREEN 5                GRAPHIC 4       G4
          SCREEN 6                GRAPHIC 5       G5
          SCREEN 7                GRAPHIC 6       G6
          SCREEN 8                GRAPHIC 7       G7
          ---------------------------------------------------
          
          Vanaf nu  zal ik alle schermen benoemen volgens de afkorting 
          zoals  die in de derde kolom staat. Per scherm zal ik nu een 
          overzicht geven  van de mogelijkheden en hoe dat in het VRAM 
          is  opgeslagen. Deze  keer komen  T1, T2  en MC  aan bod. De 
          G-modes komen in deel 8 aan de beurt.
          
          
                                  T E X T   1 
          
          Formaat:        24 regels van 40 kolommen tekst
          Kleuren:        voorgrond- en achtergrondkleur kunnen worden
                          gekozen uit een palet van 512
          Karakters:      256 karakters, 6 (hor) * 8 (vert)
          VRAM:           karakterset     2048 bytes (8 * 256)
                          scherminhoud     960 bytes (40 * 24)
          
          Voor  het selecteren  van de  schermmode gebruikt  de VDP de 
          bits M1-M5. Deze bits zijn te vinden in R#0 en R#1:
          
          --------------------------------------------------------
          MSB   7   6   5   4   3   2   1   0   LSB
          
          R#0   0   DG IE2 IE1  M5  M4  M3  0     Mode Register #0
          R#1   0   BL IE0  M1  M2  0   SI MAG    Mode Register #1
          --------------------------------------------------------
          
          Om TEXT 1 te krijgen moet de VDP als volgt staan ingesteld:
          
                  M1      M2      M3      M4      M5
          T1      1       0       0       0       0
          
          De  karakterset  staat  in  de zogenaamde  Pattern Generator 
          Table.  Deze  tabel staat  in het  VRAM. Elk  karakter wordt 
          gecodeerd door 8 bytes. Er zijn 256 karakters, genummerd van 
          0 t/m  255. Het  beginadres van een karakter is als volgt te 
          berekenen:
          
          <begin tabel> + 8 * karakternummer
          
          Bit  0 en  1 van  elke byte  worden niet  getoond, omdat een 
          karakter 6  pixels breed  is in plaats van 8. Het beginadres 
          van  de Pattern  Generator Table  moet in  R#4 staan.  De 11 
          laagste bits  van dit  adres (bit  0 t/m  10) zijn altijd 0. 
          Alleen de 6 bovenste bits (bit 11 t/m 16) staan in R#4:
          
          ---------------------------------------------------------
          MSB   7   6   5   4   3   2   1   0   LSB
          
          R#4   0   0  A16 A15 A14 A13 A12 A11    Pattern Generator
          ---------------------------------------------------------
          
          In  BASIC is  deze waarde  op te  vragen c.q.  te veranderen 
          middels BASE(2).
          
          Voorbeeld van de opbouw van de Pattern Generator Table:
          
          MSB  7 6 5 4 3 2 1 0  LSB
          
               0 0 1 0 0 0 0 0            + 520
               0 1 0 1 0 0 0 0            + 521
               1 0 0 0 1 0 0 0            + 522
               1 0 0 0 1 0 0 0            + 523
               1 1 1 1 1 0 0 0            + 524
               1 0 0 0 1 0 0 0            + 525
               1 0 0 0 1 0 0 0            + 526
               0 0 0 0 0 0 0 0            + 527
          
          De A  staat op  adres 65  (ASCII van  A) *  8 =  520. Een  1 
          betekent  dat  de  voorgrondkleur  wordt  getoond, een  0 de 
          achtergrondkleur.
          
          De  Pattern  Name  Table  geeft  aan welk  karakter op  elke 
          positie  van het  scherm wordt getoond. Het beginadres staat 
          in R#2.  De laagste  10 bits (bit 0 t/m 9) zijn altijd 0, de 
          hoogste 7 bits (10 t/m 16) staan in R#2.
          
          ----------------------------------------------------
          MSB   7   6   5   4   3   2   1   0   LSB
          
          R#2   0  A16 A15 A14 A13 A12 A11 A10    Pattern Name
          ----------------------------------------------------
          
          In  BASIC is  deze waarde  op te  vragen c.q.  te veranderen 
          middels BASE(0).
                           
          De karakternummers  staan in de Pattern Name Table van links 
          naar rechts, van boven naar onder. Dus:
          
               |  0       1       2       3       ...     39
          -----+-----------------------------------------------
          0    |  0       1       2       3               39
          1    |  40      41      42      43              79
          ...  |
          23   |  920     921     922     923             959
          
          Het  adres  behorende  bij  de positie  (X,Y) kan  als volgt 
          worden berekend:
          
          <begin adres> + Y * 40 + X
          
          De  schermkleuren  worden gedefinieerd  met R#7.  De achter- 
          grondkleur (A,  tevens de kleur van de border) wordt bepaald 
          door  de  onderste  vier bits,  de voorgrondkleur  (V) wordt 
          bepaald door de bovenste vier bits.
          
          --------------------------------------------------
          MSB   7   6   5   4   3   2   1   0   LSB
          
          R#7   V3  V2  V1  V0  A3  A2  A1  A0    Text color
          --------------------------------------------------
          
          
                                  T E X T   2 
          
          Formaat:        24 of 26.5 regels van 80 kolommen tekst
          Kleuren:        voorgrond- en achtergrondkleur kunnen worden
                          gekozen uit een palet van 512
          Karakters:      256 karakters, 6 (hor) * 8 (vert)
          VRAM:           karakterset     2048 bytes (8 * 256)
          24 regels:      scherminhoud    1920 bytes (80 * 24)
                          blinktabel       240 bytes (1920 bits)
          26.5 regels:    scherminhoud    2160 bytes (80 * 27)
                          blinktabel       270 bytes (2160 bits)
          
          Om TEXT 2 te krijgen moet de VDP als volgt staan ingesteld:
          
                  M1      M2      M3      M4      M5
          T2      1       0       0       1       0
          
          Bit 7 van R#9 bepaalt het aantal regels van T2.
          
          -------------------------------------------------
          MSB   7   6   5   4   3   2   1   0   LSB
          
          R#9   LN  0   S1  S0  IL  EO  NT  DC  Mode Reg #3
          -------------------------------------------------
          
          Het scherm  heeft 26.5  regels als LN=1. Dit wordt niet door 
          BASIC  ondersteund.  Het  kan wel  gebruikt worden,  maar de 
          regels  24-26 kunnen niet met LOCATE/PRINT commando's worden 
          beschreven. Van  de 27ste  regel (regel  26) wordt alleen de 
          bovenste helft weergegeven.
          
          De  Pattern Generator  Table werkt hetzelfde als bij T1. Zie 
          dus bij T1.
          
          De  Pattern  Name  Table  geeft  aan  welk karakter  op elke 
          positie van  het scherm  wordt getoond. Het beginadres staat 
          in  R#2. De laagste 12 bits (bit 0 t/m 11) zijn altijd 0, de 
          hoogste 5 bits (12 t/m 16) staan in R#2.
          
          ----------------------------------------------------
          MSB   7   6   5   4   3   2   1   0   LSB
          
          R#2   0  A16 A15 A14 A13 A12  1   1     Pattern Name
          ----------------------------------------------------
          
          In  BASIC is  deze waarde  op te  vragen c.q.  te veranderen 
          middels BASE(0).  Let op:  in R#2  zijn bit 1 en 0 altijd 1, 
          maar bit 11 en bit 10 van het beginadres van de Pattern Name 
          Table zijn toch 0!
                           
          De karakternummers  staan in de Pattern Name Table van links 
          naar rechts, van boven naar onder. Dus:
          
               |  0       1       2       3       ...     79
          -----+-----------------------------------------------
          0    |  0       1       2       3               79
          1    |  80      81      82      83              159
          ...  |
          23   |  2080    2081    2082    2083            2159
          
          Het  adres  behorende  bij  de positie  (X,Y) kan  als volgt 
          worden berekend:
          
          <begin adres> + Y * 80 + X
          
          
                              B L I N K   M O D E 
          
          Aan deze mode is al eerder aandacht besteed. Elke positie op 
          T2  kan in  de blink-mode worden gezet. Voor elke positie is 
          een bit gereserveerd in de blink table. Als dit bit 1 is, is 
          de  blink  mode actief.  Het startadres  van de  blink tabel 
          staat in  R#3 en R#10. De bovenste acht bits staan in R#3 en 
          R#10, de onderste negen bits zijn altijd 0.
          
          --------------------------------------------------------
          MSB   7   6   5   4   3   2   1   0   LSB
          
          R#3  A13 A12 A11 A10  A9  1   1   1     Color table low
          R#10  0   0   0   0   0  A16 A15 A14    Color table high
          --------------------------------------------------------
          
          Let op! In R#3 zijn de bits 0 t/m 2 altijd 1, maar bit 6 t/m 
          8 van het beginadres van de blink tabel zijn toch altijd 0!
          
          De  blinkbits staan in de Blink Tabel van links naar rechts, 
          van boven naar onder.
          
          Het  adres  behorende  bij  de positie  (X,Y) kan  als volgt 
          worden berekend:
          
          <begin adres> + Y * 10 + (X\8)
          
          Dit blinkbits  van de posities (8,4) t/m (15,4) staan dus op 
          adres:
          
          4 * 10 + 1 = 41
          
          De waarde op adres 41 wordt dan als volgt geinterpreteerd:
          
            7     6     5      4      3      2      1      0   bit
          (8,4) (9,4) (10,4) (11,4) (12,4) (13,4) (14,4) (15,4)
          
          R#7  werkt net  zoals bij T1. De blinkkleuren staan in R#12, 
          op  dezelfde  manier als  bij R#7.  De posities  waarvan het 
          blink-bit 1  is, zullen beurtelings in de kleuren van R#7 en 
          de kleuren van R#12 worden getoond.
          
          Het  tempo  waarmee  dat gebeurt  staat in  R#13. De  tijden 
          kunnen worden opgegeven in ca. 1/6 seconde. De bovenste vier 
          bits  bepalen de  periode waarin  de kleuren van R#12 worden 
          getoond, de  onderste vier bits bepalen de periode waarin de 
          kleuren van R#7 worden getoond.
          
          ----------------------------------------------------
          MSB   7   6   5   4   3   2   1   0   LSB
          
          R#12  B3  B2  B1  B0  N3  N2  N1  N0    Blink Colour
          ----------------------------------------------------
          (B=blinkkleur tijd, N=normale kleur tijd)
          
          
                             M U L T I C O L O U R 
          
          Formaat:        48 regels van 64 blokjes van 4*4
          Kleuren:        16 kleuren uit een palet van 512
          VRAM:           kleuren         2048 bytes
                          posities         768 bytes
          Sprite:         Sprite mode 1
          
          Om  MULTI-COLOUR  te  krijgen moet  de VDP  als volgt  staan 
          ingesteld:
          
                  M1      M2      M3      M4      M5
          MC      0       1       0       0       0
          
          De  multi-colour mode  zit nogal  ingewikkeld in  elkaar. De 
          multi-colour  mode  wordt  heel  weinig  gebruikt,  omdat de 
          graphics heel erg blokkerig zijn. Bovendien zit de opslag in 
          het VRAM  zeer ingewikkeld in elkaar. Ik zal proberen om het 
          toch enigszins te verduidelijken.
          
          MC werkt  met blokjes  van 4*4 pixels. Deze 16 pixels hebben 
          allemaal dezelfde kleur.
          
          Een patroon bestaat in MC uit 16 blokjes, 8 hoog en 2 breed. 
          De  patronen  staan  in  de  Pattern  Generator  Table.  Het 
          beginadres  is te  vinden in R#4. De 11 laagste bits van dit 
          adres (bit  0 t/m  10) zijn  altijd 0.  Alleen de 6 bovenste 
          bits (bit 11 t/m 16) staan in R#4:
          
          ---------------------------------------------------------
          MSB   7   6   5   4   3   2   1   0   LSB
          
          R#4   0   0  A16 A15 A14 A13 A12 A11    Pattern Generator
          ---------------------------------------------------------
          
          In  BASIC is  deze waarde  op te  vragen c.q.  te veranderen 
          middels BASE(17).
          
          Er  zijn 256 patronen (genummerd van #0 t/m #255), die elk 8 
          bytes in  beslag nemen. De kleuren van de twee blokjes naast 
          elkaar  in het  patroon staan  steeds in 1 byte, die van het 
          linkse blokje  in de  4 hoogste  bits en die van het rechtse 
          blokje  in de  4 laagste  bits. We  stellen een  patroon als 
          volgt voor:
          
          AB
          CD
          EF
          GH
          IJ
          KL
          MN
          OP
          
          Elke letter staat voor een blokje. Dit patroon staat dan als 
          volgt in het VRAM:
          
          <adres> = <beginadres tabel> + 8 * <patroonnummer>
          
             MSB  7  6  5  4   3  2  1  0  LSB
          
          +0      A3 A2 A1 A0  B3 B2 B1 B0
          +1      C3 C2 C1 C0  D3 D2 D1 D0
          +2      E3 E2 E1 E0  F3 F2 F1 F0
          +3 etc.
          +7      O3 O2 O1 O0  P3 P2 P1 P0
          
          Voor  elk blokje zijn telkens vier bits gereserveerd, waarin 
          het kleurnummer (0-15) staat.
          
          Voor  de Pattern  Name Table  verdelen wij het scherm in 768 
          posities, 32  horizontaal en  24 verticaal. Verder werkt dit 
          weer  net zo als bij de vorige modi. Het adres behorende bij 
          de positie (X,Y) kan als volgt worden berekend:
          
          <beginadres tabel> + Y * 32 + X
          
          Het beginadres  van de  Pattern Name  Table staat in R#2. De 
          laagste  10 bits  (bit 0  t/m 9) zijn altijd 0, de hoogste 7 
          bits (10 t/m 16) staan in R#2.
          
          ----------------------------------------------------
          MSB   7   6   5   4   3   2   1   0   LSB
          
          R#2   0  A16 A15 A14 A13 A12 A11 A10    Pattern Name
          ----------------------------------------------------
          
          In  BASIC is  deze waarde  op te  vragen c.q.  te veranderen 
          middels BASE(15).
          
          In  deze  tabel  staat  voor  elke  positie welk  patroon er 
          getoond moet  worden. De Y-coordinaat van de positie bepaalt 
          welk gedeelte  van het  patroon wordt  getoond. Dit gaat als 
          volgt:
          
          Y                       gedeelte
          --------------------------------
          00 04 08 12 16 20       ABCD
          01 05 09 13 17 21       EFGH
          02 06 10 14 18 22       IJKL
          03 07 11 15 19 23       MNOP
          --------------------------------
          
          Zoals u ziet is dit behoorlijk ingewikkeld. Gelukkig zal dit 
          scherm  (zeker op  een MSX2)  zelden gebruikt worden. En als 
          het al gebruikt wordt, dan zal het vaak in BASIC zijn. En in 
          dat geval regelt BASIC alles voor u.
          
          In BASIC is de Name Table overigens zo ingevuld dat die niet 
          gewijzigd  hoeft  te  worden. Daar  wordt alleen  de Pattern 
          Generator  Table gewijzigd.  Op de eerste 4 regels (0 t/m 3) 
          staan naast  elkaar de  patronen 0  t/m 31. Op de volgende 4 
          regels  de patronen 32 t/m 63, enz. Tenslotte staan op regel 
          20 t/m 23 de patronen 160 t/m 191.
          
          De patronen  192 t/m  255 worden in BASIC dus niet gebruikt! 
          Dat is ook niet nodig. Rekent u even mee? 64 bij 48 blokken, 
          dat  zijn 3072  blokken. Een  patroon bevat  16 blokken, dus 
          3072 / 16 = 192 patronen.
          
          Tot slot  nog even  de border  kleur. Die wordt in de multi- 
          colour  mode (en  ook in alle Graphic modes) bepaald door de 
          vier laagste  bits van  R#7. De vier bovenste bits zijn niet 
          geldig.
          
          ----------------------------------------------------
          MSB   7   6   5   4   3   2   1   0   LSB
          
          R#7  ---ongeldig----  C3  C2  C1  C0    Border color
          ----------------------------------------------------
          
          De sprites  zijn in  het vorige  deel van  de VDP  cursus al 
          behandeld.
          
          
                                T O T   S L O T 
          
          Dit was het voor deze keer. De volgende keer zullen de zeven 
          grafische  modes  worden  behandeld (ja,  volgens de  VDP is 
          SCREEN  1 een  grafische mode!).  Experimenteert u maar eens 
          wat met SCREEN 3, u zult zien dat het best meevalt.
          
          Tot de volgende keer!
          
                                                          Stefan Boer
