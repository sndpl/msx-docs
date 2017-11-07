                         S P R I T E S   I N   M L   2 
                                                        
          
          
                  S P R I T E S   I N   D E   P R A K T I J K 
          
          Hier  dan eindelijk het vervolg van de sprite besturingen in 
          ML. Het eerste deel kunt u vinden op Sunrise Special #2.
          Om  sprites  daadwerkelijk op  het beeld  te toveren  zonder 
          daarbij het  MSX-BIOS te  gebruiken is nogal een ingewikkeld 
          verhaal.  In  deel  1  hebt  u  kunnen  lezen over  de start 
          adressen  voor de sprite data. Om enig inzicht te krijgen in 
          de VDP  waarden, zal ik hier een aantal zaken van ophelderen 
          met enkele voorbeelden.
          
          
                            S P R I T E   M O D E S 
          
          Op een MSX2 of hoger zijn er nog meer zaken, wat betreft het 
          zetten  van  een  sprite,  waar  rekening  mee  moet  worden 
          gehouden. Zoals de kleuren en speciale commando bits. Daarom 
          spreken  we van  twee sprite  modes: MODE 1 (MSX1 en hoger), 
          MODE 2  (MSX2 en  hoger). Dus een MSX2 gebruikt zowel MODE 1 
          als MODE 2. MODE 2 bestaat uit alle extra MSX2 functies.
          
          Sprite Mode 1:  Graphic 1       (screen 1)
                          Graphic 2       (screen 2)
                          Multi colour    (screen 3)
                          Max. sprites op 1 horizontale as: 4
          Sprite Mode 2:  Graphic 3       (screen 4)
                          Graphic 4       (screen 5)
                          Graphic 5       (screen 6)
                          Graphic 6       (screen 7)
                          Graphic 7       (screen 8)
                          Max. sprites op 1 horizontale as: 8
          
          
                 I N S T A L L E R E N   V A N   S P R I T E S 
          
          Om  sprites  op  het  scherm  te  toveren,  moeten  eerst de 
          volgende register worden beschreven:
          
          Noot: Registers  worden afgekord met "r# xx", hiermee worden 
          de  registers aangeduid  zoals ze in ML worden gebruikt. Dus 
          denkt er  om MSX2 registers worden in BASIC 1 verhoogd. Dus: 
          VDP(12)=r#11!)
          
                  b7  b6  b5  b4  b3  b2  b1  b0
          r# 1    x   x   x   x   x   x   SZ  EXP(/MAG)   (mode 1)
          r# 8    x   x   x   x   x   x   SPD x           (mode 1)
          
          SZ:     SiZe.           Grootte (0:8x8, 1:16x16)
          EXP:    EXPanded.       Vergroting (0:normaal, 1:vergroot)
          SPD:    SPrite Display. Weergave (0:AAN, 1:UIT)
          
          Voorbeeld:
                  VDP(1)=VDP(1) and &B11111100 or &B00000010
                  VDP(8)=VDP(8) or  &B00000010
          
          De sprite  grootte is  hier: 16x16 normaal (r#1). De sprites 
          worden zichtbaar op het scherm (r#8).
          
          
             N O G M A A L S   D E   S P R I T E   A D R E S S E N 
          
                                (zie ook deel 1)
          
          De volgende VDP registers worden hiervoor gebruikt:
          
                  b7  b6  b5  b4  b3  b2  b1  b0
          r# 5    D14 D13 D12 D11 D10 1   1   1           (mode 1)
          r#11    0   0   0   0   0   0   D16 D15         (mode 2)
          r# 6    0   0   E16 E15 E14 E13 E12 E11         (mode 1)
          
          D10-D16: VRAM-adres voor spritekleur- en attribute tabellen.
          E16-E11: VRAM-adres voor spritepatroon tabel.
          
          N.B.    D10-D14: VRAM lijn adres
                  D15/D16: VRAM page nr.
                  E11-E14: VRAM lijn adres
                  E15/E16: VRAM page nr.
          
          Een beschrijving van de registers:
          
          r#  5  en  r#11 vormen  samen het  adres waar  de kleur-  en 
          positie tabellen van de sprites. Bits 0-2 moeten altijd hoog 
          zijn.  Bits 3-7  vormen het  lijn adres  van de kleur tabel. 
          Adres berekening: start-lijn * 128 or &H07.
          
          De  startlijn  is altijd  in stappen  van 8,  dus kunnen  er 
          feitelijk maar 32 verschillende waarden worden opgegeven.
          
          Voorbeeld:
          
          Stel we  willen de  sprite kleur  data op  Page 1,  lijn 200 
          plaatsten, dan worden r#5 en r#11:
          r# 5, &HCF (200 or 7)
          r#11, &H01
          
          Hierbij is het kleur tabel adres bekend: 200x128 or 7=&H6400
          Het positie tabel adres: kleur tabel adres + &H0200 = &H6600
          
          Standaard   zijn  deze  registers  gevuld  met  de  volgende 
          waardes:
          
          r# 5, &B11101111/&HEF   (VRAM adres: &H7400)
          r#11, &B00000000/&H00   (VRAM page: 0)
          
          
          r#  6  bevat  het  adres  waar  de patroon  tabellen van  de 
          sprites.  Bits  0-3  vormen  het lijn  adres van  de patroon 
          tabel.
          Adres berekening:  start-lijn *  128 *  16. De  startlijn is 
          altijd   in  stappen   van  16,   dus  kunnen   er  maar  16 
          verschillende waarden  worden opgegeven.  Bits 4/5  bevatten 
          het pagina nummer.
          
          Voorbeeld:
          Stel  we willen  de sprite  patroon data  op Page 2, lijn 32 
          plaatsen, dan worden r#6:
          r# 6, &H22 (32/2 or &H20)
          (LSB: adres, MSB: page)
          
          Hierbij    is     de    patroon    tabel    adres    bekend: 
          32*16+&H8000=&H8200.
          De tabel  bevindt zich  boven de 32 kB (page1) dus moeten we 
          er &H8000 (=16 kB) bij optellen.
          (als r#6>31 dan bovenste 64 kB VRAM.)
          
          Standaard is dit register gevuld met de volgende waarde:
          r# 6, &B00001111/&H0F   (VRAM adres: &H7800, page: 0)
          
          Het leuke van het kunnen verandereren van de sprite adressen 
          is dat  je zonder alle sprite data hoeft te veranderen, toch 
          in 1 keer alle sprites kunt veranderen.
          
          
                    P L A A T S E N   V A N   S P R I T E S 
          
          De  sprite attribute  tabel bevat de weergave waarden van de 
          sprites. Elke  sprite wordt weergeven op 1 van de 32 "sprite 
          vlakken".  De sprite  waarden voor elk vlak bestaat uit vier 
          bytes. Het  start adres hiervan wordt gezet door r#5 en r#11 
          te beschrijven (VRAM adres: r#5/r#11 + &h0200).
          
          De  vier bytes bevatten de volgende informatie voor 1 sprite 
          vlak:
          +0  Y-positie   De verticale  positie van  een sprite  wordt 
                          verhoogd  met 1. Dus de werkelijk waarde die 
                          moet  worden  geschreven is  Y-1. Door  deze 
                          waarde op  208 (mode  1) of  216 (mode 2) te 
                          zetten,  wordt de  sprite niet  zichtbaar op 
                          het scherm.
          +1 X-positie    Horizontale  positie.   Kan  "slechts"   256 
                          waarde bevatten, dus in een hogere resolutie 
                          (bijv.  screen 7)  zal de  sprite altijd een 
                          even waarde krijgen.
          +2 Sprite nr.   Bevat het  sprite patroon,  die moet  worden 
                          weer gegeven op dit vlak.
          +3 Kleur        Bevat  de kleur van de hele sprite in sprite 
                          Mode 1.  Als er  in Mode 2 wordt gewerkt, is 
                          deze  waarden onbelangrijk.  Door het EC bit 
                          (bit 7)  hoog te  zetten, wordt de sprite 32 
                          pixels naar links geschoven.
           
          Let op! deze waarden zijn bedoeld voor het sprite vlak.
          Er  zijn 32  vlakken, dus  de totale lengte van de attribute 
          tabel is: 32 * 4 = 128 bytes.
          
          Voorbeeld: (8x8 sprites, niet vergroot, mode 1)
          
          We willen  sprite 7  op vlak  11 plaatsen,  met als  positie 
          (40,50),  en de  kleur is 9. Eerst moeten we weten naar welk 
          attribute-adres we  willen schrijven.  We gebruiken vlak 11, 
          dus: 11 * 4 = 44. We weten het start adres al, door r#5/r#11 
          te lezen. Dit is bijvoorbeeld: &H7600.
          
          Het schrijf adres wordt dan: &H7600 + 44 = &h762C.
          Vervolgens beschrijven we de volgende VRAM adressen:
          
          &H762C+0, 50-1  (Y-1)
                +1, 40    (X)
                +2, 7     (nr)
                +3, 9     (kleur)
           
          Door  het EC  bit te  zetten wordt  de sprite 32 pixels naar 
          links verplaatst.  Het nut hiervan is dat deze sprite nu ook 
          pixel  voor  pixel  aan de  linker kant  van het  scherm kan 
          verdwijnen.  Dat deze  waarde 32  is, komt  omdat een sprite 
          16x16 vergroot kan wezen. Deze wordt dan immers 32x32 groot. 
          Ook deze sprite moet pixel voor pixel kunnen verdwijnen.
           
          Om  een   sprite  daadwerkelijk  op  het  scherm  te  kunnen 
          plaatsen, moeten er met nog een aantal zaken rekening worden 
          gehouden.  Een van  die zaken  is de  grootte van  de sprite 
          (16x16).
          
          Als  er  voor  8x8 sprites  wordt gekozen,  levert dit  geen 
          problemen  op. Echter  het gebruik van 16x16 sprites is iets 
          ingewikkelder. Om  een 16x16  sprite te  plaatsen, worden er 
          eigenlijk 4 (8x8) sprites geplaatst. Het totaal patroon ziet 
          er als volgt uit:
          
                  X
                Y +------++------+
                  | spr  || spr  |
                  | +0   || +2   |
                  +------++------+
                  | spr  || spr  |
                  | +1   || +3   |
                  +------++------+
          
          Dus  het totaal  oppervlak wordt aangevuld met de 3 volgende 
          sprites.
          Voorbeeld: (16x16 sprites, niet vergroot)
                  sprite patroon 3 moet worden geplaats op (10,20).
                  De eerste (8x8) sprite wordt dan: 3 * 4 = 12.
                  de opbouw wordt dan als volgt:
                X=10  
            Y=20 +--------------++--------------+
                 |spr.12 (10,20)||spr.14 (16,20)|
                 +--------------++--------------+
                 |spr.13 (10,36)||spr.15 (16,36)|
                 +--------------++--------------+
          
          Om  een 16x16  sprite te maken kun je dus net zo goed zelf 4 
          (8x8)  sprites  plaatsen. Echter,  16x16 sprites  hebben een 
          voordeel dat  je maar  een keer  een X  en een Y hoeft op te 
          geven, want de VDP regelt zelft de andere 3 (8x8) sprites.
          
          Om nu een (16x16) sprite te zetten, moet men er dus rekening 
          met  het  volgende  houden:  Het  sprite  nummer  dat  wordt 
          opgegeven moet uit stappen van 4 bestaan. Dus 0, 4, 8 etc.
          
          Wordt er bijvoorbeeld toch sprite 3 opgegeven, dan wordt dit 
          sprite  patroon  0,  en  worden  8x8-sprites  0, 1,  2 en  3 
          geplaatst.
          
          Bij vullen van de attribute tabel moet hier ook rekening mee 
          worden  gehouden.  Immers  de  3  opvolgende  sprite vlakken 
          worden ook  gebruikt. Er  zijn nus  feitelijk 32/4=8  sprite 
          vlakken,  waardoor er maar 8 sprites kunnen worden vertoond. 
          Dus om  een sprite te zetten, wordt het adres van deze tabel 
          als volgt berekend:
          
          VRAM adres van r#5/r#11 + vlaknr * 4 * 4.
          
          De  laatste vermenigvuldiging  met 4  zorgt er voor dat de 3 
          volgende  vlakken  worden overgeslagen,  deze worden  immers 
          automatisch door de VDP gevuld. Het vergroten van een sprite 
          (EXP)  heeft   geen  gevolgen  voor  het  berekenen  van  de 
          attribute tabellen.
          
          Voorbeeld: (16x16, vergroot (=32x32), mode 1)
          
          We  willen  een (16x16)  sprite op  vlak 3  plaatsen. X=132, 
          Y=40, sprite  nr=0, kleur=7.  De sprite  moet 32 pixels naar 
          links worden verschoven. r#5/r#11= &H7600
          
          Attribute adres: &h7600 + 3 * 4 * 4 = &H7630.
          
          &h7630+0, 40-1  (Y-1)
                +1, 132   (X)
                +2, 12    (nr: 3*4)
                +3, &H87  (kleur + EC=1)
           
          Wat  betreft  het  plaatsen  van  een  sprite,  moet  er  nu 
          voldoende voer  zijn geleverd. Om het hele sprite verhaal af 
          te  maken, wil  ik volgende  keer de  aandacht richten op de 
          sprite botsingen  (mode 1 �n 2), en de kleurcodes bij sprite 
          mode 2. Tot een volgende keer!
          
                                                 Roman van der Meulen
