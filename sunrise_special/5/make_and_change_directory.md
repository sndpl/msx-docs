               M A K E   A N D   C H A N G E   D I R E C T O R Y 
                                                                  
          
          Het  programma MCD.COM  is gemaakt om de volgende combinatie 
          te vereenvoudigen:
          
              MD \UTILS\INPAK
              CD \UTILS\INPAK
          
          Met MCD wordt dit dan:
          
              MCD \UTILS\INPAK
          
          De subdirectory  INPAK wordt  gemaakt in de directory \UTILS 
          en de huidige directory wordt ingesteld op \UTILS\INPAK.
          
          
                               B A T C H F I L E 
          
          De  programmeur van MCD.COM heeft echter niet gedacht aan de 
          mogelijkheid  van  een  batchfile. De volgende  twee regels 
          hebben immmers hetzelfde effect als de COM-file:
          
               MD %1
               CD %1
          
          En  als je  deze 2  regels invoert  in een batchfile genaamd 
          MCD.BAT, is de werking precies hetzelfde, op ��n ding na: de 
          mogelijkheid om de directory meteen hidden te maken.
          
          By the  way, MCD.COM is gemaakt door Fokke Post en zit in de 
          file MCD.PMA, die op deze disk staat.
          
                                                        Kasper Souren
