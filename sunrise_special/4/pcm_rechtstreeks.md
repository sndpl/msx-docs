                        P C M   R E C H T S T R E E K S 
                                                         
          
          We  hebben op  vorige Specials  al aandacht  besteed aan het 
          aansturen van  de PCM  chip in de turbo R door middel van de 
          BIOS  calls PCMPLY  en PCMREC.  De PCM  chip kan  echter ook 
          rechtstreeks  met  OUT  poorten  worden aangestuurd.  In dit 
          artikel ga  ik het  hier over  hebben, en  zal ik  tevens de 
          sources  geven van  de standaard opneem- en afspeelroutines. 
          De maximale  opneemfrequentie is  zo'n 25  kHz! Het afspelen 
          kan  zelfs nog  sneller, maar  dat heeft dus alleen zin voor 
          samples die op een andere computer zijn opgenomen.
          
          
                             I / O   P O O R T E N 
          
          Voor de PCM chip zijn twee I/O poorten gereserveerd: &HA4 en 
          &HA5. De indeling van deze poorten is als volgt:
          
                    MSB    7    6    5    4    3    2    1    0    LSB
          
          &HA5 write       0    0    0   SMPL SEL  FILT MUTE ADDA
          &HA5 read       COMP  0    0   SMPL SEL  FILT MUTE BUFF
          &HA4 write      DA7  DA6  DA5  DA4  DA3  DA2  DA1  DA0
          &HA4 read        0    0    0    0    0    0   CT1  C10
          
          De beschikbare uitleg van deze bits is slechts zeer beknopt, 
          waardoor  ik zelf  ook niet  van alle bits de exacte werking 
          weet. Ik geef hieronder de letterlijke tekst die ik heb. Als 
          je  gewoon  de standaardroutines  gebruikt heb  je het  niet 
          nodig, maar  ik geef  de tekst  toch maar  voor het geval er 
          mensen zijn die er wel iets aan hebben.
          
          
            ADDA (BUFF): Buffermode
          Bepaalt de  uitvoer van  de D/A  converter. Zet hem in geval 
          van D/A conversie (dubbele buffer) op 0, en in geval van A/D 
          conversie  op 1.  Verder, komt  hij bij een reset op dubbele 
          buffer te staan (default).
          
            MUTE: Muting-besturing
          Zet de geluidsuitvoer aan of uit.
          0: geen uitvoer (default)
          1: wel uitvoer
          
          N.B.  Bij  afspelen moet  dit uiteraard  altijd op  1 worden 
          gezet,  anders  hoor  je  niets.  Als  je het  op 1  zet bij 
          opnemen, hoor  je het geluid dat wordt opgenomen tegelijker- 
          tijd uit de luidspreker komen.
          
            FILT: Keuze van het invoersignaal van het 
                  samplehold-circuit
          Bepaalt, in  het geval van A/D conversie, of het signaal dat 
          men  in het samplehold-circuit invoert een signaal moet zijn 
          dat door een filter afgegeven wordt of een standaard signaal 
          moet zijn.  Door 0  te kiezen  wordt het  een geluid van een 
          standaard  signaal, en door 1 te kiezen wordt het een geluid 
          van een filter-uitgangssignaal. De default waarde is 0.
          
            SEL: keuze van het filter-inganssignaal
          Bepaalt  of  het  signaal  dat  men  in het  low-pass filter 
          invoert  een  signaal moet  zijn dat  door de  D/A converter 
          afgegeven wordt,  of dat  het een signaal moet zijn dat door 
          de  microfoonversterker wordt afgegeven. Met keuze 0: geluid 
          afgegeven door  de D/A converter, en door 1 te kiezen, kiest 
          men  voor  geluid  dat  door  de  microfoonversterker  wordt 
          afgegeven.
          
            SMPL: Samplehold-signaal
          Bepaalt  of het  ingangssignaal gesampled of geHOLD dient te 
          worden.
          0: Samplen (default)
          1: HOLDen
          
            COMP: Uitgangssignaal van de comparator
          Vergelijkt het  uitgangssignaal van de samplehold en dat van 
          de D/A converter.
          0: D/A uitvoer groter dan samplehold uitvoer
          1: D/A uitvoer kleiner dan samplehold uitvoer
          
            DA7 tot DA0: D/A uitvoer data
          Wanneer  men  PCM  data  afspeelt,  kan met  het PCm  geluid 
          afspelen  door de gebruikte data hier naartoe uit te voeren. 
          Het formaat  van de  data is  absoluut binair,  en 127  komt 
          overeen met level 0.
          
            CT1 en CT0: counter data
          Bij  elke 63.5  microseconde wordt  deze waarde verhoogd. De 
          data  die  op adres  &HA4 werd  geschreven tegelijk  met het 
          ophogen op het moment van een D/A conversie wordt gekopieerd 
          en uitgevoerd.  Wanneer men op adres &HA4 schrijft, wordt de 
          counter gewist.
          
          N.B. Deze  counter wordt door de BIOS routines gebruikt voor 
          de  vier opneem-  en afspeelsnelheden  die daar  beschikbaar 
          zijn.
          
          
                                A F S P E L E N 
          
          Wie het bovenste goed heeft gelezen, heeft waarschijnlijk al 
          gezien  dat het  afspelen heel  makkelijk is. Gewoon de data 
          naar &HA4  sturen. Met  de counter  van de sampler zijn maar 
          vier  snelheden mogelijk  (15.75, 7.875,  5.25 en 3.94 kHz), 
          maar door  de systeemcounter  van de  turbo R (zie SRS#3) te 
          gebruiken,  kunnen we de snelheid veel nauwkeuriger opgeven. 
          De PCM playroutine ziet er als volgt uit:
          
          ; P C M . A S C 
          ; Play/record routines voor turbo R PCM
          ; Standaardroutines van ASCII
          
          ; Sunrise Special #4
          ; (c) Stichting Sunrise 1993
          
          PMDAC:  EQU   &HA4            ; write
          PMCNT:  EQU   &HA4            ; read
          PMCNTL: EQU   &HA5            ; write
          PMSTAT: EQU   &HA5            ; read
          SYSTML: EQU   &HE6            ; 3.911 us system counter
          
          ; Play
          ; R800 only!
          ; In: HL beginadres
          ;     BC lengte
          ;     E  snelheid
          
          PLAY:   LD    A,&B00000011
                  OUT   (PMCNTL),A      ; D/A mode
                  DI
                  XOR   A
                  OUT   (SYSTML),A      ; reset counter
          
          PLAY1:  IN    A,(SYSTML)
                  CP    E
                  JR    C,PLAY1
                  XOR   A
                  OUT   (SYSTML),A
          
                  LD    A,(HL)          ; PCM data
                  OUT   (PMDAC),A
                  INC   HL
                  DEC   BC
                  LD    A,C
                  OR    B
                  JR    NZ,PLAY1
                  EI
                  RET
          
          Eerst worden MUTE en ADDA op 1 gezet en SMPL, SEL en FILT op 
          0.  Daarna wordt  de PCM data naar &HA4 gestuurd, waarbij de 
          systeemcounter op  &HE6 wordt gebruikt voor de timing. Zoals 
          u  ziet, zeer  eenvoudig. Het  opnemen gaat  helaas een stuk 
          moeizamer, omdat we dat per bit moeten doen!
          
          
                                 O P N E M E N 
          
          Hier volgt eerst maar de source:
          
          ; Record
          ; R800 only!
          ; In: HL adres
          ;     BC lengte
          ;     E  snelheid
          
          REC:    LD    A,&B00001100
                  OUT   (PMCNTL),A      ; A/D mode
                  DI
                  XOR   A
                  OUT   (SYSTML),A      ; reset counter
          
          REC1:   IN    A,(SYSTML)
                  CP    E
                  JR    C,REC1
                  XOR   A
                  OUT   (SYSTML),A
          
                  PUSH  BC
                  LD    A,&B00011100
                  OUT   (PMCNTL),A      ; data hold
                  LD    C,PMSTAT
          
                  LD    A,&B10000000    ; bit 7
                  OUT   (PMDAC),A       ; bit convert
                  DB    &HED,&H70       ; IN F,(C)
                  JP    M,RECAD0
                  RES   7,A
          
          RECAD0: SET   6,A             ; bit 6
                  OUT   (PMDAC),A
                  DB    &HED,&H70
                  JP    M,RECAD1
                  RES   6,A
          
          RECAD1: SET   5,A             ; bit 5
                  OUT   (PMDAC),A
                  DB    &HED,&H70
                  JP    M,RECAD2
                  RES   5,A
          
          RECAD2: SET   4,A             ; bit 4
                  OUT   (PMDAC),A
                  DB    &HED,&H70
                  JP    M,RECAD3
                  RES   4,A
          
          RECAD3: SET   3,A             ; bit 3
                  OUT   (PMDAC),A
                  DB    &HED,&H70
                  JP    M,RECAD4
                  RES   3,A
          
          RECAD4: SET   2,A             ; bit 2
                  OUT   (PMDAC),A
                  DB    &HED,&H70
                  JP    M,RECAD5
                  RES   2,A
          
          RECAD5: SET   1,A             ; bit 1
                  OUT   (PMDAC),A
                  DB    &HED,&H70
                  JP    M,RECAD6
                  RES   1,A
          
          RECAD6: SET   0,A             ; bit 0
                  OUT   (PMDAC),A
                  DB    &HED,&H70
                  JP    M,RECAD7
                  RES   0,A
          
          RECAD7: LD    (HL),A          ; PCM data
                  LD    A,&B00001100
                  OUT   (PMCNTL),A
                  POP   BC
                  INC   HL
                  DEC   BC
                  LD    A,C
                  OR    B
                  JR    NZ,REC1
          
                  LD    A,&B00000011
                  OUT   (PMCNTL),A      ; D/A mode
                  EI
                  RET
          
          Eerst worden SEL en FILT op 1 gezet en MUTE, HOLD en ADDA op 
          0.  De  systeemcounter  op &HE6  wordt net  als bij  de play 
          routine  gebruikt voor  de timing. Nu wordt ook de HOLD op 1 
          gezet en  wordt de  waarde &H80 naar &HA4 geschreven. De PCM 
          chip  zal nu  het COMP bit zetten als het signaal sterker is 
          dan &H80,  en het  wissen als  het signaal  minder sterk is. 
          COMP  wordt gelezen door met IN F,(C) register &HA5 in het F 
          register te  lezen, waarbij  bit 7  (COMP) dus  de sign vlag 
          wordt.
          
          Is  het signaal sterker dan &H80, dan blijft de waarde van A 
          ongewijzigd, was  het zwakker,  dan wordt  bit 7  gewist. Nu 
          wordt  bit  6  gezet.  Weer  wordt  deze  waarde  naar  &HA4 
          geschreven  om hem  te vergelijken,  en ook  weer wordt COMP 
          uitgelezen via  het sign  bit van  het vlagregister.  Is het 
          signaal sterker, dan blijft bit 6 gezet, is het zwakker, dan 
          wordt bit 6 gereset. Zo wordt bit voor bit een byte PCM data 
          samengesteld.
          
          
                     M A X I M A L E   F R E Q U E N T I E 
          
          Deze routine  is behoorlijk  lang, en dat beperkt dan ook de 
          maximale frequentie waarmee kan worden opgenomen. Ik heb het 
          getest  en het  blijkt dat  het E  register een  waarde moet 
          hebben van  minimaal 10.  Geeft u  een lagere waarde op, dan 
          zal dat toch dezelfde snelheid als bij 10 worden.
          
          Hieronder een overzicht van een aantal mogelijke waardes van 
          E en de bijbehorende frequentie in kHz:
          
          10   25.568 kHz
          11   23.244 kHz
          12   21.307 kHz
          13   19.668 kHz
          14   18.263 kHz
          15   17.045 kHz
          16   15.980 kHz
          17   15.040 kHz
          18   14.205 kHz
          19   13.457 kHz
          20   12.784 kHz
          21   12.175 kHz
          22   11.622 kHz
          23   11.117 kHz
          24   10.653 kHz
          25   10.227 kHz
          26    9.834 kHz
          27    9.470 kHz
          28    9.131 kHz
          29    8.817 kHz
          30    8.523 kHz
          31    8.248 kHz
          32    7.990 kHz
          33    7.748 kHz
          34    7.520 kHz
          35    7.305 kHz
          36    7.102 kHz
          37    6.910 kHz
          38    6.728 kHz
          39    6.556 kHz
          40    6.392 kHz
          41    6.236 kHz
          42    6.088 kHz
          43    5.946 kHz
          44    5.811 kHz
          45    5.682 kHz
          46    5.558 kHz
          47    5.440 kHz
          48    5.327 kHz
          49    5.218 kHz
          50    5.114 kHz
          
          Zoals  u ziet  is de  maximale opneemfrequentie  maar liefst 
          25.568 kHz! Dat is heel wat meer dan de 15.75 kHz die met de 
          BIOS  routines   kan  worden   gehaald.  Het   afspelen  kan 
          natuurlijk  nog  sneller,  maar  dat  heeft alleen  zin voor 
          samples die op een andere computer zijn opgenomen.
          
          De source PCM.ASC is op de diskette aanwezig.
          
          
                                 G E I N T J E 
          
          Een leuk  geintje is  tenslotte nog om de geluidstoevoer van 
          de  turbo R aan te zetten. Pas daarbij wel op dat het geluid 
          niet kan  gaan zingen tussen de microfoon en de luidspreker, 
          want  dat  geeft  een ontzettende  rotherrie. Vooral  met de 
          ingebouwde  microfoon gebeurt  dat snel. Het is heel simpel, 
          gewoon in BASIC intypen:
          
          OUT &HA5,10
          
          De doorvoer kan uiteraard ook weer uit, daarvoor typt u:
          
          OUT &HA5,8
          
          Natuurlijk kunt u dit ook in ML programmeren...
          
                                                          Stefan Boer
