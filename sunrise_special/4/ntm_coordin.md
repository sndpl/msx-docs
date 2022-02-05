                         N T M   C O O R D I N 
                                                    
          
                                 G E B R U I K 
          
          Deze  TSR was een ideetje van CTS, en ik heb hem gemaakt. Je 
          kunt zo  heel simpel in BASIC bepalen op welke coordinaat de 
          cursor staat. Deze coordinaten worden nl. op F1 en F2 gezet.
          
          Als  je  ESC  inhoudt,  blijven  de waarden  onder F1  en F2 
          ongewijzigd.  Je  kunt  zo makkelijk  de coordinaten  in een 
          BASIC-programma gebruiken.
          
          Onder  TED levert  deze TSR problemen op, omdat nu gewoon de 
          x- of  y-coordinaat op  het scherm  komt als  je op F1 of F2 
          duwt.
          
          Bij het verwijderen worden de oude functietoets-instellingen 
          weer hersteld.
          
          
                       B E S P R E K I N G   S O U R C E 
          
          Ik  zal  alleen  de gedeelten  bespreken die  specifiek voor 
          Coordin zijn.
          
          Hook            EQU    #FDA9       ;H.DSPC: aan begin van
                                             ;routine die cursor
                                             ;zichtbaar maakt
          
          Bij  het zoeken  naar een geschikte hoek vond ik H.DSPC. Als 
          je de  routine aan de interrupt hangt, wordt de routine veel 
          te vaak uitgevoerd. Dat heeft dus geen nut(h). Bij deze hook 
          wordt  de  routine  alleen  uitgevoerd  als  hij  nodig  is. 
          Namelijk vlak voordat de cursor op het scherm wordt gezet.
          
          
          X_coor:         EQU    #F3DD
          Y_coor:         EQU    #F3DC
          FNKST:          EQU    #F87F
          DSPFNK:         EQU    #00CF
          
          Dit zijn de benodigde adressen. X_coor en Y_coor zijn gewoon 
          de coordinaten  van de  cursor. FNKST  is het  adres waar de 
          data  van  de  functietoetsen staat.  Met DSPFNK  kun je  de 
          functietoetsen aan zetten.
          
          
          Kill:           LD     HL,Buffer
                          LD     DE,FNKST
                          LD     BC,32
                          LDIR
          
                          LD     A,(#F3DE)        ;F-keys aan?
                          OR     A
                          JP     NZ,DispKeys
                          RET
          
          Kopieer eerst  de data  naar een buffer om de functietoetsen 
          te   herstellen   na   verwijderen  van   de  TSR.   Als  de 
          functietoetsen  aan staan, wordt de herstelde tekst ook weer 
          op het scherm gezet. Dit met de routine DispKeys.
          
          
          Program:        PUSH   AF
                          PUSH   BC
                          PUSH   DE
                          PUSH   HL
          
          Ik  weet niet  welke registers  wel of  niet veranderd mogen 
          worden, maar dit is het veiligst.
          
          
                          DI
                          IN     A,(#AA)
                          AND    240
                          OR     7
                          OUT    (#AA),A
                          IN     A,(#A9)
                          EI
                          AND    %100
                          JR     Z,Quit
          
          Leest  rij 7  van het  toetsenbord. AND  240 is  om de  high 
          nibble niet aan te passen. OR 7 zorgt dat rij 7 wordt gezet. 
          ESC is  de derde  toets in  de rij, dus AND %100. Als ESC is 
          ingedrukt  wordt  de  routine  be�indigd.  De functietoetsen 
          mogen dan niet worden aangepast.
          
          
                          LD     A,(X_coor)
                          LD     HL,FNKST
                          CALL   HexDec
          
          A is  waarde van  toets en HexDec zet die als ASCII op adres 
          FNKST.
          
                          LD     A,(Y_coor)
                          LD     HL,FNKST+16
                          CALL   HexDec
          
          Idem.
          
                          LD     A,(#F3DE)        ;F-keys aan?
                          OR     A
                          CALL   NZ,DSPFNK
          
          Als  functietoetsen  aan staan,  wordt de  tekst ook  op het 
          scherm gezet.
          
          
          Quit:           POP    HL
                          POP    DE
                          POP    BC
                          POP    AF
                          RET
          
          Registers  weer  herstellen  en  terugspringen  naar  de TSR 
          handler.
          
          
          HexDec:         DEC    A        ;E�n verminderen, 
                                          ;coordinaten van LOCATE
                                          ;werken anders dan de
                                          ;coordinaten op #F3DC en 
                                          ;#F3DD
                          LD     E,0      ;Zet E op nul
          
          HD2:            SUB    10       ;10-tal in E
                          INC    E        
                          JR     NC,HD2
          
          Trek telkens  10 af van A, en tel telkens 1 op bij E, totdat 
          A kleiner dan 0 is.
          
                          ADD    A,10
                          DEC    E
          
          Dan wordt  E 1  verminderd en  10 opgeteld bij A. Dit stukje 
          (vanaf  HD2) is  eigenlijk E=A  MOD 10,  A=A DIV  10 (of  in 
          BASIC: A=A \ 10).
          
                          LD     B,A      ;1-heid in B
          
          P10:            LD     A,E
                          OR     A
                          JR     Z,P1
                          ADD    A,"0"
                          LD     (HL),A
                          INC    HL
          
          Maak even een ASCII waarde van E en B. Als E nul is, wordt B 
          opgeschoven. Daarom  wordt INC HL alleen gedaan als E niet 0 
          is.  (Oftewel, bij  de waarde 10 in A krijg je "10" en bij 9 
          krijg je niet niet "09" maar "9".)
          
          P1:             LD     A,B
                          ADD    A,"0"
                          LD     (HL),A
                          INC    HL
                          LD     (HL),0
                          RET
          
          Zet de waarden nog even op de juiste plaats.
          
          
          Buffer:         DS     32       ;Voor de oude waarden van de
                                          ;functietoetsen
          
          
          Nog even een stukje van de Init routine:
          
                          LD     HL,FNKST
                          LD     DE,Buffer
                          LD     B,32
          Loop:           LD     A,(HL)
                          LD     (DE),A
                          LD     (HL),0
                          INC    HL
                          INC    DE
                          DJNZ   Loop
          
          Dit  kopieert 32  bytes van de functietoetsenbuffer naar een 
          eigen buffertje;  om de functietoetsen te herstellen bij het 
          verwijderen.  Ik  gebruik  hier niet  LDIR omdat  ik ook  de 
          functietoetsen wil wissen: LD (HL),0.
          
                          LD     A,(#F3DE)        ;F-keys aan?
                          OR     A
                          JR     NZ,DispKeys
          
                          XOR    A
                          RET
          
          DispKeys:       LD     IY,(#FCC0)
                          LD     IX,DSPFNK
                          CALL   #001C
          
          Het  is wel waarschijnlijk dat de BIOS aan staat, maar ik ga 
          er niet  vanuit. Dit  is een  stuk zekerder  dan gewoon CALL 
          DSPFNK.
          
                                                        Kasper Souren
    