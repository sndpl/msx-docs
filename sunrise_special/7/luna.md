          Noot vooraf:  dit programma had op Sunrise Special #6 moeten 
          staan,  maar stond er dus niet op. Vergissen is menselijk en 
          dus ook  Kasperlijk zullen  we maar denken. Hier nogmaals de 
          tekst,  en als  er geen hele rare dingen gebeuren staat LUNA 
          nu wel op de disk...
          
          
                                    L U N A 
                                             
          
                       B E T E R E   D I S K - C A C H E 
          
          Op Sunrise  Magazine #11  werd DOS2CASH  besproken. Nu is er 
          alweer  een veel beter programma om te cachen! Het heet Luna 
          - en heeft volgens mij niet veel met een lunapark te maken - 
          en  is  dit  jaar  nog gemaakt.  Het is  dus een  vrij nieuw 
          programma.
          
          Het is net als DOS2CASH afkomstig uit Japan, en het komt van 
          een reeks  diskettes met allerlei PD programma's uit Japanse 
          BBS'en.
          
          
                               H A R D D I S K ! 
          
          Luna kan  ook de harddisk cachen, en het helpt ook nog! Luna 
          vertraagt  wel een beetje bij de eerste keer laden, maar als 
          je  iets   uit  het  cache-geheugen  laadt,  gaat  het  echt 
          razendsnel.
          
          Voor  turbo  R  gebruikers is  er het  goede nieuws  dat het 
          programma  ook een  soort R800-drive  bevat. Je  kunt nu per 
          drive instellen  of er gelezen en geschreven moet worden met 
          de R800 mode aan.
          
          
                          G E H E U G E N   N E M E N 
          
          Met de  volgende functies  kun je  aangeven hoeveel geheugen 
          LUNA.COM moet innemen.
          
          nnnn : Gewoon  een getal  van 80 tot 4096. Dit is de ruimte, 
                 in kB's, die Luna gebruikt als cache-geheugen.
          
          Snnn : Hier  geeft  het getal  niet het  aantal kB  dat Luna 
                 gebruikt,  maar  het  aantal  mapper  segmenten.  Elk 
                 segment is 16 kB groot.
          
          Ennnn: Met E wordt de vrije ruimte aangeduid.
          
          ESnnn: Hier is  het getal natuurlijk het aantal vrije mapper 
                 segmenten.
          
          De  minimale hoeveelheid  geheugen die  Luna nodig  heeft is 
          overigens 80 kB, oftewel 5 segmenten.
          
          
                              C H E C K   T I M E 
          
          Met  de letter T kun je een bepaalde tijd opgeven. Deze tijd 
          staat voor  een aantal  interrupts. Als je er nu binnen deze 
          tijd  al een  disk-aanroep is geweest, controleert Luna niet 
          of dezelfde  disk nog in de drive zit. Als je computer op 50 
          Hz staat is 50 eenheden uiteraard 1 seconde.
          
          Bijv.  LUNA T300 om bij een computer op 60 Hz pas na (300/60 
          =) 5  seconde weer te checken op de bootsector, en bij 50 Hz 
          (300/50 =) 6 seconde te wachten.
          
          
                                 F L A S H E N 
          
          Bij de  optie F  heeft de help-optie (/H of /?) van Luna het 
          over   flash  time.  Wat  dit  precies  inhoudt  is  me  nog 
          onduidelijk. Ik  vermoed dat  het gewoon het laten knipperen 
          van  een aantal LED's bij het weer opnieuw herkennen van een 
          disk. In ieder geval geef je met Fnnnnn de tijdsduur van het 
          knipperen aan.
          
          
                       L E D S   S P E C I F I C E R E N 
          
          Bij de opties die hieronder vermeld staan kun je de volgende 
          letters gebruiken:
          
          C: CAPS
          K: KANA; dit is alleen bij Japanse computers van toepassing
          P: PAUSE: alleen bij MSX turbo R
          T: turbo: idem
          
          Deze staan  voor de  bijbehorende LED's, en als de handeling 
          verricht  wordt, worden de LED's aangemaakt als ze aanstaan, 
          en uitgemaakt als ze uitstaan. Gewoon ge�nverteerd dus.
          
          Met  LC kun je de LED's specificeren die ge�nverteerd worden 
          bij  het  cachen. Dus  bij alle  lees- en  schrijfopdrachten 
          waarbij het cache-geheugen gebruikt wordt.
          
          LF geeft aan welke LED's worden gebruikt bij het flashen. LA 
          doet  hetzelfde, maar  dan bij  een zogenaamde all-flash. Ik 
          vraag me  echter af  wat al  dit geflash inhoudt. Als iemand 
          meer weet: let me know!
          
          
                       P R O T E C T E D   S E C T O R S 
          
          Om   een  of   andere  reden  is  het  mogelijk  sectors  te 
          beschermen.  Waartegen   is  me  onduidelijk,  maar  het  is 
          mogelijk. Met de optie /F kun je dit uitschakelen.
          
          Achter  F kun je gewoon de eerste normale sector opgeven, en 
          dan geldt  dat voor  alle drives. Het is echter ook mogelijk 
          om drives te zetten achter F. Dan geldt de sector alleen bij 
          die bepaalde drives. Uiteraard kun je dus ook meerdere keren 
          F gebruiken.
          
          Bijv. LUNA FABC29 als de protected sectors van drives A: t/m 
          C: van 0 tot en met 28 lopen.
          
          
                           P E R   D R I V E   U I T 
          
          Met A  kun je  aangeven welke drives geen automatische flash 
          hebben.  Het  doel  hiervan  is  -  je  raadt  het  al -  me 
          onduidelijk. De drives komen gewoon achter de A.
          
          D is om bij bepaalde drives het cachen uit te zetten. Bij de 
          HG  SCSI  interface  zou  de  cache  wel  eens  alleen  maar 
          vertragend kunnen  werken, en  dan is  deze optie natuurlijk 
          wel handig. (Luna blijft echter handig, want je moet toch af 
          en toe met gewone disks werken.)
          
          Bijv.  LUNA dACD om drive A:, C: en D: niet te cachen. (Voor 
          de duidelijkheid gebruik ik een kleine D
          
          
                              R 8 0 0 - D R I V E 
          
          Met  de optie  R en W kun je aangeven of de R800 moet worden 
          aangezet bij  respectievelijk het  lezen (Read) en schrijven 
          (Write).
          
          Als  je een  turbo R hebt, kun je het beste al je drives met 
          de R800 aan laten lezen. Schrijven gaat soms fout bij gewone 
          drives, maar harddisk en RAMdisk kunnen het zonder problemen 
          aan.
          
          Bijv. LUNA  rABCDEFH wABCDH  om, zoals in mijn geval, alleen 
          drive E: en F: niet met R800 aan beschreven te laten worden.
          
          
                                  O P T I E S 
          
          /U : Upper  RAM mode  aan. De  bedoeling hiervan  is me niet 
               compleet  duidelijk. Ik  vermoed echter dat Luna in dit 
               geval liever de hogere mapper-segmenten gebruikt.
          
          /P : Stored PCM aan. Het klinkt wel spectaculair, maar is me 
               helemaal niet duidelijk.
          
          /S : Seek speed  up uit.  Ook dit  kan ik niet verklaren. In 
               ieder geval zal het wel trager worden als je deze optie 
               gebruikt.
          
          /F : FAT protect  off. Met  deze optie kun je het beschermen 
               van de FAT tegengaan. Zie ook de uitleg bij F.
          
          /D : Auto flash off. Tja...
          
          /M : Message   off.  Zo  krijg  je  geen  overzicht  van  de 
               instellingen.   Met  redirection   naar  NUL  krijg  je 
               overigens helemaal  niks op  het scherm.  Dus met  LUNA 
               >NUL.
          
          /C : De  opties die nu achter LUNA staan, worden in LUNA.COM 
               bewaard.  Als  LUNA.COM  gePOPCOMd is,  wordt dit  weer 
               ongedaan gemaakt  door het  saven zelf.  Hierna moet je 
               dus even opnieuw POPCOMmen.
          
          /J : Laat  de bewaarde parameters op het scherm zien. Handig 
               wanneer  je maar  ��n ding  wilt aanpassen. Want bij /C 
               worden de oude paramters ongedaan gemaakt.
          
          /R : Haal Luna uit het geheugen.
          
          /H  of /? : Geeft een  beknopt overzicht van alle opties van 
                      Luna
          
          
                             L U N A   E N   M A P 
          
          Helaas pindakaas.  Als je  Luna in  het geheugen hebt, en je 
          runt  MAP, dan  blijft de  computer hangen  bij de  volgende 
          disk-access. Als  je MAP  dus toch nodig hebt, moet je eerst 
          even Luna verwijderen met LUNA /R.
          
                                                        Kasper Souren
