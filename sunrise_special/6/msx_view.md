                                M S X - V I E W


          Iedereen  kent  wel  Halos.  Het  operating  system van  HAL
          laboratories  dat  er  goed  uitziet  maar  slecht werkt  en
          eigenlijk  onwerkbaar  is. Mensen  die het  versienummer wel
          eens hebben  bekeken weten  dat de  Halos die  idereen heeft
          versie  0.0.4 is van de echte Halos die in Japan te koop is.
          De uiteindelijke Halos is weer de voorloper van MSX-View. De
          twee zijn  danook bijna  identiek. (MSX-View  is voor  tR en
          werkt onder DOS2, dus met shells e.d.)

          MSX-View  is een  programma dat  het mogelijk  maakt om  met
          windows te  werken en  nog veel  meer. MSX-View zelf bestaat
          enkel  en  alleen  uit een  kernel die  verborgen zit  in de
          memorymapper. Zijn 300 a 400 functies zijn aan te roepen via
          RST  8 DW [functienr]. Gelukkig zijn er ook zgn. applicaties
          voor  MSX-View  zoals VSHELL  (een DOS-Shell),  ViewTED (een
          tekst editor),  ViewDRAW (een  DTP programma)  etc. Daarover
          wil  ik eigenlijk niet gaan schrijven, en dit is dus bedoeld
          voor programmeurs.


                  A A N S T U R E N   V A N   M S X - V I E W

          Oftewel het  zelf schrijven  van applicaties  en .DA  files.
          (Overlay  programma's  die  tijdens  de  uitvoering van  een
          applicatie kunnen worden ingeladen, denk aan een calculator,
          notitieboekje,  etc.) Toevallig  ben ik  ooit eens tegen een
          MSX-FAN opgelopen  met daarop MSX-View library's voor MSX-C.
          Door  de informatie  die hier  in staat is het mogelijk zelf
          applicaties te  schrijven is  zowel C  als assembly,  wat je
          maar wilt. Ik heb mijn eerste applicatie al af.

          Iedere  C  of  ML programmeur  moet volgens  mij applicaties
          kunnen  schrijven.  Zo  moeilijk is  het niet.  En hoe  meer
          applicaties hoe meer ondersteuning MSX-View krijgt en dat is
          ook belangrijk.


                 W A A R O M   N I E T   I E T S   A N D E R S

          Ik hoor iedereen nu al weer roepen: MSX-View is Japans, daar
          heb  je niets aan, het is veel te ingewikkeld en te traag en
          ik schrijf  zelf wel  een goed  windows programma. Maar neem
          maar  van mij aan dat dit het beste windows programma is dat
          ooit op  de MSX zal verschijnen. MSX-View zelf is alleen een
          kernel    en   hoewel   er   standaard   messages   inzitten
          (bijvoorbeeld copy  of quit, maar dan in het Japans) hoef je
          die  natuurlijk niet  te gebruiken.  En anders  koop je toch
          gewoon de  MSX-View patch  van MGF,  dan zijn  die standaard
          messages  ook vertaald.  Wees dus  niet eigenwijs en gebruik
          MSX-View. Het is echt waanzinnig goed.


                      F U N C T I E V O O R B E E L D E N

          Een paar voorbeeldfuncties met uitleg:

          GetEvent        - Kijkt welke keuze is gemaakt/toets is
                            ingedrukt,etc.
          FrontWin        - Zet het huidige window voor alle andere
                            windows.
          DispAllCntl     - Drukt alle controls af (bv. schuif,
                            switch, checkmark)
          OpenMenu        - Opent pull-down menu systeem (gedefenieerd
                            door eigen programma)
          InitGraf        - Initializeert de MSX-View GrafPack
          FillPai         - Drukt een stuk cirkel af, gevuld met een
                            patroon
          GetDiskInfo     - Haal informatie op over een diskdrive(of
                            disk die erin zit)
          GetSin          - Haal een waarde uit de sinus-tabel
          FilePack(...)   - Laad/Save een (gedeelte van een) file
                            (Er verschijnt een keuzemenu waar
                            directories,drives en files kunen worden
                            doorlopen, een naam kan worden ingegeven
                            en vervolgens OK of ABORT kan worden
                            gekozen.)
          ErrMessage      - Plaats een error-window in het midden van
                            het scherm met een message, een of
                            meerdere buttons eronder en evt. en teken
                            ernaast (disk, uitroepteken...) keert
                            terug met de gekozen button in A

          MSX-View  werkt  ook  met  gestandariseerde  dataopslag.  De
          GetEvent  levert  het  event  bijvoorbeeld  in een  door jou
          gekozen  eventbuffer. Het  systeem is  door zijn  jarenlange
          ontwikkeling geperfectioneerd  tot in de kleinste details en
          dat blijkt uit alles.


                Z E L F   A P P L I C A T I O N S   M A K E N ?

          Als  je interesse  hebt in  hoe dit  alles in de praktijk in
          zijn werk  gaat, of je hebt belangstelling voor de library's
          en  programma's waarmee dit alles moet worden bewerkstelligd
          dan hoef je alleen maar even contact op te nemen met mij via
          post,  telefoon  of echomail,  uploads/downloads alleen  via
          Piramide BBS.

                                                      Jan van Valburg
