                              H D   I N D E L E N 
                                                   
          
          Het  indelen  van  een  harddisk  kan  natuurlijk heel  snel 
          gebeuren.  Maar  het is  verstandiger om  even erover  na te 
          denken. Er  bestaat nl.  de kans dat je veel files niet meer 
          kunt  vinden  na  verloop van  tijd, als  je gewoon  lukraak 
          subdirectory's aanmaakt.
          
          Dat  er dan  een programma  als FileFind  bestaat, doet daar 
          niks aan  af. Het  is onhandig  om telkens als je iets nodig 
          hebt weer FF te moeten gebruiken.
          
          Ook kun  je denken aan welke schil je wilt gebruiken. Gewoon 
          onder  MSX-DOS werken  met een  paar batchfiles  of een  GUI 
          (Graphical User Interface) als MSX-View of Easy.
          
          
                               P A R T I T I E S 
          
          Om te beginnen bij het begin: je moet de harddisk opdelen in 
          partities  (high level  formatteren genaamd;  meestal is  de 
          harddisk al  low level  geformatteerd). Je  kunt, als je een 
          harddisk  kleiner dan  of gelijk  aan 32 MB hebt, gewoon ��n 
          partitie aanmaken.  Maar dit is niet erg slim. Je krijgt dan 
          vrij grote clusters, soms zelfs van 32 kB. Hierdoor wordt je 
          harddisk niet erg snel.
          
          Het is dus verstandig om een kleine A: partitie aan te maken 
          met  daarop   allerlei  programma's  (aatjes)  die  je  vaak 
          gebruikt. Ikzelf heb gekozen voor 4 MB. Als je dan de FAT op 
          12 sectoren zet, heb je clusters van 1 kB.
          
          
                              F A T G R O O T T E 
          
          Je  kunt het beste altijd een FAT van 12 sectoren nemen, dan 
          is  de  clustergrootte  het  kleinst. Dit  komt ook  weer de 
          snelheid  ten  goede.  Maar  niet  alleen  de snelheid.  Bij 
          clusters van 32 kB kost ook een file van ��n byte je 32 kB.
          
          De  volgende partitie(s)  kun je dan natuurlijk wel zo groot 
          mogelijk maken.  De max.  grootte van een partitie is 32 MB. 
          Als je het �cht maximaal neemt, krijg je clusters van 32 kB. 
          Ik heb daarom even gekeken of dit niet beter kon. En het k�n 
          beter. Als je als grootte 32686 kB neemt (bij FDISK van MK), 
          krijg je met een FAT van 12 sectoren clusters van 16 kB.
          
          Ik heb op mijn 80 MB HD gekozen voor de volgende indeling:
          
                     A:   4096 kB   2 sectoren per cluster
                     B:  32686 kB  16 sectoren per cluster
                     C:  32686 kB  16 sectoren per cluster
                     D:  13456 kB   8 sectoren per cluster
          
                                 R O O T D I R 
          
          Je  moet  voorkomen dat  er veel  files in  de rootdirectory 
          komen  te  staan.  Je  kunt het  beste alleen  COMMAND2.COM, 
          MSXDOS2.SYS,  AUTOEXEC.BAT  en REBOOT.BAT  in de  root laten 
          staan. Dit  is niet  alleen voor de overzichtelijkheid, maar 
          komt  ook de snelheid ten goede. Bij het opstarten worden de 
          juiste files meteen gevonden.
          
          De  rootdirs  van  de  andere  partities  kun  je  het beste 
          helemaal  leeg  laten.  Daar  mogen  eigenlijk  alleen  maar 
          subdirectory's in staan.
          
          Bij  FDISK kun  je daarom  ook het beste de dirgrootte op 64 
          bestanden zetten. Dit is het kleinste.
          
          
                                 S U B D I R S 
          
          Zoals  gezegd  komen  op  A:  programma's die  vaak gebruikt 
          worden.  Ik heb  zelf de volgende (1e graads) subdirectory's 
          op A:
          
                  BASIC   - BASIC tools (bijv. Flexkist, KUN-BASIC)
                  BATCH   - batchfiles
                  CRUNCH  - allerlei crunchers en decrunchers
                  MEMMAN  - MemMan
                  ML      - machinetaal tools (bijv. GEN80, MSXDEBUG)
                  UTILS   - allemaal utility's
                  VIEW    - MSX-View
          
          In  MEMMAN zit  weer de  subdirectory TSRS,  dit om  het een 
          beetje  overzichtelijk  te houden.  Als er  30 TSR's  tussen 
          andere files staan, wordt het een beetje onduidelijk.
          
          UTILS moet  ik nog  eens goed  indelen: bepaalde (2e graads) 
          subdirectory's  voor bijv.  tekst-tools (TED,  wordcount) en 
          turbo R tools (READKANA, ROMTURBO).
          
          MSX-View  bevat  uiteraard  allerlei  subdirs met  files die 
          bestemd zijn voor View zelf.
          
          
                        A N D E R E   P A R T I T I E S 
          
          Ik heb  het zelf  nog niet goed ingedeeld, omdat ik nog mijn 
          BBS  op deze  HD heb staan. Maar zodra ik weer "echt" online 
          ben, zal  ik het  BBS op een aparte HD zetten (20 MB is meer 
          dan genoeg voor een BBS).
          
          Dan   wil   ik   op  B:   muziek  (MOD-files,   MoonBlaster, 
          Soundtracker,  SME  3)  zetten, op  C: tekst,  informatie en 
          (mijn) sources en op D: spellen.
          
                                                        Kasper Souren
