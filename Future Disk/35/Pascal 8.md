Jeroen programmeert verder en verder en kan het nog steeds niet...
           PASCAL (8)


 ����  Hallie hallo, hier is Smael weer met zijn PASCAL cursus.
 ����  Deel 8 deze keer (nog twee en dan is het alweer
 ����  voorbij). In deel 8 gaan we dieper in op pointers.
 ����  Vorige keer heb ik uitgelegd wat pointers zijn en wat je
       ermee kunt. Nu is het dus tijd voor een groot voorbeeld.
 Als voorbeeld neem ik een programma om namen en adressen op te
 slaan (een soort database dus). Goed, daar gaat 'ie (wat
 hebben we weer een plezier)!! (NvdR: ja nou)

 Het programma staat op de disk als 'PASCAL8.PAS'. De listing
 vind je terug in deze tekst, maar alleen ter verduidelijking.

 De declaraties veronderstel ik bekend (moet je kunnen) en daar
 ga ik dus niet verder op in. Wel staan ze hier even vermeld,
 zodat je de rest van het programma begrijpt:

  PROGRAM Pascal8 (Input,Output,Names);

    CONST
      Max = 30;

    TYPE
        Point = ^Row;
        Row  = RECORD
                 Name    : String[Max];
                 Address : String[Max];
                 Next    : Point;
               END;

    VAR
      Names  : Text;
      Choice : integer;
      Main   : Point;
      Temp   : Point;
      Number : integer;

 Ik ga dus verder met de eerste procedure, aangezien hier
 misschien dingen zijn die je niet begrijpt. Als ik zo naar
 deze procedure kijk, dan kom ik tot de ontdekking dat je
 alleen het commando 'ClrScr' niet kent, maar dat de naam alles
 duidelijk maakt. Let ook op hoe ik het probleem van de
 toetsdruk oplos. De computer wacht totdat ik op de <RETURN>
 toets heb geramd, daarvoor kun je dus alle letters inrammen
 die je wil (ze zullen zelfs zichtbaar zijn op het scherm).
 Niet erg netjes dus, maar dat kun JIJ verbeteren (gebruik de
 functie 'KeyPressed'). Verder gesneden koek:

  PROCEDURE welcome;

    BEGIN          (* welcome *)
      clrscr;
      writeln('This is an example that comes with FD#35');
      writeln;
      writeln('This little programm makes it possible');
      writeln('to create a list of addresses using');
      writeln('pointers.');
      writeln;
      writeln;
      writeln('Enjoy !!');
      writeln;
      writeln;
      writeln;
      write('Press <RETURN> to continue.');
      readln;
    END;           (* welcome *)

 Ook deze procedure moet gesneden koek zijn (alleen maar wat
 Writeln's en Readln's). Wederom kun je een en ander
 verbeteren. Als je namelijk een verkeerde keuze maakt, dan
 wordt op de volgende regel opnieuw naar je keuze gevraagd.
 Maak je ongeveer 20 keer de verkeerde keuze, dan is het
 keuzemenu niet meer te zien (uit het beeld geschoven) en dat
 is niet zo netjes. Read it and weep :

  PROCEDURE menu(VAR choise:integer);

    BEGIN          (* menu *)
      clrscr;
      writeln('You can choose from :');
      writeln;
      writeln('(1) Load adresses');
      writeln('(2) Insert address');
      writeln('(3) Show adresses');
      writeln('(4) Delete address');
      writeln('(5) Save addresses');
      writeln('(6) Stop');
      writeln;
      writeln;
      writeln;
      choise:=0;
      REPEAT
        write('Your choice : ');
        readln(choise);
      UNTIL (choise>=1) AND (choise<=6);
    END;           (* menu *)

 Hier wordt het interessant. Er wordt een file ingelezen van
 namen die ooit al eens zijn ingevoerd. Deze heb ik lekker
 gemakkelijk gehouden. Vergelijk hem maar eens met de SAVE
 procedure en je komt tot de ontdekking dat de namen omgekeerd
 ingelezen worden. Wat bedoel ik? Als de namen in alfabetische
 volgorde zijn weggeschreven (als voorbeeld), dan worden ze
 anti-alfabetisch (dus net omgekeerd van Z naar A) ingelezen.
 Let ook op de VAR (in de procedure-kop) bij Main en Names.
 Bij Names is het altijd verplicht (een file kan niet in het
 geheugen gekopieerd worden), maar bij Main zou ik het weg
 kunnen laten. Of niet (een pointer kan in het geheugen
 gekopieerd worden)? In dit geval niet!!

 Als er al ��n naam, of meerdere, ingevoerd waren, dan kon ik
 het weglaten (moet de rest wel achteraan toegevoegd worden)
 aangezien Main dan niet meer hoeft te veranderen (let vooral
 op de voorgaande opmerking). Nu wordt elke nieuwe naam vooraan
 in de lijst bijgeschoven en dus veranderd Main bij elke naam.
 Conclusie : VAR kan niet weggelaten worden (anders ben je je
 namen kwijt als je terugkeert naar het hoofdprogramma).

 Let ook op het gebruik van de EOF functie. Ik hou er rekening
 mee dat een file verminkt is. Ik zou kunnen volstaan met de
 EOF controle bij de WHILE-lus, maar als de file verminkt is,
 dan gaat het fout bij het inlezen van het adres. Gevolg is een
 crash van de computer en dus zijn al je gegevens weg. Let
 hier op bij het programmeren van een grote toepassing. Let
 tevens op de 'Close(Names)' aan het einde van de procedure,
 deze is geplaatst om problemen te voorkomen indien je na het
 inlezen het programma be�indigd (er staat immers nog een file
 open). Verder bekend (neem ik aan) :

PROCEDURE Loading(VAR Main:Point;VAR Names:Text);

  VAR
    Temp : Point;

  BEGIN            (* Loading *)
    Assign(Names,'address.txt');
    Reset(Names);
    WHILE NOT EOF(Names) DO
    BEGIN
      New(Temp);
      IF NOT EOF(Names) THEN
        Readln(Names,Temp^.Name)
      ELSE
        Temp^.Name:='';
      IF NOT EOF(Names) THEN
        Readln(Names,Temp^.Address)
      ELSE
        Temp^.Address:='';
      Temp^.Next:=Main;
      Main:=Temp;
    END;
    Close(Names);
  END;             (* Loading *)

 Ook deze procedure moet geen echte problemen opleveren (zeker
 niet als je de vorige procedure begrepen hebt). Hier worden
 namen toegevoegd aan de lijst (wederom vooraan). Ik houd er
 rekening mee dat de gebruiker gekke dingen doet, vandaar de
 dubbele controle op het invoeren van een lege regel. Let ook
 op de twee Dispose opdrachten op het einde van de procedure!!
 Again, read it and weep :

PROCEDURE Adding(VAR Main:Point);

  VAR
    Temp : Point;

  BEGIN            (* Adding *)
    New(Temp);
    ClrScr;
    Writeln('Adding an address to the list');
    Writeln;
    Writeln;
    Writeln('Give a name and an address (RETURN is stop).');
    Writeln;
    Write('Give name    : ');
    Readln(Temp^.Name);
    IF Temp^.Name<>'' THEN
    BEGIN
      Write('Give address : ');
      Readln(Temp^.Address);
      IF Temp^.Address<>'' THEN
      BEGIN
        Temp^.Next:=Main;
        Main:=Temp;
      END
      ELSE
        Dispose(Temp);
    END
    ELSE
      Dispose(Temp);
  END;             (* Adding *)

 Aan deze procedure maak ik geen woorden vuil (buiten het feit
 dat ik je wijs op het handige gebruik van de MOD functie bij
 de REPEAT-UNTIL):

PROCEDURE Print(Main:Point);

  VAR
    Counter : integer;
    Temp    : Point;

  BEGIN
    Temp:=Main;
    Counter:=0;
    REPEAT
      ClrScr;
      REPEAT
        IF Temp<>NIL THEN
        BEGIN
          Writeln(Temp^.Name);
          Writeln(Temp^.Address);
          Temp:=Temp^.Next;
        END
        ELSE
        BEGIN
          Writeln;
          Writeln;
        END;
        Writeln;
        Counter:=Counter+1;
      UNTIL (Counter MOD 7)=0;
      Write('Press <RETURN> to continue.');
      Readln;
    UNTIL Temp=NIL;
  END;             (* Print *)

 Dit is natuurlijk de meest interessante procedure. Je zou hem
 nu echter (na het lezen van Laden en Plus) zo goed als moeten
 kunnen dromen. Nee, dat is maar een grapje (NvdR: haha).
 Zelfs ik heb moeten denken hoe je dit gemakkelijk en snel
 zou kunnen oplossen. Door een truukje gebruik ik maar twee
 hulppointers in plaats van drie (ik gebruik de Next van
 Temp1 omdat die toch niet wordt gebruikt). Wat bij dit
 soort procedures lastig is zijn niet de normale gevallen
 (een naam middenin de lijst verwijderen), maar de bijzondere
 gevallen (een naam in het begin of een naam op het einde
 verwijderen). Probeer dit ook altijd goed uit als je zoiets
 maakt, zodat je niet voor verassingen komt te staan.
 Enjoy:

PROCEDURE Min(VAR Main:Point);

  VAR
    Temp1 : Point;
    Temp2 : Point;

  BEGIN            (* Min *)
    New(Temp1);
    ClrScr;
    Writeln('Deleting an address from the list');
    Writeln;
    Writeln;
    Writeln('Give a name (RETURN is stop)');
    Writeln;
    Write('Give name : ');
    Readln(Temp1^.Name);
    IF Temp1^.Name<>'' THEN
    BEGIN
      Temp2:=Main;
      IF Main^.Name=Temp1^.Name THEN
      BEGIN
        Main:=Main^.Next;
        Dispose(Temp2);
      END
      ELSE
      BEGIN
        WHILE Temp2<>NIL DO
          IF Temp2^.Name<>Temp1^.Name THEN
          BEGIN
            Temp1^.Next:=Temp2;
            Temp2:=Temp2^.Next;
          END
          ELSE
          BEGIN
            Temp1^.Next^.Next:=Temp2^.Next;
            Dispose(Temp2);
            Temp2:=NIL;
          END;
        END;
    END;
    Dispose(Temp1);
  END;             (* Min *)

 Tja, moet ik dit nog bespreken? Dacht het niet :

PROCEDURE Saven(Main:Point;VAR Names:Text);

  VAR
    Temp : Point;

  BEGIN            (* Saven *)
    Assign(Names,'adres.txt');
    Temp:=Main;
    Rewrite(Names);
    WHILE Temp<>NIL DO
    BEGIN
      Writeln(Names,Temp^.Name);
      Writeln(Names,Temp^.Address);
      Temp:=Temp^.Next;
    END;
    Close(Names);
  END;             (* Saven *)

 Hier wordt natuurlijk alles samengevoegd, maar dat het je al
 begrepen:

  BEGIN            (* MainProgramm *)
    New(Main);
    Main:=NIL;
    welcome;
    REPEAT
      menu(choise);
      CASE Choice OF
        1 : Loading(Main,Names);
        2 : Adding(Main);
        3 : Print(Main);
        4 : Min(Main);
        5 : Saven(Main,Names);
      END;
    UNTIL choise=6;
    Temp:=Main;
    WHILE Main<>NIL DO
    BEGIN
      Main:=Main^.Next;
      Dispose(Temp);
      Temp:=Main;
    END;
  END.             (* MainProgramm *)

 Dat was het. Weinig commentaar van mij dit keer, maar dat komt
 omdat je dit zelf moet uitzoeken. Het programma werkt (ik heb
 het uitvoerig getest) en dus kun je hier kijken hoe het moet.
 Wat je moet doen om zelf meer te leren is het programma
 aanpassen (daar heb ik het voor gemaakt). Dit programma bevat
 alleen maar de namen en adressen (dat adres is niet compleet)
 en moet dus eigenlijk uitgebreid worden met postcode,
 woonplaats en telefoonnummer. Vandaar de volgende opdrachten:

 (1) Verbeter de invoerroutines, zodat een en ander mooier
     uitziet als er een fout wordt gemaakt.
 (2) Breid het programma uit, zodat ook de postcode,
     woonplaats en het telefoonnummer van de betrokkene
     ingevoerd en bewaard kan worden.
 (3) Sorteer de gegevens alfabetisch (Tip : maak gebruik van
     een bubblesort algoritme, dat is het gemakkelijkst bij
     pointers).

 Smael, wat is bubblesort? Bubblesort is een sorteerroutine.
 Hoe werkt dat? Als volgt :
  (1) X <- 1
  (2) Vergelijk element X en X+1 met elkaar
  (3) ALS element X+1 < element X DAN
        wissel element X en element X+1
  (4) X <- X+1
  (5) ALS nog elementen DAN
        ga naar stap 2
  (6) ALS gewisseld (tijdens ��n doorgang) DAN
        ga naar stap 1
 En gesorteerd is die hap.

 Bubblesort werkt dus met doorgangen. Per doorgang zorg je
 ervoor dat minimaal 1 element op de goede plaats komt (je
 schuift dat element telkens voor je uit). Na ��n doorgang heb
 je dus alle elementen minimaal ��n maal bekeken. Zolang je dus
 wisselt (al is het maar ��n keer per doorgang) ben je nog niet
 klaar met sorteren. Dit lijkt niet effici�nt, maar is het wel
 (bubblesort is ongeveer 5 keer sneller dan de ouderwetse
 sorteermethode met de twee FOR-NEXT loops) en daarbij is het
 ook nog eens het gemakkelijkst bij pointers (hoe wil je dat
 met twee FOR-NEXT loops oplossen?).

 Zo, dat was het. Tot de volgende keer, dan gaan we het hebben
 over recursie (en dat is pas ECHT leuk). Groetjes,


Red XIII
