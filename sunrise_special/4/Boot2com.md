                                B O O T 2 C O M 
                                                 
          
          Dit  programma  heb  ik geschreven  om spellen  die w�l  een 
          bootsector  gebruiken om te laden, maar niet op sectorniveau 
          staan (daar  is GETDISK  al voor) toch op harddisk te kunnen 
          plaatsen.
          
          
                                 W E R K I N G 
          
          De syntax is: BOOT2COM <drive> <filename> <directory>
          
          Dit  laadt   de  bootsector  van  <drive>  (":"  mag  worden 
          weggelaten)   in  <filename>,   waarbij  .COM  de  standaard 
          extensie  is.  <Directory>  heb  ik  erbij  gezet zodat  het 
          mogelijk is  om alle  start-files in ��n directory te zetten 
          die in PATH zit.
          
          Bijv. BOOT2COM D: A:\BATCH\SSRV2.COM C:\GAMES\SSRV2
          
          Als je  dit programma hebt gerund, krijg je een file en moet 
          je  de files  van de  diskette (bij  mij D:) in de directory 
          zetten  die   je  hebt  opgegeven  bij  BOOT2COM  (hier  dus 
          C:\GAMES\SSRV2).
          
          Bij het voorbeeld dus:
          
                 MD C:\GAMES\SSRV2
                 COPY D:\*.* C:\GAMES\SSRV2
          
          En  hierna kun  je, mits je A:\BATCH in je PATH hebt zitten, 
          door SSRV2 in te typen het spel opstarten.
          
          
                              T E S T V E R S I E 
          
          Het is  een testversie dus verwacht niet dat alle fouten bij 
          het aanmaken van een file worden opgevangen.
          
          
                        W E R K E N D E   S P E L L E N 
          
          Spellen  die  ermee  werken  hebben  een  boot-sector,  maar 
          bestaan toch compleet uit files. In de boot-sector wordt dan 
          een  file   geladen.  Tot   nu  toe  werkt  alleen  Starship 
          Rendez-Vous  disk B.  Als je  nog een spel hebt gevonden dat 
          ermee werkt,  laat het dan s.v.p. weten via Sunrise BBS Nuth 
          of de postbus.
          
                                                        Kasper Souren
