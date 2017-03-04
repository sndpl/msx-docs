
        MCODE SPECIAL(1)


 Welkom bij deze speciale aflevering van de programmeercursus.
 Dit keer geen gezwam over Z-80 instructies, maar de komplete
 uitleg over hoe een spel in machinetaal geprogrammeerd wordt.
 Hierbij wordt aangenomen dat je de vorige afleveringen van
 deze programmmeercursus allemaal gevolgd hebt, dan wel
 zelf kennis hebt opgedaan over de Z-80 instructies en het
 gebruik daarvan in machinetaal programma's.
 Ik zal een poging doen om zoveeeeel uitleg te geven dat
 iedereen, die interesse heeft deze tekst kan volgen. Mocht
 het een of andere nog niet duideljik zijn: mijn
 telefoonnummer is 04746-1655 (voor 7 uur ben ik normaal niet
 thuis).
 Als voorbeeld van een spel wordt het bekende spel
 MINE-SWEEPER genomen, aangezien de omvang van de routines
 van dit spel niet zo groot is, maar er toch erg veel van
 geleerd kan worden over spelprogrammeren.
 Alvorens verder te lezen raad ik je aan het spel eerst
 zelf een paar keer te spelen en voor jezelf al een beeld
 te vormen hoe je een en andere zou programmeren. Voor de
 mensen met printer: het kan erg handig zijn om de listing
 van het spel op papier te hebben om ze erbij te kunnen
 houden terwijl je deze tekst leest(NvdR: staat op de disk
 onder de filenaam MINESW.GEN.)

 Hier volgt eerst een korte beschrijving en uitleg van het
 spel voor degenen die het spel niet kennen.
 Het is de bedoeling om alle mijnen in het mijnenveld,
 voorgesteld door een scherm met vierkantjes, te vinden en
 op veilig te stellen.
 Met de muis of de cursortoetsen kan een pijltje bestuurd
 worden en met een druk op de spatiebalk of linkerknop kan
 gekeken worden of zich onder een vierkantje
 (=hokje/blokje/veld) een mijn bevindt of niet.
 Bevindt zich geen mijn op de plaats waar gedrukt werd wordt
 in het hokje een cijfer afgebeeld dat aangeeft hoeveel
 mijnen zich rondom het blokje bevinden. Een leeg hokje
 betekent dus geen mijnen op de 8 aangrenzende plaatsen; dit
 moet er toe leiden dat alle 8 aangrenzende hokjes automatisch
 zichtbaar gemaakt worden (totdat er geen omliggende hokjes
 meer leeg zijn!).
 Wordt op een mijn gedrukt betekent dit automatisch het
 einde van het spel, alle mijnen worden dan nog even
 zichtbaar gemaakt.
 Met een druk op SHIFT of de tweede muisknop kan een hokje
 op veilig gesteld worden hetgeen m.b.v. een vlag aangegeven
 wordt.
 Zijn alle mijnen op veilig gesteld en alle niet mijnen
 zichtbaar gemaakt dan is het spel gewonnen (tevens ten
 einde).

 We hebben om dit spel te programmeren dus (hoofdzakelijk)
 nodig:

 0. Een routine, die het nodige initialisatiewerk doet.
 1. Een muisroutine/cursorroutine, die een pijltje (een sprite)
    over het scherm kan bewegen.
 2. Een routine die de 1e muisknop/spatiebalk uitleest en kijkt
    of er op de coordinaten waar het pijltje zich nu bevindt
     een mijn bevindt
 2a. zo ja, het hele veld zichtbaar gemaakt wordt
 2b. zo nee, uitzoekt hoeveel mijnen rondom zitten en dit
     nummer afdrukt of
 2c. als er geen mijnen rondom zijn de onliggende hokjes
     automatische zichtbaar maakt tot er geen lege omliggende
     hokjes meer zijn.
 3.  Een routine, die uitzoekt hoeveel hokjes nog zichtbaar
     gemaakt moeten worden voor het spel gewonnen is/
 4.  Een routine, die de SHIFT-toets en tweede muisknop
     uitleest en hokjes op veilig kan stellen/weer onveilig
     kan maken.


           STARTEN

 Uitgegaan wordt van 1 programma, geschreven in machinetaal,
 m.b.v. TED of GEN80.
 Besloten wordt nu eerst het spel in Screen 5 te gaan maken,
 voor het pijltje een sprite te kiezen en de rest van de
 grafische commando's op te lossen met COPY's.
 M.b.v. Spen wordt op de eerder beschreven methode (machinetaal
 cursus zoveel) een pijl getekend als dubbele sprite
 (dus 2 sprites over elkaar heen, om meer kleuren te kunnen
 gebruiken) en de DATA naar TED gehaald (zie labels SPRCOL en
 SPRPAT).
 Besloten wordt het scherm op te delen in 16x16 blokjes, die
 de mijnenvelden voorstellen. OP het scherm passen dan
 256:6=16 blokjes in x richting en 212:16=13.25 --> 13 blokjes
 in vertikale richting. In DD-Graph worden nu eerst de
 benodigde 16x16 symbolen getekend, die we in het spel nodig
 hebben(in volgorde zoals ze op de page staan):

xco 0       16 32 48 64 80 96 112 128 144            160  176
    Leeg H.,1, 2, 3, 4, 5, 6, 7,  8,  Onzichtbaar H.,Vlag,Mijn

 (H. = hokje)

 Besloten wordt op deze blokjes op Page 1 neer te zetten op
 Yco 0.
 (xco=x-coordinaat en yco=y-coordinaat).
 Het spel zelf zal zich gaan afspelen op Page 0.
 In DD-Graph wordt het gebruikt palet tevens omgezet naar
 DATA, die we in het hoofdprogramma kunnen gebruiken (zie het
 label:PALET).

 In een Basic-programmatje (n.l. "MINESW.BAS") worden deze
 blokjes m.b.v. een Copy commando op de juiste plaats gezet
 en dit programma start tevens het hoofdprogramma "MINESW.COM"
 op.

 We kunnen nu eindelijk beginnen met het echte werk: het
 hoofdprogramma


      HET HOOFDPROGRAMMA
        ("MINESW.GEN")
      ------------------

Initialisatie

 Er wordt gebruik gemaakt van het lijstje met benodigheden voor
 het spel en we beginnen dus uiteraard met het
 initialisatiewerk. Omdat we het programma vanuit basic willen
 kunnen opstarten m.b.v. BLOAD (en GEN80 slechts .COM-files
 maakt voor op te starten vanuit DOS) komt nu eerst een HEADER:

              DB      #FE ; geeft aan dat dit een "BLOAD-file" is
              DW      ST  ; startadres
              DW      EN  ; eindadres
              DW      ST  ; beginadres

 Bovendien wordt aan een aantal BIOS-adressen een naampje
 gegeven, zodat we later in het programma CALL GTTIG (=get
 trigger) kunnen gebruiken i.p.v. een onduidelijke CALL #D8.

 Initialisatie van de sprites gebeurt in de subrotine INIT,
 omdat het onhandig is later telkens de lange initialisatie-
 routines te moeten overslaan, terwijl je iets in het
 hoodfprogramma wilt veranderen. In deze routine, worden de
 spritekleuren en patronen in het videogeheugen opgeslagen,
 de sprites "aan"-gezet en op 16x16 formaat.
 Het besturen van een sprite komt nu er op neer dat je de
 coordinaten in de spriteattribuutgeheugen veranderd
 (label:SPRATT).
 Merk op dat deze INIT afsluit met een routine SETSPRITES,
 die de sprites de eerste keer neerzet, maar die later
 opnieuw aangeroepen kan worden als er sprites bewogen
 worden!
 De routine wordt aangeroepen met CALL, hetgeen betekent
 dat er na een RET terug gesprongen wordt naar het
 aanroeppunt. De routine sluit echter af met een JP naar
 een biosadres, waar automatisch een RET gevonden wordt
 m.a.w.:

         CALL+RET=JP

 Voor verdere uitleg over sprites : zie de eerder verschenen
 FUTUREDISK met de cursus over sprites.

 Nadat de sprites zijn geinitialiseerd wordt het Palet
 aangezet, door in HL het adres van de paletgegevens
 (uit DD-Graph) te zetten en de subroutine SETPAL aan te
 roepen. Verder uitleg over SETPAL voert hier te ver aangezien
 dat meer in een VDP-cursus thuishoort. Neem maar van mij aan
 dat de routine werkt en je hem zonder problemen in je eigen
 programma kunt gebruiken.

 Programmeertechnisch gezien kun je nu het beste verder
 gaan met de besturing van de pijl, maar om even de lijn van
 het programma te blijven volgen komen we nu uit in de
 routine VULSCHERM, die het hele scherm vult met blokjes.
 Alvorens ik de werking van VULSCHERM uit de doeken doe is
 het nodig iets meer te praten op de opslag van de gegevens
 over de mijnen.

 MAAR HELAAS DOEN WE DAT PAS DE VOLGENDE KEER! TOT DAN!

                Ruud Gelissen
