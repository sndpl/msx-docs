             V 9 9 9 0   S P E C I F I C A T I E S 
                                                            
          
                            P A T T E R N   M O D E 
          
          M O D E                 P 1             P 2 
          -----------------------------------------------------------
          resolutie               32 x 26.5       64 x 26.5
                                  (256 x 212)     (512 x 212)
          aantal schermen         2               1
          karakter grootte        8 x 8           8 x 8
          aantal kleuren          15 + transp.    15 + transparant
          palette                 4 tabellen      4 tabellen
                                  met 16 kleuren  met 16 kleuren
                                  uit 32768       uit 32768
          image space             64 x 64         128 x 64
          aantal patronen         max. 16384      max. 16384
          
          
                                 S P R I T E S 
          
          ------------------------------------------------------------
          grootte         16 x 16
          aantal sprites  125 op 1 scherm
                          16 op 1 beeldlijn
          aantal kleuren  15 + transparant voor elke pixel
                          elke sprite kan een eigen palette hebben
                          (1 van de 4!)
          aantal patronen Je hebt 256 patronen, deze zou je volgens 
                          het databoek ook samen kunnen laten vallen
                          met het "normale" scherm, om delen daarvan
                          als sprite te kunnen gebruiken; handig.
          prioriteit      Voor elke sprite kun je aangeven of deze
                          voor of achter het "normale" scherm moet
                          komen (kun je sprites ACHTER iets door laten
                          gaan i.p.v. er altijd voor laten hangen).
                          Als sprites elkaar overlappen, heeft het 
                          laagste spritenummer voorrang.
          ------------------------------------------------------------
          
          
                             B I T M A P   M O D E 
          
                  mode    resolutie       bits/dot
                  -------------------------------------------
                  B1      256 x 212       16
                          (256 x 424)     8
                                          4
                                          2
                  B2      384 x 240       16 (256 kB nodig)
                          (384 x 480)     8
                                          4
                                          2
                  B3      512 x 212       16 (256 kB nodig)
                          (512 x 424)     8
                                          4
                                          2
                  B4      768 x 240       4
                          (768 x 480)     2
                  B5      640 x 400       4 (256 kB nodig)
                                          2
                  B6      640 x 480       4 (256 kB nodig)
                                          2
                  -------------------------------------------
          
          In de  eerste kolom  staat de  resolutie van het scherm, non 
          interlaced,  en tussen  haakjes de  resolutie interlaced (er 
          zijn modes  die niet  interlaced kunnen worden weergegeven). 
          In  de tweede  kolom staat het aantal bit's wat er per pixel 
          (dot) gebruikt  wordt. Ik  vermeld niet  het aantal kleuren, 
          want  dat  hangt  af van  welke mode  (YJK, RGB  of YUV)  je 
          daarvoor gebruikt.
          
          Voorbeeld: 4  bits per dot is 2^4 = 16 kleuren, als je kiest 
          voor  palette mode  0, betekent  dat dat  je 16 kleuren kunt 
          kiezen uit de 32768 mogelijke.
          
          Elk scherm  is in feite een gedeelte van een Image, deze kan 
          groter  zijn dan  het scherm,  bv. 2048 bij 1024 pixels. Wat 
          heb je  daaraan, zul  je denken,  maar het  is eigenlijk een 
          groot  scherm, waarvan je een deel op je monitor plaatst. Je 
          kunt elk  willekeurig gedeelte  van de  Image eruit lichten, 
          wat  een omnidirectionele  scroll geeft (zegt de handleiding 
          dus). Dit  is niet  alleen makkelijk  voor spellen, maar ook 
          zeer handig voor DTP toepassingen.
          
          
                           P A L E T T E   M O D E S 
          
          bit/dot PLTM    conversie methode       aantal kleuren
          -----------------------------------------------------------
          16      0       RGB (!YS:1,G:5,R:5,B:5) 32768
          
          8       0       Palette                 64 van de 32768
                  1       RGB (G:3, R:3, B:2)     256
                  2       YJK                     19268
                  3       YUV                     19268
          
          4       0       Palette                 16 van de 32768
          
          2       0       Palette                 4 van de 32768
          -----------------------------------------------------------
          
          Je  kunt bij elke display mode een palette mode kiezen, mits 
          het aantal bits per dot maar overeenkomen. Hieronder even de 
          uitleg van de tabel.
          
          Bit/dot : aantal bits dat er per pixel gebruikt wordt.
          PLTM    : manier van decoderen van kleuren, hoogste 2 bits 
                    van register 13.
          RGB     : hierbij staat de RGB waarde in het geheugen, in de 
                    tabel staat hoeveel bits er voor welke kleur wordt
                    gebruikt. (Bijv. SCREEN 8 bij de V9958.)
                    En dan nog een extra : het !YS bit, deze bepaalt 
                    PER pixel of het bv. transparant is, en zo kan 
                    worden gebruikt voor Super Impose.
          Palette : in het geheugen staat een kleur nummer die zelf 
                    een RGB  waarde heeft, gekozen met het palette. 
                    (Bijv. SCREEN 5.)
          YJK     : zelfde methode als SCREEN 12
          YUV     : hiervan weet ik niet hoe het werkt, maar het zal 
                    veel op YJK lijken
          
          Deze  gegevens  heb  ik  allemaal uit  het V9990  aplication 
          manual.
          
                                                            Erik Maas
        