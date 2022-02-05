                  M S X 2 / 2 +   V D P   C U R S U S   ( 6  )
                                                               
          
          Zoals  beloofd  ga  ik  het deze  keer hebben  over sprites. 
          Voordat ik  begin met  het behandelen  van de echte sprites, 
          wil ik eerst nog even een misverstand uit de wereld helpen.
          
          
                         S O F T W A R E S P R I T E S 
          
          Misschien heeft u deze term wel eens gehoord of er iets over 
          gelezen. De doorsnee MSX'er weet wel dat er sprites bestaan, 
          maar  dat  er  verschil zou  zijn tussen  softwaresprites en 
          hardwaresprites  gaat de meesten boven de pet. En dat is ook 
          niet zo  vreemd, want  er bestaat  maar ��n soort sprites op 
          MSX,  de hardwaresprites.  Voor alle duidelijkheid: dit zijn 
          de  sprites  die je  in BASIC  aanstuurt met  PUT SPRITE  en 
          SPRITE$.
          
          Wat  zijn  die  softwaresprites  dan  wel?  Dat  zijn gewoon 
          plaatjes  die gekopieerd  worden. Die  plaatjes staan dan op 
          een onzichtbare  page en  worden met  een razendsnelle  COPY 
          opdracht  naar de  zichtbare pagina  gekopieerd. (Hoewel  ik 
          hier spreek  van 'razendsnel',  betekent dat niet dat er een 
          HMMM  (High-speed Move vraM to vraM) wordt gebruikt, dan kan 
          er geen  TPSET worden  gebruikt en  dat is  noodzakelijk bij 
          softwaresprites.  Er wordt  LMMM (Logical Move vraM to vraM) 
          gebruikt.)
          
          Softwaresprites  hebben  het voordeel  dat het  kleurgebruik 
          (zeker op SCREEN 8, op SCREEN 10 t/m 12 zijn softwaresprites 
          bijna onmogelijk)  onbeperkt is,  en dat  ze in theorie elke 
          grootte kunnen hebben. In praktijk kunnen ze natuurlijk niet 
          te  groot zijn,  want dan duurt het kopi�ren zo lang dat het 
          niet meer  mooi is.  Uiteraard kunnen  op de snelheidsduivel 
          turbo  R grotere  softwaresprites worden gebruikt, kijk maar 
          naar Seed of Dragon. 
          
          De  nadelen   zijn  ook   niet  mis:   het  is   trager  dan 
          softwaresprites,  moeilijker aan  te sturen  (je moet  alles 
          zelf doen) en het neemt meer VRAM in beslag. Bovendien is er 
          nog een  probleem: als de softwaresprite zich verplaats moet 
          het  VRAM dat  eronder zat  weer worden hersteld, dat is bij 
          gewone sprites niet nodig.
          
          Een ander  voordeel van  softwaresprites is  nog dat er geen 
          maximum  aantal  sprites  is  en  dat ze  nooit kunnen  gaan 
          knipperen,  maar  omdat  deze  problemen  alleen  bij  grote 
          aantallen  sprites voorkomen  en de softwaresprites dan veel 
          te langzaam worden is dat niet zo'n groot voordeel.
          
          Hardwaresprites  hebben tenslotte nog het voordeel dat je ze 
          heel makkelijk  stil kunt laten staan terwijl je de rest van 
          het  scherm  laat  scrollen (of  dat nu  hardwarematig (R#23 
          verticaal  / R#26  + R#27 horizontaal) of softwarematig (met 
          COPY) gaat).  Softwaresprites zou  je dan  elke keer  moeten 
          verplaatsen.
          
          De   softwaresprites   zullen   behandeld   worden   bij  de 
          toepassingen,  die  later  in de  cursus veelvuldig  aan bod 
          zullen komen. Ik ben behoorlijk afgedwaald, laten we nu maar 
          beginnen met het onderwerp van deze keer: (hardware)sprites.
          
          
                           S P R I T E   M O D E   2 
          
          Ik behandel hier alleen de zgn. SPRITE MODE 2, die in SCREEN 
          4 en  hoger wordt gebruikt. Dit is tenslotte een MSX2/2+ VDP 
          cursus, dus de MSX1 sprites slaan we over.
          
          Er zijn  32 sprites,  genummerd van  #0 tot  en met #31. Een 
          sprite  met een lager nummer heeft een hogere prioriteit. Er 
          kunnen maximaal  8 sprites  op een  horizontale lijn  worden 
          getoond. Zijn er meer dan 8 sprites op een horizontale lijn, 
          dan   worden  de  lijnen  van  de  sprites  met  de  laagste 
          prioriteit niet  getoond. Het  kan dus bijvoorbeeld gebeuren 
          dat een sprite voor de helft wegvalt.
          
          Als  de  ingekleurde  delen  van  twee sprites  elkaar raken 
          (botsen), zal bit 5 van S#0 worden geset. De co�rdinaten van 
          de  botsing  zullen  in S#3  t/m S#6  worden gezet  (zie VDP 
          cursus deel 5).
          
          Als  er negen of meer sprites op een horizontale lijn staan, 
          dan zal  bit 6  van S#0 worden geset. Bit 0-4 van S#0 zullen 
          dan het nummer bevatten van de negende sprite.
          
          Elke  horizontale lijn  van een  sprite mag  een eigen kleur 
          hebben uit het palet van 16 kleuren.
          
          De sprite  prioriteiten kunnen  worden uitgezet  door het CC 
          bit  van een  lijn te setten (zie verderop), de overlappende 
          gedeeltes van twee sprites worden dan geORd.
          
          
                        S P R I T E   R E G I S T E R S 
          
          De grootte van de sprite wordt bepaald door bit 1 van R#1.
          
              MSB   7   6   5   4   3   2   1   0   LSB
          R#1       0   BL IE0  M1  M2  0   SI MAG      Mode reg. #1
          
          SI = 1 : 16 x 16 pixels
          SI = 0 :  8 x  8 pixels
          
          De  sprite  kan op  normaal formaat  of vier  maal zo  groot 
          worden weergegeven. Dit wordt bepaald door bit 0 van R#1.
          
          MAG = 1 : vergroot
          MAG = 0 : normaal
          
          Met bit 1 van R#8 kan worden bepaald of de sprites aanstaan. 
          Als er geen sprites worden gebruikt, zet ze dan uit want dan 
          werkt  de VDP  sneller (meet  het maar  eens na, het scheelt 
          behoorlijk!).
          
              MSB   7   6   5   4   3   2   1   0   LSB
          R#8       MS  LP  TP  CB  VR  0  SPD  BW      Mode reg. #2
          
          SPD = 1 : disable sprites (zet sprites uit)
          SPD = 0 : enable  sprites (zet sprites aan)
          
          De vorm  van de  sprite staat in de Sprite Pattern Generator 
          Table.  Er kunnen  256 patronen  worden opgeslagen van elk 8 
          bytes lang  (#0 t/m #255). Het beginadres van een patroon is 
          als volgt te berekenen:
          
          <begin tabel> + <patroon nummer> * 8
          
          De  kleuren van  een sprite  staan in de Sprite Color Table. 
          Behalve de kleuren worden er ook drie attribuutbits per lijn 
          opgeslagen. 8x8  sprites vergen  8 bytes,  16x16 sprites  16 
          bytes. Het beginadres van de kleurinfo van een sprite is als 
          volgt te berekenen: (er is 1 positie per sprite, #0 t/m #31)
          
          <begin tabel> + <sprite nummer> * 16
          
          De  overige attributen  (coordinaten en patroonnummer) staan 
          in de  Sprite Attribute  Table, 4  bytes per  sprite (#0 t/m 
          #31). Het beginadres van de attributen van een sprite is als 
          volgt te berekenen:
          
          <begin tabel> + <sprite nummer> * 4
          
          N.B. al deze tabellen staan in het VRAM.
          
          
                  S P R I T E   A T T R I B U T E   T A B L E 
          
          Op <begin adres> + 0 staat de Y-co�rdinaat, 0 t/m 255. Is de 
          Y coordinaat gelijk aan 216, dan worden alle sprites met een 
          lagere  prioriteit niet  getoond, inclusief  de sprite zelf. 
          Let op:  de bovenste  lijn van de sprite komt ��n lijn lager 
          dan de hier opgegeven y-co�rdinaat.
          
          Op <begin  adres> +  1 staat de X-co�rdinaat, van 0 t/m 255. 
          Let  op: ook  in SCREEN  6 en 7 wordt de horizontale as voor 
          sprites in 256 stapjes verdeeld, en niet in 512!
          
          Op  <begin adres> + 2 staat het patroonnummer. Hiermee wordt 
          aangegeven  welk  patroon  uit  de Sprite  Pattern Generator 
          Table moet  worden genomen.  Let op:  als de  sprite grootte 
          16x16  is,  dan  worden er  vier patronen  genomen voor  ��n 
          sprite.  Het maakt dan niet uit welke van de vier nummers je 
          opgeeft. Er  zijn dus  in feite  maar 64  patronen bij 16x16 
          sprites.
          
              MSB   7   6   5   4   3   2   1   0   LSB
          R#5      A14 A13 A12 A11 A10  A9  1   1    Sprite attr. tab.
          R#11      0   0   0   0   0   0  A16 A15   base adress reg.
          
          Het beginadres van de attribuuttabel is hier opgeslagen. Let 
          op:  bit A9  is altijd 1. Door deze registers te manipuleren 
          is het  mogelijk om de informatie van meer dan 32 sprites in 
          het  VRAM op  te slaan  of om  bijvoorbeeld PAGE  0 te tonen 
          terwijl de attribuuttabel ergens in PAGE 2 staat.
          
          
                 P A T T E R N   G E N E R A T O R   T A B L E 
          
          In  deze  tabel staan  de bit-matrices  die de  vorm van  de 
          sprites bepalen.  Het werkt heel simpel; 0 staat voor uit en 
          1  voor aan.  Een spookje  uit Pacman zou er bijvoorbeeld zo 
          uit kunnen zien:
          
                                   &B00111100
                                   &B01111110
                                   &B11011011
                                   &B11011011
                                   &B11111111
                                   &B11111111
                                   &B11011011
                                   &B11011011
          
          Voor elk patroon worden 8 bytes gebruikt. Er zijn totaal 256 
          patronen, genummerd  van #0 t/m #255. Bij 8x8 sprites krijgt 
          elke sprite ��n patroon, een 16x16 sprite krijgt er vier.
          
              MSB   7   6   5   4   3   2   1   0   LSB
          R#6       0   0  A16 A15 A14 A13 A12 A11   Sprite patt. tab.
          
          Hier  is het  beginadres van  de tabel  opgeslagen. Door het 
          manipuleren met  dit register  kunnen bijvoorbeeld veel meer 
          dan 256 patronen gelijktijdig in het VRAM worden gezet, etc.
          
          
                      S P R I T E   C O L O R   T A B L E 
          
          Elke  horizontale lijn kan zijn eigen kleur krijgen (kleur 0 
          is altijd  transparant). Bovendien kunnen per lijn de sprite 
          prioriteit,  de botsingsdetectie  en de 'early clock' worden 
          aan- of uitgezet.
          
          Het beginadres  van deze tabel ligt altijd precies 512 bytes 
          onder  het  beginadres  van de  Sprite Attribute  Table. Per 
          Sprite   (32   stuks,   #0  t/m  #31)  worden  er  16  bytes 
          gebruikt. Dat maakt totaal 512 bytes. Elke byte is als volgt 
          opgebouwd:
          
              MSB   7   6   5   4   3   2   1   0   LSB
                    EC  CC  IC  0  ----kleur------ 
          
          EC:     Early Clock, 1: verschuif lijn 32 bits naar links
          CC:     Priority Enable, 1: OR kleuren als sprites
                                      overlappen
          IC:     Collision Detect, 1: negeer botsingen
          
          Bij CC=1 worden ook de botsingen niet gedetecteerd.
          
          
                       S P R I T E   P R I O R I T E I T 
          
          De sprite prioriteit wordt uitgezet als het CC-bit gelijk is 
          aan  1. De  lijnen van een sprite waarvan CC gelijk is aan 1 
          worden alleen  getoond als  er op diezelfde horizontale lijn 
          een sprite met een lager spritenummer voorkomt.
          
          Dit  kan ook  worden gebruikt om extra kleuren te cre�ren in 
          een samengestelde  sprite (een  'sprite' die  uit een aantal 
          afzonderlijke sprites is opgebouwd). Bijvoorbeeld:
           
          888888--     ----4444     8888CC44
          888888--     ----4444     8888CC44
          888888--  +  --------  =  888888--
          888888--     11------     998888--
          888888--     11------     998888--
          --222222     11------     11222222
          -2222222     11------     13222222
          22222222     11------     33222222
          
          Op deze manier is het dus vrij eenvoudig om op een lijn drie 
          verschillende  kleuren  te  krijgen,  terwijl  je maar  twee 
          sprites gebruikt.
          
          
                          S P R I T E   B O T S I N G 
          
          Dit  hebben we ook al bij de statusregisters behandeld, maar 
          voor de  overzichtelijkheid behandelen  we het  nog maar een 
          keer.
          
          Als  CC gelijk is aan 0 en gedeeltes van een sprite die geen 
          kleur 0  hebben overlappen  elkaar, dan  wordt er een sprite 
          botsing gedetecteerd. Bit 5 van S#0 wordt dan geset. Dit bit 
          wordt weer gereset als S#0 wordt gelezen.
          
              MSB   7   6   5   4   3   2   1   0   LSB
          S#0       F   9S  C  ----9th sprite #---   Status reg. #0
          
          C  = collision,  9S =  negen sprites/regel,  9th sprite  # = 
          nummer van negende sprite.
          
          Als  MS en  LP niet  geset zijn  (R#8, muis en lichtpen, bij 
          V9958  niet  aanwezig),  dan  worden de  co�rdinaten van  de 
          botsing in de statusregisters S#3 t/m S#6 gezet:
          
              MSB   7   6   5   4   3   2   1   0   LSB
          S#3       X7  X6  X5  X4  X3  X2  X1  X0   Status reg. #3
          S#4       1   1   1   1   1   1   1   X8   Status reg. #4
          S#5       Y7  Y6  Y5  Y4  Y3  Y2  Y1  Y0   Status reg. #5
          S#6       1   1   1   1   1   1   1   Y8   Status reg. #6
          
          Van de X co�rdinaat moet nog 12 worden afgetrokken en van de 
          Y co�rdinaat 8.
          
          
                          S P R I T E   K L E U R E N 
          
          In  SCREEN 5, 7, 10, 11 en 12 zijn de kleuren van de sprites 
          gelijk aan  de kleuren  uit het  palet. In  SCREEN 8  worden 
          andere kleuren gebruikt:
          
          Kleurcode:      Groen:  Rood:   Blauw:
          ----------------------------------------------------
          0               0       0       0       transparant
          1               0       0       2       d. blauw
          2               0       3       0       d. rood
          3               0       3       2       d. magenta
          4               3       0       0       groen
          5               3       0       2       blauw/grijs
          6               3       3       0       oker
          7               3       3       2       grijs
          8               8       7       2       zalmrose
          9               0       0       7       blauw
          10              0       7       0       rood
          11              0       7       7       magenta
          12              7       0       0       l. groen
          13              7       0       7       cyaan
          14              7       7       0       geel
          15              7       7       7       wit
          ----------------------------------------------------
          
          SCREEN  6 is  ook een speciaal geval, omdat er dan maar vier 
          kleuren zijn.  Die kleuren worden ook in de sprites gebruikt 
          en het palet kan aangepast worden. Je kunt hier 'gestreepte' 
          sprites maken. De kleurcode wordt als volgt geinterpreteerd:
          
          MSB   3   2   1   0   LSB
               -even-- -oneven            Kleurcode (0-15)
          
          De bovenste  twee bits  worden gebruikt voor de kleur van de 
          oneven pixels en in de onderste twee bits staat de kleur van 
          de oneven pixels. Beide kleuren kunnen liggen tussen 0 en 3.
          
          
                           T P   E N   S P R I T E S 
          
          Bit  5 van  R#8 (TP)  geeft aan of kleur 0 transparant is of 
          dat kleur  0 gewoon  als paletkleur  kan worden gebruikt. In 
          samenhang met sprites zijn er twee situaties:
          
          TP=0:  (gedeeltes van)  sprites met  kleurcode 0 worden niet 
          getoond,  op  die  plaats  kan dan  ook geen  botsing worden 
          gedetecteerd.
          
          TP=1:  (gedeelte)  van  sprite  met  kleurcode  0 wordt  wel 
          getoond, volgens  de paletkleur  (in SCREEN 8 altijd zwart), 
          spritebotsingen worden normaal gedetecteerd.
           
          
                                T O T   S L O T 
          
          Voor  het aansturen  van sprites  is schrijven naar het VRAM 
          noodzakelijk. Lees  de procedures  daarvoor nog maar eens na 
          in  deel 1  van de cursus. Het geeft bijvoorbeeld een enorme 
          snelheidswinst  om  eerst de  VDP klaar  te zetten  voor het 
          schrijven van data vanaf een bepaald adres, en vervolgens de 
          data rechtstreeks  naar Port  #0 (port  &H98) te sturen. Bij 
          sprites  moeten er  toch vaak achterelkaar liggende adressen 
          worden beschreven.
          
          Waarschijnlijk  zal er  in VDP  Cursus deel  15 (die komt op 
          Sunrise  Magazine   #10)  een  routine  met  sprites  worden 
          behandeld.
          
          Veel succes met sprites op MSX en tot de volgende keer.
          
                                                          Stefan Boer
