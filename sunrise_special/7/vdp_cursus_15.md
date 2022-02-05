                 M S X 2 / 2 +   V D P   C U R S U S   ( 1 5  )
                                                                
          
          Sunrise Magazine  mag dan  wel het tweede lustrum vieren, ik 
          vier  met de VDP cursus alweer het derde! Ik had beloofd het 
          deze keer over sprites te gaan hebben, maar stel dat uit tot 
          deel 16.
          
          In  plaats  daarvan  bespreek  ik deze  keer een  routine om 
          SCREEN 2 naar SCREEN 5 om te zetten, dit naar aanleiding van 
          een brief  van Bert  Hoogerdijk uit IJsselstein (zie SRM#9). 
          Hij  gebruikte een  zeer langzaam  programma (40 minuten per 
          plaatje!) uit  het Belgenblad,  en vroeg zich af of het niet 
          sneller  kon. Nou,  dat kan!  Heeft u  ook eens  zo'n vraag, 
          schrijf dan gerust, ik geef graag antwoord!
          
          
                                S C R E E N   2 
          
          Het  is alweer  een tijdje  geleden dat  we de opslag van de 
          schermen in  het VRAM  hebben behandeld,  dus als u het niet 
          meer  weet: zoek  het even  op in  een oud  deel van  de VDP 
          cursus.
          
          Omdat  SCREEN 2  vrij ingewikkeld  in elkaar zit, leg ik het 
          hier nog  maar eens uit. De beeldinformatie staat bij SCREEN 
          2 in drie tabellen:
          
          patroon generator tabel &H0000-&H17FF   (6144 bytes)
          patroon naam tabel      &H1800-&H1AFF   (768  bytes)
          kleurtabel              &H2000-&H37FF   (6144 bytes)
          
          SCREEN 2  is opgebouwd  uit 24  rijen van  elk 32  posities, 
          samen  768 positie.  In de naamtabel staat voor elke positie 
          welk karakter  er staat.  De vorm  en kleur van de karakters 
          staan in de patroontabel en kleurtabel.
          
          Om  ervoor te  zorgen dat  er 768 verschillende karakters op 
          het  scherm  kunnen  staan,  is het scherm in drie gedeeltes 
          verdeeld:
          
                          boven : rij 0 t/m 7
                          midden: rij 8 t/m 15
                          onder : rij 16 t/m 23
          
          Per  gedeelte is  er een  apart gedeelte  van de patroon- en 
          kleurtabel. Een karakter bestaat uit 8x8 pixels. De vorm van 
          het karakter staat in 8 bytes in de patroontabel en de kleur 
          in 8  bytes in  de vormtabel. In de patroontabel staat een 1 
          voor  voorgrondkleur en  een 0  voor achtergrondkleur, in de 
          kleurtabel geven  de 4 hoge bits de voorgrondkleur aan en de 
          4 lage bits de achtergrondkleur.
          
          
                                    L M M C 
          
          Om de 8x8 karakters op SCREEN 5 te zetten gebruik ik het VDP 
          commando  LMMC, wat  staat voor  Logical Move CPU to VRAM. U 
          kunt de  werking van  dit commando uitgebreid nalezen in een 
          oud deel van de VDP cursus, maar in het kort komt het hierop 
          neer:
          
          - Geef met DX, DY, NX en NY een rechthoek in het VRAM aan
          - Stuur de pixels een voor een naar R#44
          
          Een  klein addertje  onder het gras is dat we bij de aanroep 
          van de  routine het  eerste pixel  al mee  moeten geven! Dit 
          maakt een klein omweggetje in onze routine noodzakelijk.
          
          Om  het palet  hoeven we  ons geen zorgen te maken, omdat in 
          SCREEN 2 toch alleen het standaardpalet wordt gebruikt.
          
          Hoog  tijd voor  de ML source, die deze keer netjes top down 
          is  geprogrammeerd.  Zoals  gewoonlijk  staat  hij  in ASCII 
          formaat op  de disk  onder de  naam SC2SC5.ASC  (in de  file 
          SC2SC5.PMA).
          
          
          ; S C 2 S C 5 . A S M 
          ; Zet SCREEN 2 plaatje om naar SCREEN 5
          ; Door Stefan Boer
          ; Sunrise Magazine #10
          ; (c) Stichting Sunrise 1993
          
          ; SCREEN 2 plaatje moet bij aanroep in RAM staan
          ; op &H9000-&HCFFF, SCREEN 5 moet actief zijn
          
          
          SC2DAT: EQU   &H9000          ; adres van SC2 data in RAM
          
          
                  ORG   &HD000
          
          Voor de  snelheid laden  we het  SCREEN 2  plaatje (in feite 
          gewoon  het complete  MSX1 VRAM  van 16 kB) in het RAM vanaf 
          adres  &H9000,  dit kan  gewoon met  BLOAD"NAAM",&H9000. Het 
          plaatje staat dan van &H9000 t/m &HCFFF, zodat we de routine 
          op &HD000 laten beginnen.
          
          
          ; initialisatie
          
                  DI
                  XOR   A
                  LD    (REGEL),A
                  LD    HL,SC2DAT+&H1800
                  LD    (NAAMAD),HL     ; pointer naamtabel
                  LD    IX,FLAG
          
          
          ; hoofdprogramma
          
          REGLUS: CALL  DOREGL          ; zet een regel om
                  LD    A,(REGEL)
                  INC   A
                  CP    24              ; klaar?
                  JR    Z,EINDE
                  LD    (REGEL),A
                  JR    REGLUS          ; volgende regel
          EINDE:  EI
                  RET
          
          
          Het hoofdprogramma roept voor elke regel een routine aan die 
          de regel omzet naar SCREEN 5.
          
          
          ; zet een regel om
          
          DOREGL: XOR   A
                  LD    (KOLOM),A       ; initialisatie
          KARLUS: CALL  DOKAR           ; zet een karakter om
                  LD    A,(KOLOM)
                  INC   A
                  CP    32              ; klaar?
                  RET   Z
                  LD    (KOLOM),A
                  JR    KARLUS
          
          
          Ook hier  gebeurt nog  niets moeilijks,  hier wordt voor elk 
          karakter een routine aangeroepen die dat karakter omzet naar 
          SCREEN  5. Zoals  u ziet  wordt precies  de volgorde  van de 
          naamtabel aangehouden, dat scheelt rekenwerk.
          
          
          ; zet een karakter om
          
          DOKAR:  LD    HL,(NAAMAD)     ; adres naamtabel
                  LD    A,(HL)          ; nummer uit naamtabel lezen
                  INC   HL
                  LD    (NAAMAD),HL
                  PUSH  AF
                  CALL  GETCAD          ; bereken adres in kleurtabel
                  LD    (KLR_AD),HL
                  POP   AF
                  CALL  GETGAD          ; bereken adres generator tab
                  LD    (GEN_AD),HL
                  RES   0,(IX+0)        ; 1ste pixel nog niet gedaan
                  LD    B,8
          PIXLUS: CALL  DO8PIX          ; zet regeltje van 8 pixels om
                  DJNZ  PIXLUS
                  RET
          
          
          Eerst  wordt  het  nummer  van het  huidige karakter  uit de 
          naamtabel gehaald.  Zoals ik het nu geprogrammeerd heb bevat 
          (NAAMAD)  steeds het  adres dat  hoort bij  KOLOM en  REGEL, 
          waardoor  er  niet extra  gerekend hoef  te worden.  Bij het 
          karakternummer  worden   de  bijbehorende   adressen  in  de 
          patroon- en kleurtabel uitgerekend.
          
          IX+0 wijst naar een adres waar we met bit 0 bijhouden of het 
          huidige  pixel  het  eerste  pixel  is van  het huidige  8x8 
          blokje.  Dit is  nodig omdat  bij het naar de VDP sturen van 
          het LMMC commando het eerste pixel al bekend moet zijn.
          
          Een karakter  bestaat uit  8 regels  van 8 pixels, in de lus 
          PIXLUS worden deze 8 regels ��n voor ��n omgezet.
          
          
          ; Bereken adres in kleurtabel
          ; In : A=karakternummer
          ; Uit: HL=adres
          
          GETCAD: LD    L,A
                  LD    H,0
                  ADD   HL,HL
                  ADD   HL,HL
                  ADD   HL,HL           ; *8
                  LD    DE,SC2DAT+&H2000
                  ADD   HL,DE
                  LD    A,(REGEL)
                  CP    8
                  RET   C               ; bovenste gedeelte
                  LD    DE,&H0800
                  ADD   HL,DE
                  CP    16
                  RET   C               ; middelste gedeelte
                  ADD   HL,DE           ; onderste gedeelte
                  RET
          
          
          Eerst wordt 8*A uitgerekend en in HL gezet. Vervolgens wordt 
          het beginadres  van de  kleurtabel (die  in dit geval in het 
          RAM  staat vanaf  adres &H9000!) uitgerekend. Tot slot wordt 
          gekeken in  welk gedeelte van het scherm we ons bevinden, en 
          wordt  er indien nodig ��n (middelste gedeelte) of twee maal 
          (onderste gedeelte) &H800 bij het adres opgeteld.
          
          
          ; Bereken adres in patroon generator tabel
          ; In:  A=karakternummer
          ; Uit: HL=adres
          
          GETGAD: LD    L,A
                  LD    H,0
                  ADD   HL,HL
                  ADD   HL,HL
                  ADD   HL,HL           ; *8
                  LD    DE,SC2DAT
                  ADD   HL,DE
                  LD    A,(REGEL)
                  CP    8
                  RET   C               ; onderste gedeelte
                  LD    DE,&H0800
                  ADD   HL,DE
                  CP    16
                  RET   C               ; middelste gedeelte
                  ADD   HL,DE           ; onderste gedeelte
                  RET
          
          
          Deze  routine  is  bijna  gelijk  aan  de vorige,  het enige 
          verschil is dat het beginadres van de patroontabel &H0000 is 
          in plaats van &H2000.
          
          
          ; Zet een rijtje van 8 pixels om
          
          DO8PIX: CALL  GETCOL          ; lees kleuren
                  CALL  GETBYT          ; lees een byte v/h karakter
                  PUSH  BC
                  LD    B,8
          DOPIX:  LD    A,D             ; voorgrondkleur
                  RL    C
                  JR    C,DOPIX2
                  LD    A,E             ; bit 0, dus achtergrond kleur
          DOPIX2: BIT   0,(IX+0)        ; flag eerste pixel
                  JR    Z,FSTPIX
                  OUT   (&H9B),A        ; pixel naar VDP
                  DJNZ  DOPIX
                  POP   BC
                  RET
          FSTPIX: CALL  SETVDP          ; 1ste pixel, commando n. VDP
                  DEC   B
                  JR    DOPIX
          
          
          Dit gedeelte  doet het eigenlijke omzetwerk. Eerst worden de 
          gegevens  van het  huidige rijtje van 8 pixels uit de kleur- 
          en patroontabel gelezen (GETCOL en GETBYT). De kleuren staan 
          nu in  D en  E en  de 'vorm' in C. Vervolgens schuiven we de 
          bits  van C ��n voor ��n in de carry, omdat we dan makkelijk 
          kunnen testen.  Is het bit 1 dan sturen we de voorgrondkleur 
          naar   de  VDP   en  is   het  bit   0  dan   sturen  we  de 
          achtergrondkleur naar de VDP.
          
          Voor  het   schrijven  naar   R#44  gebruiken  we  indirecte 
          adressering zonder auto increment, wat erop neer komt dat we 
          de  kleuren gewoon  achter elkaar  naar poort  #3 (I/O adres 
          &H9B) kunnen sturen, zoals hier ook gebeurt.
          
          Ik heb  het al eerder gehad over het addertje onder het gras 
          met  LMMC, we moeten bij het geven van het commando de kleur 
          van  het   eerste  pixel  al  meegeven.  Daarvoor  dient  de 
          instructie  met bit 0 van (IX+0). Bij het eerste pixel wordt 
          de routine  SETVDP aangeroepen  in plaats van dat het gewoon 
          naar &H9B wordt geschreven.
          
          
          ; Lees kleuren
          ; Uit: E=achtergrondkleur, D=voorgrondkleur
          
          GETCOL: LD    HL,(KLR_AD)
                  LD    A,(HL)
                  INC   HL
                  LD    (KLR_AD),HL
                  PUSH  AF
                  AND   &H0F
                  LD    E,A             ; achtergrondkleur
                  POP   AF
                  AND   &HF0
                  RRCA
                  RRCA
                  RRCA
                  RRCA
                  LD    D,A             ; voorgrondkleur
                  RET
          
          
          Deze routine  leest een  byte uit  de kleurtabel  en zet  de 
          voorgrond- en achtergrondkleur in respectievelijk D en E.
          
          
          ; Lees een byte van het karakter
          ; Uit: C=byte
          
          GETBYT: LD    HL,(GEN_AD)
                  LD    C,(HL)
                  INC   HL
                  LD    (GEN_AD),HL
                  RET
          
          
          Deze  routine leest een byte uit de patroontabel, dit is een 
          stuk simpeler  omdat er  verder niets  meer hoeft  te worden 
          gerekend.
          
          De  nu  volgende routine  wordt steeds  voor elk  8x8 blokje 
          aangeroepen zodra  de kleur  van het eerste pixel bekend is. 
          De  VDP wordt  klaargezet om  een blokje  van 8x8 pixel voor 
          pixel  te  vullen,  waarbij  de data  naar &H9B  moet worden 
          geschreven.
          
          
          ; Zet VDP klaar om SCREEN 5 data te ontvangen
          ; In: A=kleur van eerste pixel
          
          SETVDP: EX    AF,AF
                  LD    A,36
                  OUT   (&H99),A
                  LD    A,17+128
                  OUT   (&H99),A        ; 36 --> R#17
          
                  LD    A,(KOLOM)
                  RLCA
                  RLCA
                  RLCA                  ; *8
                  OUT   (&H9B),A        ; DX
                  XOR   A
                  OUT   (&H9B),A
                  LD    A,(REGEL)
                  RLCA
                  RLCA
                  RLCA                  ; *8
                  OUT   (&H9B),A        ; DY
                  XOR   A
                  OUT   (&H9B),A
          
                  LD    A,8
                  OUT   (&H9B),A        ; NX
                  XOR   A
                  OUT   (&H9B),A
                  LD    A,8
                  OUT   (&H9B),A        ; NY
                  XOR   A
                  OUT   (&H9B),A
          
                  EX    AF,AF
                  OUT   (&H9B),A        ; kleur eerste pixel
                  XOR   A
                  OUT   (&H9B),A        ; ARG
                  LD    A,&HB0          ; LMMC
                  OUT   (&H9B),A        ; commando
          
                  LD    A,44+128        ; zonder auto increment naar
                  OUT   (&H99),A        ; R#44 schrijven
                  LD    A,17+128
                  OUT   (&H99),A        ; 44+128 --> R#17
          
                  SET   0,(IX+0)        ; flag eerste pixel gedaan
                  RET
          
          
          Het valt  je misschien op dat ik helemaal niet controleer of 
          de  VDP wel  klaar is  met het  vorige commando. Dat is hier 
          niet nodig,  want de  VDP is  zo snel  dat zelfs de R800 het 
          niet  kan bijhouden  met het  rekenwerk. Het  checken op het 
          Command Execute  bit heeft  dus geen  zin en  zou de routine 
          alleen onnodig vertragen.
          
          
          ; variabelen
          
          REGEL:  DB    0               ; huidige regel
          KOLOM:  DB    0               ; huidige kolom
          FLAG:   DB    0               ; flag eerste pixel gedaan
          NAAMAD: DW    0               ; huidig adres in naamtabel
          KLR_AD: DW    0               ; huidig adres in kleurtabel
          GEN_AD: DW    0               ; in pattern generator tabel
          
          
          Tot  slot  wordt  ruimte geserveerd  voor de  opslag van  de 
          benodigde variabelen.
          
          
                               T E N S L O T T E 
          
          In  SC2SC5.PMA zitten  verder nog  SC2SC5.BIN, SC2SC5.BAS en 
          NEMESIS.VRM, waarmee  u de routine even kunt uitproberen (de 
          .BAS file RUNnen). U zult zien dat het zeer snel gaat!
          
          Tot de volgende keer,
                                                          Stefan Boer
