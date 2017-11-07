                                      T O 
                                           
          
          Op deze disk staan twee versies van TO: TO van Fokke Post en 
          een  C-versie  van  TO  uit  Itali�,  aangepast door  Pierre 
          Gielen. Omdat  de Italiaanse  TO meerdere  drives aankan, en 
          dus  meer  geschikt  is  voor  HD-gebruikers, staat  hier de 
          handleiding daarvan.
          
          De  hier  besproken  TO  staat  op  de  disk  onder de  naam 
          TO12MSX.PMA en de Fokke-versie onder de naam TO.PMA.
          
          Inmiddels  weet ik  wel al dat Fokke zijn TO aangepast heeft 
          voor  meerdere  drives,  maar  die  versie  heb ik  nog niet 
          gezien/in bezit. Volgende keer maar weer...
          
                                                        Kasper Souren
          
          
                                T O 1 2 . C O M 
          
          TO is een handig programma dat er al lang voor MSX-DOS 2 had 
          moeten  zijn. Met  TO kun je rechtstreeks naar een directory 
          springen. Stel, je hebt de volgende indeling op je harddisk:
          
          
          B:\---- C ---- HITEC ---- SYSTEM ---- LIB
                  |        |          |
                  |        |           -------- SOURCE
                  |        |
                  |         ------- TEMP
                  |
                   ----- PROGRAM
                  |
                   ----- STANDARD
          
          
          ...  en je  wilt naar  de directory  LIB. Dan  moet je onder 
          MSX-DOS 2 intypen:
          
                  CD \C\HITEC\SYSTEM\LIB
          
          
          Dat is  nogal een  hoop type  werk. Afgezien daarvan moet je 
          het exacte pad naar de gewenste directory kennen. Heb je een 
          beetje veel directory's, dan wordt dat al snel moeilijk. Als 
          je TO.COM hebt, en je wilt naar de directory LIB, dan typ je 
          (waar je je ook bevindt) eenvoudig in:
          
                  TO LIB
          
          
          Dat  werkt  als  volgt: TO  gebruikt een  bestand waarin  de 
          complete  directory-tree  is  opgeslagen: TREEINFO.NCD  (dit 
          stamt  van de PC-utility Norton Commander). Kan TO deze file 
          niet vinden, dan maakt hij hem zelf.
          
          
                                   M E E R ! 
          
          Maar  TO  kan  meer!  Als je  niet meer  precies weet  of de 
          directory die  je zoekt  LIB of  LIBS heette, maakt dat niet 
          uit.  Dan zoek je gewoon naar LIB, of LI, en TO springt naar 
          de directory met de naam die met die letters begint. Zijn er 
          meer directory's  die aan de specificatie voldoen, dan krijg 
          je de keuze. Bijvoorbeeld:
          
                  TO S
          
          
          En TO komt op de proppen met de volgende melding:
          
                    Please choose one or ESC to abort:
          
                  1       \c\hitec\system
                  2       \c\hitec\system\source
                  3       \c\standard
          
          Met een  enkele toetsdruk  kun je dan je keuze maken. Als je 
          niet  weet met  welke letters  de naam begint, kun je op het 
          einde van  de naam zoeken door een * voor de naam te zetten. 
          Bijvoorbeeld:  je wilt  naar de  directory STANDARD, maar je 
          weet  niet  zeker  of  je  die  STANDARD  of  STANDAARD hebt 
          genoemd. Dan typ je in:
          
                  TO *ARD
          
          
          ...en TO springt naar de directory \C\STANDARD.
          
          
                         M E E R D E R E   D R I V E S 
          
          TO kan ook met meerdere drives werken. Die kun je opgeven in 
          de volgorde  waarin op  de verschillende  drives moet worden 
          gezocht. Bijvoorbeeld
          
                  TO -dABHC SYSTEM
          
          ...zoekt  achtereenvolgens op de drives A:,B:,H:, en C: naar 
          de directory SYSTEM. Omdat dit nogal veel typwerk is, kun je 
          het environment item DRIVES specificeren, als volgt:
          
                  SET DRIVES=ABHC
          
          Natuurlijk  kun je  deze regel  het beste in de AUTOEXEC.BAT 
          opnemen. Als  DRIVES zoals  hierboven is gedefinieerd, heeft 
          dat  hetzelfde  effect  als  de  drives  in  de  commandline 
          opgeven.
          
          N.B. Je  hoeft het in de commandline niet bij 1 directory te 
          laten. Je kunt er tot 10 opgeven, en daarna een keuze maken.
          
          
                             O P M E R K I N G E N 
          
          - Record  directory, zoals in TO.DOC beschreven, werkt niet. 
            Gebruik hiervoor mijn  programma PPDIR  (PPDIR PUSH, PPDIR 
            POP). (Zie een vorige Sunrise Magazine.)
          
          - De  commandline switch  -B maakt  een nieuwe TREEINFO.NCD. 
            Gebruik deze  mogelijkheid als  je nieuwe directory's hebt 
            gemaakt of  oude gewist.  Let op:  bij deze optie loopt TO 
            alle met (-D of DRIVES=) ingestelde drives af.
          
          - TO gebruikt  geen TO.INI  zoals in  TO.DOC beschreven. Het 
            environment  item  DRIVES  (natuurlijk  veel  sneller!) is 
            hiervoor in de plaats gekomen.
          
          - TO is  en blijft  een gratis programma. Als het je bevalt, 
            stuur dan een ansichtkaart naar:
          
                                        Andrea Steffanoni 
                                        Via Cherubini,  6 
                                        20145 Milano 
                                        Italy
          
          Vermeld  er  wel  bij  dat  je  een  conversie naar  MSX-DOS 
          gebruikt.
          
                                                        Pierre Gielen
