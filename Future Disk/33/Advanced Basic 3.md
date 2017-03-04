Arjan vervolgt zijn cursus Advanced Basic...
    Cursus Advanced BASIC (3)


 Advanced BASIC heeft een aantal extra diskcommando's die het 
 leven heel wat makkelijker maken voor de BASIC-programmeur.

 Een van die commando's is _DISKRD(drive,start,aantal,adres).
 Dit commando laadt een aantal sectoren achterelkaar. De 
 variabele drive staat hier voor de drivenummer (0 = A, 1 = B
 enz.), start is de eerste sector, aantal is het aantal 
 sectoren en adres is het adres waar de inhoud van de gelezen 
 sectoren terecht moet komen.

 Als er een _DISKRD is, is er natuurlijk ook een _DISKWRT, 
 die een aantal sectoren achter elkaar schrijft. De syntax is
 hetzelfde als bij _DISKRD.

 Een voorbeeldje:

 10 _DISKRD(0,0,16,&H9000)
 20 _DISKWRT(1,0,16,&H9000)
 RUN
 
 Dit programma laadt de eerste 16 sectoren van drive A en 
 slaat ze op op drive B.


BESTANDEN

 Bepalen welke bestanden er op een diskette staan is altijd 
 lastig geweest onder BASIC. Advanced BASIC heeft daar 3 
 commando's voor. Ten eerste is er _FILENAME(VAR$), welke 
 bepaald naar welke bestanden er moet worden gezocht. VAR$ 
 MOET overigens minstens 11 letters lang zijn en het *-teken 
 mag niet gebruikt worden.

 Om nu het eerste bestand dat op de diskette staat te 
 achterhalen, is er _SFFILE(VAR$) (VAR$ is weer minstens 11 
 tekens lang). De eerste filenaam die voldoet aan de met 
 _FILENAME(VAR$) opgegeven filenaam, komt nu in VAR$ terecht.
 Alle volgende filenamen kunnen met _SNFILE(VAR$) geladen 
 worden. Als er 2 keer achter elkaar dezelfde filenaam 
 gelezen is, heb je alle filenamen gehad.

 Een voorbeeld:

 10 _FILENAME("????????BAS")
 20 F$=SPACE$(11):G$=F$
 30 _SFFILE(F$):IF G$=F$ THEN END
 40 PRINT F$:G$=F$
 50 _SNFILE(F$):IF G$=F$ THEN END
 60 GOTO 40
 RUN

 Dit programma laat alle bestanden met de extensie .BAS zien.


DRIVES

 Het aantal aangesloten drives is te bepalen met het commando
 _NDRIVES(adres). Het aantal drives komt dan op het opgegeven 
 adres terecht.

 De huidige drive is op te vragen met _GETDRIVE(adres). Als 
 je de huidige drive wilt veranderen, kun je _SETDRIVE(drive)
 gebruiken.

 Een voorbeeld:

 10 _GETDRIVE(&H9000)
 20 PRINT "De huidige drive is ";CHR$(PEEK(&H9000+65))
 30 _NDRIVES(&H9000)
 40 PRINT "Er zijn";PEEK(&H9000);" drives aangesloten"
 50 PRINT "Welke drive moet de default-drive worden?";
 60 INPUT "In letters graag";D$
 70 D=ASC(D$) OR 32:D=D XOR 32:D=D-65
 80 _SETDRIVE(D)
 RUN

 Dit programma geeft de defaultdrive en het aantal drives
 weer en daarna mag je de nieuwe defaultdrive zelf bepalen.


TOETJE

 Als toetje zijn er nog de commando's _VERIFYON en _VERIFYOFF
 welke de controle op schrijffouten respectievelijke aan- en
 uitzetten.


Arjan Bakker
