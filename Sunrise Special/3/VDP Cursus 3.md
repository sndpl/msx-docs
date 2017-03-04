      M S X   2 / 2 +   V D P   C U R S U S   ( 3  )
                                                                
          
          
                   D E   C O M M A N D O   R E G I S T E R S 
          
          Zoals beloofd behandelen we deze keer de commando registers. 
          Hiermee  kunt u  de VDP laten kopiâren, lijnen trekken, etc. 
          Nu volgt eerst een overzicht van de commando's, logische be- 
          werkingen en commando registers.
          
          
                               O V E R Z I C H T 
          
          Naam:     Doel:   Bron:   Eenh:   Mnemonic: CM3 CM2 CM1 CM0
          ----------------------------------------------------------- 
          High-     VRAM    CPU     Byte    HMMC       1   1   1   1
          Speed     VRAM    VRAM    Byte    YMMM       1   1   1   0
          Move      VRAM    VRAM    Byte    HMMM       1   1   0   1
                    VRAM    VDP     Byte    HMMV       1   1   0   0
          Logical   VRAM    CPU     Dot     LMMC       1   0   1   1
          Move      CPU     VRAM    Dot     LMCM       1   0   1   0
                    VRAM    VRAM    Dot     LMMM       1   0   0   1
                    VRAM    VDP     Dot     LMMV       1   0   0   0
          Line      VRAM    VDP     Dot     LINE       0   1   1   1
          Search    VRAM    VDP     Dot     SRCH       0   1   1   0
          Pset      VRAM    VDP     Dot     PSET       0   1   0   1
          Point     VDP     VRAM    Dot     POINT      0   1   0   0
          Invalid                                      0   0   1   1
          Invalid                                      0   0   1   0
          Invalid                                      0   0   0   1
          Stop                              STOP       0   0   0   0
          ------------------------------------------------------------ 
          
          Het  commando wordt  uitgevoerd door  de VDP  door de juiste 
          waarde van  CM3-CM0 naar  R#46 te  schrijven. De juiste data 
          moet  echter  eerst  naar R#32-R#45  worden geschreven  (zie 
          verderop).  Bit 0  van S#2  (CE = Command Execute), heeft de 
          waarde 1  als het  commando wordt uitgevoerd, en de waarde 0 
          als  het klaar  is. De werking van deze commando's is alleen 
          gegarandeerd in  de modes  G4-G7 (SCREEN  5 t/m  8, op MSX2+ 
          natuurlijk  ook in  SCREEN 10  t/m 12).  Gebruik STOP om een 
          commando af te breken dat wordt uitgevoerd.
          
          
                      L O G I S C H E   O P E R A T I E S 
          
          Naam:    Werking:                           LO3 LO2 LO1 LO0
          -----------------------------------------------------------
          IMP      DC=SC                               0   0   0   0
          AND      DC=SC*DC                            0   0   0   1
          OR       DC=SC+DC                            0   0   1   0
          EOR      DC=NOT(SC)*DC+SC*NOT(DC)            0   0   1   1
          NOT      DC=NOT(SC)                          0   1   0   0
          TIMP     if SC=0 then DC=DC else IMP         1   0   0   0
          TAND     if SC=0 then DC=DC else AND         1   0   0   1
          TOR      if SC=0 then DC=DC else OR          1   0   1   0
          TEOR     if SC=0 then DC=DC else EOR         1   0   1   1
          TNOT     if SC=0 then DC=DC else NOT         1   1   0   0
          -----------------------------------------------------------
          
          SC = Source Color (bron), DC = Destination Color (doel). EOR 
          is hetzelfde  als XOR  in BASIC. De logische operaties mogen 
          worden  toegepast bij LINE, PSET en LOGICAL MOVE commando's. 
          De  juiste  waarde  van  LO3-LO0 moet  gelijktijdig met  het 
          commando naar R#46 worden geschreven.
          
          
                 H E T   C O O R D I N A T E N   S Y S T E E M 
          
          De VDP werkt met een ander coîrdinatensysteem dan MSX-BASIC. 
          Alle pages  worden als  het ware  aan elkaar geplakt tot ÇÇn 
          groot  scherm, waarbij de Y-coîrdinaten gewoon doorlopen. In 
          onderstaand  overzicht  is  te  zien hoe  dat werkt.  In het 
          midden staan de corresponderende adressen in het VRAM.
          
                G4 (SCREEN 5)        Adres:       G5 (SCREEN 6)
          
          -------------------------- 00000H -------------------------- 
          (000,000)        (255,000)        (000,000)        (511,000) 
                    Page 0                            Page 0
          (000,255)        (255,255)        (000,255)        (511,255) 
          -------------------------- 08000H -------------------------- 
          (000,256)        (255,256)        (000,256)        (511,256) 
                    Page 1                            Page 1
          (000,511)        (255,511)        (000,511)        (511,511) 
          -------------------------- 10000H -------------------------- 
          (000,512)        (255,512)        (000,512)        (511,512) 
                    Page 2                            Page 2
          (000,767)        (255,767)        (000,767)        (511,767) 
          -------------------------- 18000H -------------------------- 
          (000,768)        (255,768)        (000,768)        (511,768) 
                    Page 3                            Page 3
          (000,1023)      (255,1023)        (000,1023)      (511,1023) 
          -------------------------- 1FFFFH -------------------------- 
          
                G7 (SCREEN 8)                     G6 (SCREEN 7)
          
          -------------------------- 00000H -------------------------- 
          (000,000)        (255,000)        (000,000)        (511,000) 
                    Page 0                            Page 0
          (000,255)        (255,255)        (000,255)        (511,255) 
          -------------------------- 10000H -------------------------- 
          (000,256)        (256,256)        (000,256)        (511,256) 
                    Page 1                            Page 1
          (000,511)        (255,511)        (000,511)        (511,511) 
          -------------------------- 1FFFFH -------------------------- 
          
          Op het  scherm zijn 212 (of 192) lijnen van dezelfde PAGE te 
          zien. Welke dat zijn kunt u instellen met R#23.
          
          
                                   P A G E S 
          
          PAGEs werken  ook anders in ML, er is geen SET PAGE commando 
          of  iets dergelijks.  De PAGE  die wordt  getoond kan worden 
          ingesteld met  R#2. U  kunt daarbij  het volgende tabelletje 
          gebruiken:
          
          Page:    0         1         2         3
          R#2:     &H1F      &H3F      &H5F      &H7F
          
          Bij het schrijven van VRAM in ÇÇn van de pages moet u gewoon 
          het  juiste  adres  instellen, zie  hiervoor deel  2 van  de 
          cursus (de bits in R#14 bepalen daarbij de page).
          
          Bij  kopiâren,  lijnen  trekken,  etc.  is  er  voor de  VDP 
          helemaal  geen sprake  van PAGEs. De coîrdinaten lopen zoals 
          in bovenstaand  overzicht te  zien is gewoon door. Om de VDP 
          iets  in een  bepaalde pagina  te laten doen, moet gewoon de 
          juiste waarde bij de y coîrdinaat worden opgeteld.
          
          Voorbeeld: stel  u wilt een lijn trekken op page 3, van (23, 
          68)  naar (203,176).  Dat wordt dan (23,68+768) en (203,176+ 
          768). Het is dus niet nodig R#2 hiervoor te veranderen.
          
          In de  praktijk gaat  het werken met pages heel gemakkelijk, 
          omdat het gewoon de high byte van de y coîrdinaat is. Bij de 
          standaardmethode  om een  commando naar de VDP te sturen die 
          we nog zullen behandelen wijst zich dat vanzelf.
          
          
                      C O M M A N D O   R E G I S T E R S 
          
          Hieronder  volgt een schematisch overzicht van de registers, 
          zoals we gewend zijn op binair niveau.
          
              MSB  7   6   5   4   3   2   1   0  LSB
          ------------------------------------------------------------ 
          R#32     SX7 SX6 SX5 SX4 SX3 SX2 SX1 SX0    Source X Low
          R#33     0   0   0   0   0   0   0   SX8    Source X High
          R#34     SY7 SY6 SY5 SY4 SY3 SY2 SY1 SY0    Source Y Low
          R#35     0   0   0   0   0   0   SY9 SY8    Source Y High
          
          R#36     DX7 DX6 DX5 DX4 DX3 DX2 DX1 DX0    Dest. X Low
          R#37     0   0   0   0   0   0   0   DX8    Dest. X High
          R#38     DY7 DY6 DY5 DY4 DY3 DY2 DY1 DY0    Dest. Y Low
          R#39     0   0   0   0   0   0   DY9 DY8    Dest. Y High
          
          R#40     NX7 NX6 NX5 NX4 NX3 NX2 NX1 NX0    Nr of dots X Low 
          R#41     0   0   0   0   0   0   0   NX8    Nr of dots X Hgh 
          R#42     NY7 NY6 NY5 NY4 NY3 NY2 NY1 NY0    Nr of dots Y Low 
          R#43     0   0   0   0   0   0   NY9 NY8    Nr of dots Y Hgh 
          
          R#44     CH3 CH2 CH1 CH0 CL3 CL2 CL1 CL0    Color register
          R#45     0   MXC MXD MXS DIY DIX EQ  MAJ    Argument reg.
          R#46     CM3 CM2 CM1 CM0 LO3 LO2 LO1 LO0    Command register 
          ------------------------------------------------------------ 
          
          
          Ik zal nu alle commando's ÇÇn voor ÇÇn gaan bespreken. Omdat 
          dat  veel ruimte  in beslag  neemt, zal  het over twee delen 
          worden verdeeld. Opmerking vooraf: bit 5 van R#45 bepaalt of 
          het VRAM (0) of Expansion RAM (1) wordt gebruikt.
          
          
                                    H M M C 
          
                          High Speed Move CPU to VRAM
          
          Dit commando  doet ongeveer  hetzelfde als  COPY <array>  TO 
          (X,Y)  in  Basic.  Het  schrijft  data van  de CPU  naar een 
          rechthoek.  Geef met  DX, DY, NX, NY, DIX en DIY de gewenste 
          rechthoek aan. DIX en DIY werken als volgt:
          
          DIX:   0 = naar rechts vanaf DX, 1 = naar links vanaf DX
          DIY:   0 = naar onder vanaf DY, 1 = naar boven vanaf DY
          
          De data moet steeds naar R#44 worden geschreven. Schrijf bij 
          het geven  van het commando alvast de eerste byte naar R#44. 
          De opbouw van R#44 verschilt per schermmode:
          
          G4, G6 (SC5,7): CR7 CR6 CR5 CR4   CR3 CR2 CR1 CR0
                            linker pixel     rechter pixel
          
          G5     (SC6)  : CR7 CR6   CR5 CR4   CR3 CR2   CR1 CR0
                           links    midlinks  midrechts  rechts
          
          G7     (SC8)  : CR7 CR6 CR5 CR4 CR3 CR2 CR1 CR0
                                  ÇÇn pixel
          
          Dit  omdat de data per byte moet worden verzonden, en in G4- 
          G6 meer dan ÇÇn pixel in een byte wordt opgeslagen. Hierdoor 
          worden er ook bits van NX en DX verwaarloosd. In G5 bit 0 en 
          1, in G4 en G6 alleen bit 0.
          
          Als alles  goed is  ingevuld, schrijf je de waarde 11110000B 
          naar  R#46. Vervolgens moet steeds S#2 worden uitgelezen, en 
          gekeken worden of TR of CE gelijk zijn aan 0.
          
              MSB  7   6   5   4   3   2   1   0
          S#2      TR  VR  HR  BD  1   1   EO  CE     Status reg. 2
          
          De  volgende byte  mag naar  R#44 worden  geschreven als  TR 
          gelijk is  aan 1. Het commando is klaar als CE gelijk is aan 
          0.
          
          In schema:
          1) Schrijf de juiste waardes naar R#36-R#45
          2) Schrijf 11110000B naar R#46
          3) Lees S#2
          4) Als CE=0 dan is het klaar
          5) Als TR=0 dan 3
          6) Schrijf volgende byte naar R#44
          7) Ga naar 3
          
          
                                    Y M M M 
          
                      High Speed Move VRAM to VRAM, y only
          
          Dit  commando verplaatst  de rechthoek  die wordt aangegeven 
          door DX, SY, NY, DIX en DIY en de rand van het scherm aan de 
          rechterkant (of  linkerkant, als DIX=1) naar eenzelfde blok, 
          echter met Y-coîrdinaat DY. Omdat het met bytes gaat, worden 
          in  G4-G6 weer  dezelfde bits  van de X-coîrdinaten verwaar- 
          loosd als bij HMMC.
          
          Instellen van het YMMM commando:
          R#34 en R#35: y-coîrdinaat van bron
          R#36 en R#37: x-coîrdinaat van bron än doel
          R#38 en R#39: y-coîrdinaat van doel
          R#42 en R#43: lengte van rechthoek in Y-richting
          R#45        : bepaal richting en soort VRAM
          
          Schrijf 11100000B  naar R#46  om het commando uit te voeren, 
          het is klaar als CE gelijk is aan 0.
          
          
                                    H M M M 
          
                          High Speed Move VRAM to VRAM
          
          Dit is een van de meest gebruikte commando's. Het verplaatst 
          de  rechthoek die  wordt aangegeven door SX, SY, NX, NY, DIX 
          en DIY  naar het punt DX,DY. Weer worden de laagste bits van 
          de X-coîrdinaten verwaarloosd in G4-G6.
          
          Het commando is te vergelijken met COPY (SX,SY)- STEP(NX,NY) 
          TO  (DX,DY) in  Basic, met  het verschil  dat hierbij  de x- 
          coîrdinaat een  veelvoud van 2 (G4, G6) of 4 (G5) moet zijn. 
          Het is daarom ook sneller.
          
          Instellen van het HMMM commando:
          Schrijf de juiste waardes naar R#32-R#43 en R#45.
          
          Schrijf  11010000B naar  R#46 om het commando uit te voeren, 
          het is klaar als CE gelijk is aan 0.
          
          In  het volgende  deel volgt  de rest van de commando's. Tot 
          dan!
          
                                                          Stefan Boer
          