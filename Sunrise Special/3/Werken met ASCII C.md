                  H E T   W E R K E N   M E T   A S C I I   C 
                                                               
          
                                  D E E L   1 
          
                DE VERSCHILLEN TUSSEN ASCII C 1.1 en ASCII C 1.2
          
          
          Zoals wellicht  bekend, zijn  er diverse C compilers voor de 
          MSX   beschikbaar,   waarbij   Hisoft  C   en  ASCII   C  de 
          uitgebreidste  zijn. Mijn  favoriete C  compiler is  die van 
          ASCII corporation,  waarvan ik mijn gebruikerservaringen zal 
          bespreken.  In dit  eerste deel zal ik vooral proberen om de 
          verschillen  tussen  versie  1.1  en 1.2  aan te  geven (het 
          belangrijkste verschil  is dat versie 1.1 voor MSX-DOS 1 is, 
          terwijl  versie 1.2  voor MSX-DOS  2 is).  Hierbij ga  ik er 
          vanuit dat de lezer de beschikking heeft over de handleiding 
          van versie 1.1.
          
          De C  compiler van  ASCII corporation bestaat uit een aantal 
          verschillende  bestanden voor de verschillende delen van het 
          compilatie proces.  Dit maakt  het gebruik van deze compiler 
          relatief  ingewikkeld, maar  daar staat  wel een  zeer grote 
          flexibiliteit tegenover.
          
          De  compiler  bestaat  ten  eerste  uit  2 hoofdprogramma's, 
          namelijk: CF.COM en CG.COM, dit zijn respectievelijk de code 
          parser en de code generator.
          
          
          
                       C F . C O M ,   D E   P A R S E R 
          
          De  parser   leest  de   source  file  in,  voert  hier  een 
          syntactische analyse op uit en genereert een tijdelijke file 
          waar  de  zogenaamde  parse boom  in wordt  opgeslagen. Deze 
          parser  is afgeleid  van de  Microsoft C compiler voor CP/M. 
          Bij het inlezen van de source file verwerkt de parser tevens 
          alle include  commando's. Bij  include zijn  er 2  syntaxen, 
          hierbij  komen de verschillen tussen C 1.1 en C 1.2 voor het 
          eerst duidelijk om de hoek:
          
          'syntax'     C 1.1                     C 1.2
          include      Haal de include file van  Haal de include file
           "filename"  dezelfde drive als de     van dezelfde drive
                       hoofd source file.        als de hoofd source
                                                 file.
          include      Haal de include file van  Haal de include file
           <filename>  de default drive.         uit de directory die
                                                 in het environment
                                                 item INCLUDE staat.
          
          Bij C  1.2 kun je dus de standaard headerfiles in een aparte 
          directory  zetten, wat je vervolgens onder DOS doorgeeft aan 
          de compiler met SET INCLUDE = include directory
          
          
               C G . C O M ,   D E   C O D E   G E N E R A T O R 
          
          De code  generator leest  de tijdelijke  file in  en zet  de 
          parse  informatie hieruit  om in  machinecode. Hierbij wordt 
          een  assembler  source  file  gemaakt  in  een  formaat  dat 
          geschikt is  voor M80,  de reloceerbare  macro assembler van 
          Microsoft.    Deze   code   generator   voert   tevens   een 
          (uitschakelbare) register-optimalisatie  uit. Hierdoor is de 
          gegenereerde  code behoorlijk  snel (ik  heb bijv. een eigen 
          paint routine  geschreven, eenmaal  in C en ter vergelijking 
          ook eenmaal in assembler, de handgeschreven assembler versie 
          was  maar 10%  sneller dan  de gecompileerde  C versie),  al 
          maakt deze optimalisatie het compilatieproces natuurlijk wel 
          een stuk trager.
          
          Ook deze  code generator  is afgeleid  van de code generator 
          van  de Microsoft  C compiler voor CP/M. Dit laatste heeft 1 
          klein nadeel:
          
          Aangezien  de   CP/M  wereld   veel  Intel  8080  processors 
          (voorloper van de Z80) kende, maakt deze code generator code 
          aan  voor  deze  Intel-processor.  De  Z80 heeft  echter een 
          aantal  uitbreidingen t.o.v. de 8080 die hierdoor niet benut 
          worden. Indien  ASCII corporation de code generator dus echt 
          goed  had aangepast,  dan had de gecompileerde code nog iets 
          beter kunnen zijn.
          
          Ook bij  het aanmaken  van de  code is er een klein verschil 
          tussen  versie 1.1  en 1.2.  De eerste versie maakt namelijk 
          Intel  formaat  mnemonics  aan  (niet te  lezen voor  de Z80 
          programmeurs)  terwijl   de  tweede   versie  Zilog  formaat 
          mnemonics aanmaakt (let wel, de geproduceerde code is echter 
          nog altijd beperkt tot de 8080 subset).
          
          Behalve   deze  code  omzetters  (C  source  naar  assembler 
          source),  bestaat   de  compiler  ook  nog  uit  een  aantal 
          standaard  library's en  een aantal  header files  voor deze 
          library's.  Bij  de  C  compiler  worden  de library's  in 2 
          formaten meegeleverd:
          
          1) In  het  reloceerbare  .REL  formaat,  zodat  ze met  L80 
             meegelinkt kunnen worden.
          2) Als  een  aantal  C en  assembler source  files zodat  ze 
             eventueel   aangepast   kunnen   worden   voor   speciale 
             toepassingen.
          
          Er zijn  4 reloceerbare  files, waarbij  er weer verschillen 
          zijn tussen de .REL files van versie 1.1 en versie 1.2:
          
          CK.REL: De zogenaamde kernel, deze code wordt uitgevoerd bij 
          het  opstarten van  een C programma. De kernel onderzoekt de 
          command line  en springt  vervolgens naar  de routine "main" 
          toe. Bij versie 1.1 wordt bij het onderzoeken van de command 
          line  tevens gecontroleerd op de symbolen > (redirection) en 
          |  (pipelining),  dit omdat  MSX-DOS 1  geen redirection  en 
          pipelining ondersteunt,  terwijl een  aantal C  toepassingen 
          hier  wel gebruik  van maken omdat C uit de UNIX wereld komt 
          waar redirection en pipelining zo ongeveer zijn uitgevonden. 
          Versie 1.2 controleert hier niet meer op omdat MSX-DOS 2 dit 
          zelf doet.
          
          CRUN.REL: Hier  zitten een aantal routines in die nodig zijn 
          bij  een C  programma, zoals  het vermenigvuldigen, delen en 
          vergelijken van  16 bits  getallen. Als er in de source file 
          bijv. staat:
          
                  {
                    int a,b,c;
                    a = 3; b = 4;
                    c = a * b;
                  }
          
          dan maakt de compiler ervan:
          
                    ld     hl,3
                    ld     de,4
                    call  ?MULHD
          
          De routine MULHD is dan te vinden in CRUN.REL. Er is hierbij 
          geloof ik geen verschil tussen de 2 CRUN versies.
          
          CEND.REL:  In deze  file bevind  zich het label @endx@ zodat 
          eventuele interne routines uit CK en CRUN weten tot waar het 
          programma loopt.
          
          CLIB.REL:  Dit  is  de  echte  C  library waar  de standaard 
          functies in  zitten. Hierbij  zijn er wezenlijke verschillen 
          tussen versie 1.1 en versie 1.2, de eerste werkt bij de file 
          in/out  namelijk overal  met FCB's, terwijl de tweede overal 
          met de  file handles  van MSX-DOS 2 werkt. Het is hierom ook 
          belangrijk  dat van  CK.REL en CLIB.REL de goede versies bij 
          elkaar worden  gebruikt, dit  omdat CK  een aantal standaard 
          devices  opent voor  functies zoals  printf(). Bij CK 1.1 en 
          CLIB 1.1  worden hier dus FCB's voor gebruikt terwijl CK 1.2 
          en  CLIB  1.2  alles  met  de  file  handles  doen.  CK  1.2 
          controleert  tevens de  DOS-versie zodat  C programma's  die 
          gelinkt zijn  met deze  versie een  foutmelding geven als ze 
          onder MSX-DOS 1 worden opgestart.
          
          Behalve  deze  library's  worden ook  nog een  aantal header 
          files  meegeleverd  waar  alle  relevante  types  in  worden 
          gedefinieerd.  Bij versie  1.2 zijn de header files een stuk 
          verder  opgesplitst  dan  bij  versie  1.1.  Tevens zijn  de 
          definities van sommige types veranderd, zo is bijv. het type 
          FILE  veranderd  omdat  de  routines  uit  CLIB hier  andere 
          informatie  in  opslaan.  Het  is daarom  belangrijk dat  de 
          versie van  de header-files overeenkomt met de versie van de 
          library's!
          
                                                           Alex Wulms
