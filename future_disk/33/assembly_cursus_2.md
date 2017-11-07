 De assembly cursus gaat eindelijk verder...
ASSEMBLY CURSUS (2)


 Hier is dan eindelijk het tweede deel van de assembly 
 cursus. Dit maal bespreek ik een string-search routine. De 
 routine is zo simpel mogelijk gehouden door wildcards etc. 
 gewoon niet toe te staan. Het moet echter simpel zijn om de 
 wildcard '?' te implementeren.


DE INVOER

 De routine heeft 3 variabelen nodig, namelijk het adres waar 
 met zoeken moet worden begonnen (HL), het aantal te 
 onderzoeken bytes (BC) en het beginadres van de te zoeken 
 string (DE). De te zoeken string moet afgesloten worden met 
 #FF, evenals de strings in het geheugen.


DE ROUTINE

 Hier komt dan de routine, met tussendoor wat uitleg.

 SEARCH: PUSH   DE
         INC    HL

 SEARCH initialiseert de zoek-routine. DE moet bewaard 
 worden, omdat deze wordt gebruikt om bij te houden op welke 
 plek in de string we zitten. CPI verhoogt HL altijd, maar 
 als de bytes niet hetzelfde zijn, moet dat juist niet. 
 Daarom wordt HL alvast verhoogd, omdat HL in STRSRC verlaagt 
 MOET worden.

 STRSRC: POP    DE
         PUSH   DE
         DEC    HL
         LD     A,(DE)

 SRCHF:  CPIR
         JP     Z,FOUNDF

 STOP:   LD     A,255
         POP    DE
         RET

 Eerst wordt de pointer weer opgehaald en bewaard. HL wordt 
 verlaagd omdat CPI HL altijd verhoogt, terwijl dat juist 
 niet altijd nodig is. Dan wordt het eerste teken van de 
 string geladen en CPIR zoekt dan net zolang totdat het teken 
 gevonden is. Als het teken gevonden is, wordt er gesprongen 
 naar de routine die de volgende bytes onderzoekt en anders 
 stopt het programma met 255 in de accu om aan te geven dat 
 de string niet gevonden is.

 FOUNDF: LD     (STRADR),HL

 Adres van string wordt bewaard, omdat de string niet gelijk 
 hoeft te zijn als de zoekstring.

         INC    DE
         LD     A,(DE)
 SRCHN:  CPI
         JP     NZ,STRSRC

 Volgende teken wordt geladen en vergeleken. Als ze niet 
 gelijk zijn, wordt er teruggegaan naar de routine die het 
 eerste teken zoekt.

         LD     A,B
         OR     C
         JP     Z,STOP

 Als alle bytes geweest zijn, stopt het programma.

         INC    DE

 Volgende teken.

         LD     A,(DE)
         CP     #FF
         JP     NZ,SRCHN
         LD     A,(HL)
         CP     #FF
         JR     NZ,STRSRC

 Er wordt gekeken of de zoekstring aan het einde is. Als dat 
 niet zo is, wordt er verder gegaan met vergelijken. Als dat 
 wel zo is, wordt er gekeken of de string in het geheugen ook 
 aan het einde is. Als dat niet zo is, wordt er overnieuw 
 begonnen met zoeken.

         XOR    A
         LD     HL,(STRADR)
         DEC    HL
         POP    DE
         RET

 STRADR: DEFW   0

 Accu wordt 0 om aan te geven dat het zoeken gelukt is. Het 
 adres van de string wordt geladen en DE wordt teruggehaald 
 ivm met RET.

 Zo, dit was de routine. Voor de mensen die het willen 
 gebruiken, staat de routine ook op disk onder de naam 
 STRSRCH.ASM. Tot de volgende keer!

Arjan Bakker
