          PASCAL (8)


 ����  Hallie hallo, hier is Smael weer met zijn PASCAL cursus.
 ����  Deel 8 deze keer (nog twee en dan is het alweer
 ����  voorbij). In deel 8 gaan we dieper in op pointers. 
 ����  Vorige keer heb ik uitgelegd wat pointers zijn en wat je 
       ermee kunt. Nu is het dus tijd voor een groot voorbeeld. 
 Als voorbeeld neem ik een programma om namen en adressen op te 
 slaan (een soort database dus). Goed, daar gaat 'ie (wat 
 hebben we weer een plezier)!!(NvdR: ja nou)

 Het programma staat op de disk als 'PASCAL8.PAS'. De listing 
 vind je terug in deze tekst, maar alleen ter verduidelijking.

 De declaraties veronderstel ik bekend (moet je kunnen) en daar 
 ga ik dus niet verder op in. Wel staan ze hier even vermeld, 
 zodat je de rest van het programma begrijpt:

  PROGRAM Pascal8(Input,Output,Namen);

    CONST
      Max = 30;

    TYPE
      Wijs = ^Rij;
      Rij  = RECORD
               Naam  : String[Max];
               Adres : String[Max];
               Next  : Wijs;
             END;

    VAR
      Namen  : Text;
      Keuze  : integer;
      Hoofd  : Wijs;
      Hulp   : Wijs;
      Aantal : integer;

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

  PROCEDURE welkom;

    BEGIN          (* welkom *)
      clrscr;
      writeln('Dit is een voorbeeldprogramma bij FD #19');
      writeln;
      writeln('Dit programma maakt het mogelijk een');
      writeln('adressenbestand op te bouwen.');
      writeln('Het is een voorbeeld dat gebruik maakt');
      writeln('van pointers.');
      writeln;
      writeln;
      writeln('Enjoy !!');
      writeln;
      writeln;
      writeln;
      write('Druk <RETURN> om verder te gaan.');
      readln;
    END;           (* welkom *)

 Ook deze procedure moet gesneden koek zijn (alleen maar wat
 Writeln's en Readln's). Wederom kun je een en ander 
 verbeteren. Als je namelijk een verkeerde keuze maakt, dan 
 wordt op de volgende regel opnieuw naar je keuze gevraagd. 
 Maak je ongeveer 20 keer de verkeerde keuze, dan is het 
 keuzemenu niet meer te zien (uit het beeld geschoven) en dat 
 is niet zo netjes. Read it and weep :

  PROCEDURE menu(VAR keuze:integer);

    BEGIN          (* menu *)
      clrscr;
      writeln('Je kunt kiezen uit :');
      writeln;
      writeln('(1) Bestand inladen');
      writeln('(2) Persoon invoeren');
      writeln('(3) Overzicht personen');
      writeln('(4) Persoon verwijderen');
      writeln('(5) Bestand opslaan');
      writeln('(6) Stoppen');
      writeln;
      writeln;
      writeln;
      keuze:=0;
      REPEAT
        write('Uw keuze : ');
        readln(keuze);
      UNTIL (keuze>=1) AND (keuze<=6);
    END;           (* menu *)

 Hier wordt het interessant. Er wordt een file ingelezen van
 namen die ooit al eens zijn ingevoerd. Deze heb ik lekker
 gemakkelijk gehouden. Vergelijk hem maar eens met de SAVE 
 procedure en je komt tot de ontdekking dat de namen omgekeerd 
 ingelezen worden. Wat bedoel ik? Als de namen in alfabetische 
 volgorde zijn weggeschreven (als voorbeeld), dan worden ze 
 anti-alfabetisch (dus net omgekeerd van Z naar A) ingelezen. 
 Let ook op de VAR (in de procedure-kop) bij Hoofd en Namen. 
 Bij Namen is het altijd verplicht (een file kan niet in het 
 geheugen gekopieerd worden), maar bij Hoofd zou ik het weg 
 kunnen laten. Of niet (een pointer kan in het geheugen 
 gekopieerd worden)? In dit geval niet!!

 Als er al ��n naam (of meerdere) ingevoerd waren, dan kon ik 
 het weglaten (moet de rest wel achteraan toegevoegd worden) 
 aangezien Hoofd dan niet meer hoeft te veranderen (let vooral 
 op de voorgaande opmerking). Nu wordt elke nieuwe naam vooraan 
 in de lijst bijgeschoven en dus veranderd Hoofd bij elke naam. 
 Conclusie : VAR kan niet weggelaten worden (anders ben je je 
 namen kwijt als je terugkeert naar het hoofdprogramma).

 Let ook op het gebruik van de EOF functie. Ik hou er rekening 
 mee dat een file verminkt is. Ik zou kunnen volstaan met de 
 EOF controle bij de WHILE-lus, maar als de file verminkt is, 
 dan gaat het fout bij het inlezen van het adres. Gevolg is een 
 crash van de computer en dus zijn al je gegevens weg. Let 
 hier op bij het programmeren van een grote toepassing. Let
 tevens op de 'Close(Namen)' aan het einde van de procedure, 
 deze is geplaatst om problemen te voorkomen indien je na het 
 inlezen het programma be�indigd (er staat immers nog een file
 open). Verder bekend (neem ik aan) :

  PROCEDURE Laden(VAR Hoofd:Wijs;VAR Namen:Text);

    VAR
      Hulp : Wijs;

    BEGIN            (* Laden *)
      Assign(Namen,'adres.txt');
      Reset(Namen);
      WHILE NOT EOF(Namen) DO
      BEGIN
        New(Hulp);
        IF NOT EOF(Namen) THEN
          Readln(Namen,Hulp^.Naam)
        ELSE
          Hulp^.Naam:='';
        IF NOT EOF(Namen) THEN
          Readln(Namen,Hulp^.Adres)
        ELSE
          Hulp^.Adres:='';
        Hulp^.Next:=Hoofd;
        Hoofd:=Hulp;
      END;
      Close(Namen);
    END;             (* Laden *)

 Ook deze procedure moet geen echte problemen opleveren (zeker
 niet als je de vorige procedure begrepen hebt). Hier worden 
 namen toegevoegd aan de lijst (wederom vooraan). Ik houd er
 rekening mee dat de gebruiker gekke dingen doet, vandaar de 
 dubbele controle op het invoeren van een lege regel. Let ook 
 op de twee Dispose opdrachten op het einde van de procedure!!
 Again, read it and weep :

  PROCEDURE Plus(VAR Hoofd:Wijs);

    VAR
      Hulp : Wijs;

    BEGIN            (* Plus *)
      New(Hulp);
      ClrScr;
      Writeln('Toevoegen van naam aan lijst');
      Writeln;
      Writeln;
      Writeln('Geef naam en adres (RETURN is stoppen).');
      Writeln;
      Write('Geef naam  : ');
      Readln(Hulp^.Naam);
      IF Hulp^.Naam<>'' THEN
      BEGIN
        Write('Geef adres : ');
        Readln(Hulp^.Adres);
        IF Hulp^.Adres<>'' THEN
        BEGIN
          Hulp^.Next:=Hoofd;
          Hoofd:=Hulp;
        END
        ELSE
          Dispose(hulp);
      END
      ELSE
        Dispose(Hulp);
    END;             (* Plus *)

 Aan deze procedure maak ik geen woorden vuil (buiten het feit 
 dat ik je wijs op het handige gebruik van de MOD functie bij 
 de REPEAT-UNTIL):

  PROCEDURE Print(Hoofd:Wijs);

    VAR
      Teller : integer;
      Hulp   : Wijs;

    BEGIN
      Hulp:=Hoofd;
      Teller:=0;
      REPEAT
        ClrScr;
        REPEAT
          IF Hulp<>NIL THEN
          BEGIN
            Writeln(Hulp^.Naam);
            Writeln(Hulp^.Adres);
            Hulp:=Hulp^.Next;
          END
          ELSE
          BEGIN
            Writeln;
            Writeln;
          END;
          Writeln;
          Teller:=Teller+1;
        UNTIL (Teller MOD 7)=0;
        Write('Druk <RETURN> om verder te gaan.');
        Readln;
      UNTIL Hulp=NIL;
    END;             (* Print *)

 Dit is natuurlijk de meest interessante procedure. Je zou hem 
 nu echter (na het lezen van Laden en Plus) zo goed als moeten 
 kunnen dromen. Nee, dat is maar een grapje(NvdR: haha).
 Zelfs ik heb moeten denken hoe je dit gemakkelijk en snel
 zou kunnen oplossen. Door een truukje gebruik ik maar twee
 hulppointers in plaats van drie (ik gebruik de Next van
 Hulp1 omdat die toch niet wordt gebruikt). Wat bij dit
 soort procedures lastig is zijn niet de normale gevallen
 (een naam middenin de lijst verwijderen), maar de bijzondere
 gevallen (een naam in het begin of een naam op het einde
 verwijderen). Probeer dit ook altijd goed uit als je zoiets
 maakt, zodat je niet voor verassingen komt te staan.
 Enjoy:

  PROCEDURE Min(VAR Hoofd:Wijs);

    VAR
      Hulp1 : Wijs;
      Hulp2 : Wijs;

    BEGIN            (* Min *)
      New(Hulp1);
      ClrScr;
      Writeln('Verwijderen van naam uit de lijst');
      Writeln;
      Writeln;
      Writeln('Geef naam (RETURN is stoppen)');
      Writeln;
      Write('Geef naam  : ');
      Readln(Hulp1^.Naam);
      IF Hulp1^.Naam<>'' THEN
      BEGIN
        Hulp2:=Hoofd;
        IF Hoofd^.Naam=Hulp1^.Naam THEN
        BEGIN
          Hoofd:=Hoofd^.Next;
          Dispose(Hulp2);
        END
        ELSE
        BEGIN
          WHILE Hulp2<>NIL DO
            IF Hulp2^.Naam<>Hulp1^.Naam THEN
            BEGIN
              Hulp1^.Next:=Hulp2;
              Hulp2:=Hulp2^.Next;
            END
            ELSE
            BEGIN
              Hulp1^.Next^.Next:=Hulp2^.Next;
              Dispose(Hulp2);
              Hulp2:=NIL;
            END;
        END;
      END;
      Dispose(Hulp1);
    END;             (* Min *)

 Tja, moet ik dit nog bespreken? Dacht het niet :

  PROCEDURE Saven(Hoofd:Wijs;VAR Namen:Text);

    VAR
      Hulp : Wijs;

    BEGIN            (* Saven *)
      Assign(Namen,'adres.txt');
      Hulp:=Hoofd;
      Rewrite(Namen);
      WHILE Hulp<>NIL DO
      BEGIN
        Writeln(Namen,Hulp^.Naam);
        Writeln(Namen,Hulp^.Adres);
        Hulp:=Hulp^.Next;
      END;
      Close(Namen);
    END;             (* Saven *)

 Hier wordt natuurlijk alles samengevoegd, maar dat het je al 
 begrepen:

  BEGIN            (* HOOFDPROGRAMMA *)
    New(Hoofd);
    Hoofd:=NIL;
    welkom;
    REPEAT
      menu(keuze);
      CASE Keuze OF
        1 : Laden(Hoofd,Namen);
        2 : Plus(Hoofd);
        3 : Print(Hoofd);
        4 : Min(Hoofd);
        5 : Saven(Hoofd,Namen);
      END;
    UNTIL keuze=6;
    Hulp:=Hoofd;
    WHILE Hoofd<>NIL DO
    BEGIN
      Hoofd:=Hoofd^.Next;
      Dispose(Hulp);
      Hulp:=Hoofd;
    END;
  END.             (* HOOFDPROGRAMMA *)

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

                         Jeroen 'FD^.Next^.Nummer:=20' Smael

                                                    C YA @!!
