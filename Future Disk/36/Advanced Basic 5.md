Arjan bazelt wat over Advanced Basic...

      ADVANCED BASIC (5)


 Welkom bij alweer het vijfde deel van deze cursus. Deze keer
 ga ik het hebben over data-verplaatsingen, zoals ik de
 vorige keer beloofd had.

 Het komt nogal eens voor dat je gegevens van ram naar vram
 of andersom wilt verplaatsen. In BASIC gaat dit normaal
 gesproken nogal traag en daarom heeft ADVANCED BASIC wat
 extra commando's daarvoor, namelijk:

 _RAMRAM(startadres,aantal,bestemming)
 Verplaatst aantal bytes vanaf startadres naar
 bestemming. Een nuttig voorbeeld weet ik niet zo snel te
 verzinnen.

 _RAMVRAM(startadres,aantal,bestemming,pagina)
 Verplaatst aantal bytes van ram naar vram. Pagina is om
 aan te geven of de 1e 64 kB of de 2e 64 kB vram als
 bestemming gebruikt moet worden. Een voorbeeld:
 10 SCREEN 0
 20 A$="Dit is een tekst ":HL=&HA000
 30 FOR A=1 TO LEN(A$)
 40 POKE HL,ASC(MID$(A$,A,1)):HL=HL+1
 50 NEXT A
 60 _RAMVRAM(&HA000,LEN(A$),240,0)
 RUN

 Dit programmaatje zet een string in het geheugen en
 verplaatst het naar adres 240 van het VRAM, wat in SCREEN 0
 de vierde regel is.

 _VRAMRAM(startadres,pagina,aantal,bestemming)
 Verplaatst aantal bytes van vram naar ram. Een voorbeeld
 lijkt me overbodig.

 _VRAMVRAM(startadres,pagina,aantal,bestemming,pagina)
 Verplaatst aantal bytes van vram naar vram. Een leuk
 voorbeeldje:
 10 SCREEN 0
 20 PRINT "Deze tekst scrolled naar links"
 30 _VRAMVRAM(1,0,79,0)
 40 FOR A=1 TO 100:NEXT A
 50 IF STRIG(0)=0 THEN GOTO 30
 RUN

 Dit programmaatje print een tekstje en laat het naar links
 scrollen en stopt pas als er op spatie gedrukt word.

 _OUTM(startadres,aantal,poort)
 Stuurt aantal bytes vanaf startadres naar een I/O-poort.
 Vooral handig bij de nieuwe MSX-uitbreidingen als OPL4 en
 GFX9000.

 _INM(poort,aantal,bestemming)
 Haalt aantal bytes uit I/O-poort en zet het in ram. Zie ook
 _OUTM.


         WHAT'S NEXT

 Verder heeft Advanced BASIC nog wat commando's om het (v)ram
 met een waarde te vullen, namelijk:

 _FILLRAM(startadres,aantal,waarde)
 Vult ram met een bepaalde waarde. Een voorbeeld:
 10 _FILLRAM(&H8000,128,0)
 20 PRINT "Deze tekst zie je niet meer...."
 RUN

 Het programma wordt helemaal overschreven met nullen. De
 tekst in regel 20 zul je dus niet meer te zien krijgen en
 het programma stopt gewoon.

 _FILLVRAM(startadres,pagina,aantal,waarde)
 Vult vram met een bepaalde waarde. Een voorbeeld:
 10 SCREEN 0
 20 _FILLVRAM(0,0,24*80,65)
 30 IF STRIG(0)=0 THEN GOTO 30
 40 CLS
 RUN

 Dit programmaatje vult het hele scherm met de letter A. Na
 een druk op de spatie kom je weer in BASIC.


         WAT RESTJES

 Als laatst geef ik nog 2 commando's die niet zoveel met
 data-verplaatsingen hebben te maken, namelijk:

 _GETPAL(kleur,rood,groen,blauw)
 Leest de RGB-waardes van een kleur en zet die op de adressen
 rood, groen en blauw.

 _HOME
 Verplaatst de cursor van een window naar positie (0,0).

 De volgende en laatste keer komt hybride-programmeren onder
 Advanced BASIC aan bod, waarmee je leuke dingen kunt doen
 (onder andere Advanced BASIC combineren met KUN-BASIC!). Tot
 dan!


Arjan Bakker
