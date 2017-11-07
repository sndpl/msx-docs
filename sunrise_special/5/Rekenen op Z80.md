                          R E K E N E N   O P   Z 8 0 
                                                       
          
          Vaak kijk ik  met  afgunst  naar  de  MC68000.  Deze  fraaie
          processor kan hardwarematig vermenigvuldigen  en  delen.  De
          R800 kan alleen dat eerste,  maar  doet  dat  toch  redelijk
          snel. De Z80 kent geen van beide  en  is  daarmee  vreselijk
          beperkt.  Vermenigvuldigen  is  nl.  een  zeer   elementaire
          rekenkundige aktie en is bij het  minste  of  het  geringste
          nodig.  Voor  de  Z80  zijn  er  daarom  speciale   vermenig
          vuldigingsroutines bedacht. Zo is er in de MSX bv.  de  Math
          Pack die ook nog vele andere rekenkundige funkties kent.
          
          Voordelen van de Math Pack:
          - Groot bereik in berekeningen
          - Grote nauwkeurigheid mogelijk
          
          Nadelen:
          - Zo traag als een jichtige slak
          
          Dat traagheid van de  Math  Pack  maakt  hem  per  definitie
          ongeschikt voor bv. toepassing in demo's. Omdat ik toch  wat
          wiskundige  geintjes  in  ML  wilde  maken   was   het   dus
          noodzakelijk om snel te kunnen vermenigvuldigen. Ik ben toen
          aan het programmeren geslagen met  als  resultaat  een  stel
          ....  trage  vermenigvuldigingsroutines.  Een  paar  maanden
          later keek ik weer eens naar deze routines en besloot om het
          nog eens te proberen. De resultaten van dat werk zijn in dit
          artikel te vinden.
          
          
                              D E   T H E O R I E 
          
          Er zijn  twee  manieren  om  twee  getallen  met  elkaar  te
          vermenigvuldigen. De eerste is wat je vroeger in  de  eerste
          klas lagere school deed; herhaald optellen.
          
          Vb.  10 * 4 = 40
               10 + 10 + 10 + 10 = 40
          
          Eenvoudiger kan niet. Een praktische routine  zal  met  deze
          methode erg klein  worden,  maar  wordt  erg  traag  als  de
          lusteller   (4   in   het   voorbeeld)   groot   wordt.   De
          berekeningstijd is dus sterk afhankelijk van  de  invoer  en
          kan  ook  nog  eens  extreem  oplopen.  Het  nut  van   deze
          berekeningsmethode is dus zeer beperkt.
          
          Een betere methode is de volgende:
          
          Vb: 10 * 4
          
          10  = A = 00001010B   4 = B = 00000100B
          
          Bit A
           0  0  C = B*2^0 * 0 = 4*01*0 = 0
           1  1  C = B*2^1 * 1 = 4*02*1 = 8
           2  0  C = B*2^2 * 0 = 4*04*0 = 0
           3  1  C = B*2^3 * 1 = 4*08*1 = 32
           4  0                           0
           5  0  C = B*2^5 * 0 = 4*32*0 = 0
           6  0                           0
           7  0  C = B*2^7 * 0 = 4*128*0= 0
                                          -- +
                                          40
          
          Als je niet ziet hoe het werkt, probeer  het  dan  eens  met
          andere getallen. Indien een bit in A hoog is wordt  bij  het
          antwoord B*2^bitnummer_van_A opgeteld. De  voordelen  t.o.v.
          de eerste methode liegen er niet om:
          - Berekeningstijd is maar weinig afhankelijk van de invoer.
          - Levert snelle korte code op.
          
          Het zal dan ook niemand verbazen dat dit een veel  gebruikte
          methode is. Een vermenigvuldiging in PASCAL of C zal meestal
          zo uitgevoerd worden.
          
          
              8   B I T S   V E R M E N I G V U L D I G I N G E N 
          
          R O U T I N E   1 
          
          ; A = B.C
          A=BMLC: LD   A,0             ; 7
          BMLC.0: ADD  A,C             ; 4
                  DJNZ BMLC.0          ; B<>0 THEN 13 ELSE 8
                  RET                  ; 10
          
          De getallen achter de instrukties stellen het  resp.  aantal
          klokpulsen dat de instruktie duurt voor. Met  deze  gegevens
          is nu een formule  op  te  stellen  over  de  duur  van  een
          berekening.
          
          t = init + B * luslengte + exit + wait states
          
          MSX is helaas behept met een wait state. Dat houdt in dat de
          Z80 na elke instruktie 1 klokpuls wacht  voordat  doorgegaan
          wordt. deze  vertraging  is  daarom  ook  in  de  berekening
          opgenomen.
          
          t = 7 + 17B - 5 + 10 + 2 + 2B
            = 19B + 14
          
          t   =  Tijdsduur van routine in klokpulsen
          7   =  LD A,0
          17B =  B * (ADD A,C + DJNZ label)
          -5  =  Laatste DJNZ correctie die 5 klokpulsen sneller is.
          10  =  RET
          2   =  LD A,0 en RET wait states
          2B  =  ADD A,C + DJNZ label wait states
          
          Het aantal microseconden dat de routine duurt kan  eenvoudig
          gevonden worden door t door 3.57 te delen.
          
          
          R O U T I N E   2 
          
          ; A= A.C
          A=AMLC: LD   B,8             ; 7
                  EX   AF,AF           ; 4
                  LD   A,0             ; 7
                  EX   AF,AF           ; 4
          AMLC.0: RRCA                 ; 4
                  JP   NC,AMLC.1       ; 10
                  EX   AF,AF           ; 4
                  ADD  A,C             ; 4
                  EX   AF,AF           ; 4
          AMLC.1: RLC  C               ; 8
                  DJNZ AMLC.0          ; B<>0 THEN 13 ELSE 8
                  EX   AF,AF           ; 4
                  RET                  ; 10
          
          twc = 'worst case' berekeningstijd (A = 11111111B)
              = 22 + 8(14+12+21) - 5 + 14 + 62
              = 31 + 376 + 62
              = 469
          Twc = 469/3,57 = 131 us (microseconden)
          
          tbc = 'best case' berekeningstijd (A = 00000000B)
              = 22 + 8(14+21) - 5 + 14 + 38
              = 31 + 280 + 38
              = 349
          Tbc = 349/3,57 = 98 us
          
          Omdat de berekeningstijd altijd afhankelijk is van de invoer
          (maar in mindere mate dan bij routine 1) is er een worst  en
          best case berekening nodig.
          
          De routine begint met (22) en eindigd (14)  met  een  aantal
          instrukties. Daarnaast  is  er  ook  nog  de  loop  die  per
          definitie 8 keer doorlopen wordt. Het aantal klokpulsen  van
          de lus moet dus met 8  vermenigvuldigd  worden.  De  laatste
          loop duurt echter 5 klokpulsen korter en  deze  worden  weer
          bij het resultaat afgetrokken. Natuurlijk moeten ook hier de
          wait states weer bij de berekening betrokken worden.
          
          Het kan echter nog beter dan dit.
          
          R O U T I N E   3 
          
          ; A = A.C
          A=AMLC: LD   D,A             ; 4
                  LD   A,0             ; 7
                  LD   B,8             ; 7
          AMLC.0  RRC  D               ; 8
                  JP   NC,AMLC.1       ; 10
                  ADD  A,C             ; 4
          AMLC.1  RLC  C               ; 8
                  DJNZ AMLC.0          ; B<>0 THEN 13 ELSE 8
                  RET                  ; 10
          
          twc = 18 + 8(18+4+8+13) - 5 + 10 + 44
              = 23 + 344 + 44
              = 411
          Twc = 411/3,57 = 115 us
          
          tbc = 18 + 8(18+8+13) - 5 + 10 + 36
              = 23 + 312 + 36
              = 371
          Tbc = 371/3,57 = 104 us
          
          Deze  routine  heeft  slechtere  best  case  prestaties  dan
          routine 2, maar de worst case prestaties  zijn  beter.  Deze
          twee getallen liggen ook dichter bij elkaar dan bij  routine 
          2 waardoor de berekeningstijd dus constanter  is. Daarmee is
          routine 3 beter dan routine 2.
          
          Routine 1 is sneller dan routine 3 indien:
          
          14 + 19B = 411
          19B = 397
          B = 20.9 ==> 21
          
          Sneller indien B<21 (worst case)
          
          
             1 6   B I T S   V E R M E N I G V U L D I G I N G E N 
          
          Bij deze manier worden niet twee 16 bits getallen met elkaar
          vermenigvuldigd, maar een 8 bits en een 16 bits  getal.  Een
          echte 16 bit * 16  bit  vermenigvuldiging  levert  een  veel
          tragere routine op en is daarmee vaak ongeschikt.
          
          
          R O U T I N E   4 
          
          ; HL = B.DE
          BMULDE: LD   HL,0            ; 10
          B.DE.0  ADD  HL,DE           ; 11
                  DJNZ B.DE.0          ; B<>0 THEN 13 ELSE 8
                  RET                  ; 10
          
          t = 10 + B(11 + 13) - 5 + 10 + 2 + 2B
            = 26B + 17
          
          
          R O U T I N E   5 
          
          ; HL = A.HL
          AMULHL: LD   B,8             ; 7
                  LD   DE,0            ; 10
          A.HL.0: RRCA                 ; 4
                  JP   NC,A.HL.2       ; 10
                  EX   DE,HL           ; 4
                  ADD  HL,DE           ; 11
                  EX   DE,HL           ; 4
          A.HL.1: ADD  HL,HL           ; 11
                  DJNZ A.HL.0          ; B<>0 THEN 13 ELSE 8
                  EX   DE,HL           ; 4
                  RET                  ; 10
          
          twc = 17 + 8(14 + 19 + 11 + 13) - 5 + 14 + 36
              = 542
          Twc = 542/3,57 = 152 us
          
          tbc = 17 + 8(14 + 11 + 13) - 5 + 14 + 36
              = 366
          Tbc = 366/3,57 = 103 us
          
          
          R O U T I N E   6 
          
          ; HL = A.DE
          AMULDE: LD   B,8             ; 7
                  LD   HL,0            ; 10
          A.DE.0: RRCA                 ; 4
                  JP   NC,A.DE.1       ; 10
                  ADD  HL,DE           ; 11
          A.DE.1  SLA  E               ; 8
                  RL   D               ; 8
                  DJNZ A.DE.0          ; B<>0 THEN 13 ELSE 8
                  RET                  ; 10
          
          twc = 27 + 8(14 + 11 + 29) - 5 + 51
              = 505
          Twc = 505/3,57 = 142 us
          
          tbc = 27 + 8(14 + 29) - 5 + 43
              = 409
          Tbc = 409/3,57 = 115 us
          
          
                      N E G A T I E V E   G E T A L L E N 
          
          De  genoemde  routines  zijn   ongeschikt   voor   negatieve
          getallen. Indien de hoogste bits  van  beide  invoergetallen
          nl. als tekenbit gebruikt worden, zal overflow natuurlijk om
          de  haverklap  optreden.  Jammer  genoeg  is  het  voor  bv.
          vectorgraphics wel van essentieel belang dat  met  negatieve
          getallen gerekend kan worden. Het is echter wel mogelijk  om
          de  routines  wat  uit  te  breiden  waardoor  dit  probleem
          opgelost wordt.
          
          Het is niet helemaal waar dat de routines niet met negatieve
          getallen om kunnen gaan. In het geval van routine  6  mag  A
          bij invoer niet negatief zijn, maar DE wel. Dit  komt  omdat
          DE per lus alleen maar met 2 vermenigvuldigd wordt,  waarbij
          eventuele overflow  geen  invloed  heeft.  Dat  levert  voor
          routine 6 de volgende routine op.
          
          ; In : A , Integer parameter 1
          ;      DE, Real parameter 2
          ; Out: HL, Real antwoord
          SAMLDE: BIT  7,A
                  JP   Z,AMULDE        ; (Met LD B,7)
                  NEG                  ; Maak A positief
                  CALL AMULDE          ; (Met LD B,7)
                  LD   DE,0
                  EX   DE,HL
                  OR   A
                  SBC  HL,DE           ; Inverteer antwoord
                  RET                  ; omdat A ook ge�nverteerd is.
          
          Deze methode heeft drie consequenties:
          - De routine wordt wel iets trager (meer 'overhead')
          - Het bereik wordt verschoven naar -32768 tot 32768
          - De loopcounter in AMULDE kan nu van 8 naar 7 gezet worden
            omdat bit 7 van A altijd 0 zal  zijn.  Hierdoor  wordt  de
            extra overhead weer teniet gedaan.
          
          Met deze routine erbij is het gebruik van negatieve getallen
          geen probleem meer, terwijl  het  geheel  amper  trager  zal
          worden.
          
          
            Het volgende deel van dit artikel staat in het submenu.
          
          
          
                    Het eerste deel vindt u in het submenu.
          
          
                       P R A K T I S C H   G E B R U I K 
          
          Het grote nadeel van  alle  genoemde  routines  is  dat  het
          bereik t.o.v. de Math Pack relatief klein is. Daar komt  nog
          bij dat het moeilijk is om op  overflow  fouten  te  testen.
          Dergelijke tests zijn niet ingebouwd omdat dat  de  routines
          aanzienlijk trager maakt. Je kunt je dus  afvragen  wat  het
          pratische  nut  van  de  routines  is.  Welnu,   dat   hangt
          natuurlijk  helemaal  van  de  applicatie  af.  Zo  kan  bv.
          worteltrekken beter via de Math Pack gedaan worden.  Is  het
          de bedoeling dat een  kogeltje  in  een  schietspel  in  een
          rechte lijn van punt A naar punt B gaat, dan zijn juist deze
          routines weer uitermate geschikt.
          
          Een groot nadeel is dus het feit  dat  je  door  het  kleine
          bereik enorm op overflow situaties op moet passen. Zou je in
          routine   6   de   getallen   200   en   400   met    elkaar
          vermenigvuldigen,  dan  is  dat  al  meer  dan  65535   (het
          theoretische maximum).
          
          Overflow is echter te voorkomen  door  alleen  maar  8  bits
          getallen met elkaar te vermenigvuldigen. Het produkt  van  2
          8-bits getallen kan nl. maximaal  16  bits  groot  zijn.  De
          genoemde 8x16 bits routines kunnen dus  het  beste  als  8x8
          bits routines gebruikt worden. Overflow is  dan  onmogelijk.
          De 8x8 bits routines hebben met hun 8 bits antwoord nog  wel
          last van overflow.
          
          Het gebruik van deze routines moet in de grafische  omgeving
          gezocht  worden.  Neem  bv.  SCREEN  5,  waar  de   maximale
          resolutie 256x256 pixels is. Omdat  de  resolutie  binnen  8
          bits past is het dus mogelijk om de vermenigvuldigingen voor
          bv. pixeleffekten en zelfs vectorgraphics te gebruiken  (elk
          pixel valt binnen het bereik van de routines).
          
          
               A L T E R N A T I E V E   G E T A L N O T A T I E 
          
          Het vermenigvuldigen  van  twee  integers  (een  heel  getal
          tussen 0 en 255) is geen enkel probleem met  deze  routines,
          maar daar bereik je niet veel mee.  Om  een  goed  praktisch
          gebruik mogelijk  te  maken  is  een  andere  zienswijze  op
          hexadecimale getallen nodig.
          
          1000H = 16*256 = 4096
          Dit klopt als een bus, maar wat gebeurt er als we een  komma
          midden in het hexadecimale getal zetten ?
          
          10,00H = 16,00 = 16
          Ook dit is geen probleem. We beschouwen het HI-byte van  het
          getal als integer en het LO-byte als fraktie. Hiermee is  de
          mooie 16 bits integer verandert in een 16 bits real.
          
          Als we de zaken nu eens omdraaien. Hoe representeer  je  het
          getal 13,5 (decimaal) in hexadecimale vorm ?
          
          13,5 = 0D80H
          13   = 0DH, dit lijkt me geen probleem.
           0,5 = 80H, dit is wat lastiger.
          
          Een integer in hexadecimale notatie  heeft  dezelfde  waarde
          als dezelfde integer  in  decimale,  binaire  of  elk  ander
          willekeurig talstelsel. Hoe anders is  dit  bij  een  breuk,
          waar  het  getal  achter  de  komma  afhankelijk   van   het
          talstelsel is.
          
          Als we twee getallen achter de komma afspreken (dat past nl.
          mooi in ��n byte),  dan  kan  een  decimaal  getal  dus  100
          mogelijke  frakties  hebben   (0,00   t/m   0,99)   en   een
          hexadecimaal getal 256 (0,00 t/m 0,FF).  De  omrekening  van
          een decimale naar een hexadecimale fraktie gebeurt  door  de
          decimale fraktie met 2,56 te vermenigvuldigen.
          
          Vb: 0,5 ==> 50 ==> 50*2,56 = 128 ==> 0080H
              0,5 ==>        0,5*256 = 128 ==> 0080H
          
              20,23 decimaal is dan 143BH (afgerond)
              10,25 decimaal is dan 0A40H
              (Ter vergelijking: 1025 decimaal = 0401H)
          
          Het leuke is dat  een  hexadecimale  fraktie  2,56  keer  zo
          nauwkeurig is als een  decimale  (256  mogelijkheden  i.p.v.
          100).
          
          De mogelijkheden die het gebruik van een  fraktie  met  zich
          mee brengt zijn buitengewoon aardig. Om te beginnen  kunnnen
          we nu een integer tussen 0 en 255 vermenigvuldigen  met  een
          waarde die ligt tussen 0,0 en 0,FFH in  stappen  van  0,01H.
          Indien ook negatieve getallen mogelijk zijn loopt het bereik
          van -80,00H t/m 7F,FFH. Een praktische aanroep ziet  er  als
          volgt uit.
          
                  LD   A,200
                  LD   DE,&H0040
                  CALL AMULDE
                  (HL is nu 3200H, H = 32H = 50)
          
          Deze routine voert de berekening 200*0,25 = 50 uit.  Let  op
          dat overflow  niet  voorkomt  omdat  twee  8  bits  getallen
          vermenigvuldigd worden tot een 16  bits  antwoord.  Bij  het
          antwoord is H het getal voor de komma en L de fraktie achter
          de komma.
          
          
                      N E G A T I E V E   F R A K T I E S 
          
          Ook met  deze  getalrepresentatie  zijn  negatieve  getallen
          mogelijk. Het inverteren van een reeds naar een fraktiegetal
          omgezette decimaal gaat gelukkig op precies dezelfde  manier
          als het omzetten van een pure integer.
          
          Vb: 100 = 64H
             -100 = 256 - 100 = 9CH
          
              10,23 ==> 0A3BH
             -10,23 ==> 10000H - 0A3BH = F5C5H
          
          Je moet hier dus wel oppassen dat ook  de  fraktie  van  een
          negatief getal anders is dan  die  van  hetzelfde  positieve
          getal. Het veranderen van de fraktie  moet  echter  gebeuren
          omdat de berekeningen anders in  de  soep  lopen.  Bovendien
          kunnen  positieve  en  negatieve  getallen   ook   willeurig
          opgeteld en afgetrokken worden.
          
          Vb:
              30,23 ==> 1E3BH
             -20,75 ==> EB40H
             ------ +   ----- +
               9,48     097BH
          
          7BH = 123 ==> 123/2,56 = 48
          
          De extra code die nodig is om met bv.  routine  6  negatieve
          getalberekeningen mogelijk  te  maken  werkt  ook  met  deze
          getalnotatie (het zou niet best zijn als  dat  niet  zo  zou
          zijn).
          
          
                         L I J N T J E   R E K E N E N 
          
          Omdat we aannemen dat  DE  een  breuk  is,  is  het  nu  bv.
          mogelijk om een lijn te 'berekenen', wat  overeen  komt  met
          een punt dat door berekening in een rechte lijn van  punt  A
          naar punt B gaat. Dat gaat als volgt. Een  lijn  kan  gezien
          worden als de schuine zijde van een  rechthoekige  driehoek.
          Daarmee heeft een lijn dus een delta X en een  delta  Y.  We
          nemen deze even gelijk en stellen dat  we  een  lijn  willen
          tekenen van coordinaat (0,0) tot (49,49).
          
                  LD   B,0             ; Lusteller
          LUS:    PUSH BC
                  LD   D,0             ; DE loopt van 0,0 tot 0,FFH
                  LD   E,B
                  LD   A,50            ; = delta X = delta Y
                  CALL AMULDE
                  LD   A,H
                  CALL PSET            ; Teken pixel op coord. (A,A)
                  POP  BC
                  DJNZ LUS
          
          Dit  theoretische  programmaatje zal  256  punten  berekenen
          tussen de coordinaten (0,0) en  (49,49).  Wat  er  feitelijk
          gebeurt is dat delta X en delta Y per lus met  een  oplopend
          getal vermenigvuldigd worden dat  tussen  DE=0  en  DE=00FFH
          ligt. Na de vermenigvuldiging staat de rest in  register  L,
          maar die waarde is hier niet  van  belang  (je  kunt  immers
          tussen twee pixels niet nog een pixel tekenen). Let  op  dat
          wanneer een lijn getekend wordt van (0,0) -  (5,5),  er  ook
          256 punten berekent worden. Er worden dus  256/5  pixels  op
          dezelfde positie berekend!  Om dat  probleem  op  te  kunnen
          lossen moet je maar eens aan Piet Apegras denken (lengte van
          schuine zijde).
          
          
                          I N T E G E R S   D E L E N 
          
          Delen  is  niets  anders  dan   vermenigvuldigen   met   het
          omgekeerde. Dat klinkt heel simpel, maar dat valt  praktisch
          wat tegen. Dit is overigens zeer recente informatie. Ik  heb
          de delingsmethode die ik hier ga noemen pas bedacht toen  ik
          met dit artikel bezig was (er ging mij een lichtje op).
          
          Vb:
          
                    50 / 13 = 3,8462
          Maar ook: 50 * 1/13 = 3,8462
          
          De deling kan uitgevoerd worden door de vermenigvuldigen met
          1/13 i.p.v. delen door 13. Aangenomen  dat  we  alleen  maar
          integers door  integers  willen  delen  (met  een  real  als
          antwoord) kan een tabel gemaakt worden met  de  waarden  1/2
          t/m  1/255.  Gebruik  makend  van  de  bij   onze   vermenig
          vuldigingen toegepaste fraktie zal die tabel  er  als  volgt
          uit zien.
          
          1/002 = 0,50
          1/003 = 0,67
          1/004 = 0,25
          1/010 = 0,10
          1/100 = 0,01
          1/110 = 0,00...
          1/128 = 0,00...
          1/200 = 0,00...
          1/255 = 0,00...
          
          Bij de wat grotere delers blijkt dat twee getallen achter de
          komma niet voldoende is. Vanaf 1/101 zal het antwoord altijd
          0 zijn. Je begrijpt, een deling zal  op  deze  manier  bijna
          nooit kloppen. Met twee extra decimalen gaat het al  stukken
          beter. 1/253 wordt dan 0,0040 en  1/255  wordt  0,0039.  Een
          omzetting naar een hexadecimale fraktie levert nu echter ook
          een 16 bits getal op dat echter wel 2,56 keer zo precies is.
          
          De berekening in ons voorbeeld moet nu als volgt  uitgevoerd
          worden:
          
          50/13 = 32H/13
          
          1/13 ==> (1/13)*256*256=5041 == 13B1H Er moet 2 keer met 256
          vermenigvuldigd worden omdat we een 4 decimalen  nauwkeurige
          fraktie willen hebben.  Het  antwoord  is  nu  een  16  bits
          fraktie.
          
          Berekening gaat nu als volgt:
          
          32H * B1H =   2292H
          32H * 13H = 03B6H
                      ------- +
                      03D8H
          
          Terugrekening naar decimalen:
          D8H = 216 ==> 216/256 =   0,8438
          03H =   3 ==>             3
                                    ------ +
                                    3,8438
          Met de rekendoos: 50/13 = 3,8462
                                    ------ -
          Absolute fout:           -0,0024
          
          Het verschil wordt veroorzaakt door de  92H  van  de  eerste
          berekening die verwaarloosd wordt  en  afrondingsfouten  bij
          het omzetten van decimaal naar hexadecimaal. Deze methode is
          twee getallen achter de komma nauwkeurig en dat  is  precies
          genoeg.
          
          Om te kunnen delen moet dus eerst een tabel  gemaakt  worden
          met 16 bits frakties van de delingen 1/2  t/m  1/255  (delen
          door 0 mag niet, en delen door 1 levert  geen  fraktie  op).
          Daarnaast   moeten   om   ��n   deling    te    doen    twee
          vermenigvuldigingen en een optelling gedaan worden.
          
          Indien de nauwkeurigheid van het antwoord niet  van  uiterst
          belang is, kan de eerste vermenigvuldiging ook  overgeslagen
          worden (dus alleen 32H * 13H). De absolute fout  is  dan  1.
          Vb:
            8000H
          02F0H
          ------- +
          0370H
          
          Indien de eerste vermenigvuldiging  overgeslagen  wordt  zou
          het antwoord 02F0H zijn, zodat de absolute decimale fout 0,5
          zou worden. Het hangt van de applicatie af of dit gedaan kan
          worden, maar  het  levert  natuurlijk  wel  een  behoorlijke
          snelheidswinst op.
          
          Helaas is deze methode alleen maar geschikt om twee integers 
          te delen. Dat is dan ook meteen het nadeel van deze methode, 
          want  het maakt nogal wat uit of je 200 door 1 of 1,9 deelt. 
          Hoe  dit  het uiteindelijke  plaatje bij  bv. vectorgraphics 
          be�nvloedt  is  mij  (nog)  niet  bekend,  maar  ik  kan  me 
          situaties voorstellen  waarbij de deling van alleen integers 
          te  weinig nauwkeurigheid  biedt. Helaas zal een routine die 
          twee 'real' 16 bits getallen deelt ontzettend traag worden.
          
          
                               C O N C L U S I E 
          
          Gewapend met de vermenigvuldigingen en de deling is  het  nu
          mogelijk om 'real-time'  vector  graphics  te  maken  zonder
          tussenkomst van de Math Pack. De echte vector freaks  kunnen
          hun hart dus weer  eens  ophalen.  Ook  het  maken  van  een
          fractal berekeningsprogramma is met deze routines  mogelijk,
          maar dat is al door XelaSoft gedaan.
          
          Als je deze berekeningsroutines eens aan het werk wilt zien,
          dan moet je maar eens de Fuzzy Logic demo's 'Math Magicians'
          of 'MB Musax 1' bekijken.  De  bestuurbare  sterrenhemel  in
          deze produkten werkt  op  hetzelfde  principe  als  de  lijn
          berekening. De delta  X  en  Y  zijn  hier  per  lus  echter
          variabel waardoor de sterren  kunnen  draaien.  Dit  geintje
          wordt dus NIET bereikt door een enorme tabel in het geheugen
          op te slaan waar alle mogelijke posities in  staan.  Ook  de
          snelheid van de routines blijkt uit deze routines.  In  1/60
          seconde worden nl. 64 vermenigvuldigingen uitgevoerd  om  32
          sterren te besturen.
          
          
                                               Alex van der Wal
                                               F U Z Z Y   L O G I C 
