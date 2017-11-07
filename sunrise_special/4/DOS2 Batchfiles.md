                         D O S 2   B A T C H F I L E S 
                                                        
          
          Op  Sunrise  Special  #2  heeft  al eens  een tekst  over de 
          mogelijkheden  van MSX-DOS  2.31 gestaan. Ook stond op SRS#2 
          een tekst  over pipelining en redirectioning. Ik zal in deze 
          tekst   echter  wat   praktijkvoorbeelden  ervan  geven:  de 
          batchfiles die op mijn HD staan.
          
          Eigenlijk is dit alleen interessant voor HD-gebruikers, want 
          batchfiles  nemen toch minstens 1 kB in van een diskette, en 
          het  kost  weer  extra  laadtijd. Op  een harddisk  maakt de 
          diskruimte niet bar veel uit, en de laadtijd is vrij kort.
          
          
                            D R I E   S O O R T E N 
          
          Je kunt  batchfiles opdelen in drie soorten. De eerste zorgt 
          ervoor  dat een  aantal commando's gecombineerd worden zodat 
          er minder  hoeft te worden getikt. De tweede soort zorgt ook 
          voor  minder typewerk, maar heeft als doel het opstarten van 
          een programma,  en eventueel nog iets doen na het be�indigen 
          ervan. En de derde soort is gewoon diversen.
          
          Dit  opdelen in  soorten is  eigenlijk flauwekul,  omdat het 
          natuurlijk op  heel veel  manier kan, maar ik heb het gedaan 
          om  toch  een  beetje  een  overzichtelijke tekst  te kunnen 
          typen.
          
          
                                 F O R M A A T 
          
          Ik houd  zelf het volgende aan: namen van files, directory's 
          of  drives met  hoofdletters en  interne DOS  commando's met 
          kleine letters.  Maar in deze tekst type ik alles gewoon met 
          hoofdletters, dit voor de duidelijkheid.
          
          
                            E E R S T E   S O O R T 
          
          Ik zal  gewoon een  aantal batchfiles  bespreken. Deze staan 
          ook op deze disk onder dezelfde naam.
          
          
                  D M O V E . B A T 
          
                  COPY %1 %2
                  ECHO Y|DEL %1>NUL
          
          MOVE kan  normaal alleen  files op ��n disk verplaatsen, dus 
          van de ene dir naar de andere directory. MOVE verplaatst dan 
          ook  alleen  de  directory-entry. Ik  verplaats echter  vaak 
          files  van de  ene drive  naar de andere. Dat kost normaal 2 
          regels typewerk.  Met DMOVE  wordt dit  gereduceerd tot  ��n 
          regel.
          
          %1  is de eerste parameter die achter de batchfile staat. %2 
          de tweede,  enz. %0  is trouwens  de ECHTE eerste parameter. 
          Hier zal ik later ook nog iets mee doen.
          
          COPY  %1  %2  zorgt dus  gewoon dat  er een  file gekopieerd 
          wordt. ECHO Y|DEL %1>NUL betekent dit:
          
          ECHO Y|  stuur het teken y naar de input van het commando 
                   dat erachter staat (pipelining).
          
          DEL %1   als  de vraag  "Erase all files (Y/N)? " komt, zal 
                   hier automatisch een Y worden gegeven.
          
          >NUL     stuur de output van de voorgaande commando's naar 
                   het NUL device. Dit houdt in dat er niks op het 
                   schemr komt.
          
          
                   M O V D I R 
          
                   MD %2
                   MOVE %1 %2
                   RD %1
          
          Deze  batchfile  gebruik  ik om  snel een  directory in  een 
          andere  directory te  plaatsen. Als je bijvoorbeeld eerst de 
          subdirectory TSRS  in de rootdir hebt staan, en je wilt deze 
          verplaatsen naar de subdir MEMMAN, doe je het volgende:
          
                   MOVDIR \TSRS \MEMMAN\TSRS
          
          Nu  wordt eerst  de directory TSRS aangemaakt in MEMMAN, dan 
          worden alle  files gemoved  van \TSRS  naar \MEMMAN\TSRS  en 
          tenslotte wordt de directory \TSRS verwijderd.
          
          Eventueel  zou  dit ook  nog uitgebreid  kunnen worden  naar 
          DMOVDIR, die ik zelf echter niet op mijn HD heb staan, omdat 
          bij MOVE  de FAT  niet veranderd  wordt, en bij COPY wel. En 
          een  directory blijft  meestal toch op dezelfde drive staan. 
          Maar voor de volledigheid...
          
          
                   D M O V D I R 
          
                   MD %2
                   COPY %1 %2
                   ECHO Y|DEL %1>NUL
                   RD %1
          
          
                                D I V E R S E N 
          
          Eigenlijk  zou nu  natuurlijk de  tweede soort moeten komen. 
          Maar omdat  die files gebruiken die moeten worden aangewezen 
          met het environment item PATH en er zijn nog een paar andere 
          environment items nodig.
          
          
                   A U T O E X E C . B A T 
          
                   BUFFERS 18
          
          Zorgt  ervoor  dat  het aantal  buffers wordt  vergroot. Dit 
          staat  standaard  op  5.  In  de buffers  worden de  FAT- en 
          directorysectoren  bewaard, wat  de laadtijd  zeer ten goede 
          komt.
          
                   IF NOT %TIME%A==24A A:\UTILS\ST4TUNE
          
          ST4TUNE.COM is een programma dat Star Trek quotes laat zien. 
          Er wordt  telkens willekeurig ��n gekozen. Soms is het nodig 
          om  AUTOEXEC.BAT  nog  eens  op  te  starten,  bijv. als  de 
          environments  items  verneukt  zijn. Als  TIME dan  nog goed 
          staat,  wordt  ST4TUNE  niet gerund,  omdat het  toch ietwat 
          traag (CP/M) is.
          
                   PATH H:\UTILS; A:\; A:\BATCH; A:\UTILS
                   PATH +A:\UTILS\TEXT; A:\UTILS\CRUNCH; A:\UTILS\ML
                   PATH +A:\UTILS\TURBOR; A:\UTILS\DOS2TOOLS; A:\BASIC
          
          Een aantal  directory's in PATH zetten. BATCH moet uiteraard 
          VOOR  UTILS staan, omdat sommige batchfiles gelijknamige COM 
          files  gebruiken.  Bijv. PMARC.BAT;  zie verderop.  Ook moet 
          eerst op de RAMdisk worden gezocht.
          
                   ASSIGN F: G:
          
          Bij  mij is  E: de eerste echte diskdrive. Omdat ik maar ��n 
          diskdrive heb,  is F:  een virtuele  drive. Dat betekent dat 
          ik,  als ik  naar F:  vraag, de  mededeling "Insert  disk in 
          drive F:  and strike  a key  when ready" krijgt. Dat vind ik 
          niet  leuk, en  daarom laat  ik F:  naar G: wijzen. Omdat G: 
          niet  bestaat,  bestaat  F:  nu  ook  niet meer  voor lagere 
          systeemfuncties.
          
          Bij Disk I/O error wijst de computer overigens wel weer naar 
          de echte  drive. Als  je bijv.  ASSIGN C: A: hebt gedaan, je 
          gaat  naar C:  en er  is een  Disk I/O  error krijg  je toch 
          "drive A:" op het scherm.
          
          Met ASSIGN  zonder een  drive (of letter) erachter wordt het 
          trouwens weer default gezet.
          
                   SET TIME 24
          
          Ik ben  mijn horloge  gewend, en  heb liever  niet am  en pm 
          meldingen.
          
                   SET DATE DD-MM-YY
          
          De Nederlandse manier van data.
          
                   SET SHELL H:\UTILS
          
          COMMAND2.COM staat zo direct in H:\UTILS.
          
                   SET PROMPT ON
          
          Prompt aan. Dus niet "A>", maar "A:\>".
          
                   SET PATTERN C:\DIVERSEN\CHARSET
          
          Tja,  PATTERN.COM   is  een   programma  dat   nog  niet  is 
          vrijgegeven.  Ik denk  wel dat  het op  de volgende  Special 
          staat.
          
                   SET EXPERT ON
          
          Expert  mode  aan.  COM  files  die  van  DOS1  disks worden 
          opgestart werken gewoon.
          
                   SET TEMP H:\
          
          Tijdelijke bestanden (o.a. bij pipelining gebruikt) in H:\.
          
                   SET HELP C:\TEKST\HELP
                   SET KHELP %HELP%
          
          Japanse HELP (Kanji HELP) files hoef ik niet... Die zijn dus 
          hetzelfde als de Engels HELP files.
          
                   IF NOT A%MEMMAN%==AON SET MEMMAN OFF
          
          Het environment  MEMMAN wordt  op OFF gezet als het niet aan 
          stond. MEMMAN wordt ON gezet door MEMMAN.BAT.
          
                   RAMDISK 384
          
                   MD H:\UTILS
                   COPY A:\COMMAND2.COM H:\UTILS
                   COPY A:\UTILS\DOS2TOOLS\KEY.COM H:\UTILS
          
          RAMdisk  aanmaken, en  2 files  erop zetten.  Ik wilde eerst 
          functietoetsen gebruiken  onder DOS.  Moet ik  weer in  orde 
          maken.
          
                   ECHO N|PATTERN STANDARD>NUL
          
          Ik laad  de standaard  karakterset in,  omdat ik  anders gek 
          word van het yen-teken.
          
                   KEY ON
          
          Functietoetsen aan en dan zit ik in DOS.
          
          
                   R E B O O T . B A T 
          
                   MODE 80
                   TURBOSW 2
                   COLOR 15,0,0
          
          Scherm weer goed zetten, R800-DRAM aan.
          
                   SET R1=%REBOOT%
                   SET REBOOT=
          
          Via het  env. item REBOOT kan een commando worden meegegeven 
          dat  wordt uitgevoerd  als je  terug naar  DOS gaat.  REBOOT 
          wordt  in  een  ander  item gekopieerd  omdat je  anders het 
          effect  zou  kunnen  krijgen  dat er  de volgende  keer weer 
          hetzelfde wordt gedaan als je terug naar DOS gaat.
          
                   %R1%
          
                   SET R1=
          
          Het  env. item R1 wordt gewist als er nog wordt teruggekeerd 
          naar de batchfile of als R1 een intern DOS commando was.
          
          
                            T W E E D E   S O O R T 
          
          Nu  worden  batchfiles  besproken  die  een  ander programma 
          inladen. Om te beginnen een hele simpele.
          
          
                   P M A R C . B A T 
          
                   SET UPPER ON
                   PMARC.COM %1 %2 %3 %4 %5 %6 %7 %8 %9
                   SET UPPER OFF
          
          Het  item  UPPER  is  ervoor om  te zorgen  dat alle  kleine 
          letters die  in parameters  voorkomen in hoofdletters worden 
          omgezet.  Dit is  voor sommige  CP/M programma's, die anders 
          niet goed functioneren. Hiertoe behoren PMARC en PMEXT. 
          
          Omdat het  voor sommige  programma's ook  mooi is  om kleine 
          letters   te  kunnen  gebruiken  (waaronder  INPUT.COM,  zie 
          Sunrise Magazine #6), wordt UPPER ook weer op OFF gezet.
          
          
                   M E M M A N . B A T 
          
                   IF %MEMMAN%==ON ECHO
                   IF %MEMMAN%==ON ECHO *** MemMan already installed
                   IF %MEMMAN%==ON EXIT
          
          Als MemMan  al eens  is ingeladen,  wordt teruggekeerd  naar 
          MSX-DOS. Dit gebeurt met EXIT. Als je COMMAND2 intypt zonder 
          iets  extra, kost  dat geheugen,  en bij EXIT wordt dit weer 
          vrijgegeven. Probeer  voor de lol trouwens een COMMAND2>PRN. 
          Zorg wel dat de printer aan staat! Als je dan ECHO Tekst>CON 
          intypt,  krijg je  w�l weer  iets op het scherm. Achter EXIT 
          kan ook nog een foutcode worden opgegeven.
          
                   CLS
          
                   PATH +A:\MEMMAN
                   SET MEMMAN=ON
                   SET REBOOT=%1
                   SET TL=A:\MEMMAN\TSRS
          
          Het path  wordt uitgebreid  met A:\MEMMAN.  De TSR directory 
          wordt  goed gezet  en de  parameter die is meegegeven achter 
          MEMMAN.BAT wordt in REBOOT gezet.
          
                   MEMMAN.COM
          
          
                   A F T E R M M . B A T 
          
          Mijn   MEMMAN.COM   zet   '_SYSTEM("AFTERMM.BAT")'   in   de 
          keyboardbuffer. AFTERMM betekent "after MemMan".
          
                   CLS
                   ECHO Loading CHGCPU2.TSR and MBTSR.TSR...
          
                   TL CHGCPU2>NUL
                   TL MBTSR>NUL
          
          Laad een paar TSR's in.
          
                   REBOOT
          
          REBOOT opstarten.  Het commando  dat eerst achter MEMMAN.BAT 
          stond  wordt dan  via REBOOT.BAT  uitgevoerd. Hier volgt een 
          batchfile die een MemMan programma uitvoert...
          
          
                   M P . B A T 
          
                   IF %MP%A==A SET MP=%1
                   IF %MP%A==A SET MP=B:\MOD
          
          Als MP leeg is, wordt de parameter achet MP.BAT in MP gezet. 
          Als  MP nu  nog leeg  is, en  dus niks  in de parameter zat, 
          wordt MP gevuld met de directory waar MOD files in staan.
          
                   IF NOT %MEMMAN%A==ONA MEMMAN MP
          
          Als  MemMan  nog  niet  in het  geheugen zat,  wordt het  nu 
          geladen. Bij terugkomst in DOS wordt MP.BAT weer uitgevoerd. 
          Vandaar ook dat %1 in een environment item wordt gezet.
          
                   MP2.COM %MP%
          
          MP2.COM is een ietwat aangepaste versie van de MOD-player.
          
                   SET MP=
          
          %MP% wordt weer gewist.
          
          
                   M C M . B A T 
          
          Dit programma laadt MCM-Index in.
          
                   PPDIR PUSH
          
          De huidige directory bewaren.
          
                   CDD H:\
          
          Overgaan op H:\. CDD wordt besproken op Sunrise Special #2.
          
            IF NOT EXIST MCM.COM   COPY C:\DIVERSEN\MCMINDEX\MCM.COM
            IF NOT EXIST MCM.DAT   COPY C:\DIVERSEN\MCMINDEX\MCM.DAT
            IF NOT EXIST MCM.SET   COPY C:\DIVERSEN\MCMINDEX\MCM.SET
            IF NOT EXIST TREF.NDX  COPY C:\DIVERSEN\MCMINDEX\TREF.NDX
            IF NOT EXIST SOORT.NDX COPY C:\DIVERSEN\MCMINDEX\SOORT.NDX
          
          Als  bovenstaande files  nog niet  in H:\  staan, worden  ze 
          gekopieerd van  de directory C:\DIVERSEN\MCMINDEX. MCM Index 
          werkt namelijk een stukje sneller vanaf RAMdisk.
          
                   KEY OFF
          
          Het programma kan niet tegen functietoetsen onder DOS.
          
                   H:\MCM.COM
                   KEY ON
                   PPDIR POP
          
          Directory herstellen.
          
          
                                  G E N O E G 
          
          Ik hoop  dat dit  genoeg is  om zelf  handige batchfiles  te 
          kunnen maken. Als er echter vragen zijn, schrijf dan naar de 
          postbus of het bel Sunrise BBS Nuth.
          
                                                        Kasper Souren
