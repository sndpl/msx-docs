Een zeer nuttige tekst...

     ADVANCED BASIC CURSUS 6


 Eindelijk, het laatste deel van deze cursus. Ik heb het
 gevoel dat geen hond zich interesseert in deze cursus.
 Het aantal personen die Advanced BASIC kochten zijn op een
 hand te tellen, ondanks de goede recensies. Blijkbaar
 programmeert niemand meer in BASIC, of is men gewoon te
 gierig om wat geld uit te geven? Hoe dan ook, dit is het
 laatste deel van deze cursus en daar ben ik blij om. Nu kan
 ik wat meer tijd zinniger besteden, bijvoorbeeld aan het
 programmeren van een MoonSound-replayer voor de FD.


           ENOUGH CRAP

 Goed, ik zal maar snel verdergaan met de cursus, want daar
 gaat het toch om. Zoals gezegd gaat deze aflevering over
 hybride-programmeren onder Advanced BASIC. Hybride
 programmeren wil zeggen dat er 2 of meer programmeertalen
 met elkaar gecombineerd worden, zoals BASIC met ML of PASCAL
 met ML.

 BASIC is over het algemeen prima te combineren met ML, maar
 wat toch onhandig is, is het doorgeven van veel parameters
 aan de routines. Meestal is het een kwestie van de hele zooi
 ergens POKE'n, maar dat is niet echt netjes. Met Advanced
 BASIC gaat dit een stuk makkelijker, want je kunt namelijk
 gewoon met CALL REGreg(x) een waarde aan een register geven.
 Alle registers zijn toegestaan, op de stackpointer, program-
 counter en de registers I en R na.

 Nadat je alle registers hebt gevuld, kun je met _CALL(adres)
 een ML-routine op het aangegeven adres gebruiken. Je kunt de
 ML-routine's natuurlijk ook met DEFUSR=x:A=USR(0) aanroepen,
 maar dan worden er geen registers doorgegeven.

 Natuurlijk kun je, als je de registers kunt wijzigen, ook de
 registers weer uitlezen. Dit kan met het commando CALL
 GETreg(adres). Het register aangeduidt met 'reg' wordt dan
 gezet in het aangegeven adres. Zoals al in een vorige deel
 van deze cursus gezegd werd, kun je met een truukje ook een
 variabele als bestemming gebruiken. Dit gaat als volgt:
 10 DEFINT A-Z
 20 _CALL(&H9F)
 30 T=0:_REGA(VARPTR(T))
 40 PRINT CHR$(T)
 RUN

 Dit programmaatje roept routine CHGET op adres &H9F aan,
 welke de ingedrukte toets in register A zet. Vervolgens kun
 je met VARPTR(T) het adres van variabele T krijgen en dat
 adres wordt dus gevuld met de waarde van de ingedrukte
 toets. Tot slot wordt de ingedrukte toets eventjes geprint.
 Let erop dat als je VARPTR(var) gebruikt, de variabele al
 wel moet bestaan. Daarom wordt de variabele T eerst
 aangemaakt in regel 30. Doe je dit niet, dan krijg je een
 botte Illegal function call. De variabele moet trouwens ook
 integer zijn, anders kan het fout gaan.


ADVANCED BASIC & KUN-BASIC

 Advanced BASIC en KUN-BASIC kunnen met elkaar gecombineerd
 worden. Je moet wel eventjes een heel klein programmaatje
 maken, maar dat is zo simpel als wat. De truuk is dat je de
 beide uitbreidingen allebei in een andere geheugenpagina zet
 en de juist pagina inschakelt als je een extra commando wilt
 gebruiken.

 Het volgende programmaatje laad Advanced BASIC en KUN-BASIC
 in het geheugen:

 10 OUT &HFD,2:BLOAD "ADVBASIC.BIN",R
 20 OUT &HFD,4:BLOAD "COMPILER.BIN",R

 Als je nu een commando van Advanced BASIC wilt gebruiken,
 schakel je eerst Advanced BASIC in met OUT &HFD,2 en dan kun
 je het commando gebruiken. Wil je KUN-BASIC gebruiken, dan
 kun je die inschakelen met OUT &HFD,4.

 Een nadeel is dat je geen commando's van Advanced BASIC
 binnen een stuk programma dat gecompileerd wordt. Het
 volgende kan dus niet:
 10 OUT &HFD,4  ' Schakel KUN in
 20 _TURBO ON
 30 PRINT "This won't work"
 40 OUT &HFD,2  ' Schakel Advanced BASIC in
 50 _WINDOW(0,0,80,3)
 60 OUT &HFD,4  ' Schakel KUN in
 70 _TURBO OFF
 RUN

 Het programma loopt nu vast op het commando _WINDOW.

 Als het goed is, heb ik nu alle commando's behandeld. Zo
 niet, pech gehad, want ik heb geen zin om ook nog maar ��n
 woord aan dit onderwerp vuil te maken.

 Arjan Bakker
