 De beginnertjes hebben het goed dit keer!!! Een gehele assymbly cursus voor jullie...


   ASSEMBLY VOOR BEGINNERS


 In deze rubriek komen routine's aan bod die nuttig zijn en 
 vaak gebruikt worden. Degenen die geen assembly kunnen, 
 hebben niets aan deze cursus. In dit eerste deel komt de 
 vermenigvuldigings-routine aan bod.


     HOE DOEN WE DIT DAN?

 Okay, eerst eventjes laten zien hoe het dus niet moet, zoals 
 beginners het vaak doen (schaam je niet, ik deed het eerst 
 ook zo). Vermenigvuldigen gebeurt meestal botweg als 
 herhaald optellen van een getal. Bijvoorbeeld, 15 * 25 is 
 het resultaat van 25 + 25 + ... + 25 + 25 (in totaal 15 keer 
 het getal 25 en 14 keer het plusteken). Als source ziet dit 
 er zo uit:

         LD   BC,15
         LD   DE,25
         LD   HL,0

 MULUW:  ADD  HL,DE
         DEC  BC
         LD   A,B
         OR   C
         JP   NZ,MULUW
         RET

 Natuurlijk, bij kleine getallen werkt dit wel snel, maar als 
 je grote getallen gebruikt, is een andere methode veel 
 sneller.


   TERUG NAAR DE BASISSCHOOL

 Hoe pakken we het dan wel aan? Nou, ik zal even een voor-
 beeldje geven hoe ik het geleerd heb op de basisschool.

             15
             25
            --- x
  5 * 15 =   75
  2 * 150 = 300
            --- + 
            375

 Telkens als we een getal naar links gaan, dan vermenigvul-
 digen we het eerste getal met 10. Op de MSX gaat het net zo, 
 maar nu moeten we even binair denken, dus vermenigvuldigen 
 we met 2, in plaats van 10. Hetzelfde voorbeeldje nog eens,
 maar nu in het binaire talstelsel.

                       1111    = 15
                      11001    = 25
                   -------- x    --- x
   1 * 1111 =          1111    = 15
   0 * 11110 =            0
   0 * 111100 =           0
   1 * 1111000 =    1111000    = 120
   1 * 11110000 =  11110000    = 240
                   -------- +  ----- +
                  101110111    = 375

 Zoals je ziet, klopt dit als een bus.


         NU OP DE MSX

 We gaan er even van uit dat de te vermenigvuldigen getallen 
 in BC en DE staan en dat het resultaat in HL komt. De source 
 ziet er dan als volgt uit:

         LD    DE,25     ;De te vermenigvuldigen getallen.
         LD    BC,15

 MULUW:  LD    HL,0      ;HL moet 0 zijn bij het begin

 MULUW1: SRL   D         ;Deel DE door 2. Als DE oneven was,
         RR    E         ;wordt, de carry-flag gezet.
        
         JR    NC,MULUW2 ;Carry-flag niet gezet? Niet optellen.
         ADD   HL,BC     ;BC optellen bij resultaat.

 MULUW2: LD    A,D       ;Als DE 0 is, zijn we klaar.
         OR    E
         RET   Z

         SLA   C         ;Vermenigvuldig BC met 2.
         RL    B
         JP    MULUW1    ;Terug naar MULUW1

 Met een CALL naar MULUW kun je de getallen in DE en BC snel 
 met elkaar vermenigvuldigen, het resultaat komt in HL.

 Als het goed is, is het nu duidelijk hoe het werkt. Zo niet, 
 lees de tekst dan nog maar eens over.

 Oja mensen, er staan .ASM files op deze FD. Dus dan kun je
 het allemaal nog eens rustig bekijken (ASSEMBL1.ASM).

Arjan Bakker
