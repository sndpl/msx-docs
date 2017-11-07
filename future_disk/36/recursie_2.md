Dit is alweer het negende en laatste deel van de PASCAL cursus door de enige echte Jeroen Smael (NvdR: yeaaaah!!)...

            RECURSIE


 ����  Ja, je leest het goed, dit deel gaat over recursie. Dat
 ����  is ook waar we vorige keer gebleven waren, dus.....
 ����  Goed, niet te veel intro gezeik, ik ga gewoon beginnen
 ����  en dan zien we wel waar we uitkomen.

          Recursie (2)

 Wat is recursie? Recursie is (letterlijk) herhalen. Wat kun je
 ermee? Heel veel, maar dat zal (hopelijk) nog wel duidelijk
 worden. Een klassiek voorbeeld van recursie is machtsverheffen.

        MACHTSVERHEFFEN

 Wat is machtsverheffen?

  x^n = x^(n-1)*x
  x^(n-1) = x^(n-2)*x
  x^(n-2) = x^(n-3)*x

 Ok, je begrijpt het nu wel. De vraag is alleen wanneer stop ik
 (terminate). Ik stop als (n-?) gelijk aan 0 is. Waarom? Als
 (n-?) gelijk aan nul is dan krijg ik x^0 en dat is ALTIJD 1.
 De functie ziet er dan als volgt uit:

  PROCEDURE Macht2(n : integer) : integer;

    BEGIN
      IF n=0 THEN
        Macht2=1
      ELSE
        Macht2=Macht2(n-1)*2
    END;

 Bovenstaand is om elke willekeurige (positieve) macht van 2 te
 berekenen (dat zit immers niet standaard in PASCAL).
 Bijzonderheden zijn dat de functie zichzelf aanroept
 (Macht2=Macht2(n-1)*2). Dat zou eigenlijk niet kunnen, omdat
 de functienaam eigenlijk alleen aan de linkerkant van het =
 teken mag staan (voor een toewijzing). Maar mag in dit geval
 wel.

           IS DIT ALLES?

 Nee, dit is nog lang niet alles, maar het volgende (en tevens
 laatste) voorbeeld is wel iets lastiger. Waarom? Omdat dit een
 voorbeeld van backtracking is. Wat is Backtracking?
 Backtracking wordt gebruikt voor zoek algoritmen.

          Backtracking

 Stel je staat in een doolhof. Wat doe je? Je gaat zoeken
 natuurlijk, maar hoe? Als een kip zonder kop lijkt me niet erg
 verstandig, want dan kom je nooit waar je wil zijn (de
 uitgang). Dus ga je systematisch zoeken. Maar hoe doe je
 dat? Als je veel tijd hebt (en wat heb je anders in een
 doolhof) dan ga je altijd rechts. Is dat niet meer mogelijk,
 dan ga je rechtdoor. Gaat dat niet meer, dan ga je links. Gaat
 dat niet meer, dan ga je terug tot de plaats waar je als
 laatste een keus moest maken en doet daar hetzelfde. Op die
 manier kom je waar je wezen moet (bij de uitgang). Wat je
 natuurlijk niet moet doen (maar wat ik vergeten ben te zeggen)
 is over je eigen weg lopen (dus als je er al geweest bent, dan
 ga je niet verder, omdat je anders in een kring gaat lopen).


         Implementatie

 Ik ga de pseudo code van de implementatie geven, zodat je die
 zelf uit kunt werken. Je moet dan wel zelf de hele
 datastructuur opzetten, want dat doe ik niet voor je. Hier
 gaat 'ie:

  PROCEDURE ZoekUitgang(Doolhof , Locatie , Gevonden);

    BEGIN
      IF Locatie is buiten THEN
        Gevonden = TRUE
      ELSE
      BEGIN
        IF mogelijk naar rechts THEN
          ZoekUitgang(Doolhof(markeer Locatie) ,
                      Locatie + 1 naar rechts , Gevonden);
        IF NOT Gevonden & mogelijk naar voren THEN
          ZoekUitgang(Doolhof(markeer Locatie) ,
                      Locatie + 1 naar voren , Gevonden);
        IF NOT Gevonden & mogelijk naar links THEN
          ZoekUitgang(Doolhof(markeer Locatie) ,
                      Locatie + 1 naar links , Gevonden);
      END;
    END;

 Dat is alles. Hier horen natuurlijk een paar opmerkingen bij
 en wel de volgende:
  - Doolhof is een array waar de doolhof in staat en tevens of
    je er geweest bent.
  - Locatie is je 'X en Y' positie in de doolhof
  - mogelijk is een functie die aangeeft of je die richting uit
    mag (dit kun je ook de de procedure ZoekUitgang laten doen,
    aangezien in een muur ook niet buiten is).
  - markeer locatie is in Doolhof aangeven dat je er geweest
    bent (dat moet je daarna wel weer verwijderen, anders zoek
    je niet alles af).


    WAT MOET IK ZEKER WETEN

 Wat je zeker moet weten is dat de MSX Turbo PASCAL v3.3 geen
 recursie aankan. "Wat, geen recursie, waarom heb je ons dit
 dan allemaal verteld?"
 Omdat je recursie wel aan kunt zetten. Hoe? Door de $A+ optie
 te gebruiken. Vergeet dit niet, anders klopt er niets van je
 baksels (zelfs het machtsverheffen werkt dan al niet).

 Dat was het,

                                    Jeroen 'Backtracking' Smael
