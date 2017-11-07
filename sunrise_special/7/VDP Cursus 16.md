          
                 M S X 2 / 2 +   V D P   C U R S U S   ( 1 6  )
                                                                
          
          Ik zal  de spritefans ook deze keer weer teleurstellen, want 
          ik  kreeg naar  aanleiding van  mijn oproep om met vragen te 
          komen een  vraag van een lezer hoe je het 'Xak effect' in ML 
          maakt.  De spritefans  hoeven zich niet gelijk voor de trein 
          te  gooien,  want  op  Sunrise  Special  #5 geeft  R�man v/d 
          Meulen,  een  van  de  programmeurs  van  Fuzzy Logic,  zijn 
          geheimen op spritegebied prijs.
          
          
                              X A K   E F F E C T 
          
          Er zijn  misschien lezers die nu met opgetrokken wenkbrauwen 
          voor  de monitor  zitten. Xak effect? Wat is dat? In de demo 
          van Xak  The Tower  of Gazzel  zit een heel mooi effect, het 
          lijkt   erop  alsof  het  woord  Xak  op  het  scherm  wordt 
          'gegoten'. Als u het nog nooit gezien heeft, bekijk dan maar 
          het voorbeeld  van de  routine die ik deze keer ga bespreken 
          door XAK.BAS te RUNnen. Alle files behorende bij dit artikel 
          zitten in XAKEFFCT.PMA.
          
          Hoe werkt het nu precies? Stel het plaatje moet uiteindelijk 
          vanaf  lijn 100 op het scherm komen te staan. Dan kopieer je 
          eerst de  eerste lijn van het plaatje naar lijn 211 t/m 100, 
          daarna de tweede lijn naar lijn 211 t/m 101, etc.
          
          Voor het  voorbeeld gebruiken we het logo van Overflow!, zie 
          de rubriek Backgrounds voor meer informatie over deze groep. 
          Dit logo is 102 lijnen hoog. Om te zorgen dat het restje aan 
          het  eind wordt  uitgewist, nemen  we een  extra zwarte lijn 
          onderaan het  plaatje mee,  waardoor het totaal op 103 komt. 
          We  zetten het  plaatje bovenaan op page 1, en we willen dat 
          het midden op page 0 terecht komt.
          
          Als je het Xak effect nu zonder verder nadenken in BASIC zou 
          programmeren voor dit logo, krijg je ongeveer zoiets:
          
          FOR I=0 TO 102
              FOR Y=211 TO 55+I STEP -1
                  COPY (0,I)-(255,I),1 TO (0,Y)
              NEXT
          NEXT
          
          De  I-lus  gaat  het  plaatje lijn  voor lijn  af, de  Y lus 
          kopieert die lijn van onderaf tot aan de juiste positie. Als 
          je FOR  Y=55+I TO  211 zou doen, dan zou je het er heel stom 
          uitzien,  omdat het  kopi�ren dan tegen de 'gietrichting' in 
          gaat.
          
          Dit gaat in BASIC niet vloeiend, en ook in ML niet echt. Het 
          kan veel  handiger door  de VDP  het vuile  werk op te laten 
          knappen. Het kan met slechts twee copy's per lijn!!!
          
          
                             S L I M   W I S S E N 
          
          Het  lijkt of  ik nu naar een heel onderwerp ga, maar dat is 
          toch niet zo. Als je in ML een gebied vanaf Begin met lengte 
          Lengte met de waarde Waarde wilt vullen, dan doe je dat zo:
          
                            LD    HL,Begin
                            LD    DE,Begin+1
                            LD    BC,Lengte-1
                            LD    (HL),Waarde
                            LDIR
          
          Dit  gaat zo.  Je zet zelf de waarde al op het begin. De Z80 
          (of  R800)  leest (Begin)  en zet  die waarde  op (Begin+1). 
          Daarna leest  hij (Begin+1)  en zet die waarde op (Begin+2), 
          etc.  Zo  wordt  dus het  hele gebied  met de  juiste waarde 
          gevuld!
          
          
                             E N   N U   I N   M L 
          
          Dit truukje gebruiken we ook bij het Xak effect. We kopi�ren 
          de  lijn maar ��n keer, en wel naar (0,211). Daarna kopi�ren 
          we een  blok van  de juiste  hoogte van (0,211) naar (0,210) 
          [een  blok dat naar boven is gericht]. De VDP kopieert eerst 
          lijn 211  op lijn  210, daarna lijn 210 op 209, etc. Precies 
          wat we willen hebben!
          
          In ML ziet dit er als volgt uit:
          
          
          ; Xak effect
          ; XAK.GEN
          ; Door Stefan Boer
          ; VDP Cursus Sunrise Magazine #11
          ; (c) Stichting Sunrise 1994
          ; v1.0 20/01/94
          
          Bron_X:           EQU   0
          Bron_Y:           EQU   0
          Bron_Page:        EQU   1
          Doel_X:           EQU   0
          Doel_Y:           EQU   55
          Doel_Page:        EQU   0
          Breedte:          EQU   256
          Hoogte:           EQU   103
          
          
          Om ervoor  te zorgen  dat de routine universeel bruikbaar is 
          gebruik ik voor alle gegevens betreffende het plaatje EQU's. 
          X-co�rdinaten  en Breedte  worden als words behandeld, zodat 
          de routine  ook voor SCREEN 7 en voor plaatjes van 256 breed 
          (SCREEN 5 over de hele breedte) geschikt is.
          
          
                            DB    #FE
                            DW    Start
                            DW    Einde
                            DW    Start
          
          
          Dit is  de .BIN  header voor  GEN80, die  WB-ASS2 gebruikers 
          kunnen overslaan.
          
          
                            ORG   #D000
          
          Start:            LD    HL,Copy_Lijn
                            CALL  DoCopy
          
                            LD    HL,Copy_Effect
                            CALL  DoCopy
          
                            LD    A,(Copy_Effect+10) ; Hoogte
                            DEC   A
                            LD    (Copy_Effect+10),A
          
                            LD    A,(Copy_Lijn+2)   ; Bron_Y
                            INC   A
                            LD    (Copy_Lijn+2),A
          
                            CP    Bron_Y+Hoogte     ; klaar?
                            JR    NZ,Start
          
                            RET
          
          
          Ja,  dit is het hele hoofdprogramma. Gewoon twee copy's. Bij 
          Copy_Lijn   wordt   steeds  de   lijn  verhoogd   die  wordt 
          gekopieerd, en  bij Copy_Effect  wordt steeds  de hoogte van 
          het blok met ��n verkleind.
          
          
          Copy_Lijn:        DW    Bron_X
                            DB    Bron_Y,Bron_Page
                            DW    Doel_X
                            DB    211,Doel_Page
                            DW    Breedte,1
                            DB    0,0,#D0
          
          Copy_Effect:      DW    Doel_X
                            DB    211,Doel_Page
                            DW    Doel_X
                            DB    210,Doel_Page
                            DW    Breedte,210-Doel_Y
                            DB    0,8,#D0           ; DIY = 1
          
          
          Dit  zijn de  data's voor de copy's, waarbij zoals ik al had 
          gezegd alles  wat met  X te  maken heeft  met DW's is gedaan 
          zodat de routine universeel bruikbaar is. Ik gebruik hier de 
          High Speed copy (#D0), dus let erop dat de x-co�rdinaat even 
          moet  zijn in SCREEN 5. DIY = 1 bij de tweede copy, zodat er 
          van onder naar boven wordt gekopieerd.
          
          Nu  volgen  nog de  standaardroutines DoCopy,  ReadStatus en 
          VDPReady, die inmiddels bekend worden verondersteld.
          
          
          ; ----------------------------------------------------------
          ; DoCopy
          ; Voer VDP commando uit
          ; In      : HL = startadres 15 bytes
          ; Gebruikt: AF, HL, BC
          ; ----------------------------------------------------------
          
          DoCopy:           CALL  VDPReady
                            DI
                            LD    A,32
                            OUT   (#99),A
                            LD    A,17+128
                            OUT   (#99),A
                            EI
                            LD    BC,#0F9B
                            OTIR
                            RET
          
          
          ; ----------------------------------------------------------
          ; VDPReady
          ; Wacht tot VDP klaar is met commando executie
          ; Gebruikt: AF
          ; ----------------------------------------------------------
          
          VDPReady:         LD    A,2
                            CALL  ReadStatus
                            BIT   0,A
                            JR    NZ,VDPReady
                            RET
          
          
          ; ----------------------------------------------------------
          ; ReadStatus
          ; Lees VDP statusregister
          ; In      : A = nummer van te lezen statusregister
          ; Uit     : A = data
          ; Gebruikt: AF'
          ; ----------------------------------------------------------
          
          ReadStatus:       DI
                            OUT   (#99),A
                            LD    A,15+128
                            OUT   (#99),A
                            NOP
                            NOP
                            IN    A,(#99)
                            EX    AF,AF'
                            XOR   A
                            OUT   (#99),A
                            LD    A,128+15
                            OUT   (#99),A
                            EI
                            EX    AF,AF'
                            RET
          
          Einde:            END
          
          
                               T E N S L O T T E 
          
          Op de  disk staan  XAK.GEN, XAK.BAS, XAK.BIN en OVERFLOW.SC5 
          (in  de file  XAKEFFCT.PMA). U  kunt de  routine uitproberen 
          door XAK.BAS  te RUNnen, het Overflow! logo wordt dan op het 
          scherm 'gegoten'.
          
          Als  er  nog  meer lezers  zijn die  iets te  vragen op  VDP 
          gebied,  schrijf het  me dan! Anders is dit het laatste deel 
          van de VDP cursus.
          
                                                          Stefan Boer
          
          P.S. Ik  begin binnenkort  met een  V9990 cursus, daarop kan 
               dit effect  natuurlijk ook  worden gemaakt,  alleen dan 
               nog spectaculairder!
