                  N O G M A A L S   B D O S   R O U T I N E S 
          
              V O O R   P R E T T I G E   F I L E H A N D L I N G 
                                                                   
          
          Op Sunrise  Special #4 staan twee artikelen met bijbehorende 
          listing  van mijn  hand over interfacing met de BDOS en disk 
          foutafhandeling onder  DOS1. Geen  commentaar was mijn deel. 
          Ik bedoel dat niemand die op dat moment even wat beter op de 
          hoogte  was dan  mijzelf zich goed genoeg voelde om mij mede 
          te delen dat er wat gevaren aan de routines kleven die ik in 
          de  artikelen  niet  genoemd  heb.  Ik  doel daarbij  op het 
          wisselen van  diskettes tijdens  het lezen  en schrijven van 
          komplete   files.  Lees   ook  het   artikel  op  de  vorige 
          submenu-optie.
          
          Ik heb de eerder gepubliceerde routines herschreven. Dat heb
          ik gedaan om de error handler te debuggen en omdat ik  tegen
          de beperkingen  van  de  routines  aangelopen  ben.  Om  het
          geheugen even op te frissen:
          
          - Er kon maar ��n file tegelijkertijd open zijn
          - De maximale handelbare filelengte was 64 kB
          
          Beide beperkingen zijn voor demo's en kleine utilities  niet
          erg vervelend, maar  ik  liep  er  tegen  aan  toen  ik  wat
          ingewikkelde file-akties moest uitvoeren op  meerdere  files
          tegelijkertijd. Het was dus tijd om het  nog  eens  over  te
          doen waarbij  beide  beperkingen  tot  het  verleden  zouden
          horen.
          
          Ik heb  mij  bij  de  ontwikkeling  van  de  routines  laten
          inspireren  door  de  ANSI-C  standaard.  De  namen  van  de
          routines  kloppen  niet  met   deze   standaard,   maar   de
          mogelijkheden komen redelijk overeen.
          
          Het doel van de routines is om 'dom' lees en schrijfwerk van
          en naar diskettes,  harddisks en wat je nog maar  meer  kunt
          verzinnen wat eenvoudiger te maken. Op zich is het interface
          van de BDOS naar de gebuiker goed (ook al is het al  zo  oud
          dat Fred Flintstone het zich nog goed kan herinneren),  maar
          tijdens praktisch gebruik stoot je toch  op  wat  zaken  die
          extra aandacht verdienen.
          
          Probeer je bv. een file van 1234 bytes in te lezen door  een 
          random access block read van &H4000 bytes te doen  dan  gaat 
          dat wel goed, maar de call komt terug met de BDOS error code 
          A = 1. Wat moet je daar nu mee? Is er wat fout gegaan of is
          dit juist  een teken  dat het  goed ging?  Als je er bij een 
          blockread  voor zorgt  dat er  niet voorbij  het End Of File 
          wordt gelezen,  dan krijg  je de  foutmelding ook niet en is 
          het 'probleem' opgelost. Zo zijn er nog veel meer dingen die 
          door   wat  simpele  testjes  te  doen,  het  leven  van  de 
          programmeur wat aangenamer kunnen maken.
          
          "Heeft die  sukkel nu eindelijk DOS2 al eens ontdekt?", hoor 
          ik  je zeggen.  Welnu, ik ben een waanzinnig voorstander van 
          DOS2, maar  ik vind het toch wat ver gaan om bij bv. spellen 
          en  demo's (die  niet uitsluitend  voor de  turbo R zijn) te 
          gaan eisen  dat je  DOS2 moet hebben. Daarom werken ook deze 
          routines   nog  gewoon   met  FCB's  en  zijn  file-handles, 
          environment strings en sub-directories niet ge�mplementeerd. 
          Dat wil  natuurlijk niet zeggen dat deze routines niet onder 
          DOS2 draaien.
          
          
                             D E   F U N K T I E S 
          
          Ik noem hier nu eerst even de funkties met hun aanroep  etc.
          waarna ik wat dieper op dit nieuwe ontwerp zal ingaan.
          
          ; BDOS fileshell routines for secure file loading and saving
          ; File: BDOSR2.ASM  V3.00
          ; By: Savage '94 (Fuzzy Logic)  Last update: 29-07-94
          
          ENASLT: EQU   &H24
          PRINTC: EQU   &HA2
          F##ERR: EQU   &HF323 ; Double pointer to error routine
          BDOS:   EQU   &HF37D
          VDPBF1: EQU   &HF3DF
          VDPBF2: EQU   &HFFE7-8
          
          ; Jumpvectors
                  JP    FINSTL
                  JP    FRESET
                  JP    FOPEN
                  JP    FBLKRD
                  JP    FCREAT
                  JP    FBLKWR
                  JP    FCLOSE
                  JP    FSEEK
                  JP    GETDIR
                  JP    CHNDSK
                  JP    DRVSTP
          
          ; Install BDOS fileshell
          ; In : A  (See BIOS rout 24H)
          ;      A must contain the settings for PAGE 1 (SLOT/SUBSLOT)
          ; Out: Cy, on error
          FINSTL:
          
          Deze routine moet worden aangeroepen voordat van  de  andere
          routines gebruik gemaakt kan worden. In tegenstelling tot de
          oude situatie wordt de BDOS entry nu  niet  meer  afgebogen.
          Het aanroepen van FINSTL: heeft verder geen gevolgen voor de
          BDOS en andere  routines  zullen  dus  geen  last  van  deze
          installatie hebben.
          
          Bij  invoer  moeten  in  register  A  de  slot  en   subslot
          instellingen van  PAGE  1  staan.  Dit  is  nodig  omdat  de
          DISK-ROM tijdens de error handler vrolijk op PAGE  1  aktief
          staat en die moet door  de  error  handler  weer  verwijderd
          worden.
          
          
          ; Reset BDOS fileshell
          ; Out: Cy, on error
          FRESET:
          
          Deze routine moet als laatste aangeroepen worden voordat met
          iets anders begonnen wordt. In tegenstelling tot bij de oude
          situatie is het gebruik van de BDOS door andere  programma's
          die niet van deze routines gebruik maken tussen een  aanroep
          naar  FINSTL:  en  FRESET:  niet  langer   aan   beperkingen
          gebonden. In de oude situatie werd een eventueel openstaande
          file nog even gesloten. Nu wordt dat niet meer gedaan,  maar
          wordt alleen een waarschuwing gegeven  dat  er  nog  file(s)
          open staan.
          
          
          ;Set new 'disk error' handler and 'change disk' handler
          INIERR:
          
          Deze routine wordt door de diskroutines aangeroepen voor  ze
          daadwerkelijk fysiek met de  diskdrive  aan  de  slag  gaan.
          Binnen deze funktie worden  de  BDOS  error  handler  en  de
          'change drive' hook van de BDOS  afgebogen  naar  een  eigen
          routine. Nu hoor ik mensen al weer klagen dat dit  een  call
          naar de BDOS alleen maar trager maakt en die  mensen  hebben
          daarin wel gelijk. Deze vertraging is echter  zo  onmeetbaar
          klein dat het de moeite van de overpeinzing niet waard is.
          
          
          ;Reset disk handlers
          EXIERR:
          
          Deze routine wordt na een fysieke  aktie  met  de  diskdrive
          aangeroepen om de oude situatie van de BDOS error handler en
          drive hook te herstellen. Zodoende  zijn  deze  entries  bv.
          tussen een aanroep naar FOPEN: en FBLKRD:  niet  naar  eigen
          routines afgebogen.
          
          
          ; Open a file
          ; In : IX, Pointer to FCB
          ;      HL, Pointer to filenamestring (11 or 13 bytes long)
          ; Out: Cy, on error
          FOPEN:
          
          Bij aanroep van FOPEN: en FCREAT: hoeft  van  het  37  bytes
          lange FCB bij invoer niets ingevuld te zijn. In assembly kan
          dit FCB dus als volgt gedefinieerd worden.
          
          FCB:   DS   37
          
          In de filenamestring van 11 of 13 bytes  staat  de  filenaam
          van de file waar de aktie  op  uitgevoerd  moet  worden.  In
          assembly kan deze eenvoudig als volgt gedefinieerd worden.
          
          FLNAM1: DM   "filename","ext"
          FLNAM2: DM   "A:filename","ext"
          FLNAM2: DM   "b:FiLeNaMe","ExT"
          
          In het eerste geval wordt de huidige drive (0)  in  het  FCB
          ingevult worden. In het tweede geval wordt  specifiek  drive
          A: (1) in het FCB ingevuld en dat geldt ook voor B: (2). Een
          ongeldige driveletter resulteert in een "File not found"  of
          "Cannot create" error bij resp. FOPEN: en FCREAT: Het  maakt
          niet uit of  de  string  in  kleine  of  hoofdletters  wordt
          ingevuld.
          
          
          ; Read a block from the current fileposition
          ; In : IX, Pointer to valid FCB
          ;      DE, Blocklength
          ;      HL, Loadaddress (not between 0x4000 and 0x7FFF)
          ; Out: Cy, on physical load error
          ;      else, HL = the number of bytes actually read
          FBLKRD:
          
          FBLKRD: zet zelf het DMA adres en controleert daarna  of  de
          blokgrootte wel kan, door deze te vergelijken met de afstand
          tussen de filelengte en het random record nummer. Indien het
          blok niet past wordt de blokgrootte automatisch aangepast.
          
          
          ; Create a file
          ; In : IX, Pointer to FCB
          ;      HL, Pointer to filenamestring (11 or 13 bytes)
          ; Out: Cy, on error
          FCREAT:
          
          Zie ook FOPEN:
          
          
          ; Write fileblock
          ; In : IX, Pointer to valid FCB
          ;      DE, Blocklength
          ;      HL, Startaddress
          ; Out: Cy, on error (File will be closed and deleted)
          FBLKWR:
          
          De enige situatie waarin deze routine met een Cy terug  komt
          is als de gebruiker een (A)bort heeft  gegeven,  of  als  de
          disk vol is.
          
          
          ; Close file
          ; In : IX, Pointer to FCB
          ; Out: -
          FCLOSE:
          
          Alleen  files  waarin  geschreven  is  MOETEN  met   FCLOSE:
          afgesloten te worden.
          
          
          ; Fseek places the fileptr (random record number) on a
          ; specified position
          ; In : IX, Pointer to a valid FCB
          ;      HLDE, 32-bit offset value (HL = HIword)
          ;      A, 0 = Seek from the beginning of the file
          ;         1 = Seek from the current position (curr_pos +
          ,                                             offset)
          ;         2 = Seek from EOF (EOF - offset)
          ; Out: Cy, if the new offset is beyond EOF or A has an
          ;          invalid value
          FSEEK:
          
          FSEEK: controleert of de opgegeven offset wel binnen de file
          ligt en verandert (indien  dit  het  geval  is)  het  random
          record number in het opgegeven FCB. De routine behandelt  de
          ingevoerde offset altijd als een positief  getal.  Negatieve
          offsets leveren dus (meestal) een error op.
          
          
          ; Read dir from disk
          ; In : IX, Pointer to FCB
          ;      DE, Buffer for dir. (112*32+1 bytes)
          ;      HL, Directory search mask address
          ; Out: Cy, HI then no files found
          GETDIR:
          
          Vanaf register DE zal  de  directory  van  de  disk  in  het
          geheugen staan. Elke entry is 32 bytes lang en het einde kan
          gevonden worden door te checken op een filenaam die  met  de
          waarde 255 begint.
          
          
          ; CHNDSK Set new drive
          ;        Request for same drive as current one is ignored !!
          ; In: A, drive number (0=A: 1=B: etc.)
          CHNDSK:
          
          Zie ook de verklarende tekst verderop.
          
          
          ; Stop the diskdrive motor
          DRVSTP:
          
          Zie ook de verklarende tekst verderop
          
          
          ; BDOS error handler [system routine]
          ; In: nothing
          ; Out: nothing
          DSERPT: DW    ERRCDE          ;Pointer to error code
          ERRCDE:
          
          
          ; Standard disk error handler
          ; Request Abort or Retry input from the user
          ABORET:
          
          ; 5 byte disk error hook
          ; Out: A, "a" then abort disk action
          ;         "r" then retry last disk action
          ; Remark: This hook can be used to include a customized
          ;         error handler
          H.DERR: JP    ABORET
                  DW    0
          
          
          ; Change disk query (hooked on &HF24F)
          ; [BDOS system routine]
          ; In : A, driveletter (in ASCII)
          CHDSKQ:
          
          Deze routine moet het terugkeeradres van de aanroepende CALL
          van de stack POPpen (de tweede POP AF in de listing)  om  te
          voorkomen dat de BDOS ook nog eens een tekst print.
          
           - Deze tekst wordt vervolgd in de volgende submenu-optie -
          
          
          
                  - Dit is het tweede gedeelte van de tekst -
          
                            V E R V O L G   B D O S 
                                                     
          
                         D E   A A N P A S S I N G E N 
          
          Een hele belangrijke aanpassing vind ik  het  feit  dat  het
          BDOS  entry  point  of  &HF3DF  niet  langer  direkt   wordt
          afgetapt. Hierdoor zullen andere routines die niet  van  het
          bestaan van deze routines  afweten  er  ook  geen  last  van
          hebben.  Het  lijkt  mij  dat  deze  aanpassing  vooral   de
          betrouwbaarheid verhoogt. Daarnaast  worden  de  BDOS  error
          handler en de 'change drive' hook per funktie  ge�nstalleerd
          en aan het eind weer verwijderd. Samengevat is het hele BDOS
          systeem dus alleen afgetapt tijdens de executie van ��n  van
          deze routines. Dit is erg belangrijk als je  tussendoor  nog
          andere programma's hebt die ook van de BDOS  gebruik  maken,
          maar niet van deze routines.
          
          
                              L O S S E   F C B S 
          
          De andere belangrijke aanpassing is  het  feit  dat  nu  een
          eigen FCB aan de routines moet worden meegegeven. Dat  lijkt
          op het eerste gezicht een  nadeel  (meer  invoerparameters),
          maar het betekent wel dat er nu eenvoudig met meerdere files
          tegelijkertijd gewerkt kan worden.
          
          
                      M A X   F I L E L E N G T E   4 G B 
          
          Het feit dat de max. filelengte  waar  nu  mee  gewerkt  kan
          worden 4GB is, lijkt op het eerste  gezicht  overbodig.  Dat
          verandert echter op het moment dat je n�t eventjes wat  meer
          dan 64kB wilt saven. De oude routines laten je dan  hopeloos
          in de steek.
          
          
                      R A N D O M   F I L E   A C C E S S 
          
          Random file access is HEEL grappig. Wat sommige mensen  niet
          weten is dat random access op files niet alleen betekent dat
          je zomaar overal binnen de bestaande file mag  beginnen  met
          lezen of schrijven, maar dat er op een  random  access  file
          zowel lees als schrijfakties uitgevoerd mogen  worden.  Open
          bv,  een  bestaande  file  met  FOPEN:  en  niemand  zal  je
          weerhouden om als eerste aktie met FBLKWR: een blok geheugen
          in deze file te dumpen. Volkomen legaal, erg grappig en  (op
          PC's althans) een veel gebruikte methode om een harddisk als
          virtueel geheugen te gebruiken.
          
          
                       D E   R O U T I N E   F S E E K : 
          
          FSEEK: is een routine die in  ANSI-C  ongewijzigd  hetzelfde
          is. Het maakt het verplaatsen van het Random  Record  Number
          (ook  wel  filepointer  genoemd)  even  eenvoudig  als   het
          wijzigen van het DMA adres. Het nut blijkt bij een nog  door
          mij uit te brengen cruncher waarbij  ik  nadat  de  komplete
          file gecrunched en weggeschreven is, nog een header aan  het
          begin van de file moet invullen  waar  al  wel  ruimte  voor
          gereserveerd is.
          
          
                      D E   R O U T I N E   C H N D S K : 
          
          De  routine  CHNDSK:  in  de  oude  routines   is   volledig
          incompatible met de BDOS. Dat betekent  dat  een  diskwissel
          met deze routine niet aan de BDOS wordt doorgegeven en  vice
          versa. Nu is CHNDSK gewoon een aanroep van BDOS  funktie  14
          en werkt het wel goed.
          
          Hierbij moet opgemerkt worden dat  deze  BDOS  funktie  niet
          direkt bij aanroep om een  nieuwe  drivenummer  vraagt.  Dat
          gebeurt pas bij de volgende aktie op die drive. Let hier een
          beetje op i.v.m. spellen waar je niet te  pas  en  te  onpas
          change disk meldingen wilt hebben. Je kunt het dan beter  op
          de oude manier doen waarbij  het  drivenummer  in  de  FCB's
          altijd 0 is.
          
          Vb:
          
          ; Stel de huidige drive is A:
                    LD   A,(&HFCC1)     ;BASIC ROM aktief
                    CALL FINSTL
          
                    LD   IX,FCB1
                    LD   HL,FLNAM1
                    CALL FOPEN          ;Open TEST.DAT op A:
                    JP   C,FRESET       ;Sukkel voor de computer !!
          
                    LD   A,1            ;Drive B: !!! (0 = A: etc.)
                    CALL CHNDSK
          
                    LD   IX,FCB2
                    LD   HL,FLNAM2
                    CALL FCREAT         ;Create TEST.CPY op B:
                    JP   C,FRESET       ;Nog meer sukkels
                    CALL FCLOSE
                    JP   FRESET         ;Wat een enorme file nietwaar?
          
          FCB1:     DS   37             ;File op drive A:
          FCB2:     DS   37             ;File op drive B:
          
          FLNAM1:   DM   "a:test    ","dat"
          FLNAM2:   DM   "b:test    ","cpy"
          
          De call naar CHNDSK: heeft niet tot gevolg  dat  de  melding
          "Insert disk for drive ..." in beeld komt. Dat  gebeurt  pas
          binnen FCREAT:.  Wordt  dit  'programma'  twee  keer  achter
          elkaar opgestart dan wordt de tweede keer eerst om drive  A:
          gevraagd omdat tijdens het verlaten van het programma  drive
          B: nog ingesteld is.
          
          Deze manier van drivewisseling  is  echter  erg  leuk  omdat
          mensen met twee drives geen melding  op  hun  scherm  zullen
          krijgen omdat er gewoon  van  twee  drives  gebruik  gemaakt
          wordt. Deze routine is eenvoudig uit te breiden tot een (te)
          eenvoudig kopieerprogramma. Doordat de BDOS nu  konstant  op
          de hoogte is van diskwisselingen, mag WEL van disk gewisseld
          worden tijdens lees- en schrijfakties. De  totale  leesaktie
          moet natuurlijk wel op dezelfde disk  uitgevoerd  worden  en
          dat laatste geldt natuurlijk ook voor de schrijfaktie.
          
          
                      D E   R O U T I N E   D R V S T P : 
          
          DRVSTP: laat de drivemotor stoppen.  Hier  zijn  in  theorie
          meerdere manieren voor. Ikzelf heb lange tijd de entry in de
          BDOS zelf gebruikt omdat die onmiddellijk resultaat oplevert
          en relatief snel is. Meneer Stefan  Boer  deelde  me  laatst
          echter mede dat dit niet op alle MSX'en werkt  (zucht,  daar
          gaan we weer).  Het  blijkt  dat  bepaalde  knutselaars  een
          afwijkende DISK-ROM hebben waarbij die entry  op  een  ander
          adres terecht is gekomen. Ik ben nog nooit zo'n  ding  tegen
          het lijf gelopen, maar gezien het feit dat  veiligheid  voor
          alles gaat heb ik de routine dus maar weggeknikkerd.
          
          De vervanging die simpelweg 255 keer &HFD9F aanroept  is  de
          oudste oplossing om  de  drivemotor  te  laten  stoppen.  De
          andere methode waarbij eerst de 'motor delay counter'  op  1
          wordt gezet waarna eenmalig &HFD9F wordt aangeroepen  blijkt
          volgens sommige bronnen ook niet altijd te werken en dus heb
          ik ook die voorlopig overboord gezet.
          
          Er  is  mij  nu  wel  op  het  hart  gedrukt   dat   de   nu
          ge�mplementeerde routine de beste is omdat hij op ALLE  MSX-
          en werkt, maar ik heb um op mijn  SONY  eens  aan  het  werk
          gezet. Disassemblering van de routine op  &HFD9F  leert  mij
          dat  er  op  een  SONY  helemaal  geen  drive   stop   wordt
          aangeroepen en dat de drive volledig automatisch stopt  (dat
          was echter geen  nieuws).  Op  een  SONY  is  het  255  keer
          aanroepen  van  &HFD9F  dus  niets  meer   dan   een   domme
          vertraging. Kan iemand mij dan misschien vertellen wat er in
          hemelsnaam op een Philips  of  turbo R  gebeurt,  want  deze
          drivestop  lijkt me  een beetje  onzinnig. [Op  een turbo  R 
          stopt  de  drive  ook  automatisch,  alleen  op een  (!#$%&) 
          Philips is het �berhaupt nodig om de drive stop te zetten.]
          
          
                   I N T E R R U P T S   A A N   E N   U I T 
          
          Tijdens een BDOS aanroep  bij  de  oude  routines  wordt  de
          V_BLANK interrupt volledig uitgezet. Dat  was  innertijd  om
          mensen die nog steeds een trage DISK-ROM in hun SONY  hebben
          van enige snelheid te voorzien. Nu zit dit er niet  meer  in
          om interruptproblemen bij bv. een aanroep naar BIOS  funktie
          &H9F te voorkomen.
          
          
                               T E N S L O T T E 
          
          Ik heb deze routines al redelijk stevig getest en ik heb tot
          op  dit  moment  nog  geen   gekke   onverklaarbare   dingen
          meegemaakt. Het aantal vernielde diskjes is ook  0,  en  dat
          blijft  hopelijk  ook   zo,   zolang   een   komplete   file
          schrijfaktie maar op ��n en dezelfde disk wordt  uitgevoerd.
          Diskwisselingen die hier tussendoor komen mogen dus wel  als
          je gebruik maakt van de 'change drive' routine omdat de BDOS
          er dan rekening mee houdt.
          
          Ik ben er persoonlijk wel happy mee. Beperkingen  worden  er
          niet meer opgelegd en andere routines die wel  van  de  BDOS
          gebruik maken,  maar  niet  van  deze  routines,  hebben  er
          ab-so-luut geen last van. Het is zelfs zo  dat  er  met  bv.
          FBLKRD: gelezen kan worden uit een file die niet met  FOPEN:
          geopend is. Dat alles in combinatie met een zeer  eenvoudige
          BDOS interfacing voor het lezen en schrijven van files maakt
          deze routines voor mij in ieder geval onmisbaar.
          
                                                     Alex van der Wal
