                        M L   T E C H I E K E N   ( 1  )
                                                         
          
          Het  lijkt  mij  een  goed  idee  om  naast de  cursus BASIC 
          technieken  op Sunrise  Magazine een cursus ML technieken te 
          starten op  de Special.  Ook dit  is geen  programmeercursus 
          maar  een verzameling tips en truuks en manieren hoe je iets 
          het beste  kunt aanpakken. Ik ben overigens niet van plan om 
          deze  cursus alleen  te schrijven, maar zal zeker een aantal 
          ML programmeurs  met meer ervaring vragen hun geheimen prijs 
          te  geven  in  deze  rubriek.  Deze  keer  een paar  simpele 
          problemen waar beginners tegenaan zullen lopen.
          
          Ik zal bij vergelijking van routines tellingen van klokcycli 
          en bytes geven, bij de klokcycli ga ik uit van een Z80. Over 
          het  algemeen zal  een routine  die sneller is op de Z80 ook 
          sneller zijn  op de  R800, maar dit is geen wet van Meden en 
          Perzen.
          
          
                    B Y T E   U I T   T A B E L   H A L E N 
          
          Stel  je  hebt een  tabel en  je hebt  een waarde  in A  die 
          aangeeft de  hoeveelste byte  van de  tabel je  wilt hebben. 
          Deze byte moet uiteindelijk in A terechtkomen. Hiervoor moet 
          je  het adres  in de tabel uitrekenen, het liefst zou je dat 
          doen met
          
                  LD      HL,Tabel
                  ADD     HL,A
                  LD      A,(HL)
          
          maar de instructie ADD HL,A bestaat natuurlijk niet, want je 
          kunt geen 8 bits register bij een 16 bits register optellen. 
          We zullen  de instructie  ADD HL,A dus zelf moeten maken. Er 
          zijn  op z'n  minst twee  oplossingen mogelijk. De eerste is 
          degene die ik meestal gebruik:
          
                                          klokcycli       bytes
          
                  LD      E,A             5               1
                  LD      D,0             8               2
                  LD      HL,Tabel        11              3
                  ADD     HL,DE           12              1
                  LD      A,(HL)          8               1
                                         ----            ---
                                          44              8
          
          De  eerste  twee instructies  betekenen LD  DE,A. Vervolgens 
          wordt DE  bij HL  opgeteld en  de byte uit de tabel gelezen. 
          Het  nadeel hiervan  is dat je het DE register (of eventueel 
          het BC  register) gebruikt.  De volgende  oplossing doet dat 
          niet:
          
                                          klokcycli       bytes
                                  
                  LD      HL,Tabel        11              3
                  ADD     A,L             5               1
                  LD      L,A             5               1
                  LD      A,0             8               2
                  ADC     A,H             5               1
                  LD      H,A             5               1
                  LD      A,(HL)          8               1
                                         ----            ---
                                          47              10
          
          
          ADD A,L;  LD L,A  is een  substituut voor  ADD L,A (dat niet 
          bestaat). Vervolgens moet nog de carry bij H worden opgeteld 
          (LD  A,0; ADC  A,H; LD  H,A). Het voordeel is dat het DE (of 
          BC) register  niet wordt gebruikt, maar deze routine is iets 
          trager  (3  klokcylci  is 0.84  microseconde, niet  echt een 
          verschil  dat je  snel zult merken) en wat erger is: 2 bytes 
          langer. Ik  heb vaak problemen dat mijn code te groot wordt, 
          en   ik  heb   daarom  een   grote  voorkeur   voor  kortere 
          oplossingen, zelfs als dat een paar klokcycli trager is.
          
          Er is nog een alternatief dat sterk op de vorige lijkt, hier 
          wordt alleen  een andere manier gekozen om de carry bij H op 
          te tellen:
          
                                          klokcycli       bytes
                                  
                  LD      HL,Tabel        11              3
                  ADD     A,L             5               1
                  LD      L,A             5               1
                  JR      NC,Label        13/8            2
                  INC     H               0 /5            1
          Label:  LD      A,(HL)          8               1
                                         ----            ---
                                          42              9        
          
          Het  grappige  van  deze  routine  is  dat  hij  ondanks  de 
          voorwaardelijke  sprong altijd  even snel is, dit komt omdat 
          als hij  springt (13  klokcycli) hij  de INC H niet doet, en 
          als  hij  niet  springt  (8  klokcycli)  hij  de  INC  H  (5 
          klokcycli)  er nog bij doet, waardoor de LD A,0; ADD A,H; LD 
          H,A constructie  van de  vorige oplossing  (18 klokcycli,  4 
          bytes)  dus wordt  vervangen door  13 klokcycli  en 3 bytes. 
          Vandaar de 5 klokcycli en 1 byte winst.
          
          Deze  oplossing is  dus sneller  dan de oplossing met DE (44 
          klokcycli), maar  het kost  weer een  byte meer (9 in plaats 
          van 8). Als je ruim in je geheugen zit en die ene byte extra 
          je  geen bal  kan schelen  is deze  oplossing dus  het beste 
          (lees: snelste),  en als  je het  DE register  al voor  iets 
          anders  in gebruik hebt is het zeker de beste. Maar als elke 
          byte telt  geef ik  de voorkeur aan de eerste oplossing. Nog 
          een  nadeel van  deze oplossing  is dat  het een extra label 
          geeft en  of dat  een nadeel is is vooral afhankelijk van de 
          assembler die je gebruikt.
          
          Tot slot nog even hoe het niet moet:
          
                                          klokcycli       bytes
          
                  LD      IX,Tabel        15              4
                  LD      (Poke+2),A      14              3
          Poke:   LD      A,(IX+0)        20              3
                                         ----            ---
                                          49              10
          
          Dit  is  verreweg  de  meest  lelijke  oplossing die  ik kan 
          bedenken. Hij is ook nog eens de traagste en net zo lang als 
          de langste  oplossing tot  nu toe. Bovendien doe je iets dat 
          eigenlijk  helemaal  niet  mag  (het  kan niet  eens als  je 
          programma  op ROM  wordt gezet!),  namelijk het  poken in je 
          code. Indexregisters zijn traag maar erg handig bij tabellen 
          met  een  variabel  beginadres  waaruit  je  bytes op  vaste 
          plaatsen  wilt  lezen. (Er  zijn namelijk  geen LD  A,(IX+B) 
          instructies of  zoiets, het is altijd LD A,(IX+n), waarbij n 
          een  constante tussen  -128 en +127 is.) Ik heb daar al eens 
          een tekst over geschreven op de Special. Hier is het precies 
          andersom: een  vast beginadres  en een variabele plaats. Dat 
          doe je dus NIET met indexregisters. Het ziet er in je source 
          erg  kort  uit  (3  regels,  terwijl  de  andere oplossingen 
          respectievelijk  5, 6  en 7  regels in  beslag nemen),  maar 
          instructies met IX en IY zijn gewoon erg traag.
          
          Er  is  nog  ��n  truuk die  je kunt  gebruiken als  je echt 
          maximale snelheid nodig hebt: zorg dat je tabel op een adres 
          begint waarvan de lowbyte gelijk is aan 0! Dus bijvoorbeeld:
          
          Tabel:  EQU     #D000
          
                                          klokcycli:      bytes:
          
                  LD      H,#D0           8               2
                  LD      L,A             5               1
                  LD      A,(HL)          8               1
                                         ---             ---
                                          21              4
          
          Deze routine  is twee  keer zo snel en twee keer zo kort als 
          de   snelste  en   kortste  routines   die  we  tot  nu  toe 
          tegenkwamen, maar  dat komt  door de speciale eis die aan de 
          tabel wordt gesteld. Op zich heel mooi maar vaak onhandig om 
          toe  te passen, zeker als je veel tabellen hebt. Toch is dit 
          zeker iets  wat je in gedachten moet houden voor als je eens 
          in snelheidsnood zit.
          
          
                               J U M P T A B E L 
          
          Iets wat veel met het vorige onderwerp te maken heeft is een 
          jumptabel. Vergelijk dit maar met ON A GOTO in BASIC. Altijd 
          een  zeer snelle methode om een keuze uit een redelijk groot 
          aantal  alternatieven   te  maken.   Bij  slechts  een  paar 
          alternatieven is CP x; JP Z,y korter en sneller.
          
          Ik  heb  hiervoor  altijd een  standaardroutine met  de naam 
          Jump.  In A staat het nummer van de gewenste sprong en in HL 
          het begin van de jumptabel. Die routine is als volgt:
          
                                          klokcycli:      bytes:
          
          Jump:   DEC     A               5               1
                  ADD     A,A             5               1
                  LD      E,A             5               1
                  LD      D,0             8               2
                  ADD     HL,DE           12              1
                  LD      E,(HL)          8               1
                  INC     HL              7               1
                  LD      D,(HL)          8               1
                  EX      DE,HL           5               1
                  JP      (HL)            5               1
                                         ----            ---
                                          68              11
          
          De  DEC  A  is  nodig  omdat  het eerste  adres in  de tabel 
          natuurlijk offset 0 heeft. Daarna verdubbelen we A omdat een 
          adres 2  bytes lang is. Daarna tellen we A bij HL op via DE, 
          omdat we net hebben gezien dat dat de korste oplossing is en 
          we  DE  sowieso  nodig  hebben.  Een  instructie  LD DE,(HL) 
          bestaat  helaas niet  dus doen we dat met LD E,(HL); INC HL; 
          LD D,(HL). Tenslotte verwisselen we DE en HL (want het adres 
          waarnaar  we  moeten  springen  staat in  DE en  moet in  HL 
          terecht komen)  en springen  we naar  dat adres toe. Volgens 
          mij  is  er  geen betere  oplossing hiervoor,  de eis  dat L 
          gelijk  moet zijn  aan 0 (zodat we alleen maar LD L,A hoeven 
          te doen  in plaats  van LD  E,A; LD  D,0; ADD HL,DE) is hier 
          zeer onhandig.
          
           - Deze tekst wordt vervolgd in de volgende submenu-optie -
          
          
          
                         - Dit is het tweede gedeelte -
          
                       M L   T E C H N I E K E N   ( 1  )
                                                          
          
                                 C O M P A R E 
          
          Veel beginnende programmeurs hebben moeite met de instructie 
          CP,  wat  een afkorting  is voor  ComPare. Eigenlijk  is het 
          helemaal niet moeilijk als je maar weet hoe het werkt.
          
          De volgende CP instructies zijn mogelijk:
          
                                          klokcycli:      bytes:
          
                  CP      (HL)            8               1
                  CP      (IX+n)          20              3
                  CP      (IY+n)          20              3
                  CP      A               5               1
                  CP      B               5               1
                  CP      C               5               1
                  CP      D               5               1
                  CP      E               5               1
                  CP      H               5               1
                  CP      L               5               1
                  CP      n               8               2
          
          Bij een  CP x instructie zal de Z80 de vlaggen zo zetten als 
          ook bij een SUB x instructie gebeurt, zonder de waarde van A 
          te veranderen. Er zijn dus drie gevallen mogelijk:
          
          - als x kleiner is dan A dan komt er geen carry want je kunt 
            x  gewoon  van  A aftrekken  zonder dat  je een  negatieve 
            uitkomst krijgt
          - als x gelijk is aan A dan komt er een zero want A-x is dan 
            gelijk aan  0, er  is GEEN  carry want  er is nog net geen 
            negatieve uitkomst
          - als x groter is dan A dan komt er een carry want er is een 
            nieuwe uitkomst
          
          Samengevat:
          
          A >= x          NC
          A <  x          C
          A =  x          Z
          A <> x          NZ
          
          Je hoeft  dit niet  uit je  hoofd te  kennen, je kunt gewoon 
          beredeneren  wat er met de zero- en carryflag gebeurt als je 
          x van A af zou trekken.
          
          Als  je  zoals  ik  in  het  vorige  onderdeel  al  opmerkte 
          slechts  weinig alternatieven hebt, dan is het sneller om de 
          volgende constructie toe te passen:
          
                                          klokcycli:      bytes:
          
                  CP      1               8               2
                  JR      Z,Keuze1        8/13            2
                  CP      2               8               2
                  JR      Z,Keuze2        8/13            2
          Keuze3: .....
          
          Dit neemt 8 bytes in beslag (de jumproutine 11) en het neemt 
          8  + 13  = 21  klokcycli als  A=1 en  8 +  8 +  8 +  13 = 37 
          klokcycli in  beslag als A=2 en 8 + 8 + 8 + 8 = 32 klokcycli 
          als  A=3. Zo  heb je dus bij drie alternatieven minder bytes 
          en minder  klokcycli nodig  in vergelijking  met de  routine 
          Jump.  (Indien JP nodig is in plaats van JR neemt het aantal 
          bytes met  2 toe waardoor het op 10 komt (nog steeds kleiner 
          dan  11) en  het aantal klokcycli is respectievelijk 18 voor 
          A=1 en 36 voor A=2 en A=3. Nog steeds veel sneller dan de 68 
          klokcycli van  Jump.) We  gaan er  hier overigens wel vanuit 
          dat  A alleen  de waardes  1, 2  en 3 kan aannemen, want bij 
          deze routine  zal ook  bij A=0  en A=4  t/m A=255 de routine 
          Keuze3 worden uitgevoerd.
          
          
                                  L U S S E N 
          
          De Z80  heeft een simpele voorziening voor lussen in de vorm 
          van   DJNZ  (Decrease,  Jump  if  NonZero).  Hierbij  is  de 
          lusteller altijd  B en is de maximale luslengte dus 256. Een 
          luslengte  van  256  kan daarbij  bereikt worden  met 0  als 
          startwaarde van B!!! Een voorbeeld:
          
                                          klokcycli:      bytes:
          
                  LD      B,10            8               2
          Lus:    DJNZ    Lus             14/9            2
                                                         ---
                                                          4
          
          In totaal  vergt deze lus 8 + 9*14 + 9 = 143 klokcycli. Deze 
          lus   doet  niets   boeiend,  alleen   even  wachten,  40.04 
          microseconde  om  precies  te  zijn.  Je  kunt  het  ook  zo 
          programmeren:
                                          
                                          klokcycli:      bytes:
          
                  LD      D,10            8               2
          Lus:    DEC     D               5               1
                  JR      NZ,Lus          13/8            2
                                                         ---
                                                          5
          
          Ik heb  het expres  met D  gedaan om te laten zien dat je nu 
          niet meer aan B bent gebonden als lusvariabele. Het kost ��n 
          byte meer en verder vergt deze lus 8 + 10*5 + 9*13 + 8 = 183 
          klokcycli  (51.24  microseconde).  Met DJNZ  bespaar je  dus 
          zowel ruimte als tijd, maar alleen B kan gebruikt worden als 
          lusvariabele.
          
          Grotere  lussen  kunnen  gemaakt worden  door geneste  DJNZ- 
          lussen, bijvoorbeeld:
          
                          LD      B,100
          BuitensteLus:   PUSH    BC
                          LD      B,100
          BinnensteLus:   ......
                          ......
                          DJNZ    BinnensteLus
                          POP     BC
                          DJNZ    BuitensteLus
          
          Hierbij  wordt  de  binnenste  lus 100  * 100  = 10000  maal 
          uitgevoerd. Dit  is alleen geschikt voor vaste luslengte, en 
          dan  vooral als  er ook  echt sprake is van een binnenste en 
          een buitenste  lus. Bijvoorbeeld  de binnenste lus doet iets 
          met alle dingen van ��n regel en de buitenste lus loopt alle 
          regels  langs.  Bij  een  variabele  luslengte  of  als  het 
          eigenlijk  gewoon de bedoeling is dat de binnenste lus 10000 
          maal  wordt  herhaald, is  de volgende  oplossing het  meest 
          geschikt:
          
                          LD      BC,10000
          Lus:            ......
                          ......
                          DEC     BC
                          LD      A,B
                          OR      C
                          JR      NZ,Lus
          
          Bij een  8 bits  DEC (DEC  A, B,  C, etc.) wordt de Zeroflag 
          gezet  als 0  wordt bereikt. Bij een 16 bits DEC is dit niet 
          het geval, DEC BC zet de Z-flag dus NIET als BC gelijk wordt 
          aan 0.  Daarom kijken  we met  LD A,B; OR C of zowel B als C 
          gelijk zijn aan 0, want dan is automatisch BC gelijk aan 0.
          
          Dit doet me trouwens denken aan een verschil tussen DEC A en 
          SUB  1.  Op het  eerste gezicht  doen deze  twee instructies 
          exact  hetzelfde,  namelijk het  verlagen van  A met  1. Het 
          verschil zit  hem echter  in de  vlaggen: bij  DEC wordt  de 
          carryflag niet be�nvloed, bij SUB 1 wel! Dus als A gelijk is 
          aan  0 en  je doet  DEC A, dan wordt er geen carry gezet, en 
          als A  gelijk is  aan 0 en je doet SUB 1, dan wordt de carry 
          wel  gezet. Dit  is echt  zo'n irritant  verschil waar je op 
          moet letten,  want het  kan soms  hele vage bugs veroorzaken 
          omdat  je ervan  uitgaat dat de carryflag gezet wordt en het 
          gebeurt niet.  [Ik had  ergens SUB 1 in een source staan, en 
          toen  zei iemand aan wie ik die source liet zien dat ik daar 
          DEC  A  van moest  maken, omdat  dat hetzelfde  zou doen  en 
          sneller is  en minder bytes in beslag neemt. Ik had dat toen 
          veranderd  en merkte pas later, toen diegene alweer weg was, 
          dat het programma niet meer werkte!]
          
          
                              1 6   B I T S   L D 
          
          Veel  16 bits  instructies die  je vaak  nodig hebt zoals LD 
          DE,HL; LD IX,HL en LD DE,IY bestaan niet. Hiervoor zullen we 
          dus andere oplossingen moeten zoeken.
          
          Ten eerste deze groep:
          
          LD      BC,DE
          LD      BC,HL
          LD      DE,BC
          LD      DE,HL
          LD      HL,DE
          LD      HL,BC
          
          Deze  instructies  bestaan  helaas  niet  maar ze  zijn heel 
          makkelijk te maken met 8 bits LD instructies:
          
          LD      BC,DE   wordt:  LD      B,D
                                  LD      C,E
          LD      DE,HL   wordt:  LD      D,H
                                  LD      E,L
          etc.
          
          Zo'n  "16 bits  LD" kost  2 bytes  en 10  klokcycli. Sommige 
          programmeurs gebruiken de volgende methode:
          
          LD      BC,DE   wordt:  PUSH    DE
                                  POP     BC
          LD      DE,HL   wordt:  PUSH    HL
                                  POP     DE
          
          Dit kost ook 2 bytes maar is veel trager, want een PUSH kost 
          12  klokcycli en een POP 11, waardoor het totaal op 23 komt. 
          Ofwel meer dan twee keer zo lang.
          
          Voor de  tweede groep  is dit  (als je  je aan  de offici�le 
          instructieset van de Z80 houdt) niet mogelijk:
          
          LD      IX,HL
          LD      IX,DE
          LD      IX,BC
          LD      IX,IY
          LD      HL,IX
          LD      DE,IX
          LD      BC,IX
          LD      IY,IX
          
          en  dezelfde instructies  met IY kun je alleen maken met een 
          PUSH en een POP. Bijvoorbeeld:
          
                                                  klokc:  bytes:
          
          LD      IX,HL   wordt:  PUSH    HL      12      1 
                                  POP     IX      15      2
                                                 ----    ---
                                                  27      3
          
          LD      BC,IY   wordt:  PUSH    IY      16      2
                                  POP     BC      11      1
                                                 ----    ---
                                                  27      3
          
          Op  de R800  [of met  illegale instructies van de Z80 die je 
          dus niet  mag gebruiken  omdat ze  bij de  Z80's niet getest 
          zijn  en dus  fout kunnen  gaan] kan  het wel  met 8 bits LD 
          instructies. Dit  kan echter  alleen met  DE en BC, niet met 
          HL!  Ook LD  IX,IY en LD IY,IX kunnen alleen met PUSH en POP 
          worden  opgelost.   We  schakelen  nu  bij  het  tellen  van 
          klokcycli  nu even  over op  klokcycli van  de R800. Ik laat 
          voor  LD  IX,DE  en  LD  BC,IY  van  beide  alternatieven de 
          tellingen zien:
          
          LD      IX,DE   wordt:  PUSH    DE      4       1
                                  POP     IX      4       2
                                                 ---     ---
                                                  8       3
          
                          of:     LD      IXH,D   2       2
                                  LD      IXL,E   2       2
                                                 ---     ---
                                                  4       4
          
          LD      BC,IY   wordt:  PUSH    IY      5       2
                                  POP     BC      3       1
                                                 ---     ---
                                                  8       3
          
                          of:     LD      B,IYH   2       2
                                  LD      C,IYL   2       2
                                                 ---     ---
                                                  4       4
          
          Op de R800 is de variant met 8 bits LD dus twee keer zo snel 
          maar  neemt wel  een byte  meer in beslag. Bovendien moet je 
          wel goed  nadenken omdat niet alles mogelijk is. Maar als je 
          daar  een fout mee maakt zal de compiler je wel waarschuwen. 
          Nog een  nadeel hiervan  is dat  GEN80 en  WB-ASS2 geen IXL, 
          IXH,  IYL en  IYH ondersteunen  en je dus met DB's moet gaan 
          werken (DB  #DD voor  IX en  DB #FD  voor IY gevolgd door de 
          instructie op H of L).
          
          Dit was het voor deze keer. Veel succes met het programmeren 
          in  ML, met  vragen kun je natuurlijk altijd bij de redactie 
          terecht.
          
                                                          Stefan Boer
