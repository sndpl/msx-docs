 Hoe kunnen we verven in ML???? Awel, na het lezen van deze tekst wordt het ons allemaal duidelijk...


         PAINT IN ML



 Het is lastig om zelf een routine te maken die een vlak kan 
 inkleuren. Zo'n routine hoeft helemaal niet gemaakt te 
 worden, want in de SUB-ROM zit al zo'n routine. Deze is 
 echter niet zonder meer te gebruiken, want de routine is 
 voor de BASIC-interpreter. 

 Om de routine te gebruiken, moet je een stukje BASIC 
 aanmaken in ML. In dit geval is dit niet lastig, tenminste, 
 als je de codes weet.


       WAT IS ER NODIG

 De routine die voor het painten gebruikt wordt, heeft alleen 
 maar een klein stukje BASIC nodig, namelijk datgene dat 
 achter het commando PAINT komt.

 Dit stukje ziet er zo uit:

 db 40,28,0,0,44,28,0,0,41,44,28,0,0,44,28,0,0,0

 De 40 geeft een haakje openen aan, 28 geeft aan dat de 
 volgende 2 bytes een integer vormen, 44 betekent komma, 41 
 is haakje sluiten, de laatste nul betekent dat de regel is 
 afgelopen en daarmee zijn alle codes uitgelegd. De bytes 
 betekenen dus:

 (integer,integer),integer,integer   einde regel

 Bij het commando PAINT zien we dus dat het betekent:

 (x,y),vulkleur,randkleur   einde regel

 Het enige wat je nu moet doen is de nullen vullen met een 
 waarde en routine #0069 in het SUBROM aanroepen. De volgende 
 listing doet dat.


          DE SOURCE

 ; Kleur een vlak in (alleen scherm 5,6,7 en 8)
 ; Werkt misschien ook op scherm 10, 11 en 12
 ; In: DE = X-coordinaat
 ; HL = Y-coordinaat
 ; B = vulkleur
 ; C = randkleur
 ; Verandert: AF, BC, DE en HL

 paint: ld    (dat1+2),de
        ld    (dat1+6),hl
        ld    a,b
        ld    (dat1+11),a
        ld    a,c
        ld    (dat1+15),a
        ld    hl,dat1
        ld    ix,#0069
        call  #015f
        ret

 dat1:  db  40,28,50,0,44,28,50,0,41,44,28,15,0,44,28,15,0,0
 ; PAINT    (      X    ,     Y    )  ,     VK   ,    RK


 Zoals je ziet is het erg eenvoudig. Voor de luitjes die te 
 lui zijn om deze source even over te tikken, staat de source 
 (als het goed is)(NvdR: en alles is goed!!!) ook op de disk
 onder de naam PAINT.ASM.


Arjan Bakker

