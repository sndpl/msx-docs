                             G E T D I S K   2 . 0 
                                                    
          
          Op  deze  Sunrise  Special staan  de nieuwe  versies van  de 
          harddisk  utility's die op de vorige SRS hebben gestaan. Met 
          deze  programma's  kan  een  spel  dat  een (of  meer!) hele 
          diskette(s)  beslaat  als  een  file op  de harddisk  worden 
          opgeslagen en worden opgestart.
          
          
                            A A N P A S S I N G E N 
          
          Ik  heb  de  programma's  geheel herschreven  en nu  gebruik 
          gemaakt  van  DOS2  system  calls. Hierdoor  is de  snelheid 
          verbeterd,  maar  het werkt  nu natuurlijk  niet meer  onder 
          DOS1, maar  harddiskgebruikers onder  DOS1 kunnen natuurlijk 
          nog  gewoon  de  oude  versie  blijven  gebruiken.  Maar een 
          harddisk  onder  DOS1  is als  een normaal  mens achter  een 
          Amiga; er zullen wel niet veel DOS1 HD gebruikers meer zijn.
          
          
                   V E R D E R E   V E R A N D E R I N G E N 
          
          Het  gedeelte dat  de diskette inleest en op de harddisk zet 
          is  nu  gecombineerd  tot  de  COM-file  GETDISK.COM  i.p.v. 
          GETDISK.BAS en GETDISK.BIN.
          
          Bij het  opstarten van  GETDISK.COM wordt  er eerst gevraagd 
          van  welke drive  de diskette  ingelezen moet worden. Daarna 
          wordt om  het aantal diskettes gevraagd en als laatste onder 
          welke naam de file bewaard moet worden.
          
          Bij  zowel  GETDISK.COM  als START.COM  kan er  nu een  path 
          worden  opgegeven. START.COM  handelt nu de phydio BIOS call 
          beter af, waardoor meer spellen onder DOS2 draaien.
          
          Als  laatste is  er een  programma ADAPT.COM bij gekomen dat 
          een bestand  dat al is opgeslagen met GETDISK.COM langsloopt 
          om   te  kijken   of  de  diskrom  soms  rechtstreeks  wordt 
          aangeroepen  en   zonodig  deze   calls  verandert.  Gebruik 
          ADAPT.COM  alleen als  een spel niet met START.COM opgestart 
          kan worden, zoals bij de spellen van Bit2.
          
          
                     A L L E S   O P   E E N   R I J T J E 
          
          - Start  het programma GETDISK.COM op (zonder parameters) en 
            geef op welke driveletter de diskdrive heeft.
          - Tel nu hoeveel diskettes het spel is dat op harddisk gezet 
            moet   worden  (vergeet  niet  de  evt.  userdisk  mee  te 
            rekenen).
          - Geef dit  aantal op  en geef  de naam  (met evt.  drive en 
            path) op waaronder het spel bewaard moet worden.
          - Probeer    het   spel    op   te    starten   met    START 
            drive:\path\filename.ext.
          - Loopt  de  computer  vast,  reset  dan  en  probeer  ADAPT 
            drive:\path\filename.ext
          - Start het spel nog een keer op met START.COM
          - Werkt het  nu nog  steeds niet,  dan kan  het spel  helaas 
            (nog) niet op de harddisk.
          
          
                              D E   S P E L L E N 
          
          De spellen in dit lijstje werken met GETDISK. Als je nog een 
          spel  gevonden hebt,  kun je dat laten weten via Sunrise BBS 
          Nuth of de postbus, t.a.v. Kasper Souren.
          
          Compile:        Aleste Special
                          Golvellius 2
                          Gorby's Pipeline
                          Nyanpi
                          Puyo Puyo
                          Randar 3
                          Rune Master 1
                          Rune Master 2
                          Rune Master 3
                          Supercooks
          Konami:         F1-Spirit 3D
          Momonoki house: Peach Up summary 2
                          Peach Up 8
          DBP:            Tetris
          Falcom:         Ys 3
          Bit2:           Quinpl (ADAPT.COM gebruiken)
                          Nyancle (ADAPT)
          Wolfteam:       Niko Niko
          ?:              Herzog
          ?:              Haradius
          ?:              Pacmania
          
          
          Het  moge  duidelijk zijn  dat al  deze spellen  onbeveiligd 
          zijn. Dat  wil zeggen:  ze moeten  met FastCopy  te kopi�ren 
          zijn door gewoon track 0 t/m 79 te kopi�ren.
          
          Van  Compile  werkt  tot  nu  toe alleen  Aleste 2  niet. Ik 
          vermoed  dat ook  alle DiscStations  en Randar  1 en  2 goed 
          zullen functioneren.
          
          Ik vermoed dat Ys 1 en Ys 2 ook werken, maar dit kan ik niet 
          controleren omdat ik deze spellen niet bezit.
          
          Noot:  alleen spellen  die helemaal  op sector staan werken. 
          Spellen  die  niet alleen  op sector  staan, maar  ook files 
          laden, kunnen  niet met de huidige versie van GETDISK worden 
          gebruikt. Hierdoor vallen helaas alle MicroCabins weg.
          
          
                                    PUTDISK
          
          Op deze  Special staat  ook PUTDISK.COM.  Hiermee kun je een 
          spel  weer terug  op diskettes zetten. Handig als je even de 
          userdisk nodig hebt om ergens anders te spelen.
          
          
                                      R800
          
          Spellen die  de MSX-MUSIC chip rechtstreeks aansturen zullen 
          geen  rekening hebben  gehouden met  de R800.  Daarom is het 
          geluid  verminkt  als het  spel onder  R800 gedraaid  wordt. 
          Spellen die  alleen de  PSG gebruiken  (Ys 3, Herzog) werken 
          wel  goed. Ook  zijn er  spellen die  de FM  BIOS gebruiken, 
          bijv. Pacmania, en die werken dan wel goed.
          
                                                       Michel Shuqair
                                                        Kasper Souren
