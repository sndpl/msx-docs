Hier volgt alweer deel 5 van mijn PASCAL cursus. Ik hoop dat mensen het leuk vinden dat ik dit doe, anders kan ik beter ophouden.
            PASCAL 5
            ========


 ����  Alweer deel 5 van deze cursus! Wat gaat de tijd toch
 ����  snel(NvdR: snif). Deze keer een kleine cursus, omdat het
 ����  over een gemakkelijk onderwerp handelt. Deze cursus
 ����  behandelt namelijk het maken van eigen types.

 Bij het declareren van een variabele konden we tot nog toe
 alleen maar kiezen uit de standaard type's (integer, real,
 boolean, etc.). In PASCAL is het echter ook mogelijk om je
 eigen type's te maken en wel als volgt:

   TYPE
     OpsomType    = {Item1,Item2,Item3};
     SubrangeType = 1..10;
     ArrayType    = ARRAY[1..10] OF AnderType;

 Je kunt dus een opsommingstype maken bijvoorbeeld:

   DagenVanDeWeek = {Maandag,Dindag,Woensdag,Donderdag,Vrijdag,
                     Zaterdag,Zondag}

 Of een subrangetype (van elk ander type). Bijvoorbeeld:

   DagenVanDeMaand = 1..31  (* integers dus *)

 Of een ARRAY type. Bijvoorbeeld:

   WekenVanEenJaar = ARRAY[1..52] OF DagenVanDeWeek

 Vervolgens kun je de types gebruiken, maar let wel goed op met
 toekenningen e.d. Neem bijvoorbeeld het volgende:

   VAR
     Dag : DagenVanDeWeek;

 Dan mag

   Dag := Maandag;

 Maar niet

   Write("Geef dag : ");
   Readln(Dag);

 De type's verschillen immers. Bij Readln lees je een integer,
 real of string, maar niet een DagenVanDeWeek.

 Wat ook niet kan is

   Dag := Maandag;
   Writeln("Het is vandaag ",Dag);

 En wel om dezelde reden als bij Readln. Wat wel kan is

   Dag := Maandag;
   CASE Dag OF
     Maandag : Write("Maandag");
     Dinsdag : write("Dinsdag");
     .
     .
     .

 Experimenteer maar eens een beetje met het cre�ren van eigen
 type's en je zult zien dat dit heel handig kan zijn.

 Een functie die een beetje met deze type declaratie te maken
 heeft is de IN functie. Deze werkt als volgt:

   IF Dag IN {Maandag,Dinsdag,Woensdag,Donderdag,Vrijdag} THEN
     writeln("Dit is een werkdag");

 Je kunt dus met IN kijken of de waarde van een variabele in
 een gegeven verzameling zit. Die verzameling moet je wel eerst
 opsommen, maar dat is een kleine prijs om te betalen tegenover
 (in dit geval) 5 IF statements.

 Zo, dit was kort maar krachtig(NvdR: tja).

 Jeroen "Ik beloof beterschap" Smael
 En de Pascal cursus gaat verder...
            PASCAL 6
            ========


 ����  Tja, vorige keer was ik er niet helemaal met mijn koppie
 ����  bij, want ik heb een deel van wat ik moest vertellen
 ����  vergeten(NvdR: de eikel!). Ik heb verteld dat je eigen
 ����  types kunt maken, maar ��n van de belangrijkste types ben
       ik vergeten. Ik ben namelijk het RECORD en de string
 vergeten. Hierbij dan ook mijn excuses en de aanvullende
 informatie.


              TYPE

 Over de string kan ik heel snel zijn. De string is een "PACKED
 ARRAY[1..?] of char". Een PACKED ARRAY is een array waarvan de
 lengte intern wordt bijgehouden. Iets wat bij een string
 noodzakelijk is...

 PROGRAM StringVoorbeeld;

    TYPE
      Woord = PACKED ARRAY[1..10] OF char;

    VAR
      A : Woord;
      B : Woord;

    BEGIN          (* StringVoorbeeld *)
      A:='hallo';
      B:=A;
      Writeln(A);
      Writeln(B);
    END.           (* StringVoorbeeld *)

 Het bovenstaande programma lijkt goed (A:=B mag bij arrays,
 zolang de arrays van het zelfde type en even groot zijn), maar
 er zal twee keer het woord 'hallo' verschijnen, met daarachter
 5 ongewilde karakters (mocht het zo zijn dat het programma
 niet al een foutmelding geeft bij de opdracht A:='hallo',
 omdat dat niet voldoet aan de voorwaarde van dezelfde grootte).
 Bij een PACKED ARRAY zoekt de computer zelf uit hoe groot het
 ARRAY is en lost alle genoemde problemen dus op. Bij een
 aantal compilers is het mogelijk om een type te gebruiken dat
 "string" heet. Genoemd type wordt gebruikt bij TYPE met
 "woord = string[10]" of bij VAR met "A : string[10]".

 Dat ik RECORD's vergeten ben is veel erger, omdat daar nou
 juist de meeste leuke dingen mee gedaan kunnen worden. Een
 klein voorbeeldje:


 PROGRAM RecordVoorbeeld;

    TYPE
      datum = RECORD
                dag   : 1..31;
                maand : 1..12;
                jaar  : 0..5000;
              END;
      persoonsgegevens = RECORD
                           naam : string[30];    (* ik maak
                                                    gebruik van
                                                    het string
                                                    type *)
                           geboortedatum : datum;
                         END;

    VAR
      persoon : persoonsgegevens;

    BEGIN          (* RecordVoorbeeld *)
      persoon.naam:='Jeroen Smael';
      persoon.geboortedatum.dag:=27;
      persoon.geboortedatum.maand:=10;
      persoon.geboortedatum.jaar:=1970;
    END.           (* RecordVoorbeeld *)


 Op deze manier kun je dus hele "gegevensbomen" aanleggen. Bij
 de persoonsgegevens kun je ook nog het adres, de geboortestad,
 etc., etc. toevoegen. Alles wat je maar wilt. Zoals je ziet
 moet je wel, om bij de velden te komen een punt gebruiken.
 Deze punt is vergelijkbaar met de \ van DOS (het scheidings-
 teken van subdirectory's). Tevens zie je dat ik pas jarig ben
 geweest, maar dat terzijde(NvdR: inderdaad, dat boeit ons,
 ja).

 Met genoemde velden zijn verder natuurlijk wel alle
 bewerkingen mogelijk,  zo zou de opdracht:
 "persoon.geboortejaar:=persoon.geboortejaar+1" een geldige
 opdracht zijn.

 Een hele bruikbare "opdracht" bij records is de CASE opdracht.
 Deze maakt het mogelijk om ��n of meerdere veldnamen weg te
 laten. Met gebruikmaking van de CASE opdracht zou het vorige
 (hoofd)programma er als volgt hebben uitgezien:

    BEGIN          (* RecordVoorbeeld *)
      CASE Persoon OF
        naam:='Jeroen Smael';
        geboortedatum.dag:=27;
        geboortedatum.maand:=10;
        geboortedatum.jaar:=1970;
      END;
    END.           (* RecordVoorbeeld *)

 Hetzelfde (de CASE) kunnen we natuurlijk ook toepassen bij de
 geboortedatum en dan ziet het (hoofd)programma er als volgt
 uit:

    BEGIN          (* RecordVoorbeeld *)
      CASE Persoon OF
        naam:='Jeroen Smael';
        CASE geboortedatum OF
          dag:=27;
          maand:=10;
          jaar:=1970;
        END;
      END;
    END.           (* RecordVoorbeeld *)

 Zoals te zien spaart dit een heleboel tikken. Let er
 natuurlijk wel op dat de CASE afgesloten wordt door een END
 (anders krijg je een compiler fout).


             Files

 Het zou uiteraard ook leuk zijn als we met PASCAL toegang
 zouden hebben tot de diskette. Gelukkig hebben de makers
 daaraan gedacht en kan dat. Voor de beeldvorming zal ik
 uitleggen hoe PASCAL een file "ziet".

 Een file is in PASCAL een lange keten gegevens. De gegevens
 zitten (en zullen ten eeuwige dagen blijven zitten) in de
 volgorde waarin ze zijn weggeschreven. Bij het werken met een
 file schuift er een "window" over de file, waardoor altijd
 maar ��n gegeven per keer "zichtbaar" is. Het genoemde window
 heeft trouwens de onprettige eigenschap dat het alleen maar
 vooruit kan schuiven. Ophalen en wegschrijven van waardes
 gebeurt via het window. Het window is te bereiken met "File^"
 (waar File de naam van de filevariabele is). Ik zal nu wat
 opdrachten bespreken die mogelijk zijn met files, waarna nog
 een voorbeeld programma volgt. Opdrachten zijn:

 RESET(File)
    Opent een file zodat er in gelezen kan worden (zet het
    window aan het begin van de file).
 REWRITE(File)
    Opent de file en maakt hem geschikt om in te schrijven
    (file is nu leeg, ook al was hij dat niet!!).
 GET(File)
    Schuift het window een plaats op (de nieuwe waarde kan met
    "Waarde:=File^" opgevraagd worden).
 PUT(File)
    Zet de waarde van het window in de file en schuift het
    window een plaats op (de window waarde kan veranderd worden
    met "File^:=Waarde").
 READ(File,Waarde)
    Leest uit de file en plaats de gelezen waarde in "Waarde"
    (geen geklooi meer met het window, hier gebeurt dat
    allemaal automatisch).
 READLN(File,Waarde)
    Idem als READ, maar leest tevens tot het einde van de regel
    (wordt gebruikt bij tekstfiles).
 WRITE(File,Waarde)
    Schrijft de waarde van "Waarde" in de file en schuift het
    window door (ook hier geen geklooi meer met het window).
 WRITELN(File,Waarde)
    Idem als WRITE, maar plaatst tevens een EOLN (End OF LiNe)
    teken in de file (wederom bij tekstfiles).
 EOF(File)
    Geeft de waarde TRUE als het einde van de file bereikt is
    (EOF = End Of File).
 EOLN(File)
    Geeft de waarde TRUE als het einde van de regel bereikt is
    (EOLN = End Of LiNe).

 Offici�el zijn genoemde opdrachten alle opdrachten voor files,
 maar de meeste compilers hebben nog twee extra opdrachten, en
 dat zijn:

 ASSIGN(File,"FileNaam")
    Dit koppelt de "File" aan de "FileNaam". M.a.w. Als ik iets
    in de file schrijf, dan komt dit op de schijf in de file
    "FileNaam" te staan. Volgens de regels van PASCAL is dit
    niet nodig, omdat de fysieke file (de file op schijf)
    dezelfde naam krijgt als de variabele "File" heeft. Veel
    compilers hebben echter een ASSIGN opdracht nodig, anders
    gebeurt er niets (vaak niet eens een foutmelding en al
    helemaal geen file).
 CLOSE(File)
    Dit moge wel duidelijk zijn. Hier wordt de file gesloten.
    Deze opdracht zorgt ervoor dat de file ook voor DOS
    gesloten wordt, zodat er niet een ge-opende file achter-
    blijft als het programma stopt. Deze opdracht moet dus
    alleen op het einde van het programma staan. Ook deze
    opdracht is geen standaard, maar moet ook bij de meeste
    compilers worden toegevoegd voor een goede werking.

 Zo, en dan nu een voorbeeldje:

 PROGRAM FileVoorbeeld;

    TYPE
      FileType = FILE OF integer;      (* FILE OF AnyType is
                                          mogelijk *)

    VAR
      FileVar : FileType;
      Teller  : integer;

    BEGIN          (* FileVoorbeeld *)
      ASSIGN(FileVar,"testfile.dat");  (* voor de zekerheid *)
      REWRITE(FileVar);                (* ik wil er iets
                                          inschrijven *)
      FOR Teller:=1 TO 5000 DO
        Write(FileVar,Teller);         (* dat doe ik hier *)
      FOR Teller:=5000 DOWNTO 1 DO
        Write(FileVar,Teller);         (* en hier ook *)
      RESET(FileVar);                  (* nu wil ik lezen uit
                                          de file *)
      WHILE NOT EOF(FileVar) DO
      BEGIN
        Read(FileVar,Teller);          (* hier noem ik
                                          expliciet de file *)
        Write(Teller);                 (* hier niet, dus gaat
                                          de output naar het
                                          standaard uitvoerap-
                                          paraat en dat is het
                                          beeldscherm *)
      END;
      CLOSE(FileVar);                  (* voor de zekerheid *)
    END.           (* FileVoorbeeld *)

 Dit voorbeeld zou een heleboel duidelijk moeten maken. Alles
 wat je nu nog moet doen is verder experimenteren. Probeer eens
 een tekstfile te lezen en/of schrijven, dan kun je zien wat
 EOLN precies doet.
 De opdrachten GET en PUT worden weinig gebruikt, maar je kunt
 eens kijken hoe ze precies werken (goed voor de beeldvorming).

 Zo, dat was het dan alweer voor deze keer. Groetjes van,

               Jeroen 'ik ben nog lang niet aan mijn EOF' Smael

                                                      C YA @!!
