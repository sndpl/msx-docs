 
                                   - MEMMAN -
          
                     T S R   D E V E L O P M E N T   K I T 
                                                            
          
                  Producent  : MSX Software Team
                  Computer   :       
                               M S X 2 (80 kB RAM)
                  Medium     : 1 dubbelzijdige diskette
                  Leverancier: MCM's lezersservice
          
          
          Een tijdje  geleden alweer  dat dit  produkt voor  het eerst 
          verkocht  werd. Zolang geleden zelfs dat ik toen nog geen ML 
          kende.
          Maar hier volgt alsnog de bespreking van dit MST-produkt.
          
          
                              H E T   P A K K E T 
          
          ...bestaat  uit een  handleiding en een disk. Veel tekst uit 
          de handleiding  zullen de  meeste kopers  van het  pakket al 
          hebben:  documentatie van  MemMan calls  en het artikel over 
          interrupts dat al in MCM heeft gestaan. Nieuw zijn echter de 
          beschrijving  van  het  maken  van TSR's  volgens de  MemMan 
          standaard -  toevallig het belangrijkste - en de sources van 
          de PB.TSR en PRINT.COM.
          
          
                                    D I S K 
          
          Op  de diskette staat eveneens een groot aantal files die al 
          in het  bezit zijn  van potentiâle kopers. Alleen LT.COM, de 
          linker,  en daardoor het belangrijkste stukje bitvolgorde op 
          deze disk,  en de  raamwerken voor  TSR-listings voor M80 en 
          GEN80.  Ook  zijn  er nog  2 batchfiles,  maar dat  mag niet 
          bijster interessant worden genoemd.
          
          Wat je  op deze  disk mist  zijn de  2 sources die wel in de 
          handleiding  staan  geprint.  Misschien  is  dit gedaan  ter 
          voorkoming  van het  vrijkomen van  versies met  hele kleine 
          aanpassingen.
          
          
                           R E L - B E S T A N D E N 
          
          De  linker werkt  alleen met relocatable files. Files die op 
          een  door  de  gebruiker  of  een  ander  programma  dan  de 
          assembler op  de juiste  plaats te zetten zijn. Beschrijving 
          van deze REL-files is te vinden op Sunrise Special #1.
          
          Voor  zover ik  weet is het alleen met GEN80 en M80 mogelijk 
          om REL-files  aan te  maken. Bij  M80 is het trouwens alleen 
          maar mogelijk om REL-files te produceren. WB-ASS2 gebruikers 
          worden  hierdoor uitgesloten, maar dit is niet zo erg: GEN80 
          is toch veel beter!
          
          De linker maakt van die REL-files TSR-files. Deze files zijn 
          mijns   inziens  gewoon   veredelde  REL-files.  In  zoverre 
          veredeld dat  TsrLoad ze makkelijk in een MemMan segment kan 
          plaatsen.
          
          
                              Z E L F   M A K E N 
          
          Zelf een  TSR maken  is een koud kunstje met dit LinkTsr. In 
          je source geef je de hook(s) aan die wilt afbuigen en zet je 
          de  code die  aan die  hook(s) komt  te hangen.  Je hoeft je 
          verder dus  niet te bekommeren over het vrijmaken van ruimte 
          in  de top  van het  geheugen en het correct afbuigen van de 
          hooks.
          
          Als  voorbeeld staan  CHGCPU2.TSR en VOLINMET.TSR met source 
          en uitleg op deze Special.
          
          
                               C O N C L U S I E 
          
          Als  je dit  koopt moet je niet denken dat je iets nieuws in 
          handen krijgt.  Het grote  voordeel is  dat je  nu alles bij 
          elkaar hebt, in ÇÇn handleiding. Als je makkelijk TSR's wilt 
          maken,  is het  zeker aan  te raden!  Maar ook  als je al je 
          eigen systeem  hebt om  TSR's te  maken is het een aanrader: 
          het is wel zo leuk voor de gebruikers om een eigen versie Çn 
          een MemMan versie te verspreiden.
          
          Waarschuwing: alleen voor GEN80- en M80-gebruikers.
          
                                                        Kasper Souren
         