                            V D P   C U R S U S   8 
                                                     
          
          
                               I N L E I D I N G 
          
          We  zijn  alweer  bij  het  achtste  deel  van de  nog immer 
          populaire  VDP cursus  aangeland. Zoals beloofd gaan we deze 
          keer verder met het behandelen van de opslag van schermen in 
          het VRAM.  Deze keer  zullen we  SCREEN 1,  2 en 4 bekijken, 
          oftewel  G1, G2  en G3. De laatste schermen van de V9938 (G4 
          t/m G7) zullen in deel 9 worden besproken.
          
          Ik  was eerst  van plan om in dit deel alle Graphic Modes te 
          behandelen, maar toen ik met deze drie al over de 16 kB heen 
          zat  heb  ik  besloten  om het  daar maar  bij te  laten. De 
          betekent dus  even goed  dat dit deel van de VDP cursus niet 
          in  ��n keer  kan worden ingeladen. Dat is de eerste keer in 
          de geschiedenis van de VDP cursus dat dat gebeurt!
          
          
                               G R A P H I C   1 
          
          Formaat:        24 regels van 32 kolommen tekst
          Kleuren:        16 uit een palet van 512
                          2 kleuren per 8 karakters
          Karakters:      256 karakters, 8 (hor) * 8 (vert)
          VRAM:           karakterset     2048 bytes (8 * 256)
                          scherminhoud     768 bytes (32 * 24)
                          kleuren           32 bytes
                          sprite patronen 2048 bytes (8 * 256)
                          sprite attr.     128 bytes (4 * 32)
          Sprites:        Sprite mode 1
          
          Voor  het selecteren  van de  schermmode gebruikt  de VDP de 
          bits M1-M5. Deze bits zijn te vinden in R#0 en R#1:
          
          --------------------------------------------------------
          MSB   7   6   5   4   3   2   1   0   LSB
          
          R#0   0   DG IE2 IE1  M5  M4  M3  0     Mode Register #0
          R#1   0   BL IE0  M1  M2  0   SI MAG    Mode Register #1
          --------------------------------------------------------
          
          Om  GRAPHIC  1  te  krijgen  moet  de  VDP  als  volgt staan 
          ingesteld:
          
                  M1      M2      M3      M4      M5
          G1      0       0       0       0       0
          
          De  karakterset  staat  in  de zogenaamde  Pattern Generator 
          Table.  Deze  tabel staat  in het  VRAM. Elk  karakter wordt 
          gecodeerd door 8 bytes. Er zijn 256 karakters, genummerd van 
          0 t/m  255. Het  beginadres van een karakter is als volgt te 
          berekenen:
          
          <begin tabel> + 8 * karakternummer
          
          Het beginadres  van de  Pattern Generator  Table moet in R#4 
          staan.  De 11 laagste bits van dit adres (bit 0 t/m 10) zijn 
          altijd 0. Alleen de 6 bovenste bits (bit 11 t/m 16) staan in 
          R#4:
          
          ---------------------------------------------------------
          MSB   7   6   5   4   3   2   1   0   LSB
          
          R#4   0   0  A16 A15 A14 A13 A12 A11    Pattern Generator
          ---------------------------------------------------------
          
          In  BASIC is  deze waarde  op te  vragen c.q.  te veranderen 
          middels BASE(7).
          
          Voorbeeld van de opbouw van de Pattern Generator Table:
          
          MSB  7 6 5 4 3 2 1 0  LSB
          
               0 0 1 1 1 0 0 0            + 520
               0 1 1 0 1 1 0 0            + 521
               1 1 0 0 0 1 1 0            + 522
               1 1 0 0 0 1 1 0            + 523
               1 1 1 1 1 1 1 0            + 524
               1 1 0 0 0 1 1 0            + 525
               1 1 0 0 0 1 1 0            + 526
               0 0 0 0 0 0 0 0            + 527
          
          De A  staat op  adres 65  (ASCII van  A) *  8 =  520. Een  1 
          betekent  dat  de  voorgrondkleur  wordt  getoond, een  0 de 
          achtergrondkleur.
          
          Per groep  van 8  karakters kunnen  de voor- en achtergrond- 
          kleur  worden  ingesteld  (kar.  #0-#7,  #8-#15,  etc).  Het 
          startadres  van de kleurentabel wordt bepaald door de waarde 
          van R#3  en R#10.  Alleen de  hoogste 11 bits (bit 6 t/m 16) 
          staan in deze registers:
          
          --------------------------------------------------------
          MSB   7   6   5   4   3   2   1   0   LSB
          
          R#3  A13 A12 A11 A10  A9  A8  A7  A6    Color table low
          R#10  0   0   0   0   0  A16 A15 A14    Color table high
          --------------------------------------------------------
          
          De kleurentabel is als volgt opgebouwd:
          
          MSB  7 6 5 4 3 2 1 0  LSB
          
               ---V--- ---A---            + 0  karakters 0-7 
               ---V--- ---A---            + 1  karakters 8-15
                                               
               ---V--- ---A---            + 31 karakters 248-255
          
          Waarbij V  staat voor  de voorgrondkleur (0-15) en A voor de 
          achtergrondkleur (ook 0-15).
          
          De  Pattern  Name  Table  geeft  aan welk  karakter op  elke 
          positie  van het  scherm wordt getoond. Het beginadres staat 
          in R#2.  De laagste  10 bits (bit 0 t/m 9) zijn altijd 0, de 
          hoogste 7 bits (10 t/m 16) staan in R#2.
          
          ----------------------------------------------------
          MSB   7   6   5   4   3   2   1   0   LSB
          
          R#2   0  A16 A15 A14 A13 A12 A11 A10    Pattern Name     
          ----------------------------------------------------
          
          In  BASIC is  deze waarde  op te  vragen c.q.  te veranderen 
          middels BASE(5).
                           
          De karakternummers  staan in de Pattern Name Table van links 
          naar rechts, van boven naar onder. Dus:
          
               |  0       1       2       3       ...     31
          -----+-----------------------------------------------
          0    |  0       1       2       3               31
          1    |  32      33      34      35              63
          ...  |
          23   |  736     737     738     739             767
          
          Het  adres  behorende  bij  de positie  (X,Y) kan  als volgt 
          worden berekend:
          
          <begin adres> + Y * 32 + X
          
          De  borderkleur wordt  gedefinieerd met  R#7, en wel door de 
          vier minst significante bits.
          
          ----------------------------------------------------
          MSB   7   6   5   4   3   2   1   0   LSB
          
          R#7   --ongeldig---   B3  B2  B1  B0    Border color
          ----------------------------------------------------
          
          Het tweede  gedeelte, met  G2 en  G3, kunt  u in het submenu 
          vinden.
          
                                                          Stefan Boer
          
          
          
                              - TWEEDE GEDEELTE -
          
                            V D P   C U R S U S   8 
                                                     
          
          
                          G R A P H I C   2   E N   3 
          
          Deze  schermmodes (SCREEN  2 en 4) komen zoveel overeen, dat 
          ik ze hier in ��n keer bespreek.
          
          Formaat:        24 regels van 32 patronen
          Kleuren:        per 8 pixels 2 van de 16 kleuren
                          deze 16 kleuren uit een palet van 512
          Patronen:       3*256 patronen, 8 (hor) * 8 (vert)
          VRAM:           patronen        6144 bytes (8 * 3 * 256)
                          kleuren         6144 bytes (8 * 3 * 256)
                          scherminhoud     768 bytes (24 * 32)
                          spritepatronen  2048 bytes (8 * 256)
                          sprite attr.     128 bytes (32 * 4)
          Sprites: G2:    Sprite Mode 1
                   G3:    Sprite Mode 2
          
          Graphic 2  komt overeen met SCREEN 2, Graphic 3 komt overeen 
          met  SCREEN 4.  De enige  twee verschillen  tussen G2  en G3 
          zijn:
          
          - G2 is wel MSX1 compatibel, G3 niet
          - G2 heeft Sprite Mode 1, G3 heeft Sprite Mode 2
          
          Aan het  einde van het artikel zullen ook heel kort de beide 
          Sprite Modes en de verschillen worden behandeld.
          
          Om  G2  resp.  G3 te  krijgen moet  de VDP  als volgt  staan 
          ingesteld:
          
                  M1      M2      M3      M4      M5
          G2      0       0       1       0       0
          G3      0       0       0       1       0
          
          SCREEN 2  en 4  hebben drie pattern generators, deze pattern 
          generators werken net zo als de pattern generator van SCREEN 
          1. Het scherm is verdeeld in drie gelijke delen van 8 regels 
          hoog:
          
          Regel  0 t/m  7:        Pattern generator #1
          Regel  8 t/m 15:        Pattern generator #2
          Regel 16 t/m 23:        Pattern generator #3
          
          Voor  elke positie  op het scherm (er zijn 768 posities) kan 
          dus  een  apart  pattern  worden  gedefinieerd,  omdat  elke 
          pattern  generator  256  patterns  bevat.  Een  pattern  uit 
          pattern generator  #2 kan niet op regel 7 worden getoond! Op 
          deze   manier  wordt   er  een  256  bij  192  pixel  scherm 
          gesimuleerd,  terwijl   er  in   feite  toch   met  patterns 
          (karakters) wordt gewerkt.
          
          Het startadres van de pattern generator tables staan in R#4, 
          alleen   de  4  hoogste  bits  (13  t/m  16)  kunnen  worden 
          opgegeven. Het  startadres van  de generator  tables is  dus 
          altijd een veelvoud van 8 kB.
          
          ---------------------------------------------------------
          MSB   7   6   5   4   3   2   1   0   LSB
          
          R#4   0   0  A16 A15 A14 A13  1   1     Pattern Generator
          ---------------------------------------------------------
          
          Hoewel bit  1 en  0 van R#4 in G2 en G3 altijd op "1" staan, 
          moet  hiervoor  toch  0 worden  gelezen! In  BASIC kan  deze 
          waarde  worden opgevraagd c.q. veranderd middeld BASE(12) in 
          SCREEN 2 of BASE(22) in SCREEN 4.
          
          Deze pattern generator tables zijn als volgt opgebouwd. Voor 
          elk patroon  zijn acht  bytes gereserveerd.  Elke bit in die 
          byte  stelt een  pixel voor. Een 1 betekent dat dat pixel in 
          de voorgrondkleur wordt getoond, een 0 de achtergrondkleur.
          
          Op het  adres dat in R#4 staat begint generator #1. Voor elk 
          pattern  zijn  8  bytes  gereserveerd.  Er zijn  8 patronen, 
          genummerd  van #0 t/m #255, dus de tabel is 2048 bytes lang. 
          Daarachter volgende  generator #2  en #3 op dezelfde manier. 
          Een  aantal voorbeelden  (adres ten  opzichte van  waarde in 
          R#4):
          
          Adres:  Bevat:  
          
          0       Regel 0 van pattern   #0, generator #1
          1       Regel 1 van pattern   #0, generator #1
          7       Regel 7 van pattern   #0, generator #1
          8       Regel 0 van pattern   #1, generator #1
          2047    Regel 7 van pattern #255, generator #1
          2048    Regel 0 van pattern   #0, generator #2
          4095    Regel 7 van pattern #255, generator #2
          5572    Regel 4 van pattern #184, generator #3
          6143    Regel 7 van pattern #255, generator #3
          
          Het  beginadres van  een bepaald  pattern kan  dus als volgt 
          worden berekend:
          
          adres = (generator-1)*2048 + pattern*8
          
          
          De  kleurentabel werkt net zo, alleen wordt nu per regel van 
          een pattern  aangegeven welke  kleur de voorgrondkleur is en 
          welke  kleur de  achtergrondkleur. Hiervoor  zijn dus  ook 8 
          bytes per  pattern nodig.  Er zijn ook drie kleurentabellen. 
          De  kleurentabellen beginnen op het adres dat in R#3 en R#10 
          wordt bepaald:
          
          --------------------------------------------------------
          MSB   7   6   5   4   3   2   1   0   LSB
          
          R#3  A13  1   1   1   1   1   1   1     Color table low
          R#10  0   0   0   0   0  A16 A15 A14    Color table high
          --------------------------------------------------------
          
          Merk  op  dat  alleen  de  vier  hoogste bits  kunnen worden 
          veranderd (bit  13 t/m  16). De  bits 0  t/m 6  van R#3 zijn 
          altijd  "1", toch  moet daar  voor worden  gelezen! Ook deze 
          tabel begint dus altijd op een veelvoud van 8 kB.
          
          De kleurentabel is als volgt opgebouwd:
          
          MSB  7 6 5 4 3 2 1 0  LSB
          
               ---V--- ---A---    +    0  tabel 1 karakter   0 regel 0
               ---V--- ---A---    +    1                       regel 1
               ---V--- ---A---    +    7                       regel 7
           
               ---V--- ---A---    + 6143  tabel 3 karakter 255 regel 7
          
          Waarbij  V voor de voorgrondkleur staat en A voor de achter- 
          grondkleur. Het  beginadres van  de kleuren  van een pattern 
          kan als volgt worden gevonden: (tabel=1, 2 of 3)
          
          adres = (tabel-1)*2048 + pattern*8
          
          De  Pattern  Name  Table  geeft  aan  welk karakter  op elke 
          positie van  het scherm wordt getoond. Het scherm is hierbij 
          verdeeld  in drie  blokken: upper,  middle en lower. U raadt 
          het al:  voor de posities in het upper gedeelte (regel 0 t/m 
          7) wordt pattern generator #1 en color table #1 gebruikt, en 
          voor  middle en  lower #2  resp. #3. Op deze manier kan voor 
          elk van  de 768 posities een ander pattern en andere kleuren 
          worden  gekozen, waardoor  een 256 bij 192 pixelscherm wordt 
          gesimuleerd.
          
          Het beginadres  van de  pattern name  tabel staat in R#2. De 
          laagste  10 bits  (bit 0  t/m 9) zijn altijd 0, de hoogste 7 
          bits (10 t/m 16) staan in R#2.
          
          ----------------------------------------------------
          MSB   7   6   5   4   3   2   1   0   LSB
          
          R#2   0  A16 A15 A14 A13 A12 A11 A10    Pattern Name     
          ----------------------------------------------------
          
          In BASIC  is deze  waarde op  te vragen  c.q. te  veranderen 
          middels BASE(10) in SCREEN 2, en BASE(20) in SCREEN 4.
                           
          De karakternummers  staan in de Pattern Name Table van links 
          naar rechts, van boven naar onder. Dus:
          
               |  0       1       2       3       ...     31
          -----+-----------------------------------------------
          0    |  0       1       2       3               31
          1    |  32      33      34      35              63
          ...  |
          23   |  736     737     738     739             767
          
          Het  adres  behorende  bij  de positie  (X,Y) kan  als volgt 
          worden berekend:
          
          <begin adres> + Y * 32 + X
          
          Nogmaals de tabel met de blokken:
          
          Naam:   Regels: Pattrn: Color:
          ------------------------------
          Upper   0-7     #1      #1
          Middle  8-15    #1      #1
          Lower   16-23   #1      #1
          ------------------------------
          
          Let u hier goed op, patroon #13 is een heel ander patroon op 
          regel 7 dan op regel 8!
          
          De  borderkleur wordt  gedefinieerd met  R#7, en wel door de 
          vier minst significante bits.
          
          ----------------------------------------------------
          MSB   7   6   5   4   3   2   1   0   LSB
          
          R#7   --ongeldig---   B3  B2  B1  B0    Border color
          ----------------------------------------------------
          
          
                  S C R E E N   4   V E E L   G E B R U I K T 
          
          Deze schermen  zijn niet  bitmapped (zoals  de SCREENs 5 t/m 
          8).  Ze zijn daarom niet zo makkelijk te programmeren, om de 
          kleur van  een bepaald  pixel te  veranderen moet er op drie 
          plaatsen  in het  VRAM iets  worden veranderd,  en bovendien 
          gaan er  dan soms  andere pixels  in de buurt mee. Er kunnen 
          maar   twee  kleuren  per  acht  horizontale  pixels  worden 
          gebruikt, dat komt de kwaliteit van de graphics ook niet ten 
          goede. De  commando's van de VDP kunnen niet worden gebruikt 
          om te kopi�ren.
          
          Toch  wordt  SCREEN  4  behoorlijk  vaak  gebruikt  bij MSX2 
          software.  Om maar  eens een  paar bekende titels te noemen: 
          Hydefos,  Psycho  World,  Space  Manbow  (!!),  Firehawk  en 
          Superrunner. Misschien  gelooft u  het eerst  niet (net  als 
          ik),  maar als  u goed  kijkt kunt  u zien  dat deze spellen 
          nergens meer  dan twee  kleuren per acht pixels gebruiken in 
          het  speelscherm (vaak is het gedeelte met de score e.d. wel 
          in SCREEN 5, d.m.v. een screensplit).
          
          SCREEN 4  is op  de sprites  na gelijk aan SCREEN 2 op MSX1. 
          Waarom   zijn  bovengenoemde  spellen  dan  niet  voor  MSX1 
          gemaakt? De eerste reden noemde ik al: de sprites. In Sprite 
          Mode 2  mogen twee  maal zoveel  sprites op  een horizontale 
          regel  worden geplaatst,  dus dat  knippert een stuk minder. 
          Maar dat  is niet  de enige reden: door gebruik te maken van 
          de  verticale hardwarescroll  en het  adjustregister van  de 
          MSX2  kunnen   op  deze  computer  smooth  scrolls  in  alle 
          richtingen worden gemaakt. Dat kan op MSX1 niet.
          
          In  SCREEN 5  (veel meer  een echt MSX2 scherm) kan dat veel 
          minder  goed,   omdat  daar   veel  meer  VRAM  moet  worden 
          verplaatst  om te  scrollen. En  dat is nu precies het grote 
          voordeel van  SCREEN 4  ten opzichte  van SCREEN 5: SCREEN 4 
          gebruikt  minder VRAM  en animaties nemen dus minder tijd in 
          beslag!
          
          
                      S P R I T E   M O D E   1   E N   2 
          
          Nog  even de verschillen tussen Sprite Mode 1 en Sprite Mode 
          2.  Eerst  even een  overzicht in  welke schermen  ze kunnen 
          worden gebruikt:
          
          Scherm:         Sprite Mode:
          ----------------------------
          T1              niet
          T2              niet
          MC              1
          G1              1
          G2              1
          G3              2
          G4              2
          G5              2
          G6              2
          G7              2
          ----------------------------
          
          Kort overzicht van de verschillen:
          
                          Sprite Mode 1:          Sprite Mode 2:
          ------------------------------------------------------------
          Kleuren         1 kleur per sprite      1 kleur per lijn 
                                                  van een sprite
          Aantal op ��n
          horizontale     4                       8
          lijn
          
          Sprite 32 pixels
          naar links      mogelijk                mogelijk
          verplaatsen                             per lijn
          
          Overlappende    niet mogelijk           mogelijk
          sprites OR
          (per lijn)
          
          Botsing detectie
          uitzetten (per  niet mogelijk           mogelijk
          lijn)
          ------------------------------------------------------------
          
          In deel zes van de VDP cursus wordt Sprite Mode 2 uitgebreid 
          besproken.  Voor  Sprite  Mode  1  is er  geen kleurentabel, 
          verder  werken de  tabellen hetzelfde. Behalve de attribuut- 
          tabel,  daar  wordt de byte die bij Sprite Mode 2 niet wordt 
          gebruikt, gebruikt voor de kleur van de sprite:
          
          Attributen per sprite:
          
          +0      Y coordinaat
          +1      X coordinaat
          +2      Patroon nummer
          +3      Kleurcode (0-15)
          
          Indien  bit 7  van de kleurcode wordt gezet, wordt de sprite 
          32  pixels   naar  links   verplaatst.  Helaas   weten  veel 
          programmeurs  niet wat  hier het  nut van is. Ik zal dat dus 
          maar eens uitleggen.
          
          
                          H E T   N U T   V A N   E C 
          
          Bit 7  van de kleurcode heet bij de beide Sprite Modes "EC". 
          Wat  heeft het  nu voor nut om een (gedeelte van een) sprite 
          32 pixels naar links te verplaatsen?
          
          U  kunt dit  gebruiken om  sprites ook aan de linkerkant van 
          het  scherm  pixel  voor  pixel  te  laten  verdwijnen  c.q. 
          verschijnen.  Bij  van links  naar rechts  bewegende sprites 
          zien  we  vaak  dat  de  sprite  aan  de  rechterkant netjes 
          verdwijnt, maar  aan de linkerkant zomaar opeens helemaal op 
          het scherm staat.
          
          Dit komt omdat X-coordinaat 255 betekent dat alleen de meest 
          linkse pixels van de sprite nog uiterst rechts te zien zien, 
          en bij  X=0 staat  de sprite  in zijn  geheel strak tegen de 
          linkerkant van het scherm.
          
          U kunt  dit voorkomen  door op  de juiste  manier gebruik te 
          maken  van het  EC bit, dat dus ook op MSX1 al aanwezig was! 
          Stel u  wilt een  sprite van  rechts naar  links laten gaan, 
          terwijl de sprite aan beide kanten van het scherm pixel voor 
          pixel verschijnt c.q. verdwijnt. Dit gaat als volgt:
          
          1) Zet EC op 0
          2) Verander de X coordinaat van 255 naar 0
          3) Zet EC op 1 en de X coordinaat gelijktijdig op 32
          4) Verander de X coordinaat van 32 naar 0
          
          Het is  heel belangrijk  dat stap  3 heel snel gebeurt, want 
          anders zal er een knippering in de animatie optreden.
          
          Ik  hoop dat  veel programmeurs  dit nu  ook gaan gebruiken, 
          want het is in sommige demo's en spellen echt geen gezicht!
          
          
                    T O T   D E   V O L G E N D E   K E E R 
          
          Hiermee  sluit  ik  dit  achtste deel  af. De  volgende keer 
          worden SCREEN  5 t/m  8 behandeld,  oftewel Graphic 4 t/m 7. 
          Daarna   kunnen  we  eindelijk  beginnen  met  de  praktijk- 
          voorbeelden, met zeer veel machinetaalroutines.
          
                                                          Stefan Boer
