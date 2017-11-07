                                M S X A U D I O 
                                                 
          
          Ik  heb  dit  programma  geschreven  om de  Music Module  te 
          gebruiken  bij Compile spellen die op HD staan (en niet naar 
          BASIC te hoeven). De syntax is:
          
                  MSXAUDIO [ON|OFF]
          
          Bij  ON  wordt de  MSX-AUDIO chip  goed gezet  zodat Compile 
          spellen hem herkennen. Het kan zijn dat bepaalde programma's 
          hierdoor blijven  hangen. De  EXTBIO hook  wordt nl.  gewoon 
          aangepast  ("POKE -54,35").  Indien iemand  weet hoe het wel 
          volgens de  standaard kan,  of in het bezit is van een �chte 
          MSX-AUDIO  cartridge:  laat  het  even  weten.  Ik  ben  dus 
          ge�nteresseerd  in de  waarden die  op adres #ffca t/m #ffce 
          staan.
          
          
                               D E   S O U R C E 
          
          BDOS:           EQU    5
          FCB1:           EQU    #5c
          
                          in     a,(#c0)
                          inc    a                ;c hoog -> a was 255
                                                  ; -> geen AUDIO
                          jr     c,NoAUDIO
          
          Als op  poort #c0  de waarde 255 staat, is er geen MSX-AUDIO 
          aanwezig.
          
                          ld     hl,#ffca
                          ld     a,(FCB1+2)
                          cp     "F"
                          jr     z,Off
                          cp     "f"
                          jr     nz,On
          
          FCB1 bevat de tekst die achter de COM file wordt meegegeven, 
          maar dan  wel in zo'n vorm dat je er meteen een FCB mee kunt 
          openen.  Ik check  alleen of  er een "F" staat op het tweede 
          karakter van  de filenaam.  Alles behalve  dit betekent  dat 
          MSX-AUDIO aan wordt gezet.
          
          
          Off:            ld     a,i
                          ld     (hl),a
                          ld     de,tOff
                          jr     Print
          
          Om  hem  ook uit  te kunnen  zetten heb  ik iets  gedaan wat 
          eigenlijk niet  mag. Ik heb register I ervoor gebruikt. Voor 
          zover  ik  weet  wordt  het  niet  door  andere  programma's 
          veranderd,  en is (was...) het dus een uitstekende plaats om 
          iets in te zetten dat even bewaard moet blijven.
          
          
          On:             ld     a,(hl)
                          ld     i,a
                          ld     a,35             ;?
                          ld     (hl),a
                          ld     de,tOn
                          jr     Print
          
          Ik weet  niet precies waarom er 35 op #ffca moet komen, want 
          de  mnemonics die  dan aan die hook hangen slaan nergens op. 
          Vandaar  ook  dat de  computer blijft  hangen als  er andere 
          programma's gerund worden die die hook gebruiken. Bijv. TED. 
          Dat is tevens de reden dat ik een off-optie heb gemaakt.
          
          
          NoAUDIO:        ld     de,tNoAUDIO
          Print:          ld     c,9
                          jp     BDOS
          
          tOn:            db     "Your non-standard MSX-AUDIO chip "
                          db     "can now be used as a "
                          db     "standard Panasonic",13,10
                          db     "MSX-AUDIO module.",13,10,"$"
          
          tOff:           db     "EXTBIO hook is restored.",13,10,"$"
          
          tNoAUDIO:       db     "No MSX-AUDIO chip available.",13,10
                          db     "$"
          
          
                                    N O O T 
          
          Ik weet  ook wel  dat het  niet klopt  allemaal, en  dat het 
          eigenlijk  niet mag volgens de standaard. Maar het programma 
          is  handig  wanneer je  je Music  Module wilt  gebruiken bij 
          spellen van Compile op HD.
          
                                                        Kasper Souren
