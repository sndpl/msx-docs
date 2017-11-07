                            V D P   C U R S U S   9 
                                                     
          
          Zoals  beloofd deze keer de opslag van SCREEN 5 t/m 8 in het 
          VRAM, of  liever gezegd  de Graphic  modes 4 t/m 7. Dit zijn 
          allen  zgn. bitmap  schermen, en  de opslag in het VRAM gaat 
          dan ook bij allemaal volgens hetzelfde principe.
          
          
                                  B I T M A P 
          
          Bij  het  bitmap  systeem  wordt  de kleur  van ieder  pixel 
          bepaald  door  een  bepaald  aantal  bits. Deze  bits worden 
          alleen  voor dit pixel gebruikt, waardoor geen "color spill" 
          effecten  optreden,  zoals bij  SCREEN 2  en 4  of de  MSX2+ 
          schermen 10 t/m 12.
          
          Het  aantal  kleuren dat  tegelijkertijd op  het scherm  kan 
          worden  getoond  is  hierbij  uiteraard afhankelijk  van het 
          aantal bits dat per pixel beschikbaar is. Een overzichtje:
          
          SCREEN 5        G4      4 bits/pixel     16 kleuren
          SCREEN 6        G5      2 bits/pixel      4 kleuren
          SCREEN 7        G6      4 bits/pixel     16 kleuren
          SCREEN 8        G7      8 bits/pixel    256 kleuren
          
          Deze bits  worden zoveel mogelijk in een byte gepropt. Omdat 
          een  byte  zoals  u  ongetwijfeld  weet uit  8 bits  bestaat 
          betekent  dat in  G4 en G6 de kleuren van twee pixels in een 
          byte  worden  opgeslagen, bij  G5 zijn  dat er  4 en  bij G7 
          slechts 1.  Hierbij is  het zo  dat de  kleur van  het meest 
          linkse   pixel  ook  altijd  in  de  meest  linkse  bits  is 
          opgeslagen. Samengevat levert dit onderstaand schema op:
          
                          MSB   7   6   5   4   3   2   1   0   LSB
          G4 en G6              ------1------   ------2------
          G5                    --1--   --2--   --3--   --4--
          G7                    --------------1--------------
          
          
                                V O L G O R D E 
          
          We weten nu dus hoe de kleurinformatie over de bytes van het 
          VRAM verdeeld  is, maar  we weten nog niet in welke volgorde 
          die  bytes staan,  of waar we dus de kleurinformatie van een 
          bepaald pixel kunnen vinden.
          
          Deze informatie over de pixels wordt per lijn opgeslagen. De 
          eerste  lijn  van het  scherm bestaat  uit de  pixels (0,0), 
          (1,0), (2,0),  .... tot (255,0) op G4 en G7 resp. (511,0) op 
          G5  en G6.  De kleuren  staan van  links naar  rechts in het 
          VRAM. Als  het beeldscherm  uit slechts ��n lijn zou bestaan 
          zou dus de volgende formule gelden:
          
          <adres> = <x-co�rdinaat> \ <aantal pixels per byte>
          
          (Let op:  er staat  een "\"  (integerdeling) en niet een "/" 
          (normale  deling)!) De  kleuren van de pixels (0,0) en (1,0) 
          staan op SCREEN 5 en 7 dus samen in byte 0, die er als volgt 
          uit ziet:
          
          MSB   7   6   5   4   3   2   1   0   LSB
                ----(0,0)----   ----(1,0)----
          
          Door een  VPOKE 0,&HF1  wordt pixel  (0,0) dus  wit en pixel 
          (1,0) zwart. De kleur van het pixel (255,0) staat bij SCREEN 
          8 in byte nummer 255, en neemt het hele byte in beslag.
          
          Natuurlijk  bestaat het  scherm niet  uit slechts  ��n lijn, 
          maar uit 212 lijnen (genummerd van 0 t/m 211). De informatie 
          van lijn 1 staat direct achter die van lijn 0, en daarachter 
          weer lijn 2, enz. We kunnen dus zeggen dat de informatie van 
          linksboven naar  rechtsonder in  het VRAM  is opgeslagen. We 
          breiden de formule dus nog wat uit:
          
          <adres> = <x-co�rdinaat> \ <aantal pixels per byte> + 
                    <y-co�rdinaat> * <aantal bytes per lijn>
          
          Hierbij is de volgende tabel misschien handig:
          
                  Aantal pixels per byte: Aantal bytes per lijn:
          ------------------------------------------------------
          G4      2                       128
          G5      4                       128
          G6      2                       256
          G7      1                       256
          ------------------------------------------------------
          
          
                                  P A L L E T 
          
          De modes  4 t/m 6 werken met een palet. Bij G4 en G6 bestaat 
          dit  palet uit 16 kleuren, bij G5 uit 4. Het kleurnummer dat 
          in  de  databytes  wordt  opgeslagen correspondeert  met het 
          paletnummer.
          
          Bij G7  (SCREEN 8)  gaat het heel anders. Het kleurnummer is 
          hier als volgt opgebouwd:
          
          MSB   7   6   5   4   3   2   1   0   LSB
                ----G----   ----R----   --B--
          
          Hierbij staat  G voor de groene intensiteit (0-7), R voor de 
          rode  intensiteit  (0-7)  en  B  voor de  blauwe intensiteit 
          (0-7).  Er is hier dus geen sprake van een palet, de kleuren 
          staan vast.
          
          Het palet  wordt bij  SCREEN 5  t/m 7 door BASIC in het VRAM 
          gezet.  Bij SCREEN  5 en  6 is  dit &H7680,  en bij SCREEN 7 
          &HFA80. Het veranderen van deze waardes door VPOKE heeft pas 
          effect na  een COLOR=RESTORE. In machinetaal wordt het palet 
          normaal  gesproken  gewoon  veranderd door  naar Port  #2 te 
          schrijven  (OUT poort &H9A). Hiervoor verwijs ik u naar deel 
          1 van de VDP cursus.
          
          
                         G E H E U G E N G E B R U I K 
          
          Zoals u  weet hebben  G4 en  G5 4 pages, en G6 en G7 2. Deze 
          pages staan als volgt in het VRAM:
          
          00000H          page 0 G4/G5    page 0 G6/G7
                          ------------
          08000H          page 1 G4/G5
                          ------------    ------------
          10000H          page 2 G4/G5    page 1 G6/G7
                          ------------
          18000H          page 3 G4/G5    ------------
          
          Voor  de VDP  is een  scherm 256 lijnen hoog, en er per page 
          dan  ook  32  kB  (G4/G5) respectievelijk  64 kB  (G6/G7) in 
          beslag worden genomen. Van deze 256 lijnen worden er slechts 
          212 (of 192) op het scherm getoond. Welke, dat wordt bepaald 
          door de waarde in R#23 (beter bekend als VDP(24)).
          
          Omdat  de lijnen  vanaf 212  normaal gesproken  niet te zien 
          zijn, worden  deze gebruikt om de spritedata in op te slaan. 
          Hiervoor  verwijs ik  u naar  het deel van de VDP cursus dat 
          aan sprites was gewijd. SCREEN 5 t/m 8 hebben Sprite Mode 2.
          
          Bij het  wegsaven van  schermen met BSAVE kunt u de volgende 
          eindadressen gebruiken:
          
          SCREEN:         met palet:      zonder palet:
          ---------------------------------------------
          5               &H769F          27135
          6               &H7687          27135
          7               &HFA9F          54271
          8               nvt             54271
          ---------------------------------------------
          
          
                       S C R E E N   S E L E C T E R E N 
          
          Tot  slot  nog  even  de  juiste  bits  om  de  schermen  te 
          selecteren. U  vindt deze bits in R#0 en R#1, waarvan ik dus 
          eerst maar even de indeling geef:
          
                  MSB   7   6   5   4   3   2   1   0   LSB
          R#0           0   DG IE2 IE1  M5  M4  M3  0      Mode Reg #0
          R#1           0   BL IE0  M1  M2  0   SI MAG     Mode Reg #1
          
          Het  gaat  om  de  bits  M1 t/m  M5. Bij  de in  dit artikel 
          besproken  grafische schermen  horen de volgende waardes van 
          M1 t/m M5:
          
                  M1      M2      M3      M4      M5
          ------------------------------------------
          G4      0       0       1       1       0
          G5      0       0       0       0       1
          G6      0       0       1       0       1
          G7      0       0       1       1       1
          -----------------------------------------
          
          
                                T O T   S L O T 
          
          Hiermee is een eind gekomen aan het zeer lange overzicht van 
          alle  theorie betreffende  de Video  Display Processor in uw 
          MSX computer.  Dit betekent  niet dat de cursus nu stopt, in 
          tegendeel.  Vanaf het  volgende deel  zullen we toepassingen 
          gaan  behandelen.   Het  geleerde   zal  dan   eindelijk  in 
          machinetaal worden toegepast.
          
          Tot de volgende keer!
          
                                                          Stefan Boer
