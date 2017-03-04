  V D P   C O M M A N D O R E G I S T E R S   I N   M L 
                                                                    
          
          Als  extraatje bij  de VDP cursus op herhaling deze keer een 
          tekst over  het gebruik  van de commando's van de VDP in ML, 
          met een zeer handige standaardroutine.
          
          De theorie  die in  deze tekst gebruikt wordt staat allemaal 
          in  VDP cursus  (op herhaling)  deel 1  t/m 4,  die resp. op 
          Sunrise  Special  #2  en  deze  Sunrise  Special #3  zijn te 
          vinden. Ik zal daar hier dus weinig aandacht aan besteden.
          
          
                                I N D I R E C T 
          
          U  kunt  de  VDP  een  commando  (copy,  line,  etc.)  laten 
          uitvoeren  door de  registers R#32  t/m R#46 te beschrijven. 
          Zodra R#46  met de  code voor het commando wordt beschreven, 
          zal de VDP het commando gaan uitvoeren.
          
          Was  de VDP  op dat moment echter nog met een ander commando 
          bezig, dan  zal het  in de  soep lopen.  Daarom moet  altijd 
          eerst  gecheckt worden of de VDP al klaar was met het vorige 
          commando, door bit 0 van statusregister S#2 te lezen.
          
          Het  schrijven  van  15  opeenvolgende  registers  gaat  het 
          makkelijkst met  de indirecte  methode. Hierbij schrijven we 
          eerst  het startregister  naar R#17,  en we  sturen de  data 
          daarna naar Port #3 (I/O adres &H9B). De waarde van R#17 zal 
          automatisch worden verhoogd.
          
          Het Z80  commando OTIR  is hiervoor  uitermate geschikt. Dit 
          commando  stuurt B  bytes uit  het geheugen vanaf adres (HL) 
          naar poort  (C). Hiervan  zullen we  in de  routine dan  ook 
          gebruik maken.
          
          Meer  dan  genoeg  theorie,  het  is  nu hoog  tijd voor  de 
          universele VDP  commando routine.  Ik heb hem vroeger DOCOPY 
          gedoopt,  omdat hij  het meest  voor copy's  wordt gebruikt, 
          maar dat is dus wel een beetje misleidende naam want hij kan 
          voor  alle  commando's  worden  gebruikt. Ik  ben zo  gewend 
          geraakt aan die naam dat ik hem ben blijven gebruiken.
          
          
                                  D O C O P Y 
          
          Hier  komt  de source,  zoals gewoonlijk  in mijn  favoriete 
          assembler (WB-ASS2).
          
          ; DOCOPY
          ; Voer VDP commando uit
          ; In: HL=start data
          ; Gebruikt: AF, AF', HL, BC
          
          DOCOPY: LD    BC,&H0F9B
                  CALL  VDPRDY
                  DI
                  LD    A,32
                  OUT   (&H99),A
                  LD    A,17+128
                  OUT   (&H99),A
                  EI
                  OTIR
                  RET
          
          Eerst  wordt  B  geladen  met  de  waarde  15 (we  willen 15 
          registers  beschrijven) en  C met &H9B (we willen naar poort 
          &H9B schrijven).  Een LD BC,.. commando is korter en sneller 
          dan  een LD  B,.. en een LD C,.. commando. De routine VDPRDY 
          (VDP ReaDY)  wacht tot de VDP klaar is met het uitvoeren van 
          een eventueel vorig commando dat nog bezig is.
          
          Vervolgens  wordt  het startregister  naar R#17  geschreven. 
          Zoals gewoonlijk  moeten de interrupts hierbij uit staan. De 
          interrupts  staan ook  bij de  OTIR nog  uit, want na een EI 
          duurt het  nog een instructie voordat de interrupts weer aan 
          gaan!  Dit is overigens niet strikt noodzakelijk, alleen als 
          er in  de interrupt  routine iets  met de VDP gebeurt moeten 
          de  interrupts bij  de OTIR ook per se uit staan. Simpel hä? 
          Nu de  routines VDPRDY en RDSTAT (die weer door VDPRDY wordt 
          aangeroepen) nog:
          
          
          ; VDPRDY
          ; Wacht tot VDP klaar is met command execute
          ; Gebruikt: AF, AF'
          
          VDPRDY: LD    A,2
                  CALL  RDSTAT
                  BIT   0,A
                  JP    NZ,VDPRDY
          
          ; RDSTAT
          ; Lees VDP statusregister
          ; In: A=S#
          ; Uit: A=data
          ; Gebruikt: AF'
          
          RDSTAT: DI
                  OUT   (&H99),A
                  LD    A,15+128
                  OUT   (&H99),A
                  NOP
                  NOP
                  IN    A,(&H99)
                  EX    AF,AF
                  XOR   A
                  OUT   (&H99),A
                  LD    A,128+15
                  OUT   (&H99),A
                  EI
                  EX    AF,AF
                  RET
          
          
          Dit  is  een  bekende routine,  dus verdere  uitleg is  hier 
          overbodig.  De interrupts  worden steeds  even weer aangezet 
          tijdens het  uitvoeren van VDPRDY, omdat anders bijvoorbeeld 
          de  muziek gaat  haperen. Het  zou in  principe ook mogelijk 
          zijn om S#2 constant te lezen, maar dan zouden de interrupts 
          uit  moeten  staan  zolang  het  vorige  commando  nog  niet 
          afgelopen is, en dat kan soms te lang duren.
          
          
                                P R A K T I J K 
          
          Een praktijkvoorbeeldje  maakt dit  wat duidelijker. Stel we 
          willen  het  blok  (0,0)-(49,99)  op  page  3  kopiâren naar 
          (20,70)  op page  1 met een high speed copy (HMMM). Dit gaat 
          dan als volgt:
          
          COPY:   LD    HL,COPDAT
                  JP    DOCOPY
          COPDAT: DB    0,0,0,3    ; source
                  DB    20,0,70,1  ; destination
                  DB    50,0,100,0 ; grootte
                  DB    0,0,&HD0   ; HMMM
          
          Voor  alle VDP commando's dus een blokje van 15 bytes maken, 
          het  gaat  het handigste  als je  het op  bovenstaande wijze 
          indeelt. Je  ziet dat  de page  nummers gewoon de high bytes 
          van  de y coîrdinaten zijn, dus het opgeven van de page gaat 
          heel simpel. &HD0 is de code voor HMMM (high speed move VRAM 
          to VRAM).
          
          
                                    H M M C 
          
          Tot  slot  nog  een voorbeeldje  van een  iets ingewikkelder 
          commando,  namelijk  HMMC.  Dit is  wel een  zeer belangrijk 
          commando,  omdat je hiermee een plaatje uit het RAM naar het 
          VRAM  kunt  kopiâren, waarbij  dit plaatje  een willekeurige 
          rechthoek mag zijn. Bij gewoon RAM naar VRAM kopiâren kun je 
          alleen een  bepaald adresbereik  van het VRAM vullen, nu dus 
          gewoon een rechthoek zoals bij een normale copy.
          
          De werking van HMMC wordt uitgelegd in deel 3 van de cursus. 
          In de voorbeeldroutine wordt er een zwart 8*8 blokje met een 
          witte  letter  A  naar  het  scherm  gekopieerd  op  positie 
          (124,102) op  page 0, dit werkt zowel op SCREEN 5 als SCREEN 
          7.  Ik gebruik hier de procedure zoals omschreven in het VDP 
          Technical Databook,  dezelfde procedure  die ik ook bij HMMC 
          in VDP cursus deel 3 heb gezet.
          
          
                  LD    A,(GRPDAT)
                  LD    (COPDAT+12),A   ; vul eerste byte alvast in
                  LD    HL,COPDAT
                  CALL  DOCOPY
          
          
          Hier  wordt  het  commando  al  naar  de  VDP  gestuurd.  De 
          grafische  data staat van GRPDAT, je ziet dat hier de eerste 
          byte alvast wordt meegegeven bij het geven van het commando. 
          Dit is overigens verplicht.
          
          
                  DI
                  LD    A,44+128        ; R#44, geen auto increment
                  OUT   (&H99),A
                  LD    A,17+128
                  OUT   (&H99),A
                  EI
          
          
          We  gebruiken nu  weer indirecte adressering om de data naar 
          de VDP  te sturen.  De data  moet namelijk  steeds naar R#44 
          worden  geschreven.  We  kunnen  de  indirecte  methode  ook 
          gebruiken  om steeds naar ÇÇn register te schrijven, waarbij 
          de auto increment van R#17 dus uit staat. Dit kan door bit 7 
          van R#17 te zetten (in de routine ziet u dus 44+128 staan).
          
          
                  LD    HL,GRPDAT+1
          COPY:   LD    A,2
                  CALL  RDSTAT
                  BIT   0,A
                  RET   Z
                  BIT   7,A
                  JP    Z,COPY
                  LD    A,(HL)
                  OUT   (&H9B),A
                  INC   HL
                  JP    COPY
          
          
          Hier  wordt volgens  de gegeven  procedure de data naar R#44 
          geschreven. Steeds  wordt S#2  gelezen. Bit 0 is CE (Command 
          Execute),  als dat bit 0 is, is het commando klaar. Bit 7 is 
          TR, dat  aangeeft of  de VDP klaar is voor de volgende byte. 
          Zo  nee, dan  lezen we  S#2 opnieuw, zo ja, dan sturen we de 
          volgende byte  naar Port  #3, en  dus naar  R#44. Dit  wordt 
          steeds herhaald totdat het klaar is.
          
          
          COPDAT: DB    0,0,0,0         ; source (hier niet gebruikt)
                  DB    124,0,102,0     ; destination
                  DB    8,0,8,0         ; grootte
                  DB    0,0,&HF0        ; HMMC
          
          
          Het blokje met de 15 bytes. De eerste bytes worden hier niet 
          gebruikt, dus  daar heb  ik gewoon  0 voor  ingevuld. Je zou 
          eventueel  ook  een  aangepaste DOCOPY  kunnen maken  die 11 
          bytes  stuurt naar  R#36-R#45, maar de tijdwinst die daarmee 
          behaald wordt is zo minimaal dat dat weinig nut heeft.
          
          Let op:  de grootte  wordt altijd  in pixels aangegeven, dus 
          niet  in bytes!  Ons blokje  van 8*8  is maar 4*8 bytes. Die 
          bytes volgen nu. Een 1 staat voor zwart, een F voor wit. Als 
          je goed  kijkt kun  je de hoofdletter A erin zien. Uiteraard 
          kan  hier elke  willekeurige data  worden ingevuld, het gaat 
          hier alleen om het voorbeeld.
          
          
          GRPDAT: DB    &H11,&H1F,&H11,&H11
                  DB    &H11,&HF1,&HF1,&H11
                  DB    &H1F,&H11,&H1F,&H11
                  DB    &HF1,&H11,&H11,&HF1
                  DB    &HF1,&H11,&H11,&HF1
                  DB    &HFF,&HFF,&HFF,&HF1
                  DB    &HF1,&H11,&H11,&HF1
                  DB    &HF1,&H11,&H11,&HF1
          
          
                   K A N   H E T   N I E T   S N E L L E R ? 
          
          Dit  werkt uitstekend,  maar je  vraagt je  toch af: kan het 
          niet  sneller?   Dit  was   de  procedure  zoals  die  wordt 
          voorgeschreven door het Technical Databook, maar volgens mij 
          mag het ook op onderstaande manier. Het werkt in ieder geval 
          prima.
          
          Het blijkt  namelijk dat  de VDP de data die naar R#44 wordt 
          geschreven behoorlijk snel kan verwerken (Nvdr: misschien is 
          de  R800 of  de Z80  wel te langzaam), waardoor het helemaal 
          niet nodig  is om  TR steeds te checken. En CE checken is al 
          helemaal  onzin, we weten immers wel hoeveel bytes we moeten 
          sturen. Dan kun je het ook als volgt programmeren:
          
                  LD    A,(GRPDAT)
                  LD    (COPDAT+12),A   ; vul eerste byte alvast in
                  LD    HL,COPDAT
                  CALL  DOCOPY
                  DI
                  LD    A,44+128        ; R#44, geen auto increment
                  OUT   (&H99),A
                  LD    A,17+128
                  OUT   (&H99),A
                  EI
          
          
          Tot hier is het exact hetzelfde. Nu komt het gedeelte dat is 
          veranderd: 
          
          
                  LD    HL,GRPDAT+1
                  LD    BC,&H1F9B       ; 31 bytes naar port #3
                  OTIR
                  RET
          
          
          Dat is  alles! (Ik laat COPDAT en GRPDAT nu achterwege, zijn 
          toch  hetzelfde.) We  moesten in totaal 4*8=32 bytes sturen, 
          waarvan  er  ÇÇn  al  is  verstuurd  bij het  geven van  het 
          commando. Die  bytes moeten  naar Port  #3 worden  gestuurd, 
          ofwel  I/O  poort  &H9B. De  data staat  vanaf GRPDAT+1  (we 
          hadden  de eerste byte immers al verstuurd), dus is een OTIR 
          voldoende!  Bij  copy's  groter  dan 256  bytes kun  je OTIR 
          natuurlijk niet  gebruiken, maar  dat kan  als volgt  worden 
          opgelost:
          
                  LD    HL,GRPDAT+1
                  LD    BC,aantal bytes
          COPY:   LD    A,(HL)
                  OUT   (&H9B),A
                  INC   HL
                  DEC   BC
                  LD    A,B
                  OR    C
                  JP    NZ,COPY
          
          
          Als  je  'normaal' RAM  naar VRAM  wilt verplaatsen,  zet je 
          eerst de  VDP klaar  om VRAM te schrijven vanaf het gewenste 
          adres, en daarna stuur je de data naar Port #0 (&H98).
          
          Op de 'copy manier' (met HMMC dus) gaat het dus net zo snel, 
          en  het werkt analoog. Eerst de VDP een HMMC klaar zetten om 
          de data te ontvangen (het HMMC commando geven met DOCOPY) en 
          daarna de  data naar  Port #3 schrijven (&H9B). Het voordeel 
          hiervan  is  dat  je  nu  niet tot  een bepaald  adresgebied 
          beperkt  bent.  Als  je bijvoorbeeld  het gebied  (100,100)- 
          (199,199) met data uit het RAM zou willen vullen, zou je bij 
          de  'normale' manier voor elke lijn opnieuw de VDP in moeten 
          stellen, terwijl dat met HMMC niet nodig is.
          
          HMMC  is dus  een zeer handig commando, vooral als je in een 
          programma een  keer VRAM  te kort  komt. Je kunt de gewenste 
          graphics dan steeds razendsnel met HMMC uit het RAM halen.
          
          Veel succes  met je  eigen VDP  routines en  tot de volgende 
          keer!
          
                                                          Stefan Boer
          