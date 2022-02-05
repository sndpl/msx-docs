                       M B   S C A N   V O O R   D O S 2 
                                                          
          
          Op de  vorige Sunrise  Magazine stond  MBSCAN.BIN, een  zeer 
          handig  programmaatje om  de gegevens  van alle  MoonBlaster 
          files  op  een enkel-  of dubbelzijdige  disk onder  DOS1 te 
          bekijken.
          
          Alle  met  MoonBlaster gemaakte  muziekdisks kunnen  daarmee 
          worden  bekeken.  MB  werkt  immers  alleen  met  enkel-  of 
          dubbelzijdige diskettes onder DOS1.
          
          
                                      H D 
          
          Nu zijn er echter van die irritante HD gebruikers die overal 
          een  HD  versie  van willen  hebben, en  dus ook  van MBSCAN 
          (Nvdr.  Hmmfff...). Waar ze dat voor willen gebruiken is mij 
          een raadsel,  want een  offici�le HD  versie van MoonBlaster 
          bestaat  niet en  voor mensen die MoonBlaster niet hebben en 
          de muziekjes alleen maar afspelen heeft het weinig nut, want 
          wie verspreid er nou EDIT versies van muziekjes?
          
          Enfin,  voor  de  HD  gebruikers  die  een directory  met MB 
          muziekjes hebben  en deze  toch met  MBSCAN willen bekijken, 
          heb ik samen met Kasper (juist ja, ook zo'n HD freak) MBSCAN 
          omgebouwd tot een DOS2 versie.
          
          
                                 W E R K I N G 
          
          De werking is vrij simpel:
          
          MBSCAN [drive:][directory]
          
          Wordt  er  niets  ingevuld, dan  wordt de  huidige directory 
          genomen.  Van elke  .MBM file wordt de naam gegeven, EDIT of 
          USER,  de  titel  en  de  drumkit.  De  uitvoer  kan  worden 
          gepauzeerd met  de spatiebalk  en met  ^C kan  het programma 
          voortijdig worden be�indigd.
          
          
                          R E D I R E C T I O N I N G 
          
          De  schermuitvoer werkt  via de BDOS, wat als voordeel heeft 
          dat de redirectioning kan worden gebruikt. Dat is erg handig 
          en meteen ook de enige zinnige functie van MBSCAN.COM die ik 
          kan bedenken voor mensen die geen HD hebben, je kunt zo heel 
          eenvoudig een  overzicht van alle muziekjes naar een file of 
          naar de printer sturen. Bijvoorbeeld:
          
          MBSCAN A:\MB>H:MUSIC.TXT
          
          Zet de  info van alle muziekjes in de directory \MB op drive 
          A:  in de  file MUSIC.TXT op drive H:. Het is in verband met 
          de snelheid  zeer raadzaam  om de  tekstfile op  H: te laten 
          komen. Nog een voorbeeld:
          
          MBSCAN>PRN
          
          Print de info van de muziekjes in de huidige directory uit.
          
          
                               T E N S L O T T E 
          
          Heb  je geen  HD maar  wel DOS2, blijf dan gewoon MBSCAN.BIN 
          gebruiken.  Ik   neem  niet  aan  dat  je  de  muziekjes  in 
          directory's  hebt gezet, en MBSCAN.BIN is nu eenmaal sneller 
          dan  MBSCAN.COM,   omdat  er  rechtstreeks  sectoren  worden 
          gelezen.
          
          Voor de  HD gebruikers is er nu dus MBSCAN.COM, zet voor een 
          optimale snelheid op de turbo R de BUFFERS op maximaal. Veel 
          plezier!
          
                                                          Stefan Boer
