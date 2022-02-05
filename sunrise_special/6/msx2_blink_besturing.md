                    M S X 2   B L I N K   B E S T U R I N G


          Een van de nieuwe mogelijkheden van de MSX2 video chip is de
          zogenaamde   BLINK.  Voor   screen  0   is  dit   een  extra
          mogelijkheid om  delen van  het scherm  van een  3e en/of 4e
          kleur  te voorzien.  Ook in  andere screen modes is de blink
          uitstekend te  gebruiken. Maar  in dit  artikel beperk ik me
          tot screen 0 mode 80.

          In  de  VDP  cursus  #7  is  hier  ook al  het een  en ander
          geschreven. In dit artikel zal ik proberen de mogelijkheden,
          en  vooral het  gebruik van de blink te beschrijven, met -in
          assembly geschreven- routines.


                             B A S I S   A D R E S

          Elke  positie  op het  scherm kan  in de  blink mode  worden
          gezet. Voor  elke positie  is 1  bit gereserveerd.  Als deze
          hoog  is,  is  de blink  geactiveerd. Deze  informatie wordt
          bewaard  in de  Blink tabel.  Het start adres van deze tabel
          wordt gespecifeerd  door het  zetten van  de 8  hoge bits in
          register  #3 en  #10 (A9-A16). De 9 laagste bits zijn altijd
          "1" waardoor de tabel altijd om de 512 bytes ligt. Een blink
          tabel is 512 bytes lang, dus kunnen er 512*8 posities op het
          scherm worden aangezet (1 bit per karakter).

             MSB  b7  b6  b5  b4  b3  b2  b1  b0  LSB
          r#3     A13 A12 A11 A10 A9  1   1   1
          r#10    0   0   0   0   0   A16 A15 A14

          Formules:
                   VRAM adr = r#3 and &hF8 * 64
                       r#3  = VRAM adr / 64 and &h00F8 or &h07

          Hierbij laat ik r#10 gewoon nul, omdat in de praktijk de
          blink tabel nooit zo hoog in het VRAM zal worden gezet.

          Stel we willen de Blink tabel laten beginnen op VRAM adres
          &h0E00, dan zullen de register als volgt moeten worden gezet:

          r#3     &h3F    (&h0E00/64 or 7)
          r#10    &h00

          standaard  zijn   deze  registers  gevuld  met  de  volgende
          waarden:
          r#3     &h27   (VRAM adr: &h0800)
          r#10    &h00

          In  Assembly zal  een dergelijke  berekening voor  het basis
          adres er als volgt
          uitzien:

          ; Set Blink Base address.
          ; In:  H(L), VRAM adr. L, not necessary.
          BLNBAD: XOR A                   ; Hoogste bits:0
                  LD      (BLN_BA),A
                  LD A,H
                  AND     &HFE
                  LD      (BLN_BA+1),A
                  RLCA
                  RLCA
                  AND     &HF8
                  OR      &H07
                  DI
                  OUT     (&H99),A
                  LD      A,3 OR &H80     ; r#3
                  OUT     (&H99),A
                  EI
                  RET
          BLN_BA  DW      &H0800          ; (standaard)

          Omdat het adres altijd in stappen van 512 bytes gaan, zullen
          de 8  laagste bits  in ieder  geval "0" zijn. Vandaar dat de
          routine begind met het nul maken van de laagste bits.
          Het  VRAM  adres  wordt  bewaard,  zodat  deze  later is  te
          gebruiken bij het beschrijven van deze tabel.


                      K L E U R   E N   T I J D S D U U R

          De kleuren van de blink worden gezet in r#7. Waarbij de
          hoogste 4 bits de kleur van de karakters is, en de laatste 4
          bits de kleur van de blink achtergrond.

             MSB  b7  b6  b5  b4  b3  b2  b1  b0  LSB
          r#7     FC3 FC2 FC1 FC0 BC3 BC2 BC1 BC0

          N.B.    FC0-FC3: Foreground Colour
                  BC0-BC3: Background Colour

          voorbeeld:
          We willen de karakters waar de blink mode aan staat, geel
          maken, en de achtergrond daarvan rood. Dan wordt r#7 als
          volgt:
          r#7, &hA6       (A=geel, 6=rood)

          Voor de tijd dat de blink mode aan/uit staat, moeten we r#12
          gebruiken. De 4 hoogste bits bevatten de tijd dat de kleuren
          (r#3) worden geactiveerd, de 4 laagste is de tijd dat de
          originele karakter kleuren weer worden geactiveerd.

             MSB  b7  b6  b5  b4  b3  b2  b1  b0  LSB
          r#12    BC3 BC2 BC1 BC0 CC3 CC2 CC1 CC0

          N.B.    BC0-BC3: Blink Colour
                  CC0-CC3: Charakter Colour

          De  tijd wordt aangegeven in "vertical scans", dus elke 1/50
          (of 1/60) seconde.

          Met deze  4 genoemde  registers is  de initialisatie  van de
          blink  verklaard.  Alles  wat  we  nu  nog  moeten  doen  is
          daadwerkelijk de scherm posities aan of uit zetten.


                          S C H E R M   P O S I T I E

          De posities van de blink worden als volg berekend:
          (uitgaand van een width 80 scherm)

          [basis adres] + (Yx10) + (X/8)

          Omdat  er per  bit wordt gerekend, wordt de X gedeeld door 8
          (8  bits).  Omdat  er  maximaal 80  karakters op  een scherm
          kunnen, worden  er dus  10 bytes  gebruikt voor  een Y  lijn
          (80/8).

          Voordat we de blink gaan gebruiken, doen we er goed aan om
          eerst de hele blink tabel leeg te maken. Een soortgelijke
          routine:

          ; Clear Blink table.
          BLNCLR: LD      HL,(BLN_BA)        ; basis adres
                  LD      C,0                ; laagste 64k.
                  LD      DE,512             ; totaal aantal bytes
                  XOR     A                  ; vul met nul
                  JP      FILVRM             ; een "Vul VRAM" routine

          De "FILVRM" routine in dit voorbeeld heeft de volgende
          data nodig:
          HL/C, VRAM adres        C bepaalt welk VRAM blok (64k.)
          DE, lengte
          A, vuldata

          Natuurlijk kunt u uw eigen routine ook gebruiken.

          Nu komt eigenlijk het moeilijkste, een routine maken die een
          blink  positie aan  of uit zet. Hierbij moet het VRAM adres,
          en  het   start  bit   worden  berekend.   Dit  laatste   is
          noodzakelijk omdat immers per bit wordt gerekend. Ook is het
          belangrijk,  dat de  bits die al aan/uit stonden onveranderd
          te laten!

          De volgende routine zal het VRAM start adres berekenen:

          ; Calculate Blink VRAM adr.
          ; In:  H, pos-X.  L, pos-Y
          ; Out: HL, VRAM adr.
          C_BLAD: LD      A,H
                  RRCA            ; X/8
                  RRCA
                  RRCA
                  AND     &HF8
                  LD      H,A
                  LD A,L          ; Yx10 (Yx2 + Yx8)
                  RLCA
                  AND     &H3F
                  LD      L,A     ; L=Yx2
                  RLCA            ; Yx4 (+Yx4 = Yx8)
                  RLCA
                  ADD     A,H     ; (Ax8) + (Ax2)
                  ADD     A,L     ; + X
                  LD      L,A
                  LD      A,(BLN_BA+1)    ; basis adres
                  LD      H,A
                  LD      (BLN_AD),HL     ; bewaar VRAM adres
                  RET

          BLN_AD  DW      &H0800

          Deze routine  is uiters  snel qua  berekening, maar  heeft 1
          beperking,  dat er  maar 256  VRAM adressen  (2048 posities)
          kunnen worden  berekent Hierdoor  is het start adres bekend,
          nu nog het start bit.

          Door  gebruik  te  gaan  maken  van  een  vermenig vuldigins
          routine, is dit wel mogelijk maar dat gaat dan wel ten koste
          van  de snelheid.  Voor dergelijke rekenroutine verwijs ik u
          naar de Z80 REKEN cursus van Alex van der Wal (special #5).
          Een  blink  adres  berekening  routine  gebruik  makend  van
          vermenig vuldiging:

          ; calc Blink VRAM-adres.
          ; In:  HL, XXYY (blink pos.)
          C_BLAD: LD    A,H
                  AND   &HF8
                  RRCA
                  RRCA
                  RRCA                  ; /8
                  PUSH  AF
                  LD    A,L             ; Y*10
                  LD    DE,10
                  CALL  HL=ADE          ; Uit: HL= A x DE
                  POP   AF
                  LD    C,A
                  LD    B,0
                  ADD   HL,BC
                  LD    BC,(BLN_BA)
                  ADD   HL,BC
                  LD    (BLN_AD),HL
                  RET

          A  bevat nu een bit waarmee moet worden gestart. Stel nu dat
          we als  X positie 5 nemen. Het start bit (in reg. A) zal dan
          als volgt zijn: (uitgaand dat X de waarde 0-79 kan hebben)

          A, &b00000100   ; 5e bit van links is hoog.

          Met  deze start  gegevens kunnen  we nu  een scherm  positie
          voorzien  van  de blink  mode. De  volgende routine  zal een
          (aantal) posities aan (kunnen) zetten:

          ; Set blink position ON.
          ; In:  H, pos-X. L, pos-Y. B, total positions
          BLNON:  PUSH    BC
                  LD      A,H
                  PUSH    AF
                  CALL    C_BLAD          ; VRAM adres
                  POP     AF
                  CALL    C_STBT          ; start bit
                  POP     BC
                  LD      E,A
          BLON.1  LD      C,0             ; laagste 64k.
                  CALL    SR_VRM          ; Zet VDP in lees mode.
                  IN      A,(&H98)        ; Lees oude blink bits.
                  PUSH    AF
                  CALL    SW_VRM          ; Zet VDP in schrijf mode.
                  POP     AF
          BLON.0  OR      E
                  RRC     E               ; Schuif bit
                  JR      NC,BLON.2       ; Laagste bit gehad?
                  OUT     (&H98),A        ; ja, schrijf Blink
                  INC     HL              : volgende 8 posities.
                  DJNZ    BNON.1
                  RET
          BLON.2  DJNZ    BLON.0
                  OUT     (&H98),A
                  RET

          De routines  "SR_VRM" en  "SW_VRM" zorgen er voor dat de VDP
          in  lees of schrijf mode wordt gezet. Natuurlijk kunt u hier
          ook uw eigen routines gebruiken.
          De "OR  E" instructie zorgt ervoor dat de desbetrefende bits
          worden gezet, zodra het laatste bit gedaan is (b0), wordt de
          volgende VRAM positie (8 bits) gebruikt.

          Een routine die of een (of meer) blink positie(s) zet:

          ; Reset Blink position.
          ; In:  H, pos-X. L, pos-Y. B, total pos.
          BLNOFF: PUSH    BC
                  LD      A,H
                  PUSH    AF
                  CALL    C_BLAD          ; VRAM adres
                  POP     AF
                  CALL    C_STBT          ; start bit
                  POP     BC
                  CPL                     ; draai bits om
                  LD      E,A
          BLOF.1  LD      C,0             ; laagste 64k.
                  CALL    SR_VRM          ; Zet VDP in lees mode.
                  IN      A,(&H98)        ; Lees oude blink bits.
                  PUSH    AF
                  CALL    SW_VRM          ; Zet VDP in schrijf mode.
                  POP     AF
          BLOF.0  AND     E               ; zet 1 bit uit.
                  RRC     E               ; Schuif bit
                  JR      C,BLOF.2        ; Laagste bit gehad?
                  OUT     (&H98),A        ; ja, schrijf Blink
                  INC     HL              : volgende 8 posities.
                  DJNZ    BNOF.1
                  RET
          BLOF.2  DJNZ    BLOF.0
                  OUT     (&H98),A
                  RET

          Deze  routine is  feitelijk het  zelfe als  "BLNON", maar nu
          wordt  de  instructie "AND  E" gebruikt  om de  bits uit  te
          zetten.
          Met deze  2 laatst  genoemde routines is het natuurlijk heel
          makelijk  om een  routine te  maken die  een Blok aan of uit
          zet, waarbij  bijvoorbeeld "B"  het aantal  X posities is en
          "C"  het  aantal  Y  posities.  Hierbij  kan  gebruik worden
          gemaakt van het VRAM adres wat opgeslagen staat in "BLN_AD".
          Nu  hoeft het  adres, en  het start  bit maar  1 keer worden
          berekend. Het  VRAM adres van de volgende Y posities ligt 10
          hoger.

          Met  deze routines  en uitleg  moet het  mogelijkzijn om  de
          Blink voledig te benutten in screen 0.

                                                 R�man van der Meulen
