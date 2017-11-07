                          G E S T R U C T U R E E R D

                  P R O G R A M M E R E N   I N   M L   ( 2  )


          Na de  inleiding van Alex van de vorige keer gaan we nu echt
          beginnen.  De methode  die ik ga uitleggen in dit deel en de
          volgende delen  heet stapsgewijze verfijning of top-down. Ik
          zal het hier toespitsen op ML, in de Modula-2 cursus komt in
          principe hetzelfde verhaal, alleen dan voor Modula-2.


                 S T A P S G E W I J Z E   V E R F I J N I N G

          Het  principe is zeer simpel. Je wilt een probleem oplossen.
          Je schrijft  het probleem  eerst in  pseudo-code op, meestal
          zal  dat  een  soort  krom  Nederlands zijn.  Je splits  het
          probleem hierbij op in ongeveer drie of vier deelproblemen.

          Vervolgens leg  je voor ieder van de deelproblemen stap voor
          stap   nader  uit   wat  er   wordt  bedoeld.  Je  gaat  elk
          deelprobleem weer  op dezelfde  manier behandelen, totdat je
          op een gegeven moment op zo'n laag niveau bent aangeland dat
          je  het zonder  problemen in  ML kunt  zetten. Het verfijnen
          gaat net  zo lang  door totdat  een compleet ML programma is
          ontstaan.

          Het  voordeel  van  deze methode  is dat  je maar  een klein
          gedeelte  van het totale probleem tegelijk hoeft te overzien
          in ML, het zal je op dat moment een zorg zijn hoe de rest is
          geprogrammeerd. Hierdoor blijft het overzichtelijk.

          In  Modula-2 heb je procedures en parameters om dit allemaal
          in goede  banen te  lijden, in  ML zul  je dit allemaal zelf
          moeten  doen. Vooral  de parameters  (lees: registers)  zijn
          nogal  eens  een  probleem,  je  moet  goed  opletten  welke
          registers  je   moet  bewaren   en  welke   je  rustig  kunt
          veranderen.

          Het  is  belangrijk  dat  je  er bij  het schrijven  van een
          routine  vanuit gaat  dat de routines die je daarin aanroept
          al werken.  Dat is  dus niet zo! Je werkt topdown, van boven
          naar  beneden dus  en je schrijft dus eerst de hoofdroutine,
          dan   de   routines   die   door  de   hoofdroutines  worden
          aangeroepen,  dan  de routines  die weer  door die  routines
          worden aangeroepen, etc.

          Een  probleem  daarbij is  dat je  vantevoren vaststelt  via
          welke registers  de in-  en uitvoer  van een  routine op een
          lager niveau gaat, en het kan gebeuren dat bij het schrijven
          van  die routine  blijkt dat  je beter  andere registers had
          kunnen  kiezen.  Hier  is  weinig  aan  te  doen,  maar door
          ervaring zul je dit soort fouten steeds minder maken.


                               V O O R B E E L D

          Om  te laten  zien wat  ik bedoel zal ik het eerste gedeelte
          van de  hoofdroutine van  een willekeurig spel bespreken. Ik
          zal  hier nog  niet tot het laagste niveau afdalen, dat komt
          later.

          Stel we  hebben een  spel met een speler en vijanden, als de
          speler tegen een vijand aanloopt moet hij ermee vechten. Het
          scherm scrollt niet maar werkt met 'flipscreens' (als je het
          scherm uitloopt komt het volgende scherm).

          De eerste aanzet voor een hoofdlus:

                  Beweeg vijanden
                  Beweeg speler
                  Ga zonodig naar ander scherm
                  Zet de vijanden en speler op het scherm
                  Kijk of de speler ��n van de vijanden raakt
                  Herhaal

          Dit is vrij eenvoudig in ML om te zetten:

          Hoofdlus:       CALL    MoveEnemies
                          CALL    MovePlayer
                          CALL    OtherScreen
                          CALL    PutSprites
                          CALL    CheckHit
                          JR      Hoofdlus

          Dit  is erg  overzichtelijk en  het is zeer eenvoudig om nog
          iets  extra's   toe  te  voegen.  Als  er  bijvoorbeeld  een
          tijdslimiet is dan wordt het bijvoorbeeld:

          Hoofdlus:        CALL  MoveEnemies
                           CALL  MovePlayer
                           CALL  OtherScreen
                           CALL  PutSprites
                           CALL  CheckHit
                           CALL  CheckTimeLimit
                           JR    NC,Hoofdlus
                           CALL  OutOfTime

          Waarbij  de  routine  CheckTimeLimit  een  carry retourneert
          indien de tijd is verstreken en OutOfTime een melding op het
          scherm  zet dat  de tijd op is. Het is tijd voor de volgende
          stap in  het verfijningsproces,  de routine MoveEnemies. Het
          is  misschien erg voor de hand liggend maar de eerste aanzet
          daarvoor is:

          FOR i = 1 TO Aantal vijanden
                  Beweeg vijand nummer i
          NEXT

          Soms  is het  handig om wat BASIC (of bijvoorbeeld Modula-2)
          te  gebruiken   in  de   pseudo  code,   dat  is  korter  en
          duidelijker.   Dit  is  natuurlijk  zeer  simpel  in  ML  te
          programmeren:

          MoveEnemies:     LD    A,(AantalEnemies)
                           LD    B,A
          MoveEnemies_Lus: CALL  MoveEnemy
                           DJNZ  MoveEnemies_Lus
                           RET

          Het ontwerp van MoveEnemy ziet er bijvoorbeeld zo uit:

                  Bepaal welke richting de vijand op moet
                  Kijk of dat mogelijk is (i.v.m. obstakels)
                  Zo ja, pas dan de co�rdinaten aan in de gewenste
                  richting

          Ook dit is weer eenvoudig in ML om te zetten:

          ; MoveEnemy
          ; Beweeg een vijand
          ; In : B = nummer van vijand
          ; Uit: B is ongewijzigd

          MoveEnemy:       CALL  WhichDirection
                           CALL  CheckIfPossible
                           RET   C
                           CALL  ChangeCoors
                           RET

          Waarbij   de   headers  van   de  routines   WhichDirection,
          CheckIfPossible en ChangeCoors er zo uitzien:

          ; WhichDirection
          ; Bepaal welke richting een vijand op moet gaan
          ; In : B = nummer van vijand
          ; Uit: A = richting
          ;      B is ongewijzigd

          ; CheckIfPossible
          ; Bepaal of het mogelijk is voor monster B om in richting A
          ; te bewegen
          ; In : A = richting, B = monster
          ; Uit: carry = niet mogelijk/no carry = wel mogelijk
          ;      A en B zijn ongewijzigd

          ; ChangeCoors
          ; Monster B beweegt in richting A
          ; In : A = richting, B = monster
          ; Uit: coordinaten zijn gewijzigd
          ;      B is ongewijzigd

          En zo  kun je  dus verder  gaan totdat  je bij de echte code
          uitkomt,  ChangeCoors is  daar bijvoorbeeld al een geschikte
          kandidaat voor.


                                  L A Y O U T

          De gebruikte  layout is zeer belangrijk voor de leesbaarheid
          van  je programma.  Zelf programmeer ik 'case sensitive', ik
          zet alle  Z80-instructies in hoofdletters en labels beginnen
          met  een hoofdletter  en zijn  voor de  rest kleine  letters
          behalve als  er een nieuw woord begint in een label. Ik werk
          met  een  labellengte  van 16  tekens zodat  ik fatsoenlijke
          namen  voor routines  kan gebruiken en ook nog ruimte heb om
          er dingen als "_Lus" achter te plakken.

          De TABs zijn als volgt ingesteld:

          1       label
          19      instructie
          25      operand
          43      commentaar

          Dus bijvoorbeeld:

          Schermopbouw_Lus: LD    A,AantalKolommen  ; commentaar

          Het  lijkt hier alsof de ruimte voor commentaar erg klein is
          maar normaal  gesproken heb  je natuurlijk 80 kolommen (hier
          60).  Je kunt  zelf je  eigen layout kiezen, als je hem maar
          consequent volhoudt  en er  geen puinhoop  van maakt. Ik heb
          deze layout bij WR gebruikt en dat is erg goed bevallen.

          Het  voordeel van  lange labels  blijkt al  snel als je gaat
          bedenken  wat  het  label  Schermopbouw_Lus  in  WB-ASS2 zou
          worden: SCOB_L of zoiets. Niet echt leesbaar dus, zeker niet
          als het  alweer een  paar weken  geleden is  dat je voor het
          laatst naar die source gekeken hebt.

          De  basisprincipes zijn  nu hopelijk  duidelijk, de volgende
          keer zal ik hier verder op doorgaan.

                                                          Stefan Boer
