                          C M D   M O D E   V 9 9 5 8 
                                                       
          
          De videochip van de MSX2+ en de MSX turbo R (de V9958) heeft 
          er  ten  opzichte  van de  V9938 drie  nieuwe registers  bij 
          gekregen. Dit zijn:
          
                    7     6     5     4     3     2     1     0
          R#25      -    CMD   VDS   YAE   YJK   WTE   MSK   SP2
          R#26      -     -    HO8   HO7   HO6   HO5   HO4   HO3
          R#27      -     -     -     -     -    HO2   HO1   HO0
          
          De betekenis van de bits:
          
          HO0-HO8:        horizontaal scrollen, bij HO0-HO2 is rechts
                          de positieve richting en bij HO3-HO8 is
                          links de positieve richting
          SP2:            scroll horizontaal met 2 pagina's als dit
                          bit gezet is, met 1 pagina als het 0 is
          MSK:            de scroll hapt normaal aan de linkerkant,
                          met dit bit gezet worden 8 pixels aan de
                          linkerkant afgedekt
          WTE:            als dit bit gezet is, wordt de toegang tot
                          alle VDP poorten in de WAIT stand gezet
                          als de CPU met het VRAM bezig is. Dit
                          gebeurt niet in andere gevallen waarbij dit
                          wel handig zou zijn zoals terwijl de VDP
                          met een commando bezig is, terwijl de eerste
                          byte van een registeraccess al ontvangen is
                          of als de eerste byte van een paletaccess
                          al ontvangen is
          YJK:            YJK mode aan/uit (SCREEN 12)
          YAE:            YAE mode aan/uit (SCREEN 10/11)
          VDS:            met dit bit kun je het CPUCLK signaal
                          vervangen door het VDS signaal, wat het nut
                          daarvan is, is onbekend
          CMD:            als dit bit gezet is kunnen commando's ook
                          in SCREEN 0-4 worden gebruikt, waarbij de
                          parameters zo moeten worden ingesteld alsof
                          SCREEN 8 actief is
          
          
                                C M D   M O D E 
          
          Over  het  laatstgenoemde  bit  (bit  6 van  R#25) gaat  dit 
          artikel.  Iedereen die  het V9958 Technical Data Book of een 
          tekst over de V9958 registers heeft gelezen weet dat dit bit 
          bestaat,  maar  ik  heb  nog  nooit  gehoord dat  iemand het 
          gebruikt! En toch kun je er hele leuke dingen mee doen!
          
          Bij de  V9938 en  als het  CMD bit  0 is bij de V9958 kun je 
          alleen commando's gebruiken in SCREEN 5 en hoger. Als je het 
          CMD  bit echter  hoog maakt, kun je ook commando's gebruiken 
          in SCREEN 0-4!
          
          Nu  moet je niet denken dat je lijnen kunt trekken in SCREEN 
          0 of  dat je kunt kopi�ren zoals je zou verwachten in SCREEN 
          2,  want dat  is natuurlijk  onzin. Je  kunt niet zoiets als 
          COPY(10,15)-(20,35)TO(100,90) doen  in SCREEN  2, omdat  het 
          geen bitmap mode is.
          
          De truuk  is dat  de VDP bij het uitvoeren van de commando's 
          net  doet  alsof  SCREEN  8  actief  is!  Je moet  dus alles 
          omrekenen naar de VRAM indeling van SCREEN 8.
          
          Een paar voorbeeldjes zullen dit duidelijk maken. We gaan er 
          steeds van  uit dat  het CMD  bit is gezet. Stel je wilt een 
          "A"  zetten op  positie (0,0)  in SCREEN 0:WIDTH 80. Dan kan 
          dat met VPOKE 0,65 of met LOCATE 0,0:PRINT "A". Maar het kan 
          ook met  PSET(0,0),65! Helaas  niet in  BASIC want die geeft 
          nog  steeds een  Illegal function  call, maar wel als je het 
          rechtstreeks met de commando registers doet!
          
          Nu  willen we een B zetten op positie 8,17. Dat kan dus NIET 
          met PSET  (8,17),66!!! Je  moet namelijk  denken aan de VRAM 
          indeling  van SCREEN  8. Het  adres van  8,17 is 8 + 17*80 = 
          1368. Nu  moet je  dit omrekenen  naar het pixel in SCREEN 8 
          dat  op dat  adres staat.  X = 1368 MOD 256 = 88, Y = 1368 \ 
          256 = 5. Dus met PSET(88,5),66 zet je een B op positie 8,17.
          
          
                       W A T   H E B   J E   E R A A N ? 
          
          Wat heb je hier aan zul je denken? Wel, aan PSET heb je niet 
          veel en  aan LINE  ook niet.  Maar met  HMMM (COPY)  en HMMV 
          (LINE,BF)  kun  je  bepaalde  dingen  een  stuk  sneller  en 
          handiger doen!
          
          Bijvoorbeeld  het  scherm  bewaren  om  het  later  weer  te 
          herstellen. Met  een HMMM  kun je  het scherm naar een ander 
          stuk  VRAM kopi�ren. Het neemt 24*80 = 1920 bytes in beslag, 
          dat komt  overeen met  7.5 lijnen in SCREEN 8, dat moeten we 
          naar boven afronden naar 8. Dus doen we COPY(0,0)-(255,7) TO 
          (0,128).  Nogmaals, dit kan niet met het BASIC commando COPY 
          worden  gedaan,  ik  gebruik  alleen  deze  notatie voor  de 
          duidelijkheid. Je moet dit zelf omzetten naar de juiste data 
          om naar register R#32 t/m R#46 te sturen:
          
                  DB      0,0,0,0         ; source
                  DB      0,0,128,0       ; destination
                  DB      0,1,8,0         ; size
                  DB      0,0,#D0         ; HMMM
          
          Er  wordt zo nog iets meer gekopieerd (2048 bytes om precies 
          te  zijn),   maar  dat   maakt  niet  uit.  Met  precies  de 
          omgedraaide  copy  kun  je  het  scherm  nu  weer zeer  snel 
          herstellen.  Deze methode  kost geen RAM om het scherm op te 
          slaan (wel  VRAM natuurlijk  maar in  SCREEN 0-4  gebruik je 
          toch  alleen maar  de eerste  16 kB) en wat belangrijker is: 
          het kost  weinig processortijd!  Je hoeft alleen maar die 15 
          bytes  naar de commandoregisters te sturen en de VDP doet de 
          rest van het werk. Als je het 'normaal' programmeert kan dat 
          nooit zo snel!
          
          Met HMMV kun je het scherm snel vullen met een bepaald byte. 
          Je  kunt  het  scherm bijvoorbeeld  in een  oogwenk met  A's 
          vullen,  veel sneller  dan je  dat ooit op een andere manier 
          (gewoon met VPOKEn) zou kunnen doen. De commandodata ziet er 
          dan zo uit:
          
                  DB      0,0,0,0         ; dummy
                  DB      0,0,0,0         ; destination
                  DB      0,1,8,0         ; size
                  DB      #41,0,#C0       ; HMMV met #41 (='A')
          
          Hetzelfde resultaat kan ook bereikt worden door de VDP klaar 
          te  zetten  voor  VRAM  schrijven  vervolgens 1920  maal OUT 
          (#98),#41  doen, maar  dat kost veel meer moeite voor de CPU 
          en duurt bovendien langer.
          
          Enfin, je kunt zelf wel bedenken wat er nog meer mogelijk is 
          met de CMD mode, bijvoorbeeld in SCREEN 4. Je kunt misschien 
          ook  leuke dingen doen met logische operaties, denk erom dat 
          je dan wel LMMM etc. moet gebruiken. Ik heb al zitten denken 
          voor  toepassingen  bij de  blinkmode van  SCREEN0:WIDTH 80, 
          maar ik  geloof niet  dat dat  handiger gaat dan de routines 
          die al eerder op de Special hebben gestaan.
          
          
                                  N A D E E L 
          
          Een  nadeel van  de CMD  mode is  dat hij alleen op de V9958 
          zit, en programma's die in SCREEN 0-4 worden gemaakt meestal 
          ook voor  MSX2 zijn.  Maar er zijn natuurlijk uitzonderingen 
          zoals  een MODplayer, en daarbij zou je dit zeer goed kunnen 
          gebruiken.
          
                                                          Stefan Boer
