                           I / O   I N T E R F A C E 
                                                      
          
          
                 J O Y S T I C K   E N   T O E T S E N B O R D 
          
          Over  het gebruik  van joysticks en mouse via I/O poorten is 
          nog  niet  veel  gepubliceerd,  waardoor (nog)  vaak gebruik 
          wordt gemaakt  van de  "trage" MSX ROM BIOS. Door gebruik te 
          maken  van de  I/O poorten  kan het  uitlezen en  vooral het 
          verwerken van  de informatie  veel sneller  geschieden (tja, 
          bedenk eens een leuk woord?).
          
          
                             P S G   P O O R T E N 
          
          Zoals  mischien bekend is, wordt de PSG niet alleen gebruikt 
          voor het generen van geluiden maar ook voor het uitlezen van 
          informatie voor  de joysticks, mouse, trackball, en leespen. 
          Deze  PSG  gebruikt  twee 8-bit  I/O poorten,  poort A  en B 
          respectievelijk r#14 en r#15.
          
          De PSG gebruikt de volgende I/O poorten:
          
          - &HA0  register poort. Geef hierin op het register dat moet 
                  worden gelezen, of beschreven.
          - &HA1  schrijf poort. Hier kan de data in worden geschreven 
                  die naar het register -  opgegeven in &HA0 - moet.
          - &HA2  lees poort. Na het opgeven van welk register er moet
                  worden gelezen, zal hier de data in staan.
           
          
          Een overzicht van de (joystick) poorten:
          
              interface 1                            interface 2
          +-------------------+                  +-------------------+
          | 1   2   3   4   5 |                  | 1   2   3   4   5 |
          |   6   7   8   9   |                  |   6   7   8   9   |
          +-------------------+                  +-------------------+
          
          (8) - naar poort B:b4                  (8) - naar poort B:b5
          (5) - +5V                              (5) - +5V
          (9) - nul                              (9) - nul
          
          De  terminals  (pennen)  1,2,3,4,5  en  6 van  beide poorten 
          worden samen gevoegd en worden gezonden naar poort A:b0-b5.
           
          
                  F U N C T I E   V A N   D E   P O O R T E N 
          
          De twee poorten worden als volgt gebruikt:
          
          Poort A b7  b6  b5  b4  b3  b2  b1  b0
          r#14    cas x   trB trA DR3 DR2 DR1 DR0
          
          cas:    data input van cassette.
          trA:    Vuurknop A
          trB:    Vuurknop B
          DRx:    Joystick richting.
                  DR0 = omhoog
                  DR1 = omlaag
                  DR2 = links
                  DR3 = rechts
          
          Noot: De bits zijn ge�nverteerd, dus 0=aan en 1=uit.
          
          
          Poort B b7  b6  b5  b4  b3  b2  b1  b0
          r#15    ar  In  2t8 1t8 0   0   0   0
          
          ar:     Arabische led. (0=ON)
          in:     Interface nr. (0=interface 1)
          1t8:    Verbonden met pen 8 van interface 1, voor de mouse.
          2t8:    Verbonden met pen 8 van interface 2, voor de mouse.
          
          Noot: Ook hier zijn de bits ge�nverteerd.
          
          
                  L E Z E N   V A N   E E N   J O Y S T I C K 
          
          Om  joystick 1  te lezen,  zullen we  eerst b6  van r#15 nul 
          moeten maken.  Let hierbij op dat de andere bits niet worden 
          verwaarloosd.  Na het  schrijven van  r#15, kan  de joystick 
          status worden uitgelezen. Laat ik dit verduidelijken met een 
          voorbeeld:
          
          ; Read Joystick.
          ; In:  C, joystick nr.(&H00=stick #1, &H40=stick #2)
          ; Out: A, joystick status
          RD_JOY: DI
                  LD    A,15
                  OUT   (&HA0),A
                  IN    A,(&HA2)
                  AND   &B10001111      ; interface 1
                  OR    C
                  OUT   (&HA1),A
                  LD    A,14
                  OUT   (&HA0),A
                  IN    A,(&HA2)
                  EI
                  RET
          
          Als  eerste  wordt r#15  gelezen, omdat  somige bits  moeten 
          worden  bewaard.  Door  de  AND functie  zal b6  nul worden, 
          waardoor interface  1 zal  worden gebruikt.  Register 'C' is 
          nul, dus zal b6 ook niet worden gezet (als je joystick 2 wil 
          lezen,  moet  b6  van 'C'  gezet worden).  Hier na  wordt de 
          waarde  weer naar  de PSG geschreven. Door nu r#14 te lezen, 
          zal het A register de joystick1 waarden bevatten.
          
          
                       P R A K T I S C H   G E B R U I K 
          
          Om nu  bijvoorbeel in een spel de stick waarde te lezen, kan 
          er  gebruik worden  gemaakt van de ROM-BIOS "GTSTCK" (&HD5). 
          Deze routine doet feitelijk het volgende:
          
          Kijkt  welke stick  er moet worden gelezen (opgegeven in 'A' 
          register).  Dat  kan  zijn:  0, Keyboard  1, Joystick  #1 2, 
          Joystick #2. Stel we willen de waarde van Joystick #1 weten. 
          De  BIOS  routine leest  eerst de  stick waarde  uit de  PSG 
          poorten. Vervolgens  zal uit een tabel de stickwaarde worden 
          gelezen.  Deze waarden  zijn hopelijk  wel bekend (1=omhoog, 
          2=omhoog/rechts, 3=rechts etc.). Afhankelijk van deze waarde 
          die wordt  terug gegeven,  kan in  een eigen applicatie naar 
          een  bepaald adres worden gesprongen. Maar zou het niet veel 
          sneller zijn  als we  inplaats van  de stickwaarde, een Jump 
          adres terug zouden krijgen. Hierdoor hoeven we niet eerst de 
          stick  waarde te  onderzoeken, om  daarna naar  een label te 
          springen.
          
          Als we  even verder  borduren over  zo'n routine zou het ook 
          wel  makelijk kunnen  zijn om, in plaats van de stick nummer 
          op te  geven, gewoon alle sticks uit te laten lezen. Met als 
          prioriteiten: Keyboard, Joystick #1, Joystick #2.
          
          Als  we nu  ook nog een tabel pointer adres opgeven, waaruit 
          de adressen  kunnen worden gelezen, dan zouden we een pracht 
          van een routine hebben. Deze zou niet alleen snel zijn, maar 
          ook  zeer multifunctioneel. Immers we hoeven alleen maar een 
          tabel adres  op te  geven, waar  in staat waar naar toe moet 
          worden gesprongen als de corresponderende stick waarde wordt 
          gelezen.  Zo'n routine  zal er  dan als  volgt uit  komen te 
          zien:
          
          ; Read Stick(s)
          ; In:  HL= Tabel pointer adres.
          ; Out: HL= Jump adres. (0=invallid)
          RDSTCK: CALL  RDS_KB          ; Lees keyboard.
                  AND   &H0F
                  CP    15              ; KB=0 ?
                  LD    C,0
                  CALL  Z,RD_JOY        ; Ja, dan lees joystick #1
                  AND   &H0F
                  CP    15              ; Is deze ook nul ?
                  LD    C,&H40
                  CALL  Z,RD_JOY        ; Lees dan joystick #2  
                  AND   &H0F
                  LD    C,A
                  LD    B,0
                  ADD   HL,BC
                  ADD   HL,BC
                  PUSH  DE
                  LD    E,(HL)          ; adr Laag
                  INC   HL
                  LD    D,(HL)          ; adr Hoog
                  EX    DE,HL           ; HL=adres
                  POP   DE
                  RET
          ; Read Keyboard cursors. (same as RD_JOY, but now KB)
          RDS_KB  LD    A,8
                  CALL  RDKMAT          ; [RDKMAT] = BIOS &H0141
                  RRCA                  ;>Zorg er voor dat de bits
                  RRCA                  ; overeen komen met die van
                  RRCA                  ; de joysticks: deel door 8,
                  SET   3,A             ; swap bits 2,3 (L -> R -> L)
                  BIT   2,A
                  JP    NZ,RSKB.0
                  RES   3,A
          RSKB.0  SET   2,A
                  BIT   7,A
                  RET   NZ
                  RES   2,A
                  RET
          
          Bij het lezen van de Cursors wordt rij 8 van het toetsenbord 
          matrix  gelezen,  en  geconverteerd  zodat  de  bits van  de 
          cursors en  joystick het zelfde zijn. De cursors bits hebben 
          namelijk deze volgorde: (rij 8)
          
                  b7   b6   b5   b4   b3   b2   b1   b0
           r8     RGT  DWN  UP   LFT  x    x    x    x
          
          RGT:    Rechts
          DWN:    Omlaag
          UP:     Omhoog
          LFT:    Links
          
          Ook hier zijn de bits weer omgedraaid, dus 0=AAN en 1=UIT!
          
          Vergelijk  de bits  maar een met die van de joystick. Na het 
          converteren zal de cursor waarde het zelfde zijn als die van 
          en joystick,  zodat we  maar een pointer tabel hoeven aan te 
          maken. De bit-waarden van alle sticks:
          
          b0:     Omhoog
          b1:     Omlaag
          b2:     Links
          b3:     Rechts
          
          Als een stick-waarde gelijk ik aan 15 (wat dus nul is als de 
          bits  zijn geinverteerd),  dan zal een volgende stick worden 
          gelezen. De pointer tabel zal er als volgt uit moeten zien:
          (bevat 16 adressen)
          
          STCKTB: DB      0,   0,   0, 0, R/D, R/U, R, 0
                  DB      L/D, L/U, L, 0, L,   0,   D, U
           
           L:     Links
           R:     Rechts
           D:     Omlaag
           U:     Omhoog
           en combinaties:  U/L= Omhoog en Links.
           
          De waarde  die uit  de RDSTCK routine komt, is nu een adres. 
          Als  we in  onze eigen applicatie een jump routine aanroepen 
          met het  adres in  HL, dan  zal deze  de betreffende routine 
          aanroepen   indien  het  adres  ongelijk  is  aan  nul.  Een 
          voorbeeld van een dergelijke jump-routine:
          
          JUMPER: LD      A,H
                  OR      L
                  RET     Z       ; HL=0
                  JP      (HL)    ; Spring naar een adres. De return
                                  ; in die routine keert terug naar 
                                  ; waar JUMPER is aangeroepen.
           
          Met  deze routine  moet het nu mogelijk wezen om snel (save) 
          en simpel  gebruik te  kunnen maken  van de cursors en beide 
          joysticks.
          
          Nu  resten alleen  nog de vuurknoppen. Als we net zo'n soort 
          routine maken  als RDSTCK,  waarbij ALLE  vuurknoppen worden 
          gelezen  (natuurlijk onderscheid makend tussen vuurknoppen A 
          en B). Als we voor vuurknop 1 de spatiebalk, joystick 1 knop 
          A en  joystick 2  knop A  nemen, zal de routine er als volgt 
          uit komen te zien:
          
          ; Read Trigger #1
          ; Out: Zerro flag, 0=ON, 1=OFF
          RDTRG1: LD      A,8     ; Lees spatie
                  CALL    RDKMAT 
                  AND     &H01    ; spatie ingedrukt? (bit 0)
                  RET     Z       ; Ja!
           
                  LD      C,0     ; Joystick #1
                  CALL    RD_JOY
                  AND     &H10    ; Vuurknop A (bit 4)
                  RET     Z       ; Ja!
          
                  LD      C,&H40  ; Joystick #2
                  CALL    RD_JOY
                  AND     &H10    ; Vuurknop A
                  RET
           
          Voor  vuurknoppen B,  kunnen we  wat betreft het keyboard de 
          GRAPH toets gebruiken:
          
          ; Read Trigger #2
          ; Out: Zerro flag, 0=ON, 1=OFF
          RDTRG2: LD      A,6     ; Lees GRAPH
                  CALL    RDKMAT 
                  AND     &H40    ; ingedrukt? (bit 2)
                  RET     Z       ; Ja!
                  LD      C,0     ; Joystick #1
                  CALL    RD_JOY
                  AND     &H20    ; Vuurknop B (bit 5)
                  RET     Z       ; Ja!
                  LD      C,&H40  ; Joystick #2
                  CALL    RD_JOY
                  AND     &H20    ; Vuurknop B
                  RET
          
          De  routine  is  bijna  het  zelfde  als  RDTRG1.  Het enige 
          verschil is dat hier de B vuurknop wordt gelezen.
          
          Het extra  voordeel van  al deze  routines is dat er nu niet 
          van het ROM-BIOS gebruikt wordt gemaakt. Met deze informatie 
          moet  het mogelijk zijn om de joystick en cursors voledig te 
          kunnen gebruiken.
          
                                                 R�man van der Meulen
