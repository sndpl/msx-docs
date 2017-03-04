
                            C H A N G E   C P U   2 
                                                     
          
          Naar aanleiding van de bespreking van de TSR Development Kit 
          volgt hier uitleg bij een source van een TSR.
          
          
                              N A A M G E V I N G 
          
          TsrName:        MACRO
                            DB   "Ntm CHGCPU 2"
                          ENDM
          
          Deze  TSR heb  ik CHGCPU  2 genoemd omdat CHGCPU al bestond. 
          Met dat  programma -  van Ramon  van der Winkel - kun je van 
          processormode  wisselen met  de HAI en IIE toetsen die naast 
          de  spatiebalk  zitten. Mijn  programma gebruikt  echter het 
          schakelaartje  naast   reset,  dat  verder  alleen  bij  het 
          opstarten wordt gebruikt.
          
          Het is gebruikelijk om de initialen van je naam op de eerste 
          drie  of vier  plaatsen te zetten. Ntm is een afkorting voor 
          Nuthemagic (ja, nieuwe naam bedacht).
          
          De  tekst  moet  overigens PRECIES  12 karakters  lang zijn, 
          anders  kom je voor onherkenbare problemen te staan. Ik kwam 
          er zelf pas achter toen ik de handleiding van het pakket had 
          doorgelezen.  Iets  dat  ik  meestal  pas  doe  als  ik  een 
          programma doorheb.
          
          
                          M E M M A N - F U N K T I E 
          
          MemMan:         MACRO  @FNC
                            LD   E,@FNC
                            CALL #4002
                          ENDM
          
          Bij  MemMan 2.42  is er de mogelijkheid bijgekomen om in een 
          TSR op  naar adres #4002 te springen, i.p.v. het adres op te 
          vragen via een tijdrovende Extended BIOS CALL.
          
          
                          DB     "MST TSR",13,10  ;TSR identifier
                          TsrName                 ;ID-Name
                          DB     26               ;End Of File
                          DW     0002             ;Header file versie,
                                                  ; MemMan 2.2 in dit
                                                  ; geval
                          DW     Base             ;Code base address
                          DW     Init             ;Init address
                          DW     Kill             ;Kill address
                          DW     Talk             ;TsrCall entry
                          DW     TsrLen           ;Programma code
                                                  ; lengte
                          DW     IniLen           ;Init code lengte
          
          Hierboven  staan de  verschillende adressen die voor TsrLoad 
          nodig zijn om de TSR goed in te kunnen laden.
          
          
                           T S R - C O D E   Z E L F 
          
          Hier volgt het programma zelf. Base is de eerste byte van de 
          code  die   in  het   geheugen  moet   blijven  staan.   Het 
          Init-gedeelte  wordt  namelijk  verwijderd na  gebruik. (Erg 
          milieuvriendelijk  is het niet, hergebruik zou best mogelijk 
          zijn.)
          
          Base:                                   ;Eerste byte code
          Kill:                                   ;Geen kill-routine
          Talk:           RET                     ;Geen driver-routine
          
          Program:        EX     AF,AF'           ;Hier begint het
                                                  ; echte werk
          
          EX  AF,AF' is  noodzakelijk omdat  register A na de CALL van 
          H.TIMI (#FD9F) nog wordt gebruikt door de BIOS. EX AF,AF' is 
          hier makkelijker dan PUSH en POP omdat je in register A' een 
          code voor  de TSR  afhandelingsroutine van  MemMan mee  kunt 
          geven:  EndHook. Dan  worden de  TSR's die verder nog aan de 
          hook hangen niet meer aangeroepen.
          
          
                          LD     A,5
                          OUT    (#E4),A          ;Zet op ISS-read
                          IN     A,(#E5)          ;Read ISS-stand
                          AND    #40              ;Alleen bit 5
          
          Zie   MSX-Mozaãk   voor   uitgebreide  informatie   over  de 
          OUT-poorten  van de  turbo R. In ieder geval staat er nu #40 
          in  A  als  de  schakelaar naar  links staat  en #00  als de 
          schakelaar rechts staat.
          
          
                          LD     HL,SaveByte      ;Vergelijk met oude
                          CP     (HL)             ; stand
                          JR     Z,EndProgram     ;Gelijk? Niks doen
                          LD     (HL),A           ;Bewaar nieuwe stand
          
          Als de oude stand gelijk is aan de nieuwe, wordt er naar het 
          einde van  de hookverwerking  gesprongen. Werken  met HL  is 
          hier  handiger dan de waarde in een ander register zetten en 
          dan daarmee vergelijken.
          
          
                          XOR    #40              ;Precies andersom
                          RLA                     ;Schuif bit naar
                          RLA                     ; goede plaats
                          RLA                     ;Eentje minder voor
                          RLA                     ; R800-ROM
                          OR     #80              ;LEDje wisselen
          
          Hier wordt  de accumulator even goedgezet voor CHGCPU van de 
          BIOS.  Als  je  wilt  dat  er  naar  R800-ROM  en  niet naar 
          R800-DRAM  moet worden geschakeld, moet je ÇÇn RLA weglaten. 
          Indien je  prefereert dat  de schakelaar  rechts de Z80-mode 
          aanduidt,  moet je  XOR #40  weglaten. Als  je dit zelf niet 
          kunt  omdat  je de  TSR Development  Kit niet  hebt, kun  je 
          natuurlijk altijd  naar de postbus schrijven, dan zal ik het 
          wel doen en je de juiste TSR opsturen.
          
          
                          LD     IX,#0180         ;CHGCPU
                          LD     IY,(#FCC0)       ;BIOS-slot in IYH
                          CALL   #001C            ;CALSLT
          
          Interslot  call  naar CHGCPU  routine van  BIOS. Zou  zonder 
          problemen  een  gewone CALL  kunnen zijn,  maar ik  vind dit 
          gewoon veiliger.
          
          
          EndProgram:     XOR    A
                          EX     AF,AF'
                          RET
          SaveByte:       DB     0
          
          TsrLen:         EQU    $-Base
          
          XOR A  is nodig  om de flags voor de TSR afhandelingsroutine 
          van  MemMan goed  te zetten.  EX AF,AF'  zet de  flags op de 
          juiste plaats  en herstelt  ook weer  de accumulator voor de 
          BIOS.
          
          SaveByte is  vervolgens een  byte om  de huidige stand in te 
          bewaren. TsrLen geeft de lengte aan van de echte TSR-code.
          
          
                           I N I T I A L I S A T I E 
          
          Init:           LD     HL,tTsrName      ;Pointer naar naam
                          MemMan 62               ;Get TSR ID:
                          JR     NC,InitError     ;Ja, print tekstje
          
          Als de naam al bestaat is de carry laag en wordt er dus naar 
          de error-routine gesprongen.
          
          
                          LD     A,(#FCC1)        ;BIOS-slot in A
                          LD     HL,#2D           ;MSXTYP
                          CALL   #0C              ;RDSLT
                          CP     3                ;Kleiner dan 3?
                          JR     C,InitError      ;Ja, print tekstje
          
          Hier  wordt even  getest of  de computer wel een turbo R (of 
          hoger?)  is.  Zo  nee,  dan wordt  er naar  de error-routine 
          gesprongen.
          
          
                          LD     A,%10            ;Beâindig met tekst
                          LD     DE,tInit         ;Pointer naar tekst
                          RET                     ;Terug naar TsrLoad
          
          Geef flags mee in A en pointer naar tekst als bit 1 gezet is 
          in register DE.
          
          
          InitError:      LD     A,%11            ;Fout, met tekst
                          LD     DE,NoTRTekst
                          RET    C                ;Turbo R fout?
                          LD     DE,tDouble
                          RET
          
          Als  de computer  naar InitError  springt omdat de TSR al in 
          het  geheugen  zit  is  de  carry  laag. Als  de betreffende 
          computer geen  turbo R is, is de carry hoog. Hiervan maak ik 
          gebruik om een zo kort mogelijk stukje code te krijgen.
          
          
          tTsrName:       TsrName
          
          tInit:          DB     12,"Wissel tussen R800-DRAM-mode en "
                          DB     "Z80-mode d.m.v. de schakelaar naast"
                          DB     " de resetknop",13,10,10
                          DB     "Freeware, Nuthemagic 11/10/92"
                          DB     13,10,10,0
          
          NoTRTekst:      DB     "Helaas, pindakaas, dit werkt alleen"
                          DB     " op een turbo R.",13,10,10,0
          
          tDouble:        DB     "Twee keer dezelfde "
                          DB     "TSR installeren heeft geen zin!"
                          DB     13,10,10,0
          
          IniLen          EQU    $-Init           ;Lengte init-code
          
          De diverse  teksten. IniLen  is om  de initialisatiecode  te 
          verwijderen.
          
          
                              H O O K - T A B E L 
          
          Hooks:          DW     HokTabLen
                          DW     #FD9F            ;Hook
                          DW     Program          ;Code horende bij
                                                  ; hook
          HokTabLen:      EQU    $-Hooks          ;Lengte van
                                                  ; hook-tabel
          
          Met  de  hook-tabel  kun  je  aangeven welke  hooks je  wilt 
          afbuigen. Dit gaat dus echt heel erg simpel.
          
          De  lengte  van  de  hook-tabel wordt  bepaald door  van het 
          huidige adres  ($) het  beginadres van  de hook-tabel  af te 
          trekken.
          
          De  source  is  alleen  nuttig  ter  leringh ende  vermaeck. 
          Slechts als  je de  TSR Development  Kit koopt kun je de TSR 
          daadwerkelijk  veranderen. Zie de betreffende tekst daarover 
          voor meer informatie.
          
          Uiteraard staat de TSR wel op deze disk. Veel plezier ermee!
          
                                                        Kasper Souren
          