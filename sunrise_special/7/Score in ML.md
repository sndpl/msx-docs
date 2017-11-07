                             S C O R E   I N   M L 
                                                    
          
          Bij  het  maken  van  een  spel  loop je  al snel  tegen het 
          probleem  op   dat  de   grootste  getallen  die  je  in  ML 
          fatsoenlijk  kunt gebruiken (16 bits getallen) te klein zijn 
          om fatsoenlijk  een score of iets dergelijks in op te slaan. 
          Bij  Xak II is je maximale experience bijvoorbeeld 65535, en 
          dat  staat  gewoon  niet  professioneel. Voor  leken is  het 
          vreemd  dat  er  zo'n  vreemde bovengrens  is (voor  hen zou 
          bijvoorbeeld 99999  een veel  logischere bovengrens zijn) en 
          degenen  die een  beetje ML  kennen zien  meteen dat  ze bij 
          MicroCabin gewoon  een 16  bits getal  hebben gebruikt om de 
          experience bij te houden.
          
          Het   afdrukken  van  een  16  bits  getal  is  al  redelijk 
          ingewikkeld (zie  hiervoor de  artikelen in  MCCM, ik heb er 
          ook  al  eens  aandacht aan  besteed al  was dat  een minder 
          ingenieuze  routine), maar  als je grotere getallen wilt, 32 
          bits bijvoorbeeld,  dan wordt  het helemaal  een hele  tour, 
          vooral  om het  ook nog  een beetje  snel te doen. En als je 
          bijvoorbeeld wilt dat de score snel kan oplopen, dan is zo'n 
          32 bits  afdrukroutine simpelweg  te traag.  Hoe goed je ook 
          kunt programmeren.
          
          
                                     B C D 
          
          Deze  problemen heb je niet als je gebruik maakt van het BCD 
          formaat. BCD is een afkorting voor Binary Coded Decimal, een 
          codering waarmee  je toch  met decimale getallen kunt werken 
          op  een  computer  die  eigenlijk  alleen  geschikt  is voor 
          binair, octaal en hexadecimaal. Dit is een 'native' datatype 
          van  de Z80 processor, het wordt ondersteund door een aantal 
          vlaggen en instructies. Ik kom hier later nog op terug.
          
          Het BCD  formaat werkt  heel simpel,  je zet  gewoon in elke 
          nibble  ��n cijfer  van 0-9.  De BCD  notatie van  het getal 
          321456 is  dus #32 #14 #56. De verzameling rekenroutines van 
          MSX-BASIC  (bekend onder de naam MathPack) werkt ook met het 
          BCD formaat,  waarbij het  is uitgebreid  met een  exponent. 
          Voor  scores e.d.  is dat echter niet nodig en dus werken we 
          kale BCD, zonder exponent.
          
          
                                     D A A 
          
          Zoals ik al zei is BCD een native datatype van de Z80, en is 
          het  dus relatief  eenvoudig om  routines voor het verwerken 
          van getallen  in BCD formaat te schrijven. De Z80 instructie 
          die  het meeste  ingewikkelde werk  uit handen neemt is DAA, 
          wat  staat   voor  Decimal   Adjust  Accumulator.   DAA  zal 
          'rekenfouten'  die ontstaan  bij optellen  en aftrekken  (en 
          alleen daarbij!)  corrigeren zodat het resultaat ook weer in 
          BCD  formaat  staat.  Ik  zal  dit  verduidelijken  met  een 
          voorbeeld:
          
                          LD      A,#07
                          ADD     A,#07
          
          A bevat  nu de  waarde 14,  oftewel #0D. Het juiste antwoord 
          moet  echter #14  zijn, want we werken met BCD formaat. Door 
          nu  gewoon  een  instructie  DAA te  geven zal  de Z80  deze 
          'rekenfout' zelf corrigeren!!! Dus:
          
                          LD      A,#08
                          ADD     A,#05
                          DAA
          
          A  bevat nu  de waarde #13! De Z80 heeft voor dit corrigeren 
          een  aantal  speciale vlaggen,  waarvan je  je misschien  al 
          afvroeg waarom  ze er  zijn omdat  je ze  nooit gebruikt, er 
          zijn  zelfs geen  voorwaardelijke JP/JR/CALL/RET instructies 
          voor. Bit 1 van het F register is de N-vlag, die wordt gezet 
          als er  een aftrekking  plaatsvindt. De correctie is bij een 
          optelling  namelijk anders dan bij een aftrekking. Bit 4 van 
          het F register is de H-vlag, de Half carry. Deze wordt gezet 
          als er een halfcarry optreedt van de low nibble naar de high 
          nibble van A.
          
          In bovenstaand voorbeeld is N niet gezet (er is opgeteld) en 
          H  wel, er  is immers  een halfcarry  opgetreden (5  + 8  is 
          groter  dan  10). Hierdoor  weet de  Z80 dat  hij 6  bij het 
          resultaat  moet optellen. Dat klopt, want 13 + 6 = 19 = #13. 
          Nu een voorbeeld met aftrekken:
          
                          LD      A,#42
                          SUB     #07
                          DAA
          
          Hier  is weer een half carry, maar nu is er afgetrokken. #42 
          - #07  = #3B.  Aan de vlaggen kan de Z80 zien dat hij 6 moet 
          aftrekken  om te corrigeren, en inderdaad #3B - #06 = #35 en 
          dat is  precies het gewenste antwoord. Aan de half carry kan 
          de  Z80 dus  zien of  er een  correctie nodig  is, en aan de 
          aftrekvlag kan  hij zien  of er 6 moet worden afgetrokken of 
          worden  bijgeteld. Maar  op zich  hoef je  dit niet  eens te 
          weten om met DAA te kunnen omgaan.
          
          
                          S C O R E   D A T A T Y P E 
          
          We  gaan  nu  een  speciaal  score  datatype defini�ren.  De 
          representatie, BCD  zonder exponent, hebben we al, nu hebben 
          we  nog  een  aantal  operaties  (lees:  routines)  met  dit 
          datatype  nodig. Hieronder  volgen deze  routines. Voor  het 
          gemak gaan we ervan uit dat de lengte van de getallen altijd 
          even is,  zodat er  niet met  halve bytes  gewerkt hoeft  te 
          worden.  Bij alle  routines wordt  in B  de lengte  in bytes 
          meegegeven.
          
          
          ; SCORE.GEN
          ; Routines voor werken met getallen in BCD-formaat van
          ; willekeurige lengte
          ; Enige beperking is dat lengte altijd even moet zijn
          ; Door Stefan Boer
          
          ; Waarschuwing: alle routines mogen AF, HL, BC, DE
          ; veranderen!!!
          
          ; InitGetal
          ; In : HL = ^getal, B = lengte/2
          ; Uit: getal is 0
          
          Op zich  een overbodige  routine maar  voor de  volledigheid 
          staat  hij erbij.  Met ^  bedoel ik "pointer naar", dezelfde 
          notatie die  bij Pascal  gebruikt wordt.  Deze routine maakt 
          dus  het getal met lengte B waar HL naar wijst gelijk aan 0. 
          De implementatie is zeer eenvoudig:
          
          InitGetal:        LD    (HL),0
                            INC   HL
                            DJNZ  InitGetal
                            RET
          
          
          ; AssignGetal
          ; In : HL = ^bron, DE = ^doel, B = lengte/2
          ; Uit: doel = bron
          
          Ook dit  is weer een routine die er voor de volledigheid bij 
          zit.  HL  wijst  naar  het  te  kopi�ren  getal, DE  naar de 
          bestemming.  De implementatie is weer triviaal:
          
          AssignGetal:      LD    C,B
                            LD    B,0
                            LDIR
                            RET
          
          
          ; AddGetal
          ; In : HL = ^getal1, DE = ^getal2, B = lengte/2
          ; Uit: getal1 = getal1 + getal2, carry als uitkomst te
          ;      groot en dan 999...
          
          Tel  het getal  waar DE  naar wijst op bij het getal waar HL 
          naar wijst,  en zet  de uitkomst  op de  plaats waar HL naar 
          wijst.  Dit  is  de  eerste  routine  waarbij  we  DAA  gaan 
          gebruiken.
          
          AddGetal:         PUSH  HL
                            PUSH  BC
                            CALL  MovePointers
          
          We bewaren  HL en  B even omdat we bij een te grote uitkomst 
          het   hele  getal   met  9's   moeten  vullen.   De  routine 
          MovePointers  verplaatst  HL en  DE naar  het einde  van het 
          getal. Dat  is nodig  omdat we  van rechts naar links moeten 
          werken  bij optellen,  zoals je  je misschien  nog wel  zult 
          herinneren van de lagere school.
          
                            OR    A                 ; wis carry
          AddGetal_2:       LD    A,(DE)
                            ADC   A,(HL)
                            DAA
                            LD    (HL),A
                            DEC   HL
                            DEC   DE
                            DJNZ  AddGetal_2
          
          Dit  is  de  feitelijke  optelling.  De  cijfers worden  per 
          groepje van  twee (er  zitten twee  cijfers in ��n byte) bij 
          elkaar  opgeteld, DAA  zorgt dat  ook het resultaat weer BCD 
          formaat is en we gebruiken ADC om de carry mee te nemen.
          
                            POP   BC
                            POP   HL
                            RET   NC
          AddGetal_3:       LD    (HL),#99        ; overflow
                            INC   HL
                            DJNZ  AddGetal_3
                            SCF
                            RET
          
          Als  er geen carry is zijn we klaar, is er wel een carry dan 
          past de uitkomst niet, we vullen het getal met 9's en aan de 
          carry kan het programma zien dat er een overflow was.
          
          ; Zet DE en HL op einde van getallen
          
          MovePointers:     PUSH  BC
                            DEC   B
                            JR    Z,MovePointers_2
          MovePointers_1:   INC   DE
                            INC   HL
                            DJNZ  MovePointers_1
          MovePointers_2:   POP   BC
                            RET
          
          Als  het getal 6 cijfers lang is (B=3), moeten we de pointer 
          3-1=2 plaatsen opschuiven. Vandaar de DEC B.
          
          
          ; SubGetal
          ; In : HL = ^getal1, DE = ^getal2, B = lengte/2
          ; Uit: getal1 = getal1 - getal2, carry bij negatieve
          ;      uitkomst en dan 000...
          
          Aftrekken gaat net zo als optellen:
          
          SubGetal:         PUSH  HL
                            PUSH  BC
                            EX    DE,HL
                            CALL  MovePointers
          
          Aangezien  er geen  SBC A,(DE)  bestaat is het hier handiger 
          als DE en HL zijn verwisseld, vandaar de EX DE,HL. 
          
                            OR    A                 ; wis carry
          SubGetal_2:       LD    A,(DE)
                            SBC   A,(HL)
                            DAA
                            LD    (DE),A
                            DEC   HL
                            DEC   DE
                            DJNZ  SubGetal_2
          
          Ook  hier  hetzelfde als  bij optellen,  alleen wordt  er nu 
          natuurlijk SBC gebruikt in plaats van ADC.
          
                            POP   BC
                            POP   HL
                            RET   NC
                            CALL  InitGetal         ; underflow
                            SCF
                            RET
          
          Voor het  vullen met nullen hebben we al een routine dus die 
          gebruiken  we ook.  Omdat het vaak voorkomt dat je een getal 
          alleen maar  met ��n  hoeft te  verhogen of  verlagen heb ik 
          daar speciale routines voor.
          
          ; IncGetal
          ; In : HL = ^getal, B = lengte/2
          ; Uit: getal = getal + 1, carry als getal te groot
          
          IncGetal:         CALL  MovePointers
          IncGetal_1:       LD    A,(HL)
                            CP    #99
                            JR    NZ,IncGetal_2
                            XOR   A
                            LD    (HL),A
                            DEC   HL
                            DJNZ  IncGetal_1
                            SCF
                            RET
          IncGetal_2:       ADD   A,1
                            DAA
                            LD    (HL),A
                            OR    A
                            RET
          
          Deze  routine  vervangt elke  byte #99  vanaf rechts  in #00 
          totdat er een byte ongelijk is aan #99. Die wordt dan eentje 
          verhoogd (daarbij  treedt nooit een carry op!), gecorrigeerd 
          met DAA en klaar is kees. Let op: in plaats van de ADD,1 mag 
          NIET  door INC  A worden  vervangen! Want dan werkt DAA niet 
          meer goed. Decrease gaat net zo:
          
          ; DecGetal
          ; In : HL = ^getal, B = lengte/2
          ; Uit: getal = getal - 1, carry als getal gelijk aan 0
          
          DecGetal:         CALL  MovePointers
          DecGetal_1:       LD    A,(HL)
                            AND   A
                            JR    NZ,DecGetal_2
                            LD    A,#99
                            LD    (HL),A
                            DEC   HL
                            DJNZ  DecGetal_1
                            SCF
                            RET
          DecGetal_2:       SUB   1
                            DAA
                            LD    (HL),A
                            OR    A
                            RET
          
          Getallen  vergelijken   is  ook   een  belangrijke  en  vaak 
          voorkomende operatie:
          
          ; CompareGetal
          ; In: HL = ^getal1, DE = ^getal2, B = lengte/2
          ; Z- en C-flag worden net als bij CP gezet
          
          De  vlaggen worden  dus hetzelfde  gezet als  bij CP. Dat is 
          niet alleen  logisch maar  ook nog eens het makkelijkst voor 
          de implementatie:
          
          CompareGetal:     EX    DE,HL
          CompareGetal_1:   LD    A,(DE)
                            CP    (HL)
                            RET   NZ
                            INC   HL
                            INC   DE
                            DJNZ  CompareGetal_1
                            RET
          
          Ook  hier weer  een EX  DE,HL omdat er nu eenmaal wel een CP 
          (HL)  is  en geen  CP (DE).  Zo gauw  er een  ongelijke byte 
          gevonden  wordt  zijn we  klaar (RET  NZ). Als  de hele  lus 
          doorlopen wordt  zijn de getallen gelijk en is de Z-flag nog 
          gezet door de laatste CP.
          
          Tot  slot  nog de  routine waar  het allemaal  omdraait: het 
          afdrukken! Deze  routine is, zeker als je hem vergelijkt met 
          een  routine voor het afdrukken van 16- of 32-bits getallen, 
          zeer  snel.  Bovendien  zit  je hier  niet aan  een maximale 
          lengte vast, nou ja, de maximale lengte is 512 cijfers!
          
          ; WriteGetal
          ; In : HL = ^getal, B = lengte/2
          ; Getal wordt afgedrukt, routine PrintChar moet bekend zijn,
          ; met als invoer A: ASCII van character,
          ; mag HL, BC, DE wijzigen
          
          De manier van afdrukken ligt erg voor de hand, hierbij wordt 
          gewoon eerst  het hoogste  nibble genomen,  naar het laagste 
          nibble  geschoven, omgerekend  naar ASCII  en afgedrukt,  en 
          daarna wordt het laagste nibble afgedrukt.
          
          WriteGetal:       LD    A,(HL)
                            EXX
                            PUSH  AF
                            AND   #F0
                            RRCA
                            RRCA
                            RRCA
                            RRCA
                            ADD   A,"0"
                            CALL  PrintChar
                            POP   AF
                            AND   #0F
                            ADD   A,"0"
                            CALL  PrintChar
                            EXX
                            INC   HL
                            DJNZ  WriteGetal
                            RET
          
          Voor PrintChar kun je je eigen routine invullen om karakters 
          op  het  scherm  te  zetten.  En hiermee  is de  verzameling 
          routines voor ons eigen BCD formaat compleet.
          
          
                    V O O R D E L E N   E N   N A D E L E N 
          
          Tot  slot nog  even de voordelen en nadelen ten opzichte van 
          16- of 32-bits getallen op een rij.
          
          Voordelen:
          
          - de  lengte van  de getallen is vrijwel onbeperkt (maximaal 
            512 cijfers)
          - de snelheid van het afdrukken is zeer snel
          
          Nadelen:
          
          - optellen  en aftrekken  gaat iets  minder snel, hoewel het 
            meestal geen snelheidsproblemen zal opleveren
          - vermenigvuldigen en delen is zeer moeilijk te programmeren
          
          Het snelheidsverlies bij optellen/aftrekken wordt zeker goed 
          gemaakt door  het veel  snellere afdrukken. Vermenigvuldigen 
          en  delen  gaat  met  16-  en 32-bits  getallen ook  al niet 
          makkelijk,  maar hierbij  is het  helemaal ingewikkeld. Maar 
          vermenigvuldigen en delen heb je weinig nodig in spellen, en 
          als  het  toch  nodig  is  kun  je het  altijd met  herhaald 
          optellen/aftrekken  programmeren.  Niet echt  snel maar  het 
          werkt wel.
          
                                                          Stefan Boer
