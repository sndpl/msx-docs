 Daar gaan we dan weer: alweer zo'n programmeer-tekst...ditmaal Advanced Basic...


    CURSUS ADVANCED BASIC



 Voor diegenen die niet genoeg hebben aan een handleiding en 
 wat meer voorbeelden willen, is hier een kleine cursus, die 
 ongeveer 3 a 4 delen zal beslaan.


    I WANT TO USE WINDOWS

 Het belangrijkste gedeelte zijn natuurlijk de windows. In 
 Advanced BASIC zijn windows ALLEEN te gebruiken is scherm 0, 
 de schermbreedte moet minstens 41 zijn. Er kunnen maximaal 
 16 windows tegelijk gebruikt worden, maar afhankelijk van de 
 grootte kunnen dat er minder worden. De maximale grootte van 
 een window is 80 bij 24, in dat geval worden de functietoet-
 sen ook overschreven. De inhoud van zo'n window is 78 bij 22
 tekens. De minimale grootte van een window is 3 bij 3, de
 inhoud is dan 1 bij 1. Als een window te groot is voor het
 scherm, dan wordt dat (natuurlijk) gemeld.

 Een window is te maken met _WINDOW(X,Y,BREEDTE,HOOGTE). X en 
 Y geven de positie van de linkerbovenhoek aan en BREEDTE en 
 HOOGTE geven aan wat de naam al zegt. Een voorbeeld:

 10 REM Mijn eerste window
 20 KEY OFF:_WINDOW(0,0,80,24)
 30 A$=INPUT$(1)
 RUN

 Er verschijnt een window op locatie 0,0 die 80 bij 24 groot 
 is.

         WE WANT TEXT

 Met _PRINT(STR$) is het mogelijk om een string in de laatst 
 gemaakte window te printen. Als het einde van het window 
 bereikt is, scrollt de inhoud omhoog, tenzij een _SCROLLOFF 
 geven is. _SCROLLON doet het omgekeerde van _SCROLLOFF en 
 zorgt dus dat de inhoud wel kan scrollen.

 Maar goed, ik dwaal af. Eh, _PRINT dus. Het is niet mogelijk 
 om een getal op het scherm te zetten. Nou ja, niet direct. 
 Het is natuurlijk wel mogelijk om een getal om te zetten 
 naar een string. Een voorbeeld:

 10 REM Mijn tweede window
 20 CLS:_WINDOW(0,0,80,3)
 30 _PRINT("Dit is mijn tweede window! Joepie!")
 40 LOCATE 0,3
 50 A$=INPUT$(1)
 RUN

 Er verschijnt een leuk windowtje met daarin de tekst zoals 
 aangegeven in regel 30.

        THATS NOT ALL

 De plek waar de tekst geprint moet worden in een window, kan 
 bepaald worden met _LOCATE(X,Y) en werkt hetzelfde als 
 LOCATE in gewoon BASIC. Als de plek buiten het scherm is, 
 komt er een foutmelding. Een klein voorbeeldje: _LOCATE(0,0) 
 zet de cursor in de linkerbovenhoek (hetgeen ook kan met 
 _HOME).

 Met _WCLS wordt de inhoud van de laatst gemaakte window 
 gewist. _RWIN haalt een window weg en herstelt datgene dat 
 achter het window zat. Het is ook mogelijk om alle windows 
 te wissen, dat kan met _RALLWIN. Ook hier wordt de 
 achtergrond weer hersteld.

  I LIKE SCROLLIN' AND MOVIN'

 Omdat mensen houden van scrollende dingen, zit er in 
 Advanced BASIC ook een optie daarvoor, nee, eigenlijk wel 
 vier, namelijk: _UP, _DOWN, _LEFT en _RIGHT.

 Deze commando's scrollen de inhoud van de laatst gemaakte 
 window in de aangegeven richting.

         WE WANT MORE

 Helaas, helaas, volgende keer meer!


Arjan Bakker
