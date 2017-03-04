# Pascal (1)

 Men heeft mij gevraagd om een cursus PASCAL te schrijven. En
 omdat ik PASCAL een goede, gestructureerde en leuke taal
 vind, heb ik ja gezegd.

 Voordat ik ga beginnen met het echte werk, geef ik eerst een
 korte inleiding over wat PASCAL precies inhoudt. PASCAL is
 vernoemd naar de wiskundige Blaise Pascal die van 1623 tot
 1662 leefde. Blaise Pascal was ÇÇn van de grootste wiskundige
 die de wereld gekend heeft. Hij heeft onder andere de
 fundamenten gelegd van het kansrekenen. Maar het bekenste wat
 Blaise Pascal heeft gedaan (buiten zijn bekende driehoek) is
 het constru◊ren van een rekenmachine. Deze machine kon veel
 rekenkundige berekeningen uitvoeren, en dat terwijl het
 geheel alleen maar uit tandwielen en dergelijke bestond
 (probeer je dat eens voor te stellen).

 Rond 1970 dook de naam PASCAL (geheel in hoofdletters) weer
 op. Dit omdat ene Professor Niklaus Wirth het leuk vond om
 zijn zojuist ontwikkelde programeertaal te vernoemen naar
 Blaise Pascal. En deze taal is nu hÇÇl bekend. De meeste
 mensen hebben er ervaring mee, of hebben er wel van gehoord.
 Het voordeel van PASCAL boven BASIC is de gestructureerdheid.
 Is het verplicht bij BASIC om commentaar te plaatsen (zeker
 als iemand anders er iets van moet begrijpen) bij PASCAL wordt
 de helft van de 'documentatie' door de gestructureerdheid van
 de taal zelf verzorgd. Hiermee bedoel ik dat je er bij BASIC
 een spagetti van kunt maken (met behulp van GOTO-instructies)
 die niet te lezen is. Terwijl je bij PASCAL het programma
 veel sneller doorziet door het inspringen (wordt later
 duidelijk), de grotere verscheidenheid aan herhalingslussen
 (WHILE, REPEAT en FOR) en het (bijna) niet voorkomen van
 GOTO-instructies.

 Tot zover de inleiding. Nu gaan we dus echt beginnen. Dat
 echt beginnen houdt in dat ik de globale structuur van een
 PASCAL-programma ga uitleggen.

 De globale structuur van een PASCAL-programma is als volgt :

 PROGRAM programma_naam (INPUT,OUTPUT);

   CONST
     {Constante definitie}

   TYPE
     {Type definitie}

   VAR
     {Variabelen declaratie}

   {Procedure en functie definitie}

   BEGIN
     {Hoofdprogramma}
   END.

 Alle woorden die hierboven met hoofdletters staan geschreven
 zijn gereserveerde woorden. Dat wil zeggen dat deze naam niet
 voor iets anders gebruikt kan worden en dat ze een speciale
 functie hebben in een programma. Als we bovenaan beginnen dan
 komen we als eerste het gereserveerde woord PROGRAM, gevolgd
 door "programma_naam" tegen. Dit geeft voor de PASCAL-
 compiler aan dat hier een programma met de naam
 "programma_naam" begint. Verder volgen er de woorden INPUT en
 OUTPUT (tussen haakjes). Deze woorden geven aan dat dit
 programma gebruik maakt van de standaard in- en uitvoer
 (toetsenbord en beeldscherm). Bij deze twee (gereserveerde)
 woorden kunnen nog andere woorden staan, maar daar komen we
 pas op als we het over file's gaan hebben.

 Na de programma heading volgt een blok waarin constantes,
 types en variabelen worden gedeclareerd/gereserveerd. Het is
 bij PASCAL zo dat je ALLE variabelen die je in het programma
 gebruikt van te voren gedefinieerd (wat is het) en
 gedeclareerd (hoe heet het) moet hebben. Dus niet zoals bij
 BASIC "Oh, ik heb nog een variabele nodig. Laat ik maar iets
 nemen."

 Na dit blok volgt er een blok waarin PROCEDURE's en
 FUNCTION's gedeclareerd worden. Deze functies en procedures
 zijn ook weer bloksgewijs opgebouwd en wel hetzelfde als een
 programma. Dus ook met (functie/procedure) naam (en I/O
 parameters), variabele blok, functie/procedure blok en een
 hoofd blok. Hier zal ik ter zijner tijd nog uitgebreid op
 terugkomen.

 Na al deze voorbereidende declaraties en definities, volgt
 dan het uiteindelijke hoofdprogramma tussen de gereserveerde
 woorden BEGIN en END, gevolgt door een punt. Deze punt is
 voor de compiler het teken dat het programma klaar is.

 Voor deze keer wil ik besluiten met 2 programma's. Het eerste
 programma CD's vind je zowel als tekstfile alsook als
 gecompileerde versie (.COM file) op de FutureDisk. Het tweede
 programma vind je hierbeneden. Bekijk beide programma's eens
 en probeer eens te doorgronden wat ze doen. Bij onderstaand
 programma mag dat geen problemen opleveren, terwijl dat bij
 het programma CD's wel het geval zal zijn (behalve voor de
 echte freaks onder ons).

 In de volgende afleveringen zal ik alle belangrijke elementen
 van PASCAL de revue laten passeren (met veel voorbeelden EN
 opgaves). Daarna zal ik recursie behandelen (ja, dat is heel
 leuk) en als laatste zal ik nog aangeven wat TURBO PASCAL
 precies inhoud (beter gezegt wat kan TURBO PASCAL wat
 (standaard) PASCAL niet kan).

 Hier volgt een voorbeeld programma in PASCAL :
```
 PROGRAM simpel (INPUT,OUTPUT);

   VAR
     Getal_1 : integer;
     Getal_2 : integer;
     Som     : integer;

   PROCEDURE invoer_getal(VAR getal:integer);

     BEGIN          (* invoer_getal *)
       Write('Voer een getal in : ');
       Readln(getal);
       Writeln;
     END;           (* invoer_getal *)

   BEGIN            (* hoofdprogramma *)
     Writeln('Voorbeeld programma bij PASCAL-cursus (1)');
     Writeln('op de FutureDisk.');
     Writeln;
     Writeln('Gemaakt door :');
     Writeln('  Jeroen ''KUBIE'' Smael');
     Writeln('    Galopiahof 6');
     Writeln('      6215TK Maastricht');
     Writeln('        043-437778');
     Writeln;
     Writeln;
     invoer_getal(Getal_1);
     invoer_getal(Getal_2);
     Som:=getal_1+getal_2;
     Writeln(Getal_1:1,' + ',Getal_2:1,' = ',Som:1);
     Writeln;
   END.             (* hoofdprogramma *)
```
Jeroen 'PASCAL is leuk' Smael

 P.S. (1) Veel plezier met het uitpluizen van beide
          programma's.
      (2) Om beide programma's (uberhaupt elk PASCAL-programma)
          te compileren heb je een PASCAL-compiler nodig. ik
          vertrouw er echter op dat iedereen wel zoiets thuis
          heeft liggen (op een oude floppie).
      (3) Voor de echte freaks : Ik behandel alleen standaard
          PASCAL. Het is namelijk een kwestie van een paar
          functies bijleren als je overstapt van PASCAL naar
          TURBO PASCAL.