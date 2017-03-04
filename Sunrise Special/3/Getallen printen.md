          
                        G E T A L L E N   P R I N T E N 
                                                         
          
          Het  komt bij  programmeren vaak voor dat je getallen op het 
          scherm wilt  zetten. In BASIC gaat dat meestal decimaal, dat 
          is  in BASIC  ook het  makkelijkst. In ML is decimaal minder 
          makkelijk, binair  of hexadecimaal is daar logischer. Ik zal 
          nu  een  drietal  routines  behandelen  om  in  ML  getallen 
          respectievelijk  decimaal,  hexadecimaal  en  binair  op het 
          scherm te zetten.
          
          
                                D E C I M A A L 
          
          Getallen  in decimaal  op het  scherm zetten,  in BASIC heel 
          makkelijk maar  in ML toch iets moeilijker. Met onderstaande 
          routine  kun je  16 bits getallen in ML decimaal printen met 
          voorloopnullen en naar keuze 1, 2, 3, 4 of 5 cijfers.
          
          De voorloopnullen zijn overigens niet gedaan voor de netheid 
          maar om de routine simpel te houden. In BASIC moet je moeite 
          doen om  de voorloopnullen  erbij te  zetten, bij ML moet je 
          juist moeite doen om ze weg te laten.
          
          Tot zover de inleiding, laten we nu maar snel verdergaan met 
          de  source. Zoals  gewoonlijk staat de source op de disk als 
          ASCII file.
          
          
          ; D E C I M A A L . A S C 
          ; Roep naar keuze DIGIT1, 2, 3, 4 of 5 aan voor uitvoer
          ; met het gewenste aantal cijfers
          ; In : HL=getal
          ; Uit:Getal wordt met &HA2 op scherm gezet
          ;     Registers AF, DE, BC en HL worden gebruikt
          ; Door Stefan Boer
          ; (c) Ectry 1993
          ; Sunrise Special #3
          ; (c) Sunrise 1993
          
          DIGIT5: LD    DE,10000
                  CALL  PR_DIG
          DIGIT4: LD    DE,1000
                  CALL  PR_DIG
          DIGIT3: LD    DE,100
                  CALL  PR_DIG
          DIGIT2: LD    DE,10
                  CALL  PR_DIG
          DIGIT1: LD    DE,1
                  CALL  PR_DIG
                  RET
          
          
          Door DIGIT5  aan te  roepen wordt  het getal  met 5  cijfers 
          (digits)  geprint,  DIGIT4  met 4  cijfers, etc.  De routine 
          PR_DIG  (PRint DIGit)  kijkt hoeveel  maal DE in HL past, en 
          voert dat getal uit.
          
          
          PR_DIG: XOR   A               ; teller = 0
          NEXT:   LD    B,H
                  LD    C,L             ; LD BC,HL
                  OR    A               ; wis carry, A blijft gelijk
                  SBC   HL,DE           ; trek DE van HL af
                  JR    C,OUTPUT        ; DE > HL, dan einde lus
                  INC   A               ; verhoog teller
                  JR    NEXT
          
          
          Eerst  wordt het  A register 0 gemaakt, dit is de teller die 
          bijhoudt hoe  vaak DE  in HL  past. In de lus wordt eerst de 
          huidige  waarde van  HL bewaard  in BC.  PUSHen is hier niet 
          handig en het is sneller dan LD (...),HL.
          
          Vervolgens wordt  DE van  HL afgetrokken.  SUB HL,DE bestaat 
          niet,  dus moeten  we SBC gebruiken en eerst de carry wissen 
          met OR  A (anders  zou een  eventuele carry roet in het eten 
          kunnen gooien bij SBC, SuBtract with Carry).
          
          Treedt  er nu  een carry op, dan was DE blijkbaar groter dan 
          HL (past  er dus  0 keer  meer in)  en verlaten  we de  lus. 
          Anders verhogen we de teller en doorlopen we de lus opnieuw.
          
          
          OUTPUT: LD    H,B
                  LD    L,C             ; LD HL,BC
                  ADD   "0"
                  CALL  &HA2            ; voer waarde uit
                  RET
          
          Hier  wordt de waarde van HL voordat er de laatste keer werd 
          afgetrokken weer  hersteld, dit  is nodig  om ook  de andere 
          cijfers  te kunnen afdrukken. Vervolgens wordt de ASCII code 
          van "0"  bij A geteld (A bevat het aantal keren dat DE in HL 
          past),  en wordt  dit cijfer  op het  scherm gezet  met BIOS 
          routine CHPUT (&HA2).
          
          Niet zo'n  moeilijke routine,  maar wel  een handige  die je 
          vaak   nodig  hebt.   En  er   zijn  toch   veel  beginnende 
          programmeurs die  er moeite mee hebben. Hexadecimale uitvoer 
          is  veel  simpeler  te  programmeren,  maar dat  is voor  de 
          gebruiker  niet zo  handig. Ik  heb in ieder geval nog nooit 
          een spel gezien met de score in hexadecimaal!
          
          
                            H E X A D E C I M A A L 
          
          De  routine om  hexadecimaal te printen is niet korter, maar 
          wel  veel  simpeler.  Hexadecimaal  wordt bijvoorbeeld  vaak 
          gebruikt om  adressen weer  te geven.  Genoeg gepraat,  hier 
          komt  de routine.  De routine  zet een  getal in  DE op  het 
          scherm in  4 digits,  met voorloopnullen. De routine is vrij 
          simpel, dus meer uitleg dan wat achter ; staat is overbodig.
          
          
          ; H E X A M A A L . A S C 
          ; Print 16 bits getal hexadecimaal
          ; In : DE = 16 bits getal
          ; Uitvoer naar scherm
          ; Gebruikt: AF, HL, BC
          ; Door Stefan Boer
          ; (c) Ectry 1993
          ; Sunrise Special #3
          ; (c) Sunrise 1993
          
          PR_HEX: LD    B,0
                  LD    A,D             ; high byte
                  AND   &HF0            ; linker digit
                  RRCA
                  RRCA
                  RRCA
                  RRCA                  ; verplaats naar lage nibble
                  CALL  UITHEX          ; uitvoer
                  LD    A,D             ; high byte
                  AND   &H0F            ; rechter digit
                  CALL  UITHEX
                  LD    A,E             ; low byte
                  AND   &HF0            ; linker digit
                  RRCA
                  RRCA
                  RRCA
                  RRCA
                  CALL  UITHEX
                  LD    A,E             ; low byte
                  AND   &H0F            ; rechter digit
                  JP    UITHEX          ; CALL & RET
          
          UITHEX: LD    HL,HEXTAB       ; beginadres tabel
                  LD    C,A             ; LD BC,A (B was al 0)
                  ADD   HL,BC           ; bereken adres in tabel
                  LD    A,(HL)          ; haal ASCII code van digit
                  JP    &HA2            ; CALL & RET
          
          HEXTAB: DM    "0123456789ABCDEF"
          
          
                                  B I N A I R 
          
          Om het verhaal volledig te maken moet ook binair er nog bij, 
          al  zul je  dat normaal  gesproken toch weinig gebruiken. De 
          routine is  heel kort maar toch iets moeilijker te begrijpen 
          dan de routine voor hexadecimaal.
          
          De  routine print  het getal  in C met 8 digits en voorloop- 
          nullen. Als  je een  16 bits  getal wilt  printen, kun je de 
          routine natuurlijk gewoon twee keer aanroepen. Bijvoorbeeld:
          
                  LD   C,H
                  CALL PR_BIN
                  LD   C,L
                  CALL PR_BIN
          
          Om HL 16 bits af te drukken. De source:
          
          
          ; B I N A I R . A S C 
          ; Print 8 bits getal binair op het scherm
          ; In: C = 8 bits getal
          ; Uitvoer naar scherm
          ; Gebruikt:
          ; Door Stefan Boer
          ; (c) Ectry 1993
          ; Sunrise Special #3
          ; (c) Sunrise 1993
          
          PR_BIN: LD    B,8             ; 8 bits te gaan
          BINLUS: LD    A,24            ; "0"/2
                  RLC   C               ; roteer C, bit 7 naar carry
                  RLA                   ; roteer A, carry naar bit 0
                  CALL  &HA2            ; uitvoer
                  DJNZ  BINLUS          ; dit 8 keer
                  RET
          
          
          Hier  is denk ik nog wel wat uitleg nodig. In het A register 
          wordt eerst de waarde 24 gezet, dat is de ASCII code van "0" 
          (48), maar  dan ÇÇn  bit naar  rechts geroteerd.  Vervolgens 
          roteren we register C ÇÇn bit naar links, waarbij bit 7 (het 
          bit  dat we  willen afdrukken!)  in de  carry terecht  komt. 
          Vervolgens roteren we register A ÇÇn bit naar links met RLA, 
          wat overigens  hetzelfde is  als RL  A alleen sneller en een 
          byte minder. Hierdoor worden de bits die al in A stonden dus 
          meegeschoven,  en staat  de ASCII  van "0" in A. Maar bij RL 
          wordt  de  carry in  bit 0  geschoven. Was  de carry  0, dan 
          gebeurt er  niets en  staat er "0" in A, was de carry 1, dan 
          wordt  bit 0  gelijk en  staat er  "1"! Zo gaan we alle bits 
          langs.
          
          Kortom, het  ziet er  ingewikkeld uit maar is eigenlijk heel 
          simpel!
          
                                                          Stefan Boer