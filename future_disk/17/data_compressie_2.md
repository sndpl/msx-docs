---- Data Compressie (2) ----

 ����   Er wordt dit keer begonnen met het echtere werk.
 ����   De aflevering van de vorige keer was een inleiding
 ����   op hetgene wat ik in de volgende cursi zal behandelen.
 ����


 Allereerst beginnen we nog eens met de grondregel van
 datacompressie: we willen een groot geheel als een kleiner
 geheel opslaan.

 Om dit te doen kennen we verschillende methoden. Dit keer
 beginnen we met de makkelijkste methode:

     ---- RUNLENGTH ----

 Falco en Ivo Wubbels schreven er al over en plaatsten een
 source in de MCCM die wel erg 'basic' was. Daarom zullen we
 proberen een zo groot mogelijke compressie te behalen
 met behulp van Runlength-encoding. Maar eerst wat meer over
 Runlength.

---- Wat is RUNLENGTH ??? ----

 Runlength is een compressie-methode die ervan uitgaat dat er
 lange reeksen van gelijke data-elementen in een bestand
 voorkomen. Deze reeksen verkleint runlength door de
 achtereenvolgende gelijke data-elementen te tellen en ze als
 [Waarde] + [Aantal keer opeenvolgend] op te slaan.

---- RUNLENGTH ALGORITHME ----

 Om dit te verduidelijken, kijken we eens naar het volgende
 voorbeeld, dat de algorithme voor het opslaan van runlength
 voorstelt:

 [0]    TELLER = 1;
 [1]    LEES DATAELEMENT;
 [2]    {X} <- VOLGENDE DATAELEMENT IN BESTAND;
 [3]    IS DATAELEMENT GELIJK {X} ?;

        (JA: VERHOOG TELLER;
             GA NAAR [2];
        )
        (NEE: STUUR DE WAARDE VAN HET DATAELEMENT + AANTAL
              KEER VOORKOMEN NAAR DE CODE;
              GA NAAR [0];
        )

                   ---- SOURCE LISTING ----

 Met deze pseudo-code kunnen we makkelijk een assembly-
 listing maken:

 ; RUNLENGTH
 ; (c) 1994 FutureDisk

 DATA:          EQU     #A000
 CODE:          EQU     #B000

                ORG     #C000

                LD      C,1             ;C = teller

                LD      HL,DATA         ;orginele data
                LD      DE,CODE         ;output opslag
 OPNIEW:
                LD      A,(HL)          ;lees eerste data-
                                        ;element
 LOOP:
                INC     HL
                CP      (HL)            ;vergelijk volgende
                                        ;element
                JP      NZ,NGELYK
                INC     C               ;gelijk: verhoog de
                                        ;teller
                JP      LOOP            ;onderzoek volgende
                                        ;element
 NGELYK:
                LD      (DE),A          ;sla het data-element
                                        ;op
                INC     DE
                LD      A,C
                LD      (DE),A          ;sla de teller op
                INC     DE
                LD      C,1             ;reset de teller
                JP      OPNIEW          ;neem ongelijke
                                        ;element als nieuw
                                        ;data-element.
                END


 Zoals je al ziet gaat deze routine eeuwig door. Dit is
 natuurlijk makkelijk te verhelpen door bij elke verhoging
 van HL te bekijken of deze is aangekomen bij het einde van
 de te comprimeren reeks.

---- UITGEWERKT VOORBEELD ----

 Nu de algoritme en een kleine source-listing gemaakt zijn
 gaan we eens kijken naar een klein getallen voorbeeldje:
 Onze originele data ziet er zo uit: 'AAABBACCCDDDD'
 Gaan we stap voor stap door de algoritme. Als eerste
 data-element nemen we de A. We vergelijken die met het
 volgende element. Alweer een A --> we verhogen de teller en
 bekijken het volgende element, weer A --> teller+1 en bekijk
 volgende element, een B. We slaan nu het data-element A op
 en plaatsen daarachter de teller. De code is nu 'A3'. We
 nemen nu de B als data-element, zetten de teller weer op 1 en
 vergelijken B met het volgende element --> weer een B,
 vergelijken met het volgende element --> een A. We slaan B op
 en daarachter de teller. De code is nu 'A3B2'. We zetten de
 teller op 1, nemen A als data-element en vergelijken met de
 volgende --> een C. We slaan de A op, daarachter de teller,
 code is 'A3B2A1', zetten teller op 1 en nemen C als
 data-element. C komt 3 keer voor en D nog 4 maal, dus de
 code wordt 'A3B2A1C3D4'. Je ziet dat er een kleine
 compressie is behaald. Toch komt in dit voorbeeld ook al 1
 van de nadelen van runlength boven water, namelijk het
 opslaan van de A tussen de B en de C. Deze A komt maar 1
 keer in het orgineel voor, maar neemt twee elementen in in
 de code, namelijk A en 1.  Op zich hoeft dit nog geen
 probleem te zijn, maar zodra we een bestand krijgen waarin
 (bijna) geen opvolging van gelijke elementen zit, zal de
 compressie niet goed werken, sterker nog, zal negatief gaan
 werken. Het resultaat wordt namelijk groter dan het
 orgineel. Gelukkig is hier een, niet zo moeilijke, oplossing
 voor te vinden, waardoor runlength toch een redelijke
 compressie-methode blijft. De volgende keer gaan we hier
 echter verder op in.

   ---- VOLGENDE KEER ----

 Volgende keer volmaken we dus het runlength-encoden en
 behandelen het decoden. Ook maken we dan spelenderwijs een
 runlength encoder-decoder om grafische schermen mee te
 comprimeren.

         Jan-Willem van Helden

