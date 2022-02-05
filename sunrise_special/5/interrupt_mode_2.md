                        I N T E R R U P T   M O D E   2 
                                                         
          
          De Z80 kent 3 interrupt modes. Het hele operating system van
          MSX werkt maar met  ��n  van  deze  modes,  te  weten  IM  1
          (Interrupt Mode). Feitelijk is dit een  beperking  omdat  de
          andere  interrupt  modes  zeer  interessante  mogelijkheden
          bieden.
          
          De precieze  werking van de verschillende interrupt modes is 
          nader  beschreven in  een eerder artikel op Sunrise Special. 
          Interrupt  mode  0  is  op MSX  niet bruikbaar,  IM 1  wordt 
          normaliter gebruikt en IM 2 is een twijfelgeval.
          
          In het eerdere artikel heb ik beweerd dat ook IM  2  op  MSX
          niet bruikbaar is, maar dat is niet helemaal  waar.  Om  het
          geheugen eerst wat op te frissen volgt eerst even een  korte
          uitleg van IM 2.
          
          
                                    I M   2 
          
          IM 2 is op Z80 de zgn. 'Vector Interrupt' mode. Dat houdt in
          dat er naar verschillende ISR's (Interrupt Service  Routine)
          gesprongen kan worden na het optreden van een interrupt.  Op
          Z80 werkt dit als volgt.
          
          In het geheugen staat een tabel  van  max.  256  bytes  waar
          praktisch per twee bytes een 16 bits adres kan staan. Totaal
          zijn volgens deze rekening 128 adressen mogelijk. In  het  I
          register van de processor  moet  nu  het  HI  byte  van  het
          startadres van deze tabel komen.
          
          &HC000 = vector entry 0
          &HC002 = vector entry 1
            ...
          &HC0FE = vector entry 127
          
          Het I register moet hier de waarde &HC0 hebben  waardoor  de
          processor het startadres van de tabel weet.
          
          IM 2 gaat er van uit dat het interrupt genererende  apparaat
          niet alleen het INT lijntje aktief maakt, maar dat ditzelfde
          apparaat (bv. de VDP) ook een getal op de bus  zet.  In  ons
          voorbeeld mogen dat dan alleen positieve getallen zijn. Stel
          een chip genereert een interrupt en zet de waarde  2  op  de
          bus. De processor gaat nu  de  waarde  van  het  I  register
          combineren met de waarde die op  de  bus  komt  tijdens  een
          interrupt. Daaruit volgt dan een 16 bits waarde  (&HC002  in
          dit voorbeeld). De processor leest  nu  het  16  bits  getal
          vanaf dit gecombineerde adres en springt vervolgens naar het
          zojuist gelezen adres.
          
          Vb:
          
          &HC000 = (&H8000 = ISR 1)
          &HC002 = (&H9000 = ISR 2)
          I = &HC0
          Indien chip A een interrupt genereert en de waarde 0  op  de
          bus  zet  zal  uiteindelijk  naar  adres  &H8000  gesprongen
          worden. Indien chip  B  de  waarde  2  op  de  bus  zet  zal
          uiteindelijk naar adres &H9000 toegesprongen worden.  Indien
          chip C de waarde 1 op de bus zou zetten zal naar adres  &H80
          gesprongen worden (voer voor de pro's).
          
          Het voordeel van deze methode is  dat  er  geen  vertragende
          routines meer nodig zijn om er achter  te  komen  van  welke
          chip  de  interrupt  af  komt.  Dat  wordt   allemaal   door
          hardware geregeld. Het enige waarvoor gezorgd moet worden is
          dat  indien  er  verschillende  chips  zijn  die  interrupts
          genereren deze allen een  verschillende  waarde  op  de  bus
          zetten.
          
          
                             I M   2   E N   M S X 
          
          IM 2 is op MSX niet expliciet nodig omdat er maar 1 chip  is
          die een interrupt aan de processor door kan  geven,  nl.  de
          VDP. Daar komt nog bij dat de VDP helemaal geen waarde op de
          bus zet als deze een interrupt genereert, dus IM 2 lijkt  op
          MSX nutteloos.
          
          Vanuit die gedachtengang heb ik destijds beweerd dat IM 2 op
          MSX niet mogelijk is, wat niet helemaal waar blijkt te zijn.
          Het is op MSX mogelijk om  1  vectorinterrupt  te  gebruiken
          i.p.v. de praktische 128 in het vorige voorbeeld. Op MSX  is
          de Z80 zelf de enige chip in de computer  die  een  lees  of
          schrijfaktie op de bus kan beginnen. De CPU is dus de  enige
          'Bus Master'. Indien zelfs de Z80 niet op  de  bus  schrijft
          zal de waarde op de bus altijd &HFF zijn. De buslijnen  zijn
          van het 'Pull up'  type  (open  collector  uitgang  voor  de
          electrotechneuten) wat dus praktisch neerkomt op  de  waarde
          &HFF.
          
          Treedt er nu een interrupt op (altijd afkomstig van de  VDP)
          dan zal de Z80 indien IM 2 aktief is op de bus kijken en  de
          waarde &HFF zien. Indien het I register de waarde &HC0 heeft
          zal dus de 16 bits waarde vanaf adres &HC0FF gelezen  worden
          waarna naar deze waarde gesprongen wordt.
          
          Het praktisch nut van dit 'geintje' is dat zo voorkomen  kan
          worden dat naar adres &H38 gesprongen wordt.  We  kenden  al
          een geintje om via &HFD9A te voorkomen dat  de  hele  rimram
          die normaal gesproken bij  een  interrupt  uitgevoerd  wordt
          over te slaan. Een andere methode was het  uitschakelen  van
          de ROM-BIOS waardoor een  eigen  routine  vanaf  adres  &H38
          gezet kan worden.
          
          Het uitschakelen van  de  ROM-BIOS  is  echter  vrij  lastig
          (vooral in combinatie met de BDOS).  Via  IM  2  is  het  nu
          echter mogelijk om te  voorkomen  dat  er  naar  adres  &H38
          gesprongen wordt. Daarmee is het dus  mogelijk  om  wel  een
          kompleet eigen ISR te schrijven, terwijl de ROM-BIOS  gewoon
          'aan' kan blijven staan.
          
          Het   is   overigens   zeer   eenvoudig   om    een    klein
          testprogrammatje te maken dat met IM 2 werkt. Dat kan er bv.
          zo uit zien:
          
                 ORG  &HC000
          
          RUN:   DI
                 LD   HL,&H38
                 LD   (&HC0FF),HL
                 LD   A,&HC0
                 LD   I,A
                 IM   2
                 EI
                 RET
          
          De werking van dit programma laat zich raden.
          
          Het is best leuk om eens te stoeien met IM 2. Overigens zijn
          de termen IM 0, IM 1 en IM 2 ook assembler  instrukties  die
          als zodanig letterlijk gebruikt kunnen worden. Let wel  even
          op dat je na afhandeling van een ISR de interrupts weer  aan
          zet.
          
                                                     Alex van der Wal
