Jaja, we gaan weer verder met de cursi...
   ADVANCED BASIC CURSUS 4


 Welkom bij het vierde deel van deze cursus. In het eerste
 deel van deze cursus zei ik dat er ongeveer 4 delen zouden
 zijn, maar dat is niet zo. Na dit deel komen er nog twee
 delen, eentje over data-verplaatsingen en eentje over
 hybride programmeren met Advanced BASIC.


          KARAKTERS

 In normaal BASIC is het niet mogelijk om makkelijk een
 nieuwe karakter te maken. Advanced BASIC heeft hier een
 speciaal commando voor, namelijk _CHAR. Het eerste parameter
 geeft het karakter aan dat verandert moet worden en de
 volgende 8 parameters geven de vorm van het karakter aan.
 Een voorbeeld:

 10 SCREEN 0:WIDTH 80
 20 _CHAR(65,128,64,32,16,8,4,2,1)
 30 PRINT STRING$(80,65)
 RUN

 Dit programmaatje verandert de letter 'A' (ascii-code is 65).


         SNELLE COPY

 Advanced BASIC heeft een commando om snel een stuk van het
 scherm te kunnen copi�ren, namelijk:
 _COPY(sx,sy,sp,nx,ny,dx,dy,dp,[log_op])

 De parameters sx en sy geven de x- en y-co�rdinaten weer
 vanaf waar er gecopi�erd moet worden, sp is de bronpagina,
 nx en ny geven aan hoeveel pixels er gecopi�erd moeten
 worden, dx en dy zijn de bestemmingsco�rdinaten en dp is de
 bestemmingspagina. Het parameter log_op geeft de logische
 operatie aan, maar deze mag worden weggelaten. Een
 voorbeeld:

 10 SCREEN 5
 20 CIRCLE (50,50),50,15
 30 COPY (0,0,0,100,100,110,110,0)
 40 A$=INPUT$(1)
 RUN

 Dit programaatje tekent een cirkel en copi�ert het naar
 (110,110).

 Als je logische operatoren wilt gebruiken, moet je dat
 anders doen dan in gewoon BASIC. Bij Advanced BASIC moet je
 namelijk getallen gebruiken. Hier komt een lijstje welk
 getal welk effect heeft:

 Getal         Logical operatie
   0            -
   1            AND
   2            OR
   3            XOR
   4            NOT
   8            TIMP
   9            TAND
   10           TOR
   11           TXOR
   12           TNOT


         MEER LIJNEN

 Een MSX2 (en hoger) kan 26,5 lijnen op scherm 0 en 1 laten
 zien, maar MSX-BASIC gebruikt dit niet. Met Advanced BASIC
 it het mogelijk om meer lijnen te gebruiken met het commando
 _LINES(x). Het aantal lijnen kan alleen 24 en 26 zijn.
 Jammer genoeg werkt de BASIC-editor niet goed in de laatste
 lijnen. Hier is een voorbeeld:

 10 SCREEN 0:_LINES(26):KEY OFF
 20 FOR A=0 TO 25
 30 PRINT "Yippie, 26 lijnen op het scherm!"
 40 NEXT A
 RUN

 Het resultaat is 26 keer de tekst van regel 30 op het
 scherm.


      LIJNTJES BEWERKEN

 Advanced BASIC heeft een aantal commando's om lijnen te
 verwijderen, toevoegen en een gedeelte van een lijn te
 wissen. Lijnen tussenvoegen kan met _INSLINE(y). Y geeft aan
 op welke regel een nieuwe lijn moet komen. De lijnen eronder
 zullen ��n regel omlaag scrollen en een lege lijn komt op
 het scherm. Een voorbeeld:

 10 SCREEN 0
 20 PRINT "Dit is een tekstje"
 30 FOR A=0 TO 23
 40 _INSLINE(0)
 50 NEXT A
 RUN

 Dit programma heeft als resultaat een tekstje dat omlaag
 scrollt (scrolled?).

 Met _DELLINE(y) is het mogelijk om een lijn te wissen. De
 lijnen onder lijn y zullen omlaag scrollen. Met
 _DELREST(x,y) is het mogelijk om een deel van een lijn te
 wissen, en wel vanaf positie x,y. Een voorbeeld:

 10 SCREEN 0
 20 PRINT "Dit is een verdwijnende tekst"
 30 FOR A=31 TO 0 STEP -1
 40 _DELREST(A,0)
 50 NEXT
 RUN

 Het resultaat is een lijn dat letter voor letter verdwijnt.


         HET RESTJE

 Het is mogelijk om met Advanced BASIC het scherm aan of uit
 te zetten. Dit is ook mogelijk onder gewoon BASIC, maar met
 Advanced BASIC is het toch eventjes iets makkelijker. _SCRON
 zet het scherm aan en _SCROFF zet het scherm uit.

 Als je iets in een window print en het past er niet meer in,
 dan scrollt de inhoud van het window omhoog. Dit zal echter
 niet altijd gewenst zijn. Daarom is er een commando _SCROLLOFF
 dat het scrollen uitzet. Natuurlijk is er dan ook een commando
 dat het scrollen weer aan zet, en dat is _SCROLLON.

 Op een Turbo-R is het mogelijk om van processor te wissen.
 Dit kan gedaan worden met _SPEED(x). Hier is een lijstje met
 mogelijkheden:

 0 = Z80
 1 = R800 ROM
 2 = R800 DRAM
 anders = fout

 _SPEED werkt ook op een MSX2(+), maar dan kan je alleen de
 Z80-mode gebruiken.

 Nou, dit was het dan voor deze keer!

Arjan Bakker
