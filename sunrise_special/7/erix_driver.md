                         E R I X   D R I V E R   1 / 2 
                                                        
          
          
                      W A A R O M   E E N   D R I V E R ? 
          
          Het nut  van een  driver is  het schrijven  van toepassingen 
          voor  RS232C, zonder  dat je  voor elke  interface een apart 
          programma  moet  maken.  Zo  is  het  handig  om  een  modem 
          programma  wat  je in  combinatie met  een SONY  RS232C hebt 
          geschreven, deze  door anderen  gebruikt kan  worden terwijl 
          die bv. een Philips NMS1210 hebben.
          
          
                 D E   E I S E N   A A N   E E N   D R I V E R 
          
          Een driver  moet aan  verschillende eisen  voldoen, wat  die 
          eisen zijn ligt aan de toepassingen ervoor.
          Ik ben toen een lijstje op gaan stellen van eisen die ik aan 
          een  driver zou  gaan stellen,  de belangrijkste eisen staan 
          hieronder :
          
            - Driver  moet functioneren onder MSX-DOS1 en MSX-DOS2, en 
              het liefst ook BASIC
            - Driver  voor verschillende  typen interfaces moeten door 
              programma's   gebruikt  kunnen  worden  zonder  speciale 
              aanpasssingen
            - De driver moet snel zijn
            - De  driver moet  resident blijven  na installatie, zodat 
              het   mogenlijk  is  van  programma  naar  programma  te 
              springen, zonder  dat het  nodig is om opnieuw de driver 
              in te laden.
          
          Er  zijn  al  wel  drivers  die  een aantal  van deze  eisen 
          bevatten, maar geen van allen doet dat ook werkelijk.
          
          Huib Walta had al een driver gemaakt die onder MemMan werkt, 
          maar  die  miste  een  van  de  eisen  helemaal, en  dat was 
          snelheid.   MemMan  is  helaas  niet  snel  genoeg  om  hoge 
          baudrates aan te kunnen. Dit komt doordat een MemMan TSR pas 
          aangeroepen  wordt  als een  reeks interslot  calls gebruikt 
          zijn. Aan de rest van de eisen wordt wel voldaan.
          
          De BASIC driver van Pier Feddema is ook goed, maar is helaas 
          alleen geschikt  bij gebruik  onder DISK-BASIC. De driver is 
          wel  sneller dan de MemMan TSR die Huib Walta gemaakt heeft, 
          maar aangezien  ik onder  DOS wil werken voldoet ie voor mij 
          niet.
          
          Huib  Walta  heeft  ook  nog  een driver  gemaakt die  onder 
          Turbo-Pascal  werkt, maar  deze driver  is niet resident, en 
          zal  elke  keer  vanuit  DOS gelinked  moeten worden  aan je 
          programma.
          
          Toen ik geen drivers kon vinden die aan mijn eisen voldeden, 
          en  ik  toevallig  genoeg  informatie tegenkwam  om de  SONY 
          RS232C te  kunnen aansturen, besloot ik om zelf dan maar een 
          driver te schrijven.
          
          
                        D E   P R O B L E M E N   V A N 
                              E E N   D R I V E R 
          
          Toen  ik begonnen was met het schrijven van een driver, kwam 
          ik meerdere problemen tegen.....
          
          Er  moet een  manier zijn om alle functies van de driver aan 
          te kunnen  spreken, die voor alle drivers voor verschillende 
          interfaces  hetzelfde is. Dit is simpel op te lossen door er 
          een JUMP-TABLE  in te  zetten, die door applicaties gebruikt 
          kan worden.
          Als  dit dan  gerealiseerd is, moet de applicatie natuurlijk 
          ook  weten  of  de  driver er  is, en  zo ja,  waar dan  die 
          jump-table staat.
          Om dit  laatste te  doen, heb  ik iets  gedaan wat eigenlijk 
          niet  mag in  de MSX  standaard, ik  heb een  stukje in  het 
          werkgeheugen gebruikt om daar 4 bytes te plaatsen. De eerste 
          twee bytes  geven aan dat de driver er is, en de twee andere 
          bytes  wijzen  het  adres  aan  waar  de  jump-table in  het 
          geheugen  staat.  De  driver  kan  alleen  van  disk  worden 
          geladen,  dit omdat  de driver een aparte loader nodig heeft 
          die  een file  van DISK  wil laden. Daarom heb ik gekozen om 
          deze informatie op te slaan in het cassette-parameter table, 
          op adres  &HF3FC. Cassette  wordt toch meestal niet gebruikt 
          als  er een  diskdrive aanhangt,  en de RS232C gebruikt gaat 
          worden.
          Om effectief met het geheugen om te gaan is het noodzakelijk 
          dat de  driver zo  hoog mogenlijk  in het  geheugen komt  te 
          staan,  het liefst  hoger dan  dat MSXDOS.SYS  zich nestelt, 
          zodat er  naar BASIC,  en weer  naar DOS  gegaan kan worden, 
          zonder dat de driver aangetast wordt.
          Doordat  de top  van het geheugen niet vast gedefinieerd is, 
          is het  noodzakelijk dat  de driver op elk adres kan werken. 
          Aangezien  het  praktisch  onmogenlijk  is  om de  driver in 
          mekaar  te  zetten, met  alleen relatieve  adressen, is  het 
          noodzakelijk dat  de driver zichzelf eenmalig aan kan passen 
          aan een bepaald adres in het geheugen. Hiervoor is de driver 
          gemaakt  alsof ie van adres 0 werkt. En bij de driver is een 
          tabel  geplaatst  met informatie  over wat  er in  de driver 
          veranderd  moet   worden.  Dit   aanpassen  wordt  door  een 
          speciale loader gedaan.
          Nog een  groot probleem  van een  driver is  dat ie resident 
          moet  zijn. Een  driver inladen  en die informatie op &HF3FC 
          te plaatsen  is niet moeilijk, nu moet alleen gezorgt worden 
          dat de driver intact blijft als MSX-DOS in werking komt. Dit 
          blijkt niet erg moeilijk te zijn, het is simpel om HiMem aan 
          te  passen, en  je driver  daar net  boven te  plaatsen. Het 
          nadeel is  echter dat  MSX-DOS dit  niet slikt,  en je  deze 
          methode  vanuit BASIC  moet gebruiken.  Een methode is om de 
          driver te  laden vanuit BASIC, maar aangezien ik met MSX-DOS 
          werk  heb ik  gekozen om  een loader  de driver  in te laten 
          laden,  de driver  op het  juiste adres  neer plaatsen, naar 
          BASIC  toe   te  gaan,  wat  informatie  in  het  werkgebied 
          aanpassen, en dan verdergaan.
          Om  het mogenlijk te maken dat er automatisch nog wat gedaan 
          wordt nadat  de driver  ingeladen is, is het mogenlijk om de 
          driver  als het  ware zelf  de commando's te laten intikken. 
          Dit  is  heel  simpel  te  maken  door  een  string naar  de 
          toetsenbord buffer  te kopieren,  wat pointers aanpassen, en 
          BASIC de controle over laten nemen.
          
          Nu bleef ik nog zitten met de snelheid van de driver.
          Zoals bij meesten wel bekend, genereert een RS232C interface 
          een  interrupt als  er data  binnenkomt. Door  snel op  deze 
          interrupt te  reageren heb  je het  IC wat  de data verwerkt 
          uitgelezen  voordat  er  nieuwe  data  binnenkomt  die  deze 
          informatie overschrijft.
          Zodra  er een  interrupt optreed, zorgt de Z80 ervoor dat er 
          automatisch een  CALL uitgevoerd wordt naar adres &H0038, op 
          die  adres  staat  een jump  naar een  ander adres  waar een 
          routine staat wat netjes zorgt dat &HFD9A aangeroepen wordt, 
          even  kijkt of  de interrupt  van de VDP is (50/60 Hz) en zo 
          ja, dan even &HFD9F aanroept. In deze routine wordt ook o.a. 
          het toetsenbord afgescanned, wat tellers bijgehouden, en nog 
          meer  onzin.   Dit  alles  zorgt  ervoor  dat  deze  routine 
          behoorlijk   lastig  is   als  er   op  hoge  snelheid  data 
          binnenkomt, waardoor er data verloren gaat.
          Wat  dit  ook  inhoud,  is  dat  er  minder  processor  tijd 
          overblijft  om de  data die  ontvangen is  te verwerken. Dit 
          houd  in  dat het  onder BASIC  bij 19200  baud zo'n  beetje 
          ophoud als je een normale Z80 op 3.58 Mhz gebruikt.
          Onder MSX-DOS is dit nog erger, als er een interrupt optreed 
          wordt  er naar  &H0038 gesprongen, vanuit hier wordt er weer 
          ergens anders  hoog in  het geheugen  gesprongen, vanwaaruit 
          een  interslot call  gedaan wordt  naar de BIOS ROM op adres 
          &H0038. Dit alles verziekt de boel zo, dat het ontvangen van 
          data op hoge snelheid haast onmogenlijk wordt gemaakt.
          Dit is  onder MSX-DOS  aanzienlijk te versnellen door in het 
          RAM  deze jump  aan te passen, zodat er als er een interrupt 
          optreed, de  driver het  eerst aan  bod komt.  Ook het ik de 
          driver  zo  gemaakt,  dat als  de interrupt  voor de  driver 
          bestemd  was,  deze  dan niet  meer door  het BIOS  gebruikt 
          bekeken  wordt, of eventueel het BIOS niet meer aan te laten 
          roepen,  om  zo  de  maximale  snelheid  eruit te  halen. De 
          methode van  interrupt gebruik is in te stellen, dit om zelf 
          te  kunnen kiezen  voor keyboard gebruiken of niet, want dat 
          is wel de consequentie hiervan.
          
          (vervolg, zie text 2)                      
          
          
          
                         E R I X   D R I V E R   2 / 2 
                                                        
          
          
                           S P E C I F I C A T I E S 
          
          Controleren op aanwezigheid driver :
          
            Kijk of  er op  adres &HF3FC/&HF3FD "RS" staat, zo ja, dan 
            is de driver aanwezig, ander niet.
          
          Pointer naar jump tabel :
          
            Als  de driver  geinstalleerd is,  staat er  op &HF3FE een 
            pointer naar  de jump  tabel, deze  tabel is  momenteel 63 
            bytes lang. (21 jump entry's)
            Deze pointer heeft LSB op &HF3FE en het MSB op &HF3FF
          
          Functies :
          
          Offset  Functie       Uitleg
          +0      GetVersion    Geeft het versie nummer van de driver 
                                in HL terug. [H] = main version,
                                [L] = sub versie, beide packed BCD
          +3      Init          Initialiseert de functies van de UART, 
                                zorgt dat data ontvangen/verzonden kan 
                                worden.
          +6      DeInit        Zet de driver op non-actief
                                Dit moet gedaan worden als de driver 
                                niet meer gebruikt wordt, dit ivm. een 
                                vervelende eigenschap van de Philips 
                                NMS121x serie, die de UART niet reset 
                                als je de computer reset. Als deze 
                                functie dan niet gebruikt is, is het 
                                mogenlijk dat de computer gaat hangen 
                                als er data binnenkomt.
          +9      SetBaud       Stelt de baud-rate in
                                [H] = Transfer, [L] = Receive rate
                                Nadat de snelheid ingesteld is, komt 
                                de daadwerkelijk ingestelde baudrate 
                                weer terug in [H] en [L], dit om te 
                                kontroleren of die gekozen snelheid 
                                wel ingesteld kan worden.
                                De baud rate wordt aangegeven door een 
                                index, hieronder een lijstje...
          
                                Index     Baudrate
                                0         75
                                1         300
                                2         600
                                3         1200
                                4         2400
                                5         4800
                                6         9600
                                7         19200
                                8         38400
                                9         57600
                                10        76800
                                11        115200
                                Index 8 en hoger zijn niet bruikbaar 
                                op een SONY interface
          
          +12     Protocol      Stelt het protocol in waarmee de 
                                RS232C werkt
                                [H] Bit 0,1 is het aantal databits 
                                      00 = 5 bits
                                      01 = 6 bits
                                      10 = 7 bits
                                      11 = 8 bits
                                    Bit 2,3 is het aantal stopbits
                                      01 = 1   stopbit
                                      10 = 1.5 stopbit
                                      11 = 2   stopbits
                                    Bit 4,5 stelt de parity in
                                      00  = none
                                      01  = even
                                      11  = oneven
                                Bit 6,7 van [H] moeten op nul staan, 
                                en register [L] moet ook op nul staan, 
                                dit voor toekomstige uitbreidingen.
          +15     Channel       Stel kanaal [H] in
                                Als je een multi-channel interface 
                                hebt (zoals de NMS1211), kun je 
                                hiermee instellen welk kanaal je 
                                gebruikt.
                                De kanalen niet gebruikt worden met 
                                DeInit behandeld, en bij het kanaal 
                                wat je insteld wordt een Init 
                                uitgevoerd.
          +18     RS_In         Geef ontvangen data terug in [A]
                                De data wordt uit een ontvangstbuffer 
                                gelezen die momenteel 512 bytes groot 
                                is.
                                Als er geen data beschikbaar is staat 
                                is [A]=0, om te weten of er data is, 
                                kun je RS_In_Stat gebruiken.
          +21     RS_Out        Stuur [A] naar de RS232C
                                Deze routine keert pas terug als de 
                                data verzonden is, deze zal dus 
                                blijven wachten als de tegenpartij RTS 
                                laag heeft gemaakt!
                                Om te weten of je wel mag zenden kun 
                                je RS_Out_Stat gebruiken.
          +24     RS_In_Stat    Geeft in [A] terug of er data 
                                beschikbaar is of niet.
                                [A] =  0 Geen data in buffer
                                [A] <> 0 Data aanwezig in buffer
          +27     RS_Out_Stat   Geeft in [A] terug of er data 
                                verzonden mag worden.
                                [A] =  0 Data mag niet verzonden 
                                         worden
                                [A] <> 0 Data mag verzonden worden
          
          +30     DTR           Stelt DTR in (Data terminal ready)
                                [H] = 0   maak DTR laag
                                [H] = 255 maak DTR hoog
          +33     RTS           Stelt RTS in (Request to send)
                                [H] = 0   maak RTS laag
                                [H] = 255 maak RTS hoog
          +36     Carrier       Geeft de huidige carrier status terug
                                in [A]
                                [A] =  0 Geen carrier
                                [A] <> 0 Carrier aanwezig
          +39     Chars_In_Buf  Geeft in HL het aantal bytes dat in
                                de ontvangstbuffer zit
          +42     Size_Of_Buf   Geeft in HL de grootte van de
                                ontvangst buffer
          +45     FlushBuf      Maakt de ontvangstbuffer leeg
          +48     FastInt       Zorgt ervoor dat de hook op &H0038
                                afgebogen wordt, of hersteld wordt.
                                Deze routine controleerd zelf of de 
                                hook al of niet afgebogen is, om 
                                dubbel installeren/herstellen te 
                                voorkomen.
                                [H] = 0  Zorgt voor normale interrupt 
                                         verwerking
                                [H] = 1  Zorgt dat de interrupts veel 
                                         sneller afgehandeld worden.
                                Door eerst met [H]=0 FastInt aan te 
                                roepen, een andere int. handler op 
                                &H0038 te installeren, en uiteindelijk 
                                weer met [H]=1 FastInt aan te roepen, 
                                is het mogenlijk om nog een andere 
                                interrupt handler actief te hebben 
                                tegelijkertijd met de RS232C driver.
                                Deze methode kan, indien goed gedaan, 
                                enorm veel snelheidswinst opleveren.
                                Je kunt zo bv. een keyboard handler 
                                installeren, en het BIOS helemaal 
                                buiten spel zetten!
          +51     Hook38Stat    Stel in of normale BIOS interrupts (of
                                de oude interrupt handler die op
                                &H0038 stond) nog ondersteund dienen
                                te worden. (Om nog meer winst te
                                krijgen wat betreft processortijd)
                                Worden "normale" interrupt afgezet,
                                dan wordt er nog wel gezorgd dat
                                systeemvariabele JIFFY blijft lopen!
                                [H] =  0  "normale" interrupts
                                [H] <> 0  Alleen RS232C interrupts
                                Deze functie werkt alleen als FastInt
                                aangeroepen is met [H]=1
          +54     ChPut_Hook    Stelt in of schermuitvoer via ChPut
                                ook naar de RS232C verzonden dient te
                                worden.
                                [H] = 0  Geen uitvoer naar RS232C
                                [H] = 1  Alle uitvoer naar RS232C
                                [H] = 2  Zie 1, maar nu zonder deze
                                         text op het scherm te zien.
                                         Levert snelheidswinst op.
          +57     Keyb_Hook     Stelt in of binnenkomende data naar de
                                ontvangstbuffer moet, of in de
                                keyboard buffer moet worden gezet.
                                [H] = 0  Data naar ontvangstbuffer
                                [H] = 1  Data naar keyboard
          +60     Get_Info      Geeft in HL een pointer naar een tabel
                                met informatie over de driver.
          
          DRIVER INFO BLOCK
          
            Offset    Bytes     Description
            +0    2   Versie nummer +1 bevat het hoofd nummer
                      +0 bevat een sub-versie, allebij Packed BCD
            +2    1   Huidige ontvangs snelheid (index)
            +3    1   Huidige zend snelheid (index)
            +4    1   Huidig protocol
                      bit 0-1 Data Bits
                                00 5 bits
                                01 6 bits
                                10 7 bits
                                11 8 bits
                          2-3 Stop Bits
                                01 1 stopbit
                                10 1.5 stopbits
                                11 2 stopbits
                          4-5 Parity
                                00 geen
                                01 even
                                11 oneven
                          6-7 0 niet gedefinieerd
            +5    1   ChPut_Hook status
            +6    1   Keyboard_Hook status
            +7    1   Huidige RTS status
            +8    1   Huidige DTR status
            +9    1   Huidig kanaal  
            +10   1   Hardware info
                      0 = Geen informatie
                      1 = ASCII monochannel compatible
                      2 = NMS121x interface (multichannel)
                      3!= MT-Telcom, aangepast voor RS232C
                      4!= NMS1250, aangepast voor RS232C
                      5!= Gradiente interface (brazilie)
                      6!= Multichannel ASCII compatible
                      Versies met een "!" zijn nog niet gemaakt, maar 
                      er is een kans dat dezen er nog zullen komen.
                      
          
                     L A A T S T E   O P M E R K I N G E N 
          
          Deze driver is nog niet compleet uitontwikkeld, dus er 
          zullen nog wel uitbreidingen komen. Deze uitbreidingen 
          zullen compatible blijven met oudere versies van de driver.
          Ik ben bezig met een terminal te schrijven voor deze driver, 
          genaamd ERIX, deze heeft minimaal een MSX2 nodig met 64 Kb, 
          en minimaal MSX-DOS1. ERIX ondersteund Xmodem(1k) en Ymodem, 
          zowel up/download. De schermuitvoer is behoorlijk snel, en 
          emuleert momenteel VT-52, ANSI wordt nog gemaakt.
          De driver heeft geen bezwaar tegen een turbo-R op R800-Dram 
          mode, maar werkt natuurlijk ook goed op een 3.58 Mhz Z80.
          Om steeds de nieuwste versie van ERIX en de drivers te 
          bemachtigen, kun je naar The Games BBS (04120-40358) bellen, 
          en daar de nieuwste versies te zoeken.
          Als je nog aan- en/of opmerkingen hebt, laat dan een 
          berichtje voor me achter in MSX-net (inlogpunt bv. 
          RoefSoft), of stuur een berichtje naar me in The Games BBS.
          Gewoon onder de naam :
                                                             Erik Maas
