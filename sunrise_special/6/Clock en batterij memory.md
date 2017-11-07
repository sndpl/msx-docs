                 C L O C K   E N   B A T E R I J   M E M O R Y 


          In  de MSX2 en hoger zit een CLOCK-IC voor het bijhouden van
          de  tijd  en  datum.  Omdat dit  IC door  een baterij  wordt
          gevoed,  blijft   het  aktief  ook  als  de  computer  wordt
          uitgeschakeld. Ook is er een kleine hoeveelheid RAM voor het
          bewaren van o.a de prompt en diverse screen instellingen.


                       C L O C K - I C   F U N C T I E S

          Het IC heeft de volgende 3 functies:

          - CLOCK functies:

            - Bijhouden, lezen en schrijven van de TIJD en DATUM
            - Keuze uit 24-uur/12-uur klok aanduiding.
            - Bijhouden aantal dagen van de maand, en schrikkeljaar.

          - ALARM functies:

            - Als de actuele tijd is gelijk wordt aan de alarm tijd,
              zal de clock een signaal doorgeven.

          - Baterij geheugen:

            - Bevat 26 x 4 bits registers, met diverse data
            - MSX2 bewaart de volgende data in zijn geheugen:

            1. "Adjust"
            2. Scherm keuze, "WIDTH" en kleuren
            3. "BEEP" patroon en volume
            4. Opstart scherm kleur
            5. Land code
            6. Paswoord
            7. BASIC prompt
            8. Opstart titel

          Van  de laatste  3 (pasword,  promt, title) kan er slechts 1
          aktief zijn.


                  S T R U K T U U R   V A N   C L O C K - I C

          Het  clock-IC  bestaat  uit  vier  blokken  zoals  hieronder
          afgedrukt.  Elk blok  bestaat uit  13 registers welke 4 bits
          bevatten (LSB).  Deze registers  worden gespecificeerd van 0
          to  12. Daarnaast  zijn er nog drie control registers (MODE,
          TEST  en   RESET),  waarmee  diverse  akties  kunnen  worden
          uitgevoerd  (register  13  to  15).  De register  in blokken
          (0-12)  en het  MODE register  (13) kunnen worden gelezen en
          beschreven.  De  TEST (14)  en RESET  (15) registers  kunnen
          alleen worden beschreven.

          Blok 0          Blok 1          Blok 2          Blok 3
          (CLOCK)         (ALARM)         (RAM-1)         (RAM-2)
          13 registers    13 registers    13 registers    13 registers

          r#13 MODE
          r#14 TEST  (write only)
          r#15 RESET (write only)


                           M O D E   R E G I S T E R

          Dit register heeft 3 functies, te weten:

            - Blok selecteren
          Om een  register te  lezen of te beschrijven (0-12), moet er
          van  te voren  wel worden  aangegeven met  welk blok er moet
          worden  gewerkt.  De  laagste  twee bits  bevatten het  blok
          nummer. Waneer  eenmaal een  blok is  geselecteerd, kan  een
          register  meerdere malen  worden gebruikt zonder daarna weer
          het blok nummer op te geven.

          Registers  #13,   #14  en   #15  zijn   altijd  toegankelijk
          onafhankelijk van het blok nummer.

            - ALARM aan/uit
          Om  het alarm  aan of uit te zetten, moet bit 2 worden gezet
          of gereset.  "0" is  uit. Omdat  de standaard  MSX2 de alarm
          functies  niet gebruikt,  zal er niets gebeuren als de alarm
          tijd is bereikt. Er wordt alleen ergens een signaal gegeven.

            - Stop CLOCK teller
          Door  bit  3  van  het  MODE  register te  resetten, zal  de
          klok-seconden teller  stil gaan  staan, en  niet meer verder
          tellen.  Door dit  bit weer  te zetten,  zal het tellen weer
          doorgaan.

          MODE register opbouw:

                  b3  b2  b1  b0
          r#13    TE  AE  M1  M0

          M1/M0:  Blok nummer
          AE:     Alarm aan/uit (0 is uit)
          TE:     Klok (0 is uit, in seconden)


                           T E S T   R E G I S T E R

          Dit register  (r#14) wordt gebruikt voor het direct verhogen
          van de hoogste teller, en om na te gaan of de overdracht van
          de  tijd en datum juist is gedaan. Door een van de bits hoog
          te  maken,  wordt  er  een  puls  van  2^14  (is  16384)  Hz
          doorgegeven aan de dag, uur, minuut en seconden tellers.

          TEST register opbouw:

                  b3  b2  b1  b0
          r#14    T3  T2  T1  T0  (puls bits)

          T3:     Dag
          T2:     Uur
          T3:     Minuut
          T4:     Seconden


                          R E S E T   R E G I S T E R

          Het reset register (r#15) heeft de volgende functies:

          - Alarm reset
          Als dit  bit hoog  wordt gemaakt, worden alle alarm register
          ge-reset (nul).

          - Seconden afronden
          Het  klok IC telt ook in honderdste seconden. Om de seconden
          exact  te  kunnen  zetten,  kunnen  de  nivo's voor  de hele
          seconden worden  gereset (nul).  Maak het  bit hoog voor het
          resetten.

          - Clock puls aan/uit
          Het  clock IC genereerd ook 2 lage pulsen. Een puls van 16Hz
          of een  puls van  1Hz. Door  bit 2  "0" te maken zal de 16Hz
          puls  aan zijn.  Door bit 3 "0" te maken zal de 1Hz puls aan
          zijn.  De  MSX  ondersteund  dit  echter  niet, waardoor  er
          feitelijk niets mee kan worden gedaan.

          Reset register opbouw:

                  b3  b2  b1  b0
          r#15    C1  C16 CR  AR

          C1:     1Hz puls
          C16:    16Hz puls
          CR:     Reset fracties voor seconden
          AR:     Reset alarm functies.


                                B L O K   0 / 1

                    T I J D ,   D A T U M   E N   A L A R M

          Blok schema:

                          BLOK 0: KLOK
                  reg. Omschrijving. Dec. b3 b2 b1 b0
                  (0)  Seconden      1e   x  x  x  x      00-59
                  (1)  Seconden      2e   .  x  x  x
                  (2)  Minuten       1e   x  x  x  x      00-59
                  (3)  Minuten       2e   .  x  x  x
                  (4)  Uren          1e   x  x  x  x      00-23
                  (5)  Uren          2e   .  .  x  x
                  (6)  Dag van week  1    .  x  x  x      01-07
                  (7)  Dag           1e   x  x  x  x      01-31
                  (8)  Dag           2e   .  .  x  x
                  (9)  Maand         1e   x  x  x  x      01-12
                  (10) Maand         2e   .  .  .  x
                  (11) Jaar          1e   x  x  x  x      00-255
                  (12) Jaar          2e   x  x  x  x      (+80)

          Bits  aangegeven  met "."  zijn altijd  nul, en  kunnen niet
          worden gemodificeerd.

          Blok nul wordt gebruikt voor het lezen/schrijven van de tijd
          en datum. De data is in max. 4 bits opgeslagen, waardoor een
          ASCII teken  in twee  delen wordt  opgesplitst. Bijvoorbeeld
          een  tijd eenheid bestaat uit 1x4 bits en 1x3 bits. Door bij
          deze bits  het ASCII  teken "0" er bij op te tellen, onstaat
          er 1 decimaal van een getal.

          Voorbeeld: We willen de tijd in uren weten.
                     Als we uit blok 0 dan de volgende register lezen;

           (5)    +"0"    2e decimaal (wordt als eerste afgedrukt!)
           (4)    +"0"    1e decimaal (wordt als tweede afgedrukt!)

          Door  deze twee ASCII waarden achterelkaar af te drukken, is
          de tijd in uren bekend.

          Het jaar  dat wordt bijgehouden, start bij 1980, dus moet er
          bij de tweede decimaal "8" bij worden opgeteld in plaats van
          "0".  De datum  zoals we  die hier in Nederland afdrukken is
          meestal van de volgende orde:

                  Dag/Maand/Jaar

          De dag  van de  week wordt  bijgehouden van  0 tot en met 6.
          Deze wordt vernieuwd waneer de datum ook wordt vernieuwd.


                          BLOK 1: ALARM
                  reg. Omschrijving. Dec. b3 b2 b1 b0
                  (0)      ----      1e   x  x  x  x
                  (1)      ----      2e   .  x  x  x
                  (2)  Minuten       1e   x  x  x  x      00-59
                  (3)  Minuten       2e   .  x  x  x
                  (4)  Uren          1e   x  x  x  x      00-23
                  (5)  Uren          2e   .  .  x  x
                  (6)  Dag van week  1    .  x  x  x      01-07
                  (7)  Dag           1e   x  x  x  x      01-31
                  (8)  Dag           2e   .  .  x  x
                  (9)      ----      1e   x  x  x  x
                  (10) 12 of 24 Uur  2e   .  .  .  x
                  (11) Schrikel jaar 1e   .  .  x  x      00-03
                  (12)     ----      2e   x  x  x  x

          Het  alarm kan  alleen worden  opgegeven in  dagen, uren  en
          minuten. Dit  blok bevat  ook nog  2 register  die betreking
          hebben op de tijd aanduiding, en het schrikkeljaar.

          - 12 of 24 uur selectie
          Er  kunnen twee  klok tijden  worden geselecteerd; de een is
          een 24-uurs  aanduiding, welke  de tijd  in de  middag/avond
          weergeeft als 13:00, en de andere en een 12-uurs aanduiding,
          welke  het als 1 p.m. aangeeft. Door bit 0 van r#10 van blok
          1 hoog te maken, wordt er voor 24-uur gekozen.

                  b3  b2  b1  b0
          r#10/B1 .   .   .   TM

          TM:     0: 12 uren, 1: 24 uren.

          Als er  voor 12-uren  wordt gekozen, zal b1 van r#5 (blok 1)
          aangeven of het 's morgens of 's middags is.

                  b3  b2  b1  b0
                  .   .   HR  .

          HR:     0: Voor de middag (am), 1: namiddag (pm).

          - Schrikkeljaar.

          Register  #11 houd  bij of er dit jaar een schrikkel jaar is
          of niet.  Als dit  zo is, zal de inhoud van dit register nul
          zijn.  Aangezien een  schrikkeljaar om  de vier  jaar plaats
          vindt (februari  heeft dan  29 dagen),  zal dit  een 2  bits
          teller zijn (0-3).


                     I N H O U D   C L O C K - M E M O R Y

          blok  twee en  drie van  het clock-IC wordt gebruikt voor de
          13x4 bits  baterij geheugen  blokken. Deze zien er als volgt
          uit:

          - Inhoud blok 2

          reg.    b3      b2      b1      b0
          (0)              -- ID --
          (1)     X-adjust (-8 tot +7)
          (2)     Y-adjust (-8 tot +7)
          (3)     .       .       Interl  Screen
          (4)     Width (laage bits)
          (5)     Width (hoge bits)
          (6)     Voorgrond kleur
          (7)     Achtergrond kleur
          (8)     Rand kleur
          (9)     Cass spd Pinter  Key Clk Key ON/OFF
          (10)    BEEP soort      Beep Volume
          (11)    .       .       Titel kleur
          (12)    Native Code

          Ik denk dat dit blok wel voor zich spreekt.

          - Inhoud blok 3

          Dit  blok bestaat  ook nog  eens uit  3 sub-bloks.  Deze kan
          worden  gekozen   door  een  ID  code  naar  block  drie  te
          schrijven,     hierna     kan     een    sub-blok     worden
          gelezen/beschreven.

                    Sub-blok 1, openings titel (6 karakters)
          reg.    b3 t/m b0
          (0)     ID:0
          (1)     1e karakter (laag)
          (2)     1e karakter (hoog)
          .
          .
          .
          (11)    6e karakter (laag)
          (12)    6e karakter (hoog)

                              Sub-blok 2, password
          reg.    b3 t/m b0
          (0)     ID:1
          (1)     gebruik ID:1
          (2)     gebruik ID:2
          (3)     gebruik ID:3
          (4)     Paswoord *
          (5)     ,,
          (6)     ,,
          (7)     ,,
          (8)     Key cartridge flag
          (9)     Key cartridge waarde
          (10)    ,,
          (11)    ,,
          (12)    ,,

          * Het paswoord word gecompreseerd opgeslagen in 4x4 bits.
          Hoe  dit wordt  opgeslagen is  mij niet bekend. Waar de rest
          voor dient  zou ik ook precies weten. Gezien ik deze blokken
          toch nooit gebruik.

                     Sub-blok 3, Basic prompt (6 karakters)
          reg.    b3 t/m b0
          (0)     ID:2
          (1)     1e karakter (laag)
          (2)     1e karakter (hoog)
          .
          .
          .
          (11)    6e karakter (laag)
          (12)    6e karakter (hoog)

          De registers van sub-blok 1 & 3 spreken voor zichzelf.


                T O E G A N G   T O T   H E T   C L O C K - I C

          In  het MSX  SUB-ROM Bios  zitten 2 routines voor de toegang
          van deze  clock IC. Om deze aan te roepen moet de inter-slot
          call  routine &h1F5 worden gebruikt. Deze routines gebruiken
          de volgende entries:

          - REDCLK (01F5/SUB), Read CLOCK-IC data
          In:     C, register en block.
          Uit:    A, register data.
          Funtie: Lees CLOCK-IC register.

          - WRTCLK (01F9/SUB), Write CLOCK-IC data
          In:     C, register en block.
                  A, register data.
          Funtie: Schrijf naar CLOCK-IC register.

          Het  C register bevat twee waarden; het register/sub-blok en
          het blok nr. De volgende bits worden gebruikt:

                  b7  b6  b5  b4  b3  b2  b1  b0
          C reg.  .   .   M1  M2  A3  A2  A1  A0

          M1/M2:  Blok nr.
          A3-A0:  Register/sub-blok nr

          Maar natuurlijk kan er ook gebruik worden gemaakt van de OUT
          poorten (dit  doen de  BIOS routines  ten slotte  ook). Maar
          aangezien  de BIOS  routines vaak meer doen dan gedaan hoeft
          te worden,  kunnen er  beter eigen routines worden gebruikt.
          Om  namelijk meerdere keren een register uit het zelfde blok
          te kunnen  lezen/schrijven, hoeft  er maar  1 keer te worden
          opgegeven  wat voor  (sub) blok er moet worden gebruikt. Bij
          de bios  routines wordt  dit altijd  het blok gezet, wat dus
          tijds verlies is.
          De poorten die het IC gebruikt zijn:

          &hB4  -  register/block nr
          &hB5  -  Data.

          Hieronder zijn  enkele routines  geplaats als voorbeeld voor
          het lezen, schrijven van het IC.

          ;+----- Clock Routines -----+
          ; written by: SHADOW from FUZZY LOGIC.
          ; last update: 29/02/94 for sunrise special.

          ; Read Clock_Register
          ; note: Set block nr with "SC_BLK" first!
          ; In:   A, Clock register/block ID  to read
          ; Out:  A, clock data
          RD_CLK: DI
                  OUT   (&HB4),A
                  IN    A,(&HB5)
                  AND   &H0F
                  EI
                  RET

          ; Write Clock_Register
          ; note: Set block nr with "SC_BLK" first!
          ; In:   A, clock register/block ID  to write
          ;       B, register data
          WR_CLK: DI
                  OUT   (&HB4),A
                  IN    A,(&HB5)
                  AND   &H0F
                  EI
                  RET

          ; Set Clock_Block
          ; In:   B, bloc nr (0-3)
          SC_BLK: CALL  RC_MOD
                  AND   &H0C            ; save count/alarm bits
                  OR    B               ; add block
                  DI
                  OUT   (&HB5),A
                  EI
                  RET

          ; Read Clock_Mode reg
          RC_MOD: LD    A,13
                  JP    RD_CLK


          Dit  zijn feitelijk  alle routines  die nodig  zijn voor het
          gebruik  van  de CLOCK-IC.  Op deze  special staat  een file
          "CLOCK-IC.asc" die  een paar  routines bevat  die de tijd en
          datum op het beeld zet.
          Voor  het beschrijven  van het  MODE register is het wel van
          belang dat  de rest  van de  bits bewaard  blijven, dus lees
          eerst even dit register uit met "RC_MOD".

          Mocht  iemand weten  hoe het pasword-sub-blok wordt beruikt,
          laat het dan even weten, want zelf sta ik voor een raadsel.

                                                 R�man van der Meulen
