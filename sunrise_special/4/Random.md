                                  R A N D O M 
                                               
          
          In  MCCM 58  stond deze  source. Deze  was echter zonder het 
          -TIME gedeelte.  Zonder eerst  RND(-TIME) te  doen krijg  je 
          geen  goede random. Het begint dan telkens hetzelfde. Verder 
          was vergeten de DAC op integer te zetten.
          
          De  Math-pack routines  zitten helaas niet alleen in de MSX1 
          BIOS. Ze zitten deels in de BASIC ROM, en dus dien je die in 
          te  schakelen  bij gebruik.  Je moet  dus even  opletten met 
          terugschakelen indien  je page  1 nog  nodig hebt. En zorgen 
          dat je geen data op page 1 gebruikt.
          
          
                        W B A S S 2   V S .   G E N 8 0 
          
          Ikzelf  gebruik GEN80,  maar ik heb ook maar even een WBASS2 
          source op  disk gezet. Misschien dat ik GEN80 maar eens moet 
          bespreken  vanuit mijn  oogpunt, want ik herinner me nog dat 
          op Sunrise Special #1 stond dat WBASS2 "beter" is.
          
          
                            B I O S   A A N R O E P 
          
          De math-pack  aanroepadressen zitten in de BIOS. Daarom even 
          een macrootje.
          
          BIOS:           MACRO  @Address
                          ld     iy,(#fcc0)
                          ld     ix,@Address
                          call   #001c
                          ENDM
          
          
          Start:          ld     h,#40            ;Page 1
                          ld     a,(#fcc1)        ;BIOS slot
                          call   #0024            ;ENASLT
          
          Om  te  beginnen  wordt  de BASIC  ROM op  page 1  gezet. De 
          aanroepadressen zitten dan wel in de BIOS, maar je moet toch 
          zorgen dat de BASIC ROM op page 1 staat.
          
          
                          ld     de,0             ;Benedengrens
                          ld     hl,#1000         ;Bovengrens
                          call   Random
                          call   PrintHL
          
          Dit behoort duidelijk te zijn.
          
          
                          ld     h,#40
                          ld     a,(#f342)        ;RAM voor page 1
                          call   #0024
                          rst    0
          
          Even page 1 terugzetten, ik weet niet of het ook goed zonder 
          dit, maar dit is altijd een stuk netter.
          
          
          ;PrintHL
          ;Doel     : zet hl op scherm
          ;In       : hl
          ;Verandert: alles
          
          PrintHL:        ld     a,h              ;eerst high byte
                          push   hl               ;hl bewaren
                          call   PrintA           ;print a
                          pop    hl
                          ld     a,l              ;dan low byte
          PrintA:         push   af               ;af bewaren
                          and    %11110000        ;high nibble
                          rrca                    ;verplaatsen naar
                          rrca                    ;low nibble
                          rrca
                          rrca
                          call   PrintNibble      ;nibble printen
                          pop    af
                          and    %1111            ;low nibble
          PrintNibble:    cp     10               ;ook A t/m F
                          jr     c,NoHex          ;goed printen
                          add    a,"A"-"0"-10
          NoHex:          add    a,"0"
                          BIOS   #a2              ;print karakter
                          ret
          
          Dit  is een handige manier om hl op het scherm te zetten. De 
          routine bevat  ook meteen routines om a of de low nibble van 
          a op het scherm te zetten.
          
          
          ;Random
          ;Doel     : genereert een random-getal tussen bepaalde
          ;           grenzen
          ;In       : hl=bovengrens, de=benedengrens
          ;Uit      : hl=random getal
          ;Verandert: alles
          ;Noot     : op page 1 moet de BASIC ROM staan
          
          ;Math-pack entry's
          FRCINT:         EQU    #2f8a            ;DAC -> integer
                                                  ;[op (DAC+2)]
          FRCDBL:         EQU    #303a            ;DAC -> dubbele
                                                  ;precisie
          RND:            EQU    #2bdf            ;DAC = RND (DAC)
          DECMUL:         EQU    #27e6            ;DAC = DAC * ARG
          
          ;Data voor Math-pack
          DAC:            EQU    #f7f6
          ARG:            EQU    #f847
          VALTYP:         EQU    #f663
          
          JIFFY:          EQU    #fc9e            ;TIME in BASIC
          
          De gebruikte adressen.
          
          
          Random:         ld     a,2              ;DAC is integer
                          ld     (VALTYP),a
                          ld     (Offset),de      ;Bewaar ondergrens
                          or     a                ;Carry laag
                          sbc    hl,de            ;Bereken vermenig-
                                                  ;vuldigingsfactor
                          ld     (Multiply),hl
          
                          ld     a,(FlagRnd)      ;Routine al
                                                  ;aangeroepen?
                          or     a
                          jr     z,RndFirst
          
          Als  Random  de  eerste  keer  wordt aangeroepen  wordt niet 
          RND(1)  maar  RND(-TIME)  gedaan.  Hierdoor  krijg je  echte 
          random, voor zover dat mogelijk is.
          
          
                          ld     hl,1             ;Neem RND(1)
          
          RndContinue:    ld     (DAC+2),hl
                          BIOS   RND
                          ld     hl,DAC           ;Verplaats DAC naar
                                                  ;ARG
                          ld     de,ARG
                          ld     bc,8
                          ldir
                          ld     hl,(Multiply)    ;Vermenigvul-
                                                  ;digingsfactor =>
                                                  ;dubbele precisie
                          ld     (DAC+2),hl
                          ld     a,2              ;Is nu nog een
                                                  ;integer
                          ld     (VALTYP),a
                          BIOS   FRCDBL
                          BIOS   DECMUL
                          BIOS   FRCINT
                          ld     hl,(DAC+2)       ;Offset erbij
                                                  ;optellen
                          ld     de,(Offset)
                          add    hl,de
                          ret
          
          RndFirst:       ld     a,255            ;Zet flag
                          ld     (FlagRnd),a
                          ld     hl,(JIFFY)       ;hl=TIME
                          ex     de,hl            ;hl=-hl
                          ld     hl,0             ;-) Dit kan vast op
                          or     a                ; een betere manier,
                          sbc    hl,de            ; maar ik heb nu
                                                  ; geen zin (tis
                                                  ; 2:28u)
                          jr     RndContinue
          
          
          Multiply:       dw     0
          Offset:         dw     0
          FlagRnd:        db     0                ;Wordt 255 na eerste
                                                  ;aanroep
          
          Einde:          END
          
                                                        Kasper Souren
