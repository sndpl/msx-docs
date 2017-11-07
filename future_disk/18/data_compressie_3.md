---- Data Compressie (3) ----


 ����   Zoals vorige keer beloofd, ga ik verder met het vol-
 ����   maken van de RUNLENGTH-methode. Of we aan het voor-
 ����   beeld toekomen hangt af van mijn beschikbare tijd en
 ����   de beschikbare diskruimte (NvdR: Ja, schuif maar af!)

 Laten we nog eens het voorbeeld van vorige keer herhalen:
 We hadden de data 'AAABBACCCDDDD'. Na de stappen van het
 RUNLENGTH-algoritme te hebben gevolgd kwamen we tot de
 conclusie dat de gecrunchte data 'A3B2A1C3D4' zou worden.
 Als je het allemaal niet meer helemaal weet, lees het dan
 nog eens na op FD #17.

 Zoals vorige keer al opgemerkt is het element 'A1' storend.
 We slaan immers dit als 2 bytes op (een byte voor 'A' en een
 byte voor het aantal keer dat ie voorkomt '1'), terwijl er
 in het orgineel maar 1 byte voor die 'A' nodig was.
 Als dit maar weinig voorkomt treedt er natuurlijk geen
 probleem op, maar als dit heel vaak gebeurt, is je
 compressie zeer inefficient. Zo is het mogelijk om een 2
 keer zo grote file als output te krijgen.

 Om dit op te lossen kunnen we een extra byte bij de
 crunch-data nemen.

Eeuh, wat ?

 Nou kijk; we gaan nu in ons algoritme opnemen dat een byte
 aangeeft of er crunch-data achterstaan of dat het
 data-element gewoon ongecodeerd is opgeslagen. We noemen
 deze byte even "CMP-ON" (CoMPression-ON). Voor onze
 pseudo-code houdt dat dit in:

 [0]    TELLER = 1;
 [1]    LEES DATAELEMENT;
 [2]    {X} <- VOLGENDE DATAELEMENT IN BESTAND;
 [3]    IS DATAELEMENT GELIJK {X} ?;

        (JA: VERHOOG TELLER;
             GA NAAR [2];
        )
        (NEE:
                [4]     IS TELLER > 3 ?;
                (JA:    STUUR "CMP-ON"-BYTE NAAR DE CODE;
                        STUUR DE WAARDE VAN HET DATAELEMENT +
                        AANTAL KEER VOORKOMEN NAAR DE CODE;
                        GA NAAR [0];
                )
                (NEE:   STUUR TELLER * HET DATAELEMENT NAAR DE
                        CODE;
                        GA NAAR [0];
                )
        )

 Zoals je ziet lijkt het geheel nog steeds op het oude
 voorbeeld. Opvallend is het toevoegen van een extra routine,
 n.l. [4]. Deze routine kijkt of de TELLER groter is dan 3.

Waarom 3 ??

 Logisch, laten we even ervan uitgaan dat we het aantal
 opslaan in een byte, het aantal in een byte en dat de
 "CMP-ON" ook een byte inneemt. Samen maken dit 3 bytes. Als
 we nu een data-element hebben dat maar 1 of 2 keer voorkomt,
 heeft het natuurlijk weinig zin om dit gecomprimeerd op te
 slaan. Dit zou namelijk tot gevolg hebben dat de code 3* zo
 groot zou worden als het origineel. Een nog slechtere
 compressie dan "normale" RUNLENGHT al zou hebben. Vandaar
 deze extra stap [4].

 Oplettende en zeer intelligente lezers hebben al opgemerkt
 dat het programma voor de "CMP-ON" ��n byte gebruikt. Wat
 gebeurt er nu als de waarde van het data-element gelijk zou
 zijn aan de waarde van "CMP-ON"? Voor zowel het encoden als
 decoden zou dit niets uitmaken, als de TELLER > 3 zou zijn.
 Dat werkt gewoon. Als de TELLER <=3 zou zijn zou het encoden
 ook geen probleem opleveren. Het programma zet gewoon 1 of 2
 (of 3, ligt aan de realisatie van je algoritme) bytes met
 de waarde van "CMP-ON" in de code. Echter bij het decoden
 de hele heisa fout gaan lopen. Kijken we maar eens wat er
 gebeurt:

 Stel onze code is dit:

                [CMP-ON],  "A" , "CMP-ON","A","40"
                    \       \                  \
 byte waarde "CMP-ON"       \              frequentie
                            \
                    ongecodeerde byte

 Pakken we dat eens uit: Eerst vindt de decoder een byte met
 de waarde "CMP-ON". Helaas herkent de decoder deze niet als
 zijnde een waarde maar leest nu dat dit betekent dat de
 volgende 2 bytes bestaan uit eerst een data-element en
 daarna het aantal keer dat dit element voorkomt. De decoder
 plaatst dan in het geheugen "CMP-ON" * "A"-tekens, terwijl
 bedoeld was om eerst een byte met waarde "CMP-ON", dan een
 byte met waarde "A" en dan een reeks van 40 bytes met waarde
 "A". De decoder zal dus weinig begrijpen van de data.

Hoe lossen we dit op???

 Simpel! In het algoritme moeten we als de TELLER lager is
 dan 3 de clausule opnemen dat een byte met de waarde van
 "CMP-ON" moet worden opgeslagen alsof de TELLER > 3. In
 pseudo code is er dan de aanpassing:

 [1]    TELLER = 1;
  |      | | | | |
 [3]    IS DATAELEMENT GELIJK {X} ?;

        (JA: VERHOOG TELLER;
             GA NAAR [2];
        )
        (NEE:
                [4]     IS TELLER > 3 ?;
                (JA:    STUUR "CMP-ON"-BYTE NAAR DE CODE;
                        STUUR DE WAARDE VAN HET DATAELEMENT +
                        AANTAL KEER VOORKOMEN NAAR DE CODE;
                        GA NAAR [0];
                )
                (NEE:
                        [5]     IS DATAELEMENT = "CMP-ON" ?;
                        (JA:    GA NAAR DE AFHANDELING VOOR
                                TELLER > 3;
                        )
                        (NEE:   STUUR TELLER * HET DATAELEMENT
                                NAAR DE CODE;
                        GA NAAR [0];
                )
        )


 Nu werkt dus het gehele encoden en decoden goed, maar treedt
 er nog steeds soms negatieve compressie op. Immers als je
 net een aantal keer "CMP-ON" afgewisseld met andere bytes
 tegenkomt in je data, dan slaat de encoder als deze
 "CMP-ON"'s op als 3 bytes.

LEES VERDER IN TEKST 2!

 ---- Data Compressie 3-2 ----


....VERVOLG

 Het moge duidelijk zijn dat de waarde "CMP-ON" moet worden
 toegekend aan een byte die weinig in onze data voorkomt.
 Om dus maximale compressie uit RUNLENGHT te halen kunnen we
 het beste gaan zoeken naar een byte die zo weinig mogelijk
 voorkomt. Dit houdt in dat het begin van het programma een
 beetje veranderd:

 [0]    ZOEK MINST VOORKOMENDE WAARDE;
 [1]    etc. etc.

 Deze stap [0] bestaat ook weer uit een aantal stappen, die
 we wat dieper belichten. Eigenlijk is dit gewoon een
 algoritme om de minst voorkomende byte te zoeken, maar
 aangezien ik helaas niks over de standaard-vormen van zulke
 algoritmen heb kunnen vinden heb ik er zelf maar een
 ontwikkeld. Het werkt wel, maar zal waarschijnlijk sneller
 en beter kunnen.
 Werken we stap [0] eens uit:

 [0]    ZOEK MINST VOORKOMENDE WAARDE
        (
        [1]     BYTE-WAARDE = 0;
                AANTAL = 0;
        [2]     LAATSTE DATA-ELEMENT?;
                (JA:    SLA BYTE-WAARDE EN AANTAL OP IN
                        FREQUENTIE-TABEL;
                        BYTE-WAARDE = BYTE-WAARDE + 1;
                        AANTAL = 0;
                [3]     BYTE-WAARDE = 0 ?;
                        (JA:    FREQUENTIE TABEL = AF;
                                END;
                        )
                        (NEE:   GA NAAR [2];
                        )
                )
                (NEE:
                        [4]     LEES DATA-ELEMENT;
                        [5]     IS DATA-ELEMENT = BYTEWAARDE;
                        (JA:    AANTAL = AANTAL + 1;
                                GA NAAR [2];
                        )
                        (NEE:   GA NAAR [2];
                        )
                )
        )

 Je ziet dat het algorithme iets omslachtig is en ver is
 uitgewerkt, maar door dit uit te voeren hebben we een
 frequentie-tabel. Vervolgens hebben we nog een routine nodig
 die het minst voorkomende element eruit vist, dus:


        (
        [1]     FREQUENTIE = 0;
                TABELPOINTER = FREQUENTIE-TABEL
        [2]     LEES AANTAL
        [3]     IS FREQUENTIE = AANTAL?;
                (JA:    "CMP-ON" = BYTE-WAARDE";
                        RETURN;
                )
                (NEE:   TABELPOINTER = TABELPOINTER + 1;
                [4]     EINDE FREQUENTIE-TABEL?;
                        (JA:    TABELPOINTER=FREQUENTIE-TABEL;
                                FREQUENTIE = FREQUENTIE + 1;
                                GA NAAR [2];
                        )
                        (NEE:   GA NAAR [2];
                )
        )

 Deze routine werkt ook heel simpel: Eerst zoeken we of er
 een AANTAL is dat gelijk is aan FREQUENTIE = 0. Vinden we
 dit, dan maken we "CMP-ON" gelijk aan de bijbehorende
 BYTE-WAARDE. Vinden we in de gehele tabel geen enkel AANTAL
 dat gelijk is aan FREQUENTIE, dan verhogen we de FREQUENTIE
 1 en zoeken we weer vanvoorafaan. Dit doen we net zolang
 totdat we een kloppende waarde gevonden hebben. Deze
 BYTE-WAARDE dan de minst voorkomende.
 Voor de gehele routine houdt dit in:

 -- Complete Source --

 [0]    ZOEK MINST VOORKOMENDE WAARDE
 /* Stel de frequentie tabel op */
        (
        [1]     BYTE-WAARDE = 0;
                AANTAL = 0;
        [2]     LAATSTE DATA-ELEMENT?;
                (JA:    SLA BYTE-WAARDE EN AANTAL OP IN
                        FREQUENTIE-TABEL;
                        BYTE-WAARDE = BYTE-WAARDE + 1;
                        AANTAL = 0;
                [3]     BYTE-WAARDE = 0 ?;
                        (JA:    FREQUENTIE TABEL = AF;
                                END;
                        )
                        (NEE:   GA NAAR [2];
                        )
                )
                (NEE:
                        [4]     LEES DATA-ELEMENT;
                        [5]     IS DATA-ELEMENT = BYTEWAARDE;
                        (JA:    AANTAL = AANTAL + 1;
                                GA NAAR [2];
                        )
                        (NEE:   GA NAAR [2];
                        )
                )
        )
 /* Zoek naar minst voorkomende waarde */
        (
        [1]     FREQUENTIE = 0;
                TABELPOINTER = FREQUENTIE-TABEL
        [2]     LEES AANTAL
        [3]     IS FREQUENTIE = AANTAL?;
                (JA:    "CMP-ON" = BYTE-WAARDE";
                        RETURN;
                )
                (NEE:   TABELPOINTER = TABELPOINTER + 1;
                [4]     EINDE FREQUENTIE-TABEL?;
                        (JA:    TABELPOINTER=FREQUENTIE-TABEL;
                                FREQUENTIE = FREQUENTIE + 1;
                                GA NAAR [2];
                        )
                        (NEE:   GA NAAR [2];
                )
        )

 /* Begin het echte comprimeren */

 [1]    TELLER = 1;
 [2]    LEES DATAELEMENT;
 [3]    {X} <- VOLGENDE DATAELEMENT IN BESTAND;
 [4]    IS DATAELEMENT GELIJK {X} ?;

        (JA: VERHOOG TELLER;
             GA NAAR [3];
        )
        (NEE:
                [5]     IS TELLER > 3 ?;
                (JA:    STUUR "CMP-ON"-BYTE NAAR DE CODE;
                        STUUR DE WAARDE VAN HET DATAELEMENT +
                        AANTAL KEER VOORKOMEN NAAR DE CODE;
                        GA NAAR [1];
                )
                (NEE:   STUUR TELLER * HET DATAELEMENT NAAR DE
                        CODE;
                        GA NAAR [1];
                )
        )

 -- Next Time: --

 Zo, nu hebben we een geheel algoritme voor maximale
 RUNLENGTH compressie. Het lijkt wat geknutseld, maar werkt
 wel. Volgende keer maken we van dit algoritme een mooi
 code-programma en als de tijd het toelaat, maken we een
 begin met de LZW-methode om data te comprimeren.

                                Jan-Willem van Helden

 p.s.   Voor de al wat verder gevorderden onder ons, de
        pseudo-code die ik gebruik, lijkt verdacht veel op de
        hogere programmeer-talen. Daar is die dan ook van af-
        geleid. Als je nu een beetje handig bent, kun je deze
        routines zo omzetten naar C (+/++/+++) of PASCAL
        omzetten. Wie weet besteed ik daar ook nog eens wat
        textjes aan.
