                  H E T   W E R K E N   M E T   A S C I I   C 
          
                                  D E E L   3 
          
          
                H E T   G E N E R E R E N   V A N   S N E L L E 
          
                    C O D E   M E T   A S C I I   M S X   C 
                                                             
          
          Zoals  ook  al  in  deel  1  van deze  serie staat  vermeld, 
          genereert  de  code optimizer  van ASCII  MSX C  vrij snelle 
          code. Er  kan echter met deze compiler in verschillende modi 
          gewerkt  worden die  allemaal hun eigen invloed hebben op de 
          snelheid van de gegenereerde code.
          
          In  dit  deel  van  de  cursur  zullen  achtereenvolgens  de 
          volgende onderwerpen worden besproken:
          
          1) De compiler directives 'nonrec' en 'recursive'
          2) De compiler directives 'noregalo' en 'regalo'
          3) Algemene wenken voor het maken van snelle code
          4) De werking van XESCO
          5) Het gebruik van XESCO
          6) Aanwijzingen m.b.t. de source van XESCO
          
          [Nvdr.  Omdat XelaSoft's  Code Optimizer een beetje lang is, 
          heb ik de afkorting XESCO verzonnen.]
          
          
               1  )  D E   C O M P I L E R   D I R E C T I V E S 
          
                     N O N R E C   E N   R E C U R S I V E 
          
          Normaal  gaat  de  compiler  ervan  uit  dat  alle  functies 
          recursief zijn.  Daarom wordt  de code zo aangemaakt dat een 
          functie  alle locale variabelen op de stack bijhoudt. Dit is 
          echter  niet  bevordelijk  voor  de snelheid.  Aangezien ook 
          ASCII dit  heeft ingezien, hebben ze hier een oplossing voor 
          verzonnen:  Als  je  weet  dat  een  bepaalde  functie  niet 
          recursief is dan kun je dit aan de compiler vertellen met de 
          compiler directive 'nonrec'.
          
          V O O R B E E L D 
          
          Stel je  hebt de  volgende functie  die de som van een lijst 
          getallen berekent
          
            int sum(lijst, aantal) 
            int *lijst; 
            int aantal; 
            { 
              int sum;
          
              while (aantal--) 
                sum += *(lijst++); 
              return sum; 
            }
          
          Deze functie  is niet  recursief (ze roept zichzelf namelijk 
          nergens aan, ook niet via een omweg).
          
          Om  dit aan  de C compiler door te geven dient de header van 
          de functie vervangen te worden door:
          
            nonrec int sum(lijst, aantal)
          
          
          Als je wilt hebben dat de compiler vanaf een bepaald punt in 
          de source voor alle functies niet recursieve code genereert, 
          dan kan dit met een compiler directive, namelijk:
          
            #pragma nonrec     /* alle functies vanaf
                                          hier zijn niet recursief */
          
          En dit kan weer worden uitgezet met:
          
            #pragma recursive  /* alle functies vanaf
                                          hier zijn recursief      */
          
          
               2  )  D E   C O M P I L E R   D I R E C T I V E S 
          
                      N O R E G A L O   E N   R E G A L O 
          
          De     compiler     van     ASCII    doet     normaal    aan 
          register-optimalisatie,  dit houdt  in dat locale variabelen 
          zoveel mogelijk  in de registers worden bijgehouden. Pas als 
          dit  niet kan  (er zijn  bijv. teveel variabelen tegelijk in 
          gebruik),  dan  worden  sommige  variabelen in  het geheugen 
          bijgehouden. Dit  laatste kan dus op twee manieren gebeuren, 
          namelijk  op de  stapel (bij  recursieve procedures)  of via 
          directe  adressering  in  het  'normale' geheugen  (bij niet 
          recursieve procedures).
          
          Soms kan de register-optimalisatie misgaan, in dit geval kan 
          de register-optimalisatie worden uitgezet met:
          
            #pragma noregalo /* vanaf hier wordt geen register-
                                          optimalisatie gebruikt */
          
          Het weer aanzetten kan vervolgens met:
          
            #pragma regalo   /* vanaf hier wordt wel register-
                                          optimalisatie gebruikt */
          
          Een voorbeeld  waarin de register-optimalisatie misgaat (uit 
          de C handleiding).
          
            int n; 
            int *p;
          
            p = &n; 
            n = 10; 
            *p = 100; 
            printf("%d", n);
          
          In dit  geval zijn er slechts 2 variabelen, namelijk n en p. 
          Deze kunnen dus in registers worden bijgehouden. De compiler 
          zal  de waarde  10 dan  ook in  het register zetten waarin n 
          wordt bijgehouden  en dit  register vervolgens doorgeven bij 
          de printf instructie. Het feit dat de echte waarde van n dan 
          via de pointer p is veranderd ziet de compiler niet. Dit zou 
          wel goed gaan als de code er als volgt uit had gezien:
          
            int n; 
            int *p;
          
            n = 10; 
            p = &n;       /* nu zijn n = 10 en p = &n verwisseld */
            *p = 100; 
            printf("%d",n);
          
          De  compiler zal nu namelijk de waarde van n in het geheugen 
          opslaan voordat in de pointer p het adres van de variabele n 
          wordt gestopt,  en bij  de printf  zal de  waarde van deze n 
          weer worden opgehaald.
          
          Bij  de register-optimalisatie  kijkt de  compiler naar  het 
          totale  variabelen   gebruik  binnen  een  functie.  In  het 
          algemeen  is het  zo dat  als er  binnen een lus maar weinig 
          variabelen worden gebruikt, dat dan de variabelen binnen die 
          lus in  registers worden  bijgehouden. Buiten  de lus kan de 
          register-optimalisatie weer anders zijn.
          
          Verder  is het  zo dat  de compiler  bij korte functies vaak 
          beter herkent  wat in  registers kan dan bij grote, complexe 
          functies.  Het  is  daarom verstandig  om een  grote functie 
          zoveel  mogelijk op te splitsen in kleinere functies die dan 
          worden  aangeroepen  vanuit  die  grote functie  (vanuit het 
          standpunt  van  gestructureerd programmeren  bekeken is  dit 
          toch al aan te bevelen!).
          
          
                 3  )  A L G E M E N E   W E N K E N   V O O R 
          
               H E T   M A K E N   V A N   S N E L L E   C O D E 
          
          Kort  samengevat is de algemene werkwijze voor het maken van 
          snelle code als volgt:
          
          - Geef aan welke functies niet recursief zijn zodat de 
            compiler bij deze functies de variabelen niet op de stack 
            hoeft bij te houden.
          
          - Laat de register-optimalisatie zoveel mogelijk aanstaan. 
            Mocht de register-optimalisatie in een zeldzaam geval 
            misgaan, kijk dan of je de code kunt herschrijven. Gebruik 
            de '#pragma noregalo' directive pas als het niet anders 
            kan.
          
          - Maak liever veel kleine functies dan een paar grote. Let 
            er bij de opsplitsing in kleine functies wel op, dat je 
            geen extra complexiteit introduceert doordat je een 
            ingewikkeld algoritme te ver probeert op te splitsen!
          
          
                 4  )  D E   W E R K I N G   V A N   X E S C O 
          
          Als bovenstaande  werkwijze wordt gevolgd, kan er behoorlijk 
          snelle  code worden  verkregen. Het  kan echter nog sneller. 
          Zoals  namelijk  ook  al  in  deel  1 stond,  maakt de  code 
          generator alleen  gebruik van  instructies die de Intel 8080 
          kent.   Hierdoor  worden   sommige  dingen  een  beetje  dom 
          aangepakt  (vanuit  Z80  standpunt  bekeken), een  voorbeeld 
          hiervan is het volgende stuk code:
          
                  push    hl 
                  ld      hl,(variable) 
                  ld      c,l 
                  ld      b,h 
                  pop     hl
          
          Dit kan op de Z80 natuurlijk een stuk eenvoudiger, op de Z80 
          kan het namelijk als volgt:
          
                  ld      bc,(variable)
          
          Om  dit  soort  stukken  Intel 8080  code te  vervangen door 
          equivalente  Z80  instructies  heb  ik  een  code  optimizer 
          geschreven. Deze  optimizer kan  tussen de code generatie en 
          de  assemblatie  in  komen.  De  C  batch  file  kan  er dan 
          bijvoorbeeld als volgt komen uit te zien:
          
          (Staat op disk als C.BAT.)
          
          cf %1 
          cg -k %1 
          optimize %1.mac %1.opt 
          del %1.mac 
          ren %1.opt %1.mac 
          m80 =%1 
          del %1.mac 
          l80 ck,%1,clib/s,crun/s,cend,%1/n/e:xmain
          
          XESCO  kan  de meeste  8080 LD  instructie-groepen vervangen 
          door de  juiste Z80  LD instructie.  Verder worden  de shift 
          instructies  iets  effici�nter  opgelost.  Normaal  maakt de 
          compiler  voor  iedere  shift instructie  een CALL  naar een 
          systeemroutine  (uit CRUN.REL).  De routines  die hier staan 
          zijn  dan  ook nog  op zo'n  algemene manier  geschreven dat 
          zelfs een >>0 of <<0 goed wordt uitgevoerd. Dit introduceert 
          echter extra overhead (namelijk de CALL en de RET instructie 
          en  de  controle op  een 0-count).  Daarom vervangt  de code 
          optimizer een call naar een shift instructie door een stukje 
          code  dat  deze shift  instructie uitvoert.  Hierbij kan  de 
          optimizer 2 verschillende stukken code aanmaken:
          
          - Code die wel controleert op een 0-count (het standaard 
            geval)
          
          - Code die niet controleert op een 0-count. Om dit 2de geval 
            te krijgen dien je de optie /z op te geven. Doe dit alleen 
            als je zeker weet dat er geen shift over een afstand van 0 
            bits kan voorkomen!
          
          Verder is  het zo  dat de code optimizer ook nog controleert 
          of  je een  shift over  een constante  afstand doet.  Als je 
          namelijk over  een afstand  van 1 of van 2 schuift (dus >>1, 
          >>2,  <<1 of <<2), dan wordt er geen lus gemaakt om de shift 
          instructie uit  te voeren,  maar er worden een paar register 
          shift instructies achter elkaar gezet.
          
          Behalve  deze  Z80-optimalisaties  kent  XESCO ook  nog twee 
          R800-optimalisaties. Deze  kunnen worden  geactiveerd met de 
          optie  /R. Als  deze optie is opgegeven worden namelijk alle 
          calls naar de multiply routines vervangen door R800 multiply 
          instructies. In  dit geval  is de  code uiteraard wel alleen 
          nog maar geschikt voor de MSX turbo R.
          
          
                5  )  H E T   G E B R U I K   V A N   X E S C O 
          
          Het gebruik van de code optimizer is als volgt:
            OPTIMIZE sourcefile destinationfile [optionele parameters]
          
          Hiermee  wordt  het  bestand 'sourcefile'  ingelezen, en  de 
          geoptimaliseerde   code  wordt   weggezet  in   het  bestand 
          'destinationfile'.
          
          De optionele parameters zijn:
          /Z: controleer niet op een 0-count bij de shift instructies 
          /R: gebruik de multiply instructie op de R800
          
          Op  deze  disk  staan  2  versies  van  de  code  optimizer, 
          namelijk:
          
          OPTD1.COM: de optimizer gecompileerd met MSX C 1.1, voor 
                     onder MSX-DOS 1.
          OPTD2.COM: de optimizer gecompileerd met MSX C 1.2, voor 
                     onder MSX-DOS 2
          
          
               6  )  A A N W I J Z I N G E N   M . B . T .   D E 
          
                        S O U R C E   V A N   X E S C O 
          
          Behalve de gecompileerde versies, staat ook de source van de 
          optimizer op de disk, deze source bestaat uit 2 bestanden:
          OPTIMIZE.C: de source van de optimizer
          PCMSX.H   : een headerfile om de optimizer zowel op de PC 
                      als op de MSX te kunnen compileren.
          
          Deze  source  is  public domain,  ze dient  ter studie  ende 
          vermaak  en mag  NIET gewijzigd worden verspreid (veranderen 
          voor eigen  gebruik mag, maar verspreid de veranderde source 
          en  de object code die erbij hoort niet!). Laat het me weten 
          als je nog interessante idee�n hebt voor toekomstige versies 
          van XESCO zodat ze verwerkt kunnen worden. Ik ben bereikbaar 
          op   onderstaand   adres  of   via  email   op  het   adres: 
          wulms@stpc.leidenuniv.nl
          
                                                           Alex Wulms
