                  M S X 2 / 2 +   V D P   C U R S U S   ( 5  )
                                                               
          
          In dit  vijfde deel  worden de statusregisters behandeld. De 
          naam  zegt het  al, deze  registers geven informatie over de 
          status  van  de  Video  Display Processor.  Uiteraard kunnen 
          statusregisters alleen gelezen worden en niet beschreven.
          
          Het  lezen  van  een  statusregister is  in deel  1 (Sunrise 
          Special #2) behandeld, daar kunt u dat nog eens nalezen.
          
          
               O V E R Z I C H T   S T A T U S R E G I S T E R S 
          
                  MSB    7   6   5   4   3   2   1   0    LSB
          S#0            F   5S  C   ---vijfde sprite---  Status #0
          
          F:      Vertical scanning interrupt flag
                  Wordt  geset als  de VDP  bovenaan het scherm begint 
                  met de  beeldopbouw. F  wordt gereset  als S#0 wordt 
                  gelezen.  Dit is  de normale  interrupt, die behalve 
                  naar de hook &HFD9A ook naar de hook &HFD9F springt.
          5S:     Flag voor de vijfde/negende sprite
                  Deze vlag geeft aan dat er meer sprites op een hori-
                  zontale lijn voorkomen dan de VDP kan weergeven.  In
                  G3 (SCREEN 4) en hoger zijn dat er acht, in de ande-
                  re schermen vier.
          C:      Collision flag
                  Wordt gezet als twee sprites elkaar raken.
          vijfde  
          sprite: Bevat het nummer van de vijfde of negende sprite.
          
          
                  MSB    7   6   5   4   3   2   1   0    LSB
          S#1            FL  LPS -Identification #-  FH   Status #1
          
          FL:     Lightpen flag (Lightpen flag is geset, LP bit 6 R#8)
                  Als de lichtpen licht waarneemt moeten zowel dit bit 
                  als  IE2 (bit 5 van R#0) geset zijn om een interrupt 
                  te  veroorzaken.  FL  wordt  gereset  als  S#1 wordt 
                  gelezen.
          
                  Mouse Switch 2 (Mouse flag is geset, MS bit 7 R#8)
                  De 2e  vuurknop van  de muis  is ingedrukt. FL wordt 
                  niet gereset als S#1 wordt gelezen.
          
          LPS:    Lightpen switch (Lightpen flag is geset)
                  De  vuurknop van de lichtpen is ingedrukt. LPS wordt 
                  niet gereset als S#1 wordt gelezen.
          
                  Mouse switch 1 (Mouse flag set)
                  De 1e  vuurknop van  de muis is ingedrukt. LPS wordt 
                  niet gereset als S#1 wordt gelezen.
          
          Identification number:
                  Het ID# van de VDP. Hieraan kun je zien of het V9938 
                  of V9958 is (bit 2 van S#1 is geset als V9958).
          
          FH:     Horizontal scanning interrupt flag
                  FH wordt geset als een zgn. line-interrupt optreedt, 
                  dus  als de VDP begint met het weergeven van de lijn 
                  die   in   R#19  staat.   Er  wordt   een  interrupt 
                  gegenereerd als  IE1 (bit  4 van  R#0) is  geset. FH 
                  wordt gereset als S#1 wordt gelezen.
          
          Let op:  de lichtpen-  en muisuitlezing  wordt bij  de V9958 
          niet  meer door  de VDP gedaan. De bits FL en LPS hebben dus 
          hun  betekenis verloren.  Bij MSX2 wordt het uitlezen van de 
          muis overigens ook al niet met de VDP gedaan.
          
          
                  MSB    7   6   5   4   3   2   1   0    LSB
          S#2            TR  VR  HR  BD  1   1   EO  CE   Status #2
          
          TR:     Transfer ready flag
                  Als de CPU (microprocessor) data naar de VDP stuurt, 
                  moet er gewacht worden tot dit bit geset is. Dan mag 
                  de data verstuurd worden.
          
          VR:     Vertical scanning line timing flag
                  Tijdens verticaal scannen is deze flag geset.
          
          HR:     Horizontal scanning line timing flag
                  Tijdens horizontaal scannen is deze flag geset.
          
          BD:     Boundary color detect flag
                  Bij  het  Search commando  (zie VDP  cursus deel  4) 
                  geeft deze  flag aan  of de randkleur is gevonden of 
                  niet.
          
          EO:     Display field flag
                  Geeft bij het afwisselend weergeven van pagina's aan 
                  welke pagina er wordt weergegeven.
          
          CE:     Command execute flag
                  Geeft  aan dat  een commando wordt uitgevoerd. Wacht 
                  tot dit bit gereset is voordat het volgende commando 
                  naar de VDP wordt gestuurd. Zie VDP cursus deel 3 en 
                  4.
          
          
                  MSB    7   6   5   4   3   2   1   0    LSB
          S#3            X7  X6  X5  X4  X3  X2  X1  X0   Column low
          S#4            1   1   1   1   1   1   1   X8   Column high
          S#5            Y7  Y6  Y5  Y4  Y3  Y2  Y1  Y0   Row low
          S#6            1   1   1   1   1   1   Y9  Y8   Row high
          
          Bovenstaande registers worden gebruikt voor het aangeven van 
          de de plaats van de spritebotsing, de plaats van de lichtpen 
          of   de  relatieve   beweging  van  de  muis.  Zie  voor  de 
          spritebotsing verderop.  Lichtpen en muis worden niet via de 
          VDP uitgelezen.
          
          
                  MSB    7   6   5   4   3   2   1   0    LSB
          S#7            C7  C6  C5  C4  C3  C2  C1  C0   Color reg
          
          Dit register  wordt gebruikt  bij de  POINT en  VRAM to  CPU 
          commando's.  De VRAM  data staat  in dit  register. (Zie VDP 
          cursus deel 4).
          
                  MSB    7   6   5   4   3   2   1   0    LSB
          S#8            BX7 BX6 BX5 BX4 BX3 BX2 BX1 BX0  Border X low
          S#9            1   1   1   1   1   1   1   BX8  Border X hig
          
          De X  co�rdinaat die  wordt gevonden bij het Search commando 
          wordt in deze registers gezet. (Zie VDP cursus deel 4).
          
          Nu  we de  theorie hebben  behandeld komen er nog een aantal 
          toepassingen aan bod.
          
          
                          S P R I T E   B O T S I N G 
          
          Bij  een spritebotsing  wordt C  (bit 5  van S#0) geset. Als 
          zowel MS  als LP  (bit 7  en 6  van R#8)  zijn gereset,  dan 
          zullen de co�rdinaten van de botsing naar S#3 t/m S#6 worden 
          geschreven.  Let op:  bij het  lezen van S#5 wordt de inhoud 
          van S#3 t/m S#6 gereset.
          
          De  X die  in S#4  en S#3  staat is  12 meer dan de X van de 
          botsing, de  Y die  in S#5  en S#6  staat is  8 meer  dan de 
          botsing. Oftewel:
          
          XS#3S#4 = XBOTSING + 12
          YS#5S#6 = YBOTSING + 8
          
          
                             S C R E E N S P L I T 
          
          Het  is   met  de  statusregisters  zeer  eenvoudig  om  een 
          nauwkeurige  screensplit te  maken. We gebruiken daarvoor HR 
          en FH. Eerst zetten we de lijn waarop de split moet optreden 
          in R#19.  Vervolgens wachten we tot FH (bit 7 van S#1) wordt 
          gezet.  Nu kunnen  we de  screensplit strak  tegen de border 
          krijgen door  HR uit  te lezen.  Als HR  (bit 5  van S#2) is 
          geset  begint de  VDP met het weergeven van een nieuwe lijn. 
          Voer  nu  zo  snel  mogelijk  de  nodige akties  uit, en  de 
          screensplit is nagenoeg perfect.
          
          Een   korte   aflevering  dit   keer.  De   sprites  en   de 
          beeldschermopslag komen  nog aan  bod, daarna gaan we verder 
          met  de toepassingen. De volgende keer neem ik u mee naar de 
          wereld van de sprites.
          
                                                          Stefan Boer
