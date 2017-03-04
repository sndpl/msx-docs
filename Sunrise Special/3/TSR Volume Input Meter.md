                          - MEMMAN -
          
                      V O L U M E   I N P U T   M E T E R 
                                                           
          
          Naar aanleiding van de bespreking van de TSR Development Kit 
          volgt  hier uitleg  bij een source van een TSR. Het is beter 
          om eerst  de tekst  over CHGCPU2.TSR  te lezen, want in deze 
          tekst ga ik er vanuit dat die tekst al gelezen is.
          
          
                              N A A M G E V I N G 
          
          TsrName:        MACRO
                            DB   "Ntm VolInMet"
                          ENDM
          
          VolInMet is  de afkorting  voor Volume Input Meter. Deze TSR 
          gebruikt  de  PCM  input  om  de  5  LED'jes  als  meter  te 
          misbruiken. Ntm is uiteraard van Nuthemagic.
          
          
                        B E G I N   V A N   S O U R C E 
          
          MemMan:         MACRO  @FNC             ;Macro voor MemMan
                            LD   E,@FNC           ; funkties
                            CALL MemManEntry
                          ENDM
          
                          DB     "MST TSR",13,10  ;TSR identifier
                          TsrName                 ;ID-Name
                          DB     26               ;^Z
                          DW     0002             ;Header file versie
                                                  ; MemMan 2.2 in dit
                                                  ; geval
                          DW     Base             ;Code base adres
                          DW     Init             ;Init adres
                          DW     Kill             ;Kill adres
                          DW     Talk             ;TsrCall entry
                          DW     TsrLen           ;Programma code
                                                  ; lengte
                          DW     IniLen           ;Init code lengte
          
          
          Base:           EQU    $                ;Eerste byte van
                                                  ; programma
          
          Dit hele stuk is al uitgelegd in de andere tekst.
          
          
                               K I L L - C O D E 
          
          Kill:           LD     A,(CAPS)         ;Lees voormalige
                                                  ; stand van CAPS in
                          OUT    (#AA),A          ;en zet het weer
                                                  ; goed
          
                          LD     A,15             ;Lees KANA stand in
                          OUT    (#A0),A          ; en herstel
                          LD     A,(KANA)
                          OUT    (#A1),A
          
                          LD     A,(R800nPAUSE)   ;Herstel R800- en
                          LD     (#FCB1),A        ; PAUSE LED'jes
                          OUT    (#A7),A
          
                          CALL   Drive_off        ;Drive-LED'je uit
                          LD     A,(Sound)
                          OUT    (#A5),A
          Talk:           RET                     ;Geen driver routine
                                                  ; (Waarom twee keer
                                                  ; RET als ÇÇn keer
                                                  ; genoeg is?)
          
          
          Bij de  initialisatie-routine wordt  de huidige stand van de 
          LED'jes  ingelezen, en de LED'jes worden weer goed gezet bij 
          het verwijderen van de TSR.
          
          Een driver routine is niet nodig, dus daar staat gewoon RET.
          
          
                         H O O K - V E R W E R K I N G 
          
          Program:        PUSH   AF               ;Bewaar AF
                          CALL   Sample           ;Neem monster,
                                                  ; output in A
                          CALL   ChangeLEDs       ;Verander LED's
                                                  ; aan de hand van de
                                                  ; waarde in A
                          XOR    A                ;MemMan TSR handler:
                          EX     AF,AF'           ; doorgaan met hook-
                          POP    AF               ; verwerking
                          RET
          
          Lekker  duidelijk  programmeren! Er  zullen wel  geen vragen 
          ontstaan over bovenstaand stukje.
          
          
                             S U B R O U T I N E S 
          
          ChangeLEDs:     SUB    50               ;A kleiner dan 50?
                          JR     C,Caps_off       ;CAPS e.v. uit
          
          50 aftrekken van A, als A kleiner is dan 50, ontstaat er een 
          carry en wordt alle LED'jes (te beginnen bij CAPS) uitgezet. 
          Ik gebruik in dit geval CP omdat de routines dan duidelijker 
          zijn, er staat nu overal SUB 50, i.p.v. CP 50, 100, 150 enz.
          
          
          Caps_on:        EX     AF,AF'           ;A veiligstellen
                          IN     A,(#AA)          ;Alleen CAPS-bitje
                          AND    191              ; aanpassen
                          OUT    (#AA),A
                          EX     AF,AF'
                          SUB    50               ;A kleiner dan 100?
                          JR     C,Kana_off       ;KANA e.v. uitzetten
          
          Kana_on:        EX     AF,AF'           ;A bewaren
                          LD     A,15             ;KANA LED'je
                          OUT    (#A0),A          ; aanzetten
                          IN     A,(#A1)
                          AND    127
                          OUT    (#A1),A
                          EX     AF,AF'
                          SUB    50               ;A kleiner dan 150?
                          JR     C,Pause_off      ;PAUSE e.v. uit
          
          Pause_on:       EX     AF,AF'           ;A bewaren
                          LD     HL,#FCB1         ;In HL is
                                                  ; makkelijker
                          LD     A,(HL)           ;In (#FCB1) staat
                                                  ; stand van PAUSE
                                                  ; en R800 LED
                          OR     1                ;PAUSE aan
                          LD     (HL),A           ;Terugzetten
                          OUT    (#A7),A
                          EX     AF,AF'
                          SUB    50               ;A kleiner dan 200?
                          JR     C,R800_off
          
          R800_on:        EX     AF,AF'
                          LD     A,(HL)           ;HL is nog #FCB1
                          OR     128              ;Idem, wel met R800
                          LD     (HL),A           ; LED
                          OUT    (#A7),A
                          EX     AF,AF'
                          SUB    50               ;A kleiner dan 250?
                          JR     C,Drive_off
          
          Drive_on:       LD     E,#14            ;Drive LED'je aan-
                          LD     A,#8B            ; zetten d.m.v.
                          LD     HL,#7FF2         ; klooien in drive-
                          JP     #0014            ; controller
                                                  ;Met dank aan Mr. X
                                                  ; (kweenie meer wie)
          
          Waarde  #14 in slot 3.2 (#8B) op adres #7FF2 zetten. JP komt 
          i.p.v. CALL en RET.
          
          
          Caps_off:       IN     A,(#AA)          ;CAPS uit
                          OR     64
                          OUT    (#AA),A
          
          Kana_off:       LD     A,15             ;KANA uit
                          OUT    (#A0),A
                          IN     A,(#A1)
                          OR     128
                          OUT    (#A1),A
          
          Pause_off:      LD     HL,#FCB1         ;PAUSE uit
                          LD     A,(HL)
                          AND    254
                          LD     (HL),A
                          OUT    (#A7),A
          
          R800_off:       LD     A,(HL)           ;R800 uit, HL
                          AND    127              ; staat nog goed!
                          LD     (HL),A
                          OUT    (#A7),A
          
          Drive_off:      LD     E,#04            ;Drive uit
                          LD     A,#8B
                          LD     HL,#7FF2
                          JP     #0014
          
          Sample:         LD     B,250
                          LD     D,B
                          LD     A,12             ;Opnemen
                          OUT    (#A5),A
                          LD     A,128+120        ;Grens
                          LD     (#A4),A
          
          Startwaarde  wordt op  250 gezet.  Telkens als  er een "aan" 
          binnenkomt, wordt dat getal met ÇÇn verminderd.
          
          
          Sample_loop:    IN     A,(#A5)
                          AND    #80              ;Bit 7 gezet?
                          JR     NZ,Count         ;Nee, dan aftrekken
                          DJNZ   Sample_loop
                          LD     A,D
                          RET
          
          Count:          DEC    D
                          DJNZ   Sample_loop
                          LD     A,D
                          RET
          
          CAPS:           DB     0                ;Bytes om voormalige
          KANA:           DB     0                ; stand LED'jes in
          R800nPAUSE:     DB     0                ; te bewaren
          Sound:          DB     0
          
          
          MemManEntry:    JP     0                ;Ingevuld door init.
          
          TsrLen          EQU    $-Base           ;Lengte van TSR
          
          
                           I N I T I A L I S A T I E 
          
          Init:           LD     A,(#FCC1)        ;BIOS-slot
                          LD     HL,(#002D)       ;MSX-type
                          CALL   #0C              ;Readslot
                          CP     3                ;turbo R?
                          JR     C,Error          ;Nee he? Toch geen
                                                  ; simpele computer?
          
          Eerst  bepalen of het wel een turbo R is. Hopelijk hebben de 
          toekomstige versies  (al komen die er volgens mij nooit) ook 
          dezelfde LED'jes, die op dezelfde manier aangestuurd worden. 
          :-)
          
          
                          IN     A,(#A5)
                          LD     (Sound),A
                          IN     A,(#AA)          ;CAPS aan?
                          LD     (CAPS),A
                          LD     A,15
                          OUT    (#A0),A          ;KANA aan?
                          IN     A,(#A1)
                          LD     (KANA),A
                          LD     A,(#FCB1)
                          LD     (R800nPAUSE),A
          
          Stand van LED'jes bewaren.
          
          
                          LD     B,6              ;MemMan entry
                                                  ; opvragen
                          LD     DE,256*'M' + 50  ;MemMan info funktie
                          CALL   #FFCA            ;via extended BIOS
                                                  ; hook
          
                          LD     (MemManEntry+1),HL ;Bewaar MemMan
                                                  ;   entry
          
                          LD     HL,tTsrName      ;Pointer naar TSR
                                                  ; naam
                          MemMan 62               ;TSR al aanwezig?
                          JR     NC,Error
          
                          LD     A,%10            ;Tekstje printen
                          LD     DE,tIntro        ;Tekstpointer in DE
                          RET                     ;Terug naar TsrLoad
          
          Error:          LD     DE,tDouble
                          LD     A,%11            ;Init mislukt, toch
                                                  ; tekst afbeelden
                          RET    NC
          
                          LD     DE,tIntro
                          LD     A,7              ;Ook introtekst
                          LD     (tNoTurboR-1),A  ; printen
                          RET
          
          Zelfde foefje als bij CHGCPU2.
          
          
          tTsrName:       TsrName                 ;Macro, zie boven
          
          tIntro:         DB 12
                          DB "Volume Input Meter",13,10
                          DB "------------------",13,10,10
                          DB "Misbruikt de 5 LED'jes van de MSX "
                          DB "turbo R als meter van het geluid!"
                          DB 13,10,10
                          DB "Freeware door Nuthemagic op een"
                          DB " kille donderdagmiddag, 10 december "
                          DB "1992, luisterend naar een opname van "
                          DB "Rave Radio.",13,10,10
                          DB "Mochten er vragen of opmerkingen zijn:"
                          DB 13,10
                          DB "Sunrise BBS Nuth, 045-245910, 24 uur "
                          DB "per dag.",13,10,10,10
                          DB 0
          tNoTurboR:      DB "Minstens ÇÇn turbo R benodigd om deze"
                          DB " TSR te draaien.",13,10
                          DB 13,10,10,10,0
          
          tDouble:        DB "Jammer, maar 2 keer laden lukt echt "
                          DB "niet, hoor.",13,10,10,0
          
          IniLen:         EQU    $-Init           ;Length of init-code
          
          Hooks:          DW     HokTabLen        ;Lengte van
                                                  ; hook-tabel
                          DW     #FD9F            ;Hook
                          DW     Program          ;Programma aan hook
          
          HokTabLen:      EQU    $-Hooks          ;Lengte van tabel
          
                                                        Kasper Souren
         