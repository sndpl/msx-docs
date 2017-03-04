
      MCODE SPECIAL (3)


 We gaan verder waar we op FD #21 waren blijven steken...


MAIN (het hoofdprogramma)

 Eigenlijk is dit de routine waaraan je het beste kunt
 beginnen te programmeren.
 Zorg dat de MAIN routine niet te lang wordt (zodat je hem in
 ieder geval nog in 1 keer op het scherm kun krijgen) en zet
 hier ook alleen de belangrijkste zaken in.
 Je kunt dit bereiken door van elke onderdeel een CALL te
 maken. Het is in het begin vaak al mogelijk om te zeggen wat
 voor routines later in de hoofdroutine aangeroepen zullen
 worden. Zet in de hoofdroutine dan alvast de CALL's klaar,
 maak alvast de LABEL's aan maar zet op de plaats waar later
 de eigenlijke routine komt een RET: dit bevordert het
 overzicht, en bovendien blijf je op de hoogte van wat je
 nog programmeren moet. De routine MAIN spreekt verder voor
 zich:

MAIN:
     CALL    SETSPRITES        ; zet de sprites op goede coord.
     EI
     HALT
     HALT
     XOR     A                 ; spatie balk ?
     CALL    GTTRIG
     AND     A
     CALL    Z,GEDRUKTQ        ; misschien wel vuurknop
     CALL    NZ,GEDRUKT        ; op knop gedrukt: maak zichtbaar
     LD      A,3               ; 2de knop ?
     CALL    GTTRIG
     AND     A
     CALL    Z,SHIFTQ          ; misschien wel op shift
     CALL    NZ,VEILIG         ; maak veilig
     CALL    BESTUUR           ; besturingsroutines
     CALL    TESTEIND
     LD      A,7
     CALL    SNSMAT
     AND     %00000100         ; escape
     JP      NZ,MAIN
EINDE:
       XOR     A
       JP      #5F

 Zoals je ziet kan met ESCape het spel afgebroken worden: dit
 is een van de opties die je er het eerst in moet bouwen,
 zodat je altijd uit het spel kan en kan controleren (als je
 een programmeer fout maakt en de computer niets meer doet) of
 je met ESCape nog weg kunt en het programma dus nog in de
 hoofdlus rondliep.

 De spatiebalk en de linkermuisknop moeten er beide toe leiden
 dat GEDRUKT aangeroepen wordt. Je test nu eerst op een van
 beide (hier de spatiebalk) en als deze niet ingedrukt wordt
 ( AND    A levert zero ) test je in een aparte loop of de
 linkermuisknop wel ingedrukt werd (GEDRUKTQ)

GEDRUKTQ:
              LD      A,1
              CALL    GTTRIG
              AND     A
              RET

  p.s. de Q in sommige labels heeft voor mij de betekenis van
       een ?, aangezien GEN80 labels met een ? niet aankan.
  p.s.s. je ziet ook dat achter elk label een : geplaatst is.
       Dit om het zoeken naar labels m.b.v. de TED-zoekfunctie
       te vergemakkelijken, omdat je de aanroepplaatsen van
       de labels niet tegenkomt.

 In de hoofdlus zitten ook nog 2 HALT's.
 Deze zijn bedoeld om het geheel soepeler te laten verlopen.
 Een hoofdlus zonder HALT duurt niet elke keer dat hij
 doorlopen wordt even lang, b.v. omdat soms wel en soms niet
 op een toets gereageerd moet worden.  Dit heeft tot gevolg
 dat b.v. de muisbesturing niet altijd even vaak per seconde
 uitgevoerd wordt en de muis dus niet soepel loopt.
 Zet daarom (als het even kan) altijd een of meerdere HALT's
 in de hoofdlus. Het is verstandig om deze vooraf te laten
 gaan door het commando: EI.

 Verder worden in de hoofdlus de routines BESTUUR en TESTEIND
 altijd aangeroepen. VEILIG en GEDRUKT alleen als er op een
 knop gedrukt wordt. De hooflus moet nooit te ingewikkeld
 worden, anders verlies je het overzicht van het verloop van
 het spel.

BESTUUR

 In de routine BESTUUR wordt eerst de muis uitgelezen door
 een aanroep van de subroutine MOUSE.
 Je ziet in deze routine hoe dat er getest kan worden of er
 een muis aangesloten is.

 M.b.v. de biosroutine GeTPAD worden in de adressen
 #FAFE en #FB00 de verandering van de respectievelijk xco
 en yco gezet. De inhoud van deze adressen wordt dan opgeteld
 bij de huidige xco en yco van de 2 sprites, die staan op de
 adressen aangeduid door de labels XCO en XCO+4(of YCO en
 YCO+4) in de sprite-attribuuttabel. Zoals je ziet worden
 hier nog niet deze attributen in het VRAM gezet, dat
 gebeurt pas als in de hoofdlus opnieuw een CALL SETSPRITES
 gebeurt.
 Bovendien wordt m.b.v. het uitgebreid behandelde CP-commando
 getest of het pijltje zich nog binnen de grenzen van het
 beeld bevindt.

 Als de muisbesturing afgehandeld is, wordt gekeken of er
 met de cursortoetsen of joystick bewogen wordt.
 In het label SPEED staat hoeveel pixels er per druk op een
 cursortoets verschoven wordt.
 Normaal staat hierin 2 maar als de GRAPH-toets ingehouden
 wordt, wordt dit getal 4.

 De Biosroutine GTSTCK levert de beweegrichting van cursor
 of joystick. Aan de hand van deze waarde wordt bepaald welke
 richtingsroutine aangeroepen wordt.

 Let er op dat voor een beweging in een diagonale richting
 gewoon gebruik gemaakt wordt van de 2 bijbehorende
 horizontale en vertikale richtingsroutines.

 Over de BESTUUR routine wil ik verder niet teveel uitwijden,
 omdat deze niet bijster interessant is. Ik moet toegeven dat
 ik bijna altijd als ik in een programma een besturings-
 afhandeling nodig heb, ik deze routine (met enige aanpassing
 van bv. de randcoordinaten) gebruik, omdat ze toch al zo
 efficient mogelijk in elkaar zit.
 Als je wil mag je gerust deze routine gebruiken in je eigen
 programma's. Opnieuw programmeren is tijdsverspilling!

LEES VERDER IN DEEL (2)

      MCODE SPECIAL (3)

           part 2



GEDRUKT (het spel zelf)

 We hebben nu een pijl, een veld met blokjes, besturingsroutines
 en kunnen de knoppen uitlezen maar het spel zelf draait er
 voornamelijk om dat er blokjes zichtbaar gemaakt kunnen worden.
 De routine GEDRUKT regelt dit.

 GEDRUKT begint al meteen met een aanroep van een andere
 routine: HAALBLOKNR.

 HAALBLOKNR zoekt uit op welk bloknr. het pijltje staat,
 tevens worden DX en  DY, de coordinaten waar naar toe straks
 het blokje gekopieerd gaat worden al goed gezet.

HAALBLOKNR:
              LD      A,(XCO)
              AND     %11110000         ; omlaag afronden
              LD      (DX),A            ; wordt de plaats waar
                                          een blok komt
              RRCA
              RRCA
              RRCA
              RRCA                      ; delen door 16:
                                          het bloknr door xco
              LD      B,A               ; even bewaren
              LD      A,(YCO)
              AND     %11110000         ; omlaag afronden
              LD      (DY),A            ; is tevens bloknr.
                                          door yco
              ADD     A,B               ; optellen levert het
                                          echte bloknr.
              RET


 In A staat nu het bloknummer, dit is dus de plaats in het
 dataveld waarop gedrukt werd en die de speler van het spel
 graag zichtbaar wil maken. Wij moeten nu laten zien wat er
 op die plaats staat en hierop zonodig reageren.
 Met de volgende routine komen erachter wat de gegevens (zie
 bespreking DATAveld) zijn over het blokje waarop gedrukt
 werd.


              LD      HL,DATA
  LD          C,A
              LD      B,0               ; nu in HL het DATA-
                                          blokje, waarop je
              ADD     HL,BC             ; met je pijlje hebt
                                          gedrukt.
              LD      A,(HL)            ; in A de inhoud van
                                          je blok

 Bit voor bit wordt nu afgegaan wat die data voorstelt:

              BIT     5,A
              RET     NZ                ; drukken op een vlag
                                          heeft geen zin
              BIT     6,A
              RET     NZ                ; op een zichtbaar
                                          blokje ook niet
              BIT     7,A
              JP      NZ,DOOD           ; op een mijn gedrukt
              AND     %00001111
              JP      Z,CLEARDATA       ; op een leeg hokje

 Een RET be�indigt de routine GEDRUKT, omdat deze met een
 CALL aangeroepen wordt. Als je op een leeg hokje drukt wordt
 doorverwezen naar een nieuwe routine, die de omgeving
 automatisch zichtbaar gaat maken: CLEARDATA. Hierop ga ik
 later in. Verreweg de meeste keren zal een speler drukken
 op een getal dan moet het blokje zichtbaar gemaakt worden.
 De hiervoor gegeven routine wordt dan gewoon doorlopen en
 de computer komt aan bij de volgende routine:

MAAKZICHTBAAR:
               SET     6,(HL)            ; in DATA aangeven
                                           dat dit blokje al
               LD      HL,AANTALZICHTBAAR; zichtbaar is
               INC     (HL)

 Het aantal zichtbare hokjes wordt geteld, omdat we dan
 eenvoudig kunnen nagaan of alle blokjes zichtbaar zijn.
 Bovendien wordt in de data aangegeven, dat dit blokje nu
 zichtbaar is zodat, als er nog een keer op dit blokje
 gedrukt wordt, de computer niet meer reageert.
 In het A register staan nu nog steeds de 4 onderste bit's
 van de DATA. Dit is het getal wat afgedrukt moet worden.
 Net zoals dit in de routine vulscherm gebeurde wordt de
 Source Xcoordinaat als volgt berekend:

              ADD     A,A
              ADD     A,A
              ADD     A,A
              ADD     A,A               ; *16 bereken SX
              LD      (SX),A            ; DX en DY al in
                                          HAALBLOKNR gezet.

 DX en DY zijn al berekent, de rest van de gegevens ligt
 vast --> met de volgende regels wordt het blokje
 daadwerkelijk neergezet:

              LD      HL,BLOKDATA       ; laat ook zien waarop
                                          gedrukt is
              JP      COPY              ; einde GEDRUKT

 Na deze  JP     COPY wordt weer teruggekeerd naar de hoofdlus.

 Maar genoeg voor deze keer! De cursus gaat weer verder op de
 volgende FD! Veel succes met programmeren!

Ruud Gelissen

