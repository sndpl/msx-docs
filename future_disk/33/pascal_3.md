Dit is een poging om de langste scroll regel ooit op de FutureDisk te maken (NvdR: dat is mislukt Jeroen, ik heb hem afgekapt!)

          PASCAL (3)


 ����  Welcome to the third PASCAL course. Ofwel, hallo, dit
 ����  is de derde PASCAL cursus op de FD. Het mag dan wel
 ����  FutureDisk 33 zijn, maar aangezien de PASCAL cursus pas
 ����  zijn derde nummer ingaat zou alles dus helemaal okie
       dokie moeten zijn. Wat een gezeur h�! Ik zal maar be-
 ginnen, voor ik nog meer onzin uitkraam.

 Vorige keer heb ik het over Koen gehad (helemaal in het begin)
 en ja hoor, hij had weer iets te zeiken, dus nu even mijn
 antwoord: "Ja Koen, je kunt ook 100 minuten bandjes door mijn
 programma laten uitrekenen.". Zo, dat is ook weer opgelost.
 (NvdR: ja stomme ezel, dat had ik ook al door!)

 Nu even echt serieus. Deze keer ga ik lussen bespreken. In
 PASCAL zijn drie soorten lussen: FOR-TO-DO, REPEAT-UNTIL en
 WHILE-DO. Uit de namen kan al ��n en ander afgeleid worden.
 Maar ik zal de verschillende lussen met een voorbeeld
 behandelen.

 Stel ik wil de getallen 1 t/m 10 op het scherm toveren. Hoe
 doe ik dat? Hier komen de drie mogelijkheden:

FOR-TO-DO:

   PROGRAM ForNext (input,output);

     VAR
       teller : integer;

     BEGIN
       FOR teller := 1 TO 10 DO
         writeln(teller);
     END.

REPEAT-UNTIL:

   PROGRAM RepeatUntil (input,output);

     VAR
       teller : integer;

     BEGIN
       teller := 0;
       REPEAT
         teller := teller + 1;
         writeln(teller);
       UNTIL teller = 10
     END.

WHILE-DO:

   PROGRAM WhileDo (input,output);

     VAR
       teller : integer;

     BEGIN
       teller := 0;
       WHILE teller < 10 DO
       BEGIN
         teller := teller + 1;
         writeln(teller);
       END;
     END.

 Dat waren ze. Maar nu de uitleg. In het eerste voorbeeld
 (FOR-TO-DO) zie je, dat de variabele "teller" van 1 t/m 10 zal
 lopen. Er staat tenslotte dat de teller zal lopen van 1 tot en
 met 10, en zolang moet je teller afdrukken. Een FOR lus kan
 niet worden onderbroken (voor de echte crack wel, maar dat is
 met een GOTO en dat is weer niet gestructureerd en dus niet
 netjes). Een FOR lus loopt altijd van onder- tot bovengrens.
 Nu denk je natuurlijk: Ik wil ook van 10 naar 1 kunnen lopen,
 dat kan, maar dan moet je DOWNTO in plaats van TO gebruiken.
 Voor de goede orde: Als de ondergrens groter is dan de
 bovengrens (FOR teller := 2 TO 1 DO) dan gebeurt er dus
 niets.

 Het tweede voorbeeld (REPEAT-UNTIL) werkt heel anders. Hier
 wordt de lus op een bepaalde conditie verlaten. In het
 voorbeeld staat dan ook HERHAAL ... TOTDAT teller=10. Zolang
 teller niet 10 is, gaat de lus door. Probeer maar eens wat er
 gebeurt als je de regel "teller := teller + 1;" weglaat. Je
 zult zien dat de lus eindeloos doorgaat; teller wordt ook
 nooit 10.

 Het verschil met het tweede en derde voorbeeld is eigenlijk
 minimaal. Ook hier wordt de lus verlaten bij een bepaalde
 conditie (teller=10), alleen wordt hier eerst gecontroleerd en
 daarna pas beslist of de lus doorlopen moet worden. Het grote
 verschil is dan ook dat de REPEAT-UNTIL minimaal 1 maal
 doorlopen wordt, terwijl dat bij de WHILE-DO niet het geval
 is.

 Zo, dat was een snel overzicht van de lus functies in PASCAL.
 En nu maar oefenen. Tja, ik kan het niet vaak genoeg zeggen,
 veel oefenen, anders leer je het niet. Ik zal, hoewel vorige
 keer niemand een reactie heeft gegeven(gek h�) (NvdR: ligt
 gewoon aan jou!), ook deze keer wat opdrachten geven.

 OPDRACHTEN:
 (1) Maak een programma dat alle mogelijke combinaties van 4
     (hoofd)letters op het scherm zet (doe dit met alleen
     FOR-TO-DO, REPEAT-UNTIL of WHILE-DO lussen).
 (2) Maak een programma dat alle priemgetallen tot een
     ingegeven getal berekend (doe ook dit met alleen
     FOR-TO-DO, REPEAT-UNTIL of WHILE-DO lussen).

 Voordat jullie kunnen beginnen moet ik nog even snel wat
 uitleggen over compound-statements. In PASCAL is het namelijk
 zo, dat na bepaalde opdrachten (FOR-TO-DO, WHILE-DO,
 IF-THEN-ELSE) maar ��n opdracht mag staan. Bij de FOR-TO-DO
 lus mag dus niet staan:

   WHILE teller < 10 DO
     write(teller);
     teller := teller + 1;

 De opdracht "write(teller)" is namelijk 1 opdracht en de
 volgende opdracht "teller := teller + 1" zou pas worden
 uitgevoerd als de WHILE-DO lus voorbij is (nooit(!)). Vaak is
 het echter noodzakelijk dat meerdere opdrachten in een lus
 worden uitgevoerd. De oplossing hiervoor is het compound-
 statement. Een compound-statement is een heleboel statements
 omgeven door een BEGIN en een END. Het voorbeeld zal dus
 worden:

   WHILE teller < 10 DO
   BEGIN
     write(teller);
     teller := teller + 1;
   END;

 Bij het bovenstaande programma zal alles wel naar wens
 verlopen. Samenvattend komt het er op neer dat als je bij
 lussen (REPEAT-UNTIL is hier de bevestigende uitzondering)
 meerdere statements wil laten uitvoeren, dat je die statements
 moet laten voorafgaan door BEGIN en eindigen met END.

 Zo, met bovenstaande informatie moet je in staat zijn om de
 zes opdrachten te maken.

   Veel plezier,

                                Jeroen "In de herhaling" Smael
