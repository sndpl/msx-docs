                      R 8 0 0   R O M / R A M   L E Z E N 
                                                           
          
          Met   het  volgende  BASIC-programmaatje  heb  ik  even  het 
          snelheids-verschil tussen  ROM- en  RAM-gebruik van  de R800 
          gemeten. (Staat op disk onder de naam TESTER.BAS.)
          
               100 TIME=0
               110 _TURBO ON
               120 FOR I=0 TO 16384
               130 IF I/100=I\100 THEN LOCATE 0,0: PRINT I/100
               140 NEXT I
               150 PRINT TIME
          
          Regel   130  kan  natuurlijk  veel  beter.  I/100=I\100  kan 
          namelijk worden  vervangen door  I MOD  100=0. Het "\" teken 
          staat  voor integerdeling. Met MOD is de rest te bepalen van 
          een integerdeling. Maar doordat ik "/" gebruik duurt het wat 
          langer, en zijn de resultaten nauwkeuriger.
          
          
                              T A B E L L E T J E 
          
                                      |  RAM |  ROM
                          ------------+------+-------
                           Z80        | 2074 | 2075
                          ------------+------+-------
                           R800-ROM   |  255 | 1000
                          ------------+------+-------
                           R800-DRAM  |  246 |  981
          
          
          De getallen  zijn de  resultaten bij  mijn turbo R op 60 Hz. 
          Bij RAM heb ik KUN-BASIC in RAM (de diskversie dus) gebruikt 
          en bij ROM KUN-BASIC in een ROMmetje.
          
          Het  valt direct  op dat er een groot verschil is tussen RAM 
          en ROM  bij de R800-modes. In de R800-modes is hij gemiddeld 
          3.95  keer langzamer,  terwijl het  in de  Z80-mode geen ene 
          moer uitmaakt.
          
          De  conclusie  uit  deze  cijfertjes  is  niet  moeilijk  te 
          trekken: bij het lezen van ROM schakelt de engine terug naar 
          de Z80-mode!
          
          Als  je dus  toevallig KUN  op EPROM  hebt, kun  je toch nog 
          beter KUN  in het  geheugen inladen,  omdat dat het zaakje 4 
          keer  versnelt in de R800-mode. Het maakt overigens niks uit 
          of  dat   geheugen  het   DRAM  geheugen  is  (met  READKANA 
          erinladen)  of de gewone memory mapper. Wel vrees ik dat ook 
          een externe langzamer wordt aangestuurd.
          
                                                        Kasper Souren
