
                           PASCAL 5
                           ========

 ����  Alweer deel 5 van deze cursus! Wat gaat de tijd toch
 ����  snel. Deze keer een kleine cursus, omdat het over
 ����  een gemakkelijk onderwerp handeld. Deze cursus
 ����  behandeld namelijk het maken van eigen types.

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

   Dag:=Maandag;

 Maar niet

   Write("Geef dag : ");
   Readln(Dag);

 De type's verschillen immers. Bij Readln lees je een integer,
 real of string, maar niet een DagenVanDeWeek.

 Wat ook niet kan is

   Dag:=Maandag;
   Writeln("Het is vandaag ",Dag);

 En wel om dezelde reden als bij Readln. Wat wel kan is

   Dag:=Maandag;
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

 Zo, dit was kort maar krachtig(NvdR: geloof je dat nou
 zelf?).

 Jeroen "Ik beloof beterschap" Smael

                                                 C YA @!!
