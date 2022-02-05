                 M S X 2 / 2 +   V D P   C U R S U S   ( 1 3  )
                                                                
          
          Bijgelovige mensen  moeten dit deel maar overslaan, maar dat 
          zou  zonde zijn want ook dit deel is weer erg leerzaam. Deze 
          keer behandel ik een ML routine om SCREEN 5 naar SCREEN 8 om 
          te zetten.  Hiervoor is  de theorie nodig over het aansturen 
          van  het VRAM en over de opslag van de schermen in het VRAM, 
          wat uitgebreid  in eerdere  delen is terug te vinden. Ik zal 
          het deze keer alleen kort nog even herhalen.
          
          
                         S C R E E N   5   N A A R   8 
                                                        
          
          SCREEN 5 en SCREEN 8 hebben dezelfde resolutie, namelijk 256 
          bij  212  pixels.  Op dat  vlak dus  geen problemen.  Wat er 
          uiteindelijk moet worden omgezet zijn de kleuren.
          
          In  SCREEN 5  wordt er  gewerkt met 16 kleuren uit een palet 
          van 512,  terwijl er  in SCREEN  8 slechts 256 kleuren zijn, 
          die  wel allemaal tegelijkertijd op het scherm kunnen worden 
          getoond. We moeten de 16 SCREEN 5 kleuren dus omrekenen naar 
          SCREEN  8  kleuren.  Hierbij  zal  wel  een  klein  verschil 
          ontstaan omdat in SCREEN 8 maar 256 van de 512 verschillende 
          kleuren  precies  kunnen  worden  nagemaakt. Van  de overige 
          kleuren moeten  we de  kleur nemen die er het dichtst bij in 
          de buurt komt.
          
          
                                   P A L E T 
          
          Voor het palet in SCREEN 5 zijn er drie bits voor rood, drie 
          bits voor groen en drie bits voor blauw. Deze moeten naar de 
          paletregisters  van de  VDP worden  geschreven (zie hiervoor 
          deel 1 van de cursus), maar kunnen niet worden gelezen.
          
          In MSX  BASIC is dat opgelost door het palet ook nog eens in 
          het  VRAM op  te slaan. In SCREEN 5 staat deze kopie van het 
          palet vanaf  adres &H7680. Omdat het palet 32 bytes lang is, 
          eindigt  het op adres &H769F. Het palet kan bij een SCREEN 5 
          plaatje dus als volgt worden meegesaved:
          
          BSAVE "FILENAAM.SC5",0,&H769F,S
          
          Als het  plaatje weer wordt ingeladen kan het palet dan weer 
          worden  teruggehaald met  COLOR=RESTORE. Het palet wordt dan 
          van het VRAM naar de paletregisters van de VDP gekopieerd.
          
          De data vanaf adres &H7680 is als volgt opgebouwd:
          
                  MSB   7   6   5   4   3   2   1   0   LSB
          
          &H7680        0   ----R----   0   ----B----     palet #0
          &H7681        0   0   0   0   0   ----G----
          &H7682        0   ----R----   0   ----B----     palet #1
          etc.
          
          Hierbij staat  R voor  de rode intensiteit, B voor de blauwe 
          intensiteit  en G  voor de  groene intensiteit,  dit alles 3 
          bits dus tussen 0 en 7.
          
          
                        S C R E E N   8   K L E U R E N 
          
          In SCREEN 8 is er geen palet, maar kan de kleur rechtstreeks 
          uit  de opslag  van het scherm in het VRAM worden afgelezen. 
          Elke byte is als volgt opgebouwd:
          
                  MSB   7   6   5   4   3   2   1   0   LSB
          
                        ----G----   ----R----   --B--
          
          Hierbij  staan R, G en B weer voor de rode, groene en blauwe 
          intensiteit. Het  valt meteen  op dat  er voor blauw slechts 
          twee  bits zijn,  zodat dit maar een getal tussen 0 en 3 kan 
          zijn. Dit is de reden waarom een SCREEN 5 plaatje niet exact 
          kan worden geconverteerd naar SCREEN 8.
          
          
                                  B I T M A P 
          
          De opslag van de schermdata in het VRAM gaat bij SCREEN 5 en 
          8  volgens de  bitmap methode.  Voor elk  pixel zijn  er een 
          aantal  bits  gereserveerd,  waarin  de informatie  over dat 
          pixel staat.  Elk pixel  heeft z'n  eigen bits,  waardoor er 
          geen  beperkingen zijn  aan het aantal kleuren dat bij naast 
          elkaar  liggende   pixels  mag  worden  gebruikt,  zoals  in 
          bijvoorbeeld SCREEN 2 en SCREEN 12.
          
          In  SCREEN 5 wordt van elk pixel het paletnummer opgeslagen, 
          dit is een getal van 0-15 en daarvoor zijn dus 4 bits nodig. 
          In elke  byte staan  daarom twee  pixels, waarbij het linker 
          pixel in de hoge nibble (bit 4-7) staat en het rechter pixel 
          in  de lage nibble (bit 0-3). In SCREEN 8 zijn er 8 bits per 
          pixel nodig,  dus staat  er in elke byte het kleurnummer van 
          ��n pixel.
          
          Nu  we  alle  benodigde informatie  over de  schermen hebben 
          kunnen we met de conversieroutine beginnen. Zoals gewoonlijk 
          is  deze in  WB-ASS2 geschreven, staat de source in ASCII op 
          de disk  en heb ik de source uitgebreid van extra commentaar 
          en uitleg voorzien.
          
          Het inladen van het SCREEN 5 plaatje en het wegschrijven van 
          het  SCREEN  8  plaatje  laten  we  aan  BASIC over,  dit is 
          tenslotte  geen  diskcursus.  Op  de  diskette  vindt  u  in 
          SC5SC8.PMA  een BASIC programma (SC5SC8.BAS), dat de ML file 
          (SC5SC8.BIN)   inlaadt   en  zorgt   voor  het   inladen  en 
          wegschrijven  van  de  grafische  files. Ook  de source  zit 
          erbij.
          
          
          ; S C 5 S C 8 . A S M 
          ; SCREEN 5 omzetten naar SCREEN 8
          ; Stefan Boer, april 1993
          ; (c) Ectry 1993
          ; Sunrise Magazine #8
          ; (c) Stichting Sunrise 1993
          
          ; SCREEN 5 plaatje in SCREEN 8 inladen, met palet
          ; SC8 plaatje wordt door routine over SC5 plaatje gezet
          ; en kan worden gesaved met BSAVE"NAAM.SC8",0,54271,S
          
                  ORG   &HD000
          
                  LD    A,(&HF342)      ; RAM slot &H4000-&H7FFF
                  LD    H,&H40
                  CALL  &H24            ; RAM op &H4000
                  IN    A,(&HFE)        ; mapper poort &H8000-&HBFFF
                  LD    (OLDMAP),A      ; bewaar mapper instelling
                  LD    A,3             ; selecteer ander RAM
                  OUT   (&HFE),A
          
          
          Op  adres  &HF342  staat  de  slot-ID  byte  van het  RAM op 
          &H4000-&H7FFF  (page 1). We gebruiken de BIOS routine ENASLT 
          (adres &H0024)  om op  adres &H4000  RAM te zetten in plaats 
          van  de BASIC interpreter. We gebruiken dit RAM om tijdelijk 
          schermdata in op te slaan.
          
          Op &H8000  selecteren we  een ander  gedeelte van de mapper, 
          zodat   het  BASIC  programma  dat  daar  staat  niet  wordt 
          overgeschreven. I/O  poort &HFE  bepaalt welke mapperpage er 
          op adres &H8000 staat.
          
          
          ; kopieer SCREEN 5 data van VRAM naar RAM
          
                  LD    HL,0
                  LD    C,0             ; VRAM lezen vanaf adres
                  CALL  SETRD           ; &H00000 (bitmap data)
          
          
          De standaardroutine SETRD kennen we nog uit het begin van de 
          cursus. Deze  routine stelt de VDP in om VRAM te lezen vanaf 
          het  adres dat  in HL en C staat, hierbij is bit 0 van het C 
          register het  MSB (most significant bit) van het VRAM adres. 
          HL  bevat  de  rest van  het adres.  Het beginadres  van het 
          scherm in het VRAM is &H00000.
          
          
                  LD    DE,&H4000       ; beginadres SC5 data in RAM
                  LD    BC,27136        ; lengte SC5 data (128*212)
          
          COPY:   IN    A,(&H98)        ; lees byte uit VRAM
                  LD    (DE),A          ; sla op in RAM
                  INC   DE
                  DEC   BC
                  LD    A,B
                  OR    C
                  JP    NZ,COPY         ; herhaal tot BC=0
          
          
          Hier wordt  de SCREEN  5 data naar het RAM gekopieerd, vanaf 
          adres  &H4000. Een  scherm is  212 lijnen  hoog en elke lijn 
          neemt  128  bytes  in  beslag (256  pixels, twee  pixels per 
          byte), dus  de totale lengte van een SCREEN 5 plaatje in het 
          VRAM is 27136 bytes.
          
          Met  IN A,(&H98)  wordt het  VRAM gelezen.  De VDP  verhoogt 
          daarna   zelf   het  leesadres,   zodat  de   volgende  keer 
          automatisch  de  volgende  byte  wordt  gelezen.  Dit  wordt 
          herhaald totdat het hele plaatje in het RAM staat.
          
          LD  A,B en  daarna OR C is een truukje om te kijken of BC al 
          gelijk is aan 0.
          
          
          ; kopieer palet data van VRAM naar RAM
          
                  LD    HL,&H7680
                  LD    C,0             ; VRAM lezen vanaf adres
                  CALL  SETRD           ; &H07680 (palet data)
          
                  LD    HL,PALET        ; beginadres paletdata in RAM
                  LD    B,32            ; lengte palet data
                  LD    C,&H98          ; Port #0 (VRAM lezen)
                  INIR                  ; lees B bytes van poort (C)
          ;                             ; en zet in RAM vanaf HL
          
          
          Hier  kopi�ren we  de paletdata  (die zoals we eerder hebben 
          gezien vanaf  adres &H7680  in het VRAM staat) naar het RAM. 
          Het SCREEN 5 plaatje moet dus wel met palet zijn weggesaved.
          
          Met  de instructie INIR worden B bytes gelezen van poort (C) 
          en vanaf  (HL) in het RAM gezet. Bij het vorige gedeelte had 
          het  weinig zin  om deze  instructie te  gebruiken, omdat er 
          toen 27136  bytes moesten worden gelezen en er met INIR maar 
          maximaal 256 bytes per keer kunnen worden gelezen. Het palet 
          wordt dus vanaf (PALET) in het RAM gezet.
          
          [Nvdr.  De tekst  was langer  dan 16 kB, u kunt verder lezen 
          door de volgende optie in het submenu te kiezen.]
          
          
          
                 M S X 2 / 2 +   V D P   C U R S U S   ( 1 3  )
                                                                
          
          [Nvdr. Dit is het tweede gedeelte.]
          
          
          ; Converteer palet naar SCREEN 8 kleuren
          
                  LD    B,16            ; 16 paletten om te rekenen
                  LD    HL,PALET
                  LD    DE,COLORS       ; beginadres SC8 kleurentabel
          PALCNV: LD    A,(HL)          ; lees eerste byte paletdata
                  AND   &B01110000      ; alleen rood (bit 4-6)
                  RRCA
                  RRCA                  ; verplaats rood naar bit 2-4
                  LD    C,A
          
          
          Dit is het moeilijkste gedeelte van de routine, hier rekenen 
          we  de 16  paletkleuren om  naar SCREEN  8 kleuren. We lezen 
          eerst een  byte, en filteren alleen het rood eruit dat zoals 
          we  eerder gezien  hebben op  bit 4-6  staat. In de SCREEN 8 
          kleurcode moet  het rood op bit 2-4 staan, we moeten dit dus 
          twee posities naar rechts verschuiven. Daarvoor gebruiken we 
          RRCA,  deze instructie  roteert het  A register naar rechts. 
          Het resultaat  wordt in  C bewaart,  zodat we later blauw en 
          groen erbij kunnen zetten.
          
          
                  LD    A,(HL)
                  AND   &B00000111      ; alleen blauw (bit 0-2)
                  SRL   A               ; verplaats blauw naar bit 0-1
                  OR    C
                  LD    C,A             ; rood en blauw samen
          
          
          Op  analoge  wijze  wordt  hier  het  blauw  toegevoegd.  De 
          instructie  SRL  A schuift  de bits  in het  A register  een 
          plaatsje naar rechts, terwijl in bit 7 een 0 wordt gezet. We 
          kunnen hier  geen RRCA  gebruiken, omdat  bit 0 dan in bit 7 
          terecht  zou komen,  en dat  is niet  de bedoeling. Met OR C 
          worden rood en blauw samen in een byte gezet.
          
          
                  INC   HL
                  LD    A,(HL)
                  AND   &B00000111      ; groen (bit 0-2)
                  RRCA
                  RRCA
                  RRCA                  ; verplaats groen naar bit 5-7
                  OR    C
          
          
          Groen  staat  in  de  volgende  byte.  Dit moet  eigenlijk 5 
          plaatsen naar  links worden  verplaats, maar 3 plaatsen naar 
          rechts  is  natuurlijk  precies hetzelfde  en is  korter. Nu 
          staat  de complete SCREEN 8 kleurcode die overeenkomt met de 
          SCREEN 5 paletkleur in het A register. Het enige verschil is 
          dat de blauwe intensiteit nu minder nauwkeurig is.
          
          
                  LD    (DE),A          ; SC8 kleurcode naar tabel
                  INC   HL
                  INC   DE
                  DJNZ  PALCNV
          
          
          De zojuist  berekende SCREEN  8 kleurcode  wordt in de tabel 
          gezet,  en zo worden alle 16 paletkleuren afgewerkt. We gaan 
          nu de feitelijke schermdata converteren.
          
          
          ; converteer SCREEN 5 data naar SCREEN 8
          
                  LD    HL,0
                  LD    C,0
                  CALL  SETWRT          ; VRAM schrijven vanaf &H00000
          
          
          We beginnen  weer met  de VDP  klaar te zetten voor een VRAM 
          actie,  dit keer schrijven. Hiervoor gebruiken we SETWRT, de 
          tegenhanger van SETRD. Het SCREEN 8 plaatje wordt op adres 0 
          gezet, dus gewoon over het SCREEN 5 plaatje heen.
          
          
                  LD    DE,&H4000       ; SC5 data vanaf &H4000 in RAM
                  LD    BC,27136        ; aantal te converteren bytes
          CONVRT: PUSH  BC
                  LD    A,(DE)          ; lees SC5 byte (2 pixels)
                  AND   &HF0            ; linker pixel
                  RRCA
                  RRCA
                  RRCA
                  RRCA                  ; verplaats naar lage nibble
          
          
          DE  bevat het  huidige adres in het RAM tijdens het omzetten 
          en BC  het aantal  nog te  converteren bytes. Hier wordt een 
          SCREEN  5 byte  uit het  RAM opgehaald  en wordt  het linker 
          pixel eruit  gehaald. Het paletnummer van dit pixel staat in 
          de   linker  nibble,  en  die  verplaatsen  we  met  4  RRCA 
          instructies  naar  het  rechter  nibble.  Nu  kunnen  we  de 
          bijbehorende SCREEN 8 kleurcode uit de tabel halen:
          
          
                  LD    HL,COLORS
                  LD    C,A
                  LD    B,0
                  ADD   HL,BC           ; bereken plaats in tabel
                  LD    A,(HL)          ; haal kleurnummer uit tabel
                  OUT   (&H98),A        ; schrijf naar VRAM
          
          
          Een  ADD HL,A  instructie bestaat  niet, dus dat doen we via 
          een klein  omweggetje. Tot  slot wordt de byte naar het VRAM 
          geschreven  met  de  OUT  (&H98),A  instructie.  Nu  nog het 
          rechter pixel:
          
          
                  LD    A,(DE)
                  AND   &H0F            ; rechter pixel
                  LD    HL,COLORS
                  LD    C,A
                  LD    B,0
                  ADD   HL,BC
                  LD    A,(HL)
                  OUT   (&H98),A
          
          
          Dat  gaat  een  stuk makkelijker  omdat die  al op  de goede 
          plaats staat. Verder is het analoog.
          
          
                  POP   BC
                  INC   DE
                  DEC   BC
                  LD    A,B
                  OR    C
                  JP    NZ,CONVRT
          
          
          Hier  wordt de  lus afgesloten, zodat het hele plaatje wordt 
          geconverteerd.  Door   de  hier  gekozen  methode  gaat  dat 
          razendsnel!  Probeer  het  maar  eens,  het programma  staat 
          immers niet voor niets op de diskette.
          
          Het programma is zo snel omdat de SCREEN 5 data eerst in ��n 
          keer naar het RAM wordt gekopieerd, waardoor de VDP maar ��n 
          keer  hoeft te  worden ingesteld om VRAM te lezen, en daarna 
          slechts Port  #0 (&H98) hoeft te worden gelezen. De SCREEN 8 
          data  wordt  ook  in  ��n  keer  naar  het VRAM  geschreven, 
          waardoor  de VDP  ook maar  ��n keer voor schrijven hoeft te 
          worden ingesteld. De routine zou veel en veel langzamer zijn 
          indien er  steeds ��n  SCREEN 5 byte zou worden gelezen, dan 
          weer twee SCREEN 8 bytes schrijven, etc. Dan moeten de SETRD 
          en   SETWRT  routines   namelijk  elke   keer  weer   worden 
          aangeroepen.
          
          Tot slot  moeten we nog even de slot- en mapperschakeling in 
          de  oude toestand herstellen, en een aantal geheugenplaatsen 
          defini�ren met  DB en  DS instructies. Op adres &HFCC1 staat 
          de  slot-ID byte  voor het  MAIN ROM,  voor zowel page 0 als 
          page 1.
          
          
          ; zet geheugen weer terug in oude stand
          
                  LD    A,(OLDMAP)
                  OUT   (&HFE),A        ; herstel mapper
                  LD    A,(&HFCC1)
                  LD    H,&H40
                  CALL  &H24            ; BASIC ROM weer op &H4000
                  RET
          
          OLDMAP: DB    1
          COLORS: DS    16
          PALET:  DS    32
          
          
          Tot slot  nog de standaardroutines om de VDP klaar te zetten 
          voor  VRAM lezen resp. schrijven. Er staat genoeg commentaar 
          in de  source, voor  verdere uitleg  verwijs ik  u naar  het 
          begin van de cursus.
          
          
          ; SETWRT
          ; Zet VDP klaar om naar VRAM te schrijven
          ; In: HL: bit 0-15 van adres
          ;     C : bit 16 van adres
          
          SETWRT: LD    A,H
                  RES   7,A
                  SET   6,A             ; 0 1 is VRAM schrijven
                  JP    RDWRT
          
          ; SETRD
          ; Zet VDP klaar om uit VRAM te lezen
          ; In: HL: bit 0-15 van adres
          ;      C: bit 16 van adres
          
          SETRD:  LD    A,H
                  AND   &B00111111      ; 0 0 is VRAM lezen
          
          ; Dit gedeelte is voor beide routines hetzelfde
          
          RDWRT:  PUSH  AF              ; deze byte moet pas als
          ;                               laatste naar Port #1
                  LD    A,C             ; bit 16 van adres
                  AND   1               ; alleen bit 0
                  LD    C,A
                  LD    A,H
                  AND   &HC0            ; alleen bit 6 en 7
          ;                               (bit 14 en 15 van adres)
                  OR    C               ; bit 16 van adres erbij
                  RLCA
                  RLCA                  ; schuif bit 0, 6 en 7 naar
          ;                               bit 0, 1 en 2
                  DI
                  OUT   (&H99),A
                  LD    A,14+128
                  OUT   (&H99),A        ; schrijf bit 14-16 van adres
          ;                               naar R#14
                  LD    A,L             ; bit 0-7 van adres
                  OUT   (&H99),A        ; naar Port #1
                  POP   AF              ; bit 8-13 van adres, plus VDP
          ;                               instructie
                  OUT   (&H99),A        ; naar Port #1
                  EI
                  RET
          
          
                                 G E B R U I K 
          
          Tot slot nog een aantal wenken voor het geval u de converter 
          wilt  gaan  gebruiken. Omdat  de converter  het palet  nodig 
          heeft, moeten  de plaatjes  met palet zijn weggeschreven, op 
          de  manier  zoals  eerder in  deze tekst  vermeld. Indien  u 
          andere  files wilt  gebruiken (bijvoorbeeld .SR5 en .PL5 van 
          GraphSaurus),  kunt  u  daarvoor  het  BASIC  programma zelf 
          aanpassen.
          
          Het hele  programma draait  onder SCREEN 8. Als het SCREEN 5 
          plaatje  wordt ingeladen  ziet dit  er dus niet uit. Maakt u 
          zich daarover  niet druk,  dat hoort dus zo. U zult zien dat 
          resultaat  qua kleur  ietsje afwijkt  van het origineel, dat 
          komt  zoals  eerder  gezegd  doordat het  blauw in  SCREEN 8 
          minder nauwkeurig  kan worden  opgegeven. Hierdoor wordt het 
          plaatje  ook iets  donkerder. Hier  is verder  niets aan  te 
          doen.
          
          Het zal in de praktijk wel eens voorkomen dat u een SCREEN 5 
          plaatje naar  SCREEN 8 wilt omzetten, bijvoorbeeld als u een 
          SCREEN  5 plaatje  in Dynamic Publisher wilt inladen. DP kan 
          namelijk alleen SCREEN 8 plaatjes converteren.
          
          Ook deze  keer wens  ik u weer veel succes met het toepassen 
          van het geleerde. Tot de volgende keer!
          
                                                          Stefan Boer
