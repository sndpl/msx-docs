                     C U R S U S   M O D U L A - 2   ( 1  )


                               I N L E I D I N G

          Aangezien het  niet mogelijk  is om  in ��n  deel iemand die
          helemaal niets van Turbo Pascal weet Modula-2 te leren ga ik
          de  volgende keer  van nul  af aan beginnen. Dit deel van de
          cursus is daarom bedoeld voor MSX'ers die al ervaring hebben
          met Turbo Pascal.

          Als  je  niet  wilt  wachten  tot  Special  #7  dan  kun  je
          natuurlijk  altijd eens  inloggen bij  de Games BBS, daar is
          een Engelstalige  Modula-2 cursus  te vinden  en een  flinke
          hoeveelheid  voorbeeldsources. Bovendien  is er  een .PMA te
          vinden met uitleg van Pierre Gielen over het naar modulevorm
          overzetten van ML-routines.


                                  O P B O U W

          Onderstaand voorbeeld laat de opbouw van een Modula-2 source
          zien.  Het programma vult een array met wortels en drukt dat
          array vervolgens af. Wat het programma doet is niet boeiend,
          het gaat om de opbouw.


          MODULE Opbouw;

          (*
                  Auteur      : Stefan Boer
                  Versie      : 1.0
                  Last updated: 09/06/94
          *)


          FROM MathLib IMPORT
                  Sqrt;


          CONST
                  AantalWortels = 10;


          TYPE
                  WortelIndexType = CARDINAL [1..AantalWortels];
                  WortelLijstType = ARRAY WortelIndexType OF REAL;


          VAR
                  WortelLijst: WortelLijstType;


          PROCEDURE BerekenWortels(VAR WortelLijst: WortelLijstType);

          VAR
                  i: WortelIndexType;

          BEGIN
                  FOR i := 1 TO AantalWortels DO
                          WortelLijst[i] := Sqrt(FLOAT(i));
                  END;
          END BerekenWortels;


          PROCEDURE PrintWortels(WortelLijst: WortelLijstType);

          VAR
                  i: WortelIndexType;

          BEGIN
                  FOR i := 1 TO AantalWortels DO
                          WRITELN(WortelLijst[i]);
                  END;
          END PrintWortels;


          (* Hoofdprogramma *)

          BEGIN
                  BerekenWortels(WortelLijst);
                  PrintWortels(WortelLijst);
          END Opbouw.


          Merk op  dat de  definitie van  MathLib afwijkend is van wat
          Wirth  voorschrijft  bij  Turbo  Modula-2!  Wirth noemt  hem
          MathLib0  en de  vierkantswortel heet bij hem sqrt, niet met
          een hoofdletter dus!

          Pascalkenners zal de volgende dingen opvallen:

          - De source begint niet met PROGRAM maar met MODULE
          - ENDs van modules en procedures zijn gelabeld
          - De syntax van het FOR-statement is anders
          - Sqrt moet worden ge�mporteerd
          - Declaratie van subrange en array gaat anders

          Ik  zal  nu een  aantal verschillen  tussen Turbo  Pascal en
          Turbo Modula-2 bespreken.


                      C O M P O U N D   S T A T E M E N T

          Bij  Turbo Pascal  ziet een  IF statement er bijvoorbeeld zo
          uit:

          IF boolean expressie THEN
                  statement1
          ELSE
                  statement2;

          Om toch  meerdere statements te kunnen gebruiken heeft Wirth
          het  compound statement  bedacht, door  BEGIN en  END om een
          groep statements te zetten worden ze samen ��n statement.

          Dit kan  erg verwarrend zijn, vooral als je een zootje maakt
          van de layout. Bijvoorbeeld:

          IF a = 3 THEN
                  WRITE('a is 3');
                  WRITE('hallo');

          Door  de  layout  suggereer  ik  hier dat  het tweede  WRITE
          statement alleen wordt uitgevoerd als a gelijk is aan 3. Dit
          is  niet zo,  alleen het eerste WRITE statement hoort bij de
          IF. De juiste Pascal formulering zou zijn:

          IF a = 3 THEN
          BEGIN
                  WRITE('a is 3');
                  WRITE('hallo');
          END;

          Hier  hebben  we  dus  een compound  statement gebruikt.  In
          Modula-2 is het veel logischer:

          IF boolean expressie THEN
                  statement sequence 1
          ELSE
                  statement sequence 2
          END;

          Waarbij  een 'statement  sequence' bestaat  uit 0  (!), 1 of
          meer statements!  Er staat dus NOOIT een BEGIN en ALTIJD een
          END!  Bovendien  MOGEN  er  in  Modula-2 altijd  ; geplaatst
          worden. Bij Pascal is dat niet zo. Bijvoorbeeld:

          IF a = 3 THEN
                  WRITE('hallo');
          ELSE
                  WRITE('doei');

          geeft  een foutmelding in Pascal, de ; achter WRITE('hallo')
          is in Pascal niet toegestaan. In Modula-2 moet er natuurlijk
          nog een  END achteraan  en de puntkomma mag wel! (Maar hoeft
          niet.)

          Omdat een  puntkomma achter een statement in Modula-2 altijd
          mag zet ik er ook altijd ��n, dan kan ik me nooit vergissen!

          Bij WHILE, FOR en WITH gaat het net zo. Dus:

          WHILE boolean expressie DO
                  statement sequence
          END;

          FOR loopvariable := startwaarde TO eindwaarde DO
                  statementsequence
          END;

          WITH record-variabele DO
                  statementsequence
          END;


                                   T Y P E S

          Turbo Modula-2 kent de volgende atomaire types:

          BITSET          standaard settype
          BOOLEAN         waar/niet waar
          CHAR            character
          CARDINAL        0, 1, 2, ..., 65535
          INTEGER         -32768, ..., 0, ..., 32767
          LONGINT
          REAL            single precision floating point (6 cijfers)
          LONGREAL        double precision floating point (14 cijfers)

          Voorbeelden van simple types:

          subrange:    IndexType   = CARDINAL [1..10];
          enumeration: StatusType  = (Slecht, Matig, Redelijk, Goed);

          Voorbeelden van gestructureerde types:

          record:      PatientType = RECORD
                                       Temperatuur: REAL;
                                       Bloeddruk  : REAL;
                                     END;
          array:       LijstType   = ARRAY IndexType OF PatientType;
          set:         Verzameling = SET OF IndexType;

          Verder zijn er nog pointertypes en procuduretypes, maar daar
          kom ik  in een  later deel  van de  cursus nog  wel eens  op
          terug.


                                F U N C T I E S

          In Pascal ziet een functie er bijvoorbeeld als volgt uit:

          FUNCTION Kwadraat(x: REAL): REAL;

          BEGIN
                  Kwadraat := x * x;
          END;

          In Modula-2 wordt dit:

          PROCEDURE Kwadraat(x: REAL): REAL;

          BEGIN
                  RETURN x * x;
          END Kwadraat;

          Er zijn 3 belangrijke verschillen:

          1) PROCEDURE in plaats van FUNCTION
          2) RETURN in plaats van functienaam :=
          3) Bij een Modula-2 functie staan altijd haakjes, ook als er
             geen parameters zijn!

          Met  RETURN wordt  ook meteen  de functie  be�indigd, RETURN
          zonder  iets  erachter kan  in een  gewone procedure  worden
          gebruikt om uit de procedure te springen.


                                M C C A R T H Y

          Modula-2 gebruikt  McCarthy evaluatie  bij het evalueren van
          boolean  expressies.  Dit  komt  erop  neer  dat  de boolean
          expressie  niet verder  wordt ge�valueerd  dan nodig  is. In
          Pascal   wordt   de   boolean   expressie  altijd   helemaal
          ge�valueerd!

          McCarthy evaluatie heeft geen nadelen, de voordelen zijn dat
          het  sneller  is en  dat onderstaand  statement altijd  goed
          gaat:

          IF (n = 0) OR (a / n > 10) THEN ...

          Bij Turbo  Pascal gaat dit fout, want bij het uitrekenen van
          a  / n  wordt er door 0 gedeeld! Bij Modula-2 gaat het goed,
          want  als  n  gelijk  is aan  0 dan  wordt a  / n  niet meer
          uitgerekend!


                                  S T R E N G

          Modula-2 is  strenger bij  de controle  op types.  Operaties
          zoals  aftrekken, vermenigvuldigen,  etc. mogen  alleen maar
          worden gedaan op gelijke types!!!

          Het onderstaande mag dus niet!

          VAR
                  x: INTEGER;
                  y: CARDINAL;

                  WRITE(x*y);


                               T E N S L O T T E

          Dit  waren in  het kort  de belangrijkste verschillen tussen
          Turbo Modula-2 en Turbo Pascal. De volgende keer ga ik vanaf
          het   begin   beginnen   met   een   cursus   gestructureerd
          programmeren in Modula-2.

          Tot dan!
                                                          Stefan Boer
