           THIS TEXT WAS NOT SPONSORED BY WEBBER...

 De turbo en basic, een prima combinatie.
 Veel mensen hebben de heilzame invloed van de R800 in hun
 Turbo-R wel gemerkt, vooral in basic, wat vroeger nog in ML
 moest kan nou ook in basic, met de R800 aan, dat wel.

 Toch weten veel mensen niet hoe alles nu zo'n beetje wordt
 aangestuurd, en dat is jammer. Want zelfs de dingen die in
 eerste instantie niet zo nuttig lijken zijn toch leuk om aan
 te sturen, en dat kan zelfs in basic.. Lees, en ontdek.

DE LEDJES

 Op de Turbo zitten een aantal ledjes, 7 om precies te zijn. 't
 is altijd leuk om hiermee van die Knight Rider effectjes uit 
 te halen, vandaar even de aansturing. De Caps en de Kana leds
 zijn neem ik aan bekend. De power led en de FDD led kun je in
 basic niets mee, hoewel je, als je heel moeilijk doet, in ML
 de FDD led meen ik WEL aan kan zetten. Blijven er drie leds
 over, de RENSHA led, de R800 led en de PAUSE led. De RENSHA
 turbo schakeling valt helaas niets mee te doen op ons MSX-je.
 Dit hele ding is hardwarematig waarbij er vrijwel geen
 software-matige truukjes worden uitgehaald. Er kan met de
 potmeter worden ingesteld hoe lang de blokpuls die
 gegenereerd wordt moet zijn en vervolgens word deze geORd
 met PSG register 14 bit 4 en de 0de bit van rij8 van de
 Toetsenbord matrix. Zie hiervoor vorige FD. Dit gebeurt
 dus eigenlijk allemaal hardware-matig en je hebt hier dus
 niets aan. Zou je de lengte van een puls willen meten moet
 er nog eerst op een trig A worden gedrukt. Blijven de TURBO
 en PAUSE led over:

 TURBO led:     OUT &HA7,&B00000001
 PAUSE led:     OUT &HA7,&B00000001

 Waarbij de 1 natuurlijk aan is op de juiste plaats. Alle
 combinaties met bit 0 en 7 zijn natuurlijk mogelijk. Op de
 disk staat nog wel even en replayertje dat even leuk laat
 zien hoe de ledjes bijvoorbeeld kunnen worden gebruikt.

DE FIRM-SOFTWARE-SWITCH.

 In beide turbo's zit een aanzienlijke hoeveelhed zogenaamde
 FIRM-SOFTWARE. VIEW op de GT en HIRO op de ST. Deze software
 staat op ROM en is dmv de schakelaar op de kast aan en uit te
 schakelen. Het leuke is dat de stand van deze schakelaar in
 basic is uit te lezen. Dit is op zich wel grappig om ook
 eens in eigen programmatjes te gebruiken zoals je ook in
 mijn replayertje kan zien. In $E4 wordt een 5 gezet om de
 swith uit te lezen waarna de INP waarde nog even met 64
 geANDd moet worden om 't uiteindelijke resultaat te
 krijgen.

 10 OUT &HE4,5 : SW=INP (&HE5) AND 64
 20 IF A=64 THEN PRINT "ON" ELSE PRINT "OFF"

CANCEL & CONFIRM TOETSEN

 De Turbo's hebben ook nog een drietal extra toetsen. De, om ze
 maar een naam de geven, Cancel en Confirm toetsen en dus de
 Pauze toets. De pauze toets zat al eerder op 2+n maar toen was
 het nog een hardware matige schakelaar. Bij de Turbo is de 
 pauze toets echter software matig en is naar mijn mening nu
 pas een echte toets. Met de pauze toets kun je echter niets in
 basic omdat onder basic de interrupt normaal gesproken altijd 
 aan is en dan dus de computer ook echt gaat pauzeren. In MC is
 het echter wel degelijk mogelijk om met alle Turbo-toetsen te
 spelen. Dan blijven dus de CANCEL en CONFIRM toetsen, naast de
 spatiebalk, over. Deze zitten gewoon in de toetsenbord buffer
 en wel op resp bit 3 en bit 1 van rij 11. Zie over details van
 toetsenbord uitlezing de vorige FD. Om deze toetsen uit te 
 lezen brijg je dus het volgende programmaatje.

 OOPS, toen bleek dit inderdaad rij 11 te zijn, en of die ook
 echt op adres '11' staat weet ik niet, nou goed, een gokje dan
 maar hoe 't zou kunnen werken in BASIC, of dit zo is weet
 ik niet.

 10 IF (PEEK(&HFBF0)AND&B00000001)=0 THEN PRINT "CONFIRM"
 20 IF (PEEK(&HFBF0)AND&B00000100)=0 THEN PRINT "CANCEL"
 30 CLS:GOTO10

 Anders kan 't altijd nog in ML met:

 GETKEY: LD   A,11
         CALL $0141
         BIT  1,A
         JP   Z,CONFIR
         BIT  3,A
         JP   Z,CANCEL
         JP   GETKEY

DE PCM-SAMPLER

 En dan als laatste nog de PCM waar ik niet al te veel tijd
 aan wil besteden omdat die eigenlijk al gesneden koek is 
 voor de meesten.
 De syntaxis:

 CALL PCMREC  (@<SA>,<EA>,<FR>,[SV],[RL],[S])
 CALL PCMPLAY (@<SA>,<EA>,<FR>,[S])

 SA: Start Adres, &H0000 - &HFFFF
 EA: Eind Adres,  &H0000 - &HFFFF
 FR: Frequentie,       0 -      3

 SV: Start Volume, reageert bij bepaald volume.
 RL: Run Length, soort runlength, telt aantal 'nullen'
 S:  opslag in VRAM in plaats van RAM.

 Die runlength wil ik nog wel even op terug komen. Die is
 namelijk niet wat je zou vermoeden. Als de computer
 namelijk een bepaalde tijd 'stilte' registreert dan treedt,
 indien aangezet, deze optie in werking. De tijd die de
 stilte duurt wordt opgeslagen i.p.v de data die deze stilte
 beschrijft. Nog even een voorbeeld programmaatje.

 05 ' SAMPLE.BAS
 10 IF PEEK(&HF677)=&HC0 THEN GOTO 30
 20 POKE &HF677,&HC0 : POKE &HC000,0 : RUN "SAMPLE.BAS"
 30 FORP=2TO15:OUT &HFE,P:_PCMREC (@&H8000,&HBFFF,0):NEXTP
 40 FORP=2TO15:OUT &HFE,P:_PCMPLAY(@&H8000,&HBFFF,0):NEXTP
 50 OUT &HFE,1 : POKE &HF677,&H80 : POKE &H8000,0 : NEW

 En zie daar, en bere lange sample. Nou dat was 't dan wel
 voor deze keer want ik ben 't nu wel zat, see ya'll...

Keizer

 Wat ben ik toch ook een lekker slim joch, heb ik 't over de
 Turbo-R, vergeet ik 't belangrijkste, de R800. Deze is he-
 laas niet in BASIC met een call of zo aan te zetten, dit moet
 echt in machinetaal gebeuren. Door echter een zestal getallen
 in het geheugen te poken wordt dit probleem opgelost...

 10 FOR L=0 TO 5: READ V: POKE &HC000+L,V: NEXT L
 20 POKE &HC001,&B10000000 'Z 80 MODE
 30 POKE &HC001,&B10000001 'R800 MODE
 40 POKE &HC001,&B10000010 'DRAM MODE
 50 DEFUSR=&HC000:A=USR(0): DATA &H3E,&H00,&HCD,&H80,&H01,&HC9

 Nou, als 't goed is werkt dit wel dus dan is deze text nu echt
 aan z'n einde gekomen. Nog even melden dat de DRAM mode dus de
 laatste 64kB van je geheugen afsnoept, maar deze is dan ook 'n
 stuk sneller dan de R800 modus, als je veel BIOS routines gaat
 gebruikem tenminste... Maar dat zal in BASIC vaak wel het 
 geval zijn....

 Groeten, Keizer...

 En dit werkt natuurlijk niet nu ik er over na denk...
 Die routine zit natuurlijk in de SUB-ROM en niet in de
 Main Rom...
 Een nieuwe routine dus....

 10 FOR L=0 TO 6:READ V:POKE &HC000+L,V:NEXT L:DEFUSR=&HC000
 20 A=&B10000000 'Z 80 MODE
 30 A=&B10000001 'R800 MODE
 40 A=&B10000010 'DRAM MODE
 50 Z=USR(A): DATA &HDD,&H21,&H80,&H01,&HCD,&H5F,&H01,&HC9

 Hierbij mag je de eerste 1 weglaten als je niet wilt dat de 
 led mee veranderd. Well, now it should work....

 Keizer.

 En daar ben ik weer... -ik zal ook eens een tekst in een keer
 typen- En weer eens met nieuws omdat ik nooit ene flikker uit
 voer. Nou ja, bijna nooit. Door het poker spelletje van deze
 keer heb ik weer eens geen tijd gehad om het beloofde
 programmaatje te maken dus U kunt vrolijk vloeken en
 schelden op het nummer 046-374322, het nummer van de
 hoofdredacteur...(NvdR: die U vervolgens doorstuurt naar de
 crimineel in kwestie Dhr. T.K.K. Keizer. 05202-31343)

 Tobias Keizer.
