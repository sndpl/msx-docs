       SCHUIVEN & ROTEREN

                             ◊◊◊◊
                             ◊◊◊◊
                             ◊◊◊◊
                             ◊◊◊◊


 De moeilijkste commando's die de Z80 kent, zijn wat mij 
 betreft wel de schuif- en roteerinstructies, en dan wel met 
 name het gedoe rond het gebruik van de carry-flag. Zelfs 
 mijn MSX Machinetaalboek (Dullin/Strassenburg) gaat op dit 
 gebied de mist in. Gelukkig staan daar ook copi◊n van de 
 Z80-tabellen in, waar alles uiteraard correct instaat, dus 
 dat maakt het wel weer goed, maar lastig blijven de 
 instructies wel.

 De schuifinstructies zijn overigens wel makkelijk, maar 
 omdat schuif-instructies dicht bij roteerinstructies zitten, 
 neem ik ze hier toch maar mee. De instructies RLD en RRD 
 laat ik hier buiten beschouwing. Op een vorige FD (38 geloof 
 ik) is hier meer informatie over te vinden.


        SCHUIVEN MAAR

 Er zijn 3 verschillende (offici◊le) soorten schuifinstruc-
 ties, namelijk SLA, SRA en SRL. Deze instructies kunnen als 
 parameter meekrijgen: een register (A,B,C,D,E,H,L) of een 
 pointer ( (HL),(IX+d),(IY+d) ).

 De instructie SLA is te vergelijken met een vermenigvuldi-
 ging van 2. De bits worden rekenkundig (de letter A van SLA) 
 ÇÇn plaatsje naar links (de letter L) geschoven (de letter 
 S), waarbij bit 0 nul wordt en de oorspronkelijke bit 7 in 
 de carry-flag komt.

 De instructie SRA en SRL doen allebei ongeveer hetzelfde: 
 delen door 2. De bits worden ÇÇn plaatsje naar rechts 
 geschoven en bit 0 komt in de carry-flag terecht. Het 
 verschil tussen SRA en SRL is dat bij SRA bit 7 gelijk 
 blijft (zodat signed getallen correct gedeeld worden), 
 terwijl bij SRL bit 7 nul wordt.


           ROTEER ZE

 Het hele probleem van de roteer-instructies wordt veroor-
 zaakt door de naamgeving van de instructies. Volgens mijn 
 logica zou de instructie RLCA de bits van register A naar 
 links schuiven, waarbij bit 7 in de carry-flag terecht komt 
 en de oude carry-flag in bit 0 komt. Het zou dus eigenlijk 
 ee soort 9-bits rotatie opleveren. Dit is echter niet het 
 geval. RLCA schuift de bits wel ÇÇn positie naar links, en 
 bit 7 komt ook wel in de carry-flag terecht, maar in plaats 
 van dat de oude carry-flag in bit 0 terecht komt, komt bit 7 
 terecht in bit 0! Een voorbeeldje om alles duidelijk te 
 maken:

 Register A bevat de waarde &B01000001 ofwel 65 en de carry- 
 flag is gezet. Na een RLCA instructie wordt register A 
 &b10000010 (bit 0 wordt 0 omdat bit 7 nul was) en de 
 carry-flag wordt gereset (bit 7 was immers gereset).

 De instructie RLA daarentegen zou volgens mijn logica de 
 bits ook ÇÇn positie naar rechts schuiven, waarbij bit 7 in 
 de carry-flag terecht komt en bit 0 gelijk aan de oude bit 7 
 wordt. Dit is echter niet het geval, want bij RLA wordt bit 
 0 niet gelijk aan de oude bit 7, maar gelijk aan de oude 
 carry-flag! Weer even een voorbeeldje om alles duidelijk te 
 maken:

 Register A bevat de waarde &b01000001 ofwel 65 en de carry-
 flag is gezet. Na een RLA instructie wordt register A 
 &b10000011 (bit 0 wordt 1 omdat de carry-flag gezet was) en 
 de carry-flag wordt gereset (omdat bit 7 oorspronkelijk 
 gereset was).

 Helemaal onlogisch is de naamgeving niet. De letter C uit de 
 instructie RLCA staat namelijk voor Circular (rondgaand in 
 het Nederlands), waarmee de makers proberen duidelijk te 
 maken dat de bits in hetzelfde register blijven. Wat mij 
 betreft is dit w◊l onlogisch, aangezien de instructie RLA 
 ook circulair is, alleen duurt het daar een ronde voordat 
 een oorspronkelijk bit weer terugkomt (na 9 keer RLA bevat 
 de accumulator weer de oorspronkelijke waarde, terwijl dit 
 bij RLCA na 8 keer al het geval is).

 De namen van de instructies zouden eigenlijk omgewisseld 
 moeten worden, waarbij de letter C de betekenis 'with Carry-
 flag' ofwel 'met Carry-flag' zou moeten krijgen. De makers 
 van de R800 zagen dit ook in, daar heet de Z80-instructie 
 RLCA (die RLA zou moeten heten) ROLA (zonder C dus) en de 
 instructie RLA heet ROLCA, zoals het volgens mijn logica ook 
 zou moeten zijn (afgezien van die extra 'O' dan).

 Er zijn overigens wel meer instructies van naam verandert 
 bij de R800. Wat te denken van ROL4 (HL) in plaats van RLD, 
 of, nog leuker, MOVEM (HL++),(DE++) in plaats van LDIR. Maar 
 nu dwaal ik af geloof ik.

 Terug naar de roteerinstructies. Afgezien van het gedoe rond 
 die carry-flag, hebben de instructies wel een logische naam. 
 De eerste letter, R, staat voor Rotate. De tweede letter kan 
 een R of een L zijn. R betekent Right en L betekent dus 
 Left, om aan te geven welke richting de bits geschoven 
 moeten worden. Tot slot KAN er de letter C volgen. Als deze 
 er staat, betekent het (onlogisch) dat de carry-flag NIET 
 naar bit 0 getransporteerd wordt, zonder de letter C gebeurt 
 het (ook weer onlogisch) w◊l. 

 De roteer-instructies kunnen dezelfde parameters krijgen als 
 de schuif-instructies, dus daar maak ik geen woorden meer 
 aan vuil.


         RLCA = RLC A???

 Van de instructies die met de accumulator werken, bestaan er 
 twee versies, namelijk:

 RLCA en RLC A
 RLA  en RL  A
 RRCA en RRC A
 RRA  en RR  A

 Het is een misverstand om te denken dat de instructies uit 
 de linker kolom hetzelfde doen als de instructies uit de 
 rechter kolom. De meeste programmeurs weten wel dat de 
 instructies uit de linker kolom ÇÇn byte korter en twee keer 
 zo snel zijn, maar dat de flag-be◊nvloeding anders is weet 
 bijna niemand. Bij de instructies RLCA/RLA/RRCA en RRA 
 worden de zero-flag, de P/V-flag en de Sign-flag namelijk 
 niet be◊nvloed, terwijl dit bij de langere versies van deze 
 instructies w◊l het geval is. Let hier dus goed op!!!

Arjan