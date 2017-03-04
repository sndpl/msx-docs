# Screen 4


 Bij alle besprekingen van de MSX2 video chip, wordt er altijd
 aandacht besteed aan de bitmapped modes, screen 5 tot en met
 acht, maar nooit aan de mogelijkheden van Screen 4.
 Waarschijnlijk is dit, omdat Screen 4 te zeer een MSX1
 verleden heeft. Toch zijn de mogelijkheden van deze
 schermmode groot en kunnen er leuke dingen mee gedaan worden.

## Opbouw

 Screen 4 is een pattern screen. Dit wil zeggen, dat het
 scherm niet bestaat uit pixels, maar wordt opgebouwd uit
 informatie uit verschillende tabellen. Dit klinkt erg
 moeilijk, maar het komt erop neer dat wanneer je pixel (0,0)
 wit wil maken, je eerst in de patroon tabel een volgend
 patroon aanmaakt:

 &b10000000
 &b00000000
 &b00000000
 &b00000000
 &b00000000
 &b00000000
 &b00000000
 &b00000000

 Plaatsen we dit patroon in het begin van de eerste
 patroontabel (er zijn er meer) dan krijgt dit patroon nummer
 nul. Vervolgens vullen we de kleur tabel met dit patroon:

 &hf0
 &h00
 ..        (volmaken tot de acht)
 &h00

 En tot slot plaatsen we in de naam tabel op de eerste positie
 een nul en we zien een witte pixel.
 Hieruit blijkt al dat het dus een stuk moeilijker is om te
 tekenen op Screen 4.

 Samenvattend komen we nu uit bij de drie tabellen:


## PATROON TABEL

 De locatie van de patroontabel in het videogeheugen kan
 worden ingesteld middels register 4:

        Msb      7    6    5    4    3    2    1    0
        Reg 4    0    0   A16  A15  A14  A13   1    1

 Als je de patroon tabel dus op adres nul wil laten beginnen,
 dan laad je register 4 met %b11.

 De patroontabel bestaat uit drie blokken van elk 256
 patterns. Hiermee is het mogelijk om het gehele scherm te
 vullen. Het eerste block bevat de informatie voor de de
 bovenste 64 beeldlijnen (0-63). Hieruit volgt al ÇÇn van de
 grappige eigenschappen van Screen 4, als je een patroon
 binnen lijn 0-63 en lijn 64-127 wil gebruiken, dan moet je
 dit patroon twee keer defini◊ren.

 De opbouw van het patroon ziet er als volgt uit:
 &01
    Aanvullen tot acht
 &00

 Zoals al duidelijk wordt, kun je maar twee kleuren in een
 acht bij acht blokje gebruiken. Je hebt enkel variatie 1 en
 0. Dankzij de kleuren tabel echter kun je per lijn de kleur
 veranderen. Het idee komt overeen met dat van sprites.

 Nog een leuke bij de patroontabel. Toen ik met vdp23 in de
 weer ging op een Screen 4 scherm, kreeg ik met geen
 mogelijkheid de lijnen van (192-255) gevuld. Maar toen ik
 effe naar de layout keek en erg simpel even wat adressen in
 het video geheugen opschoof, viel op dat de patroontabel
 eigenlijk uit vier blokken bestaat: (0-63),(64-127),(128-
 191),(192-255). Maar omdat dit alleen kan als je meer dan 16k
 video geheugen hebt, wordt dit nooit in een of ander databoek
 vermeld.


## KLEURENTABEL

 De kleurentabel bepaalt welke kleuren geassoci◊erd moeten
 worden met de nul en een in de patroon tabel. De plaats van
 de kleurentabel bepaal je met registers 3 en 10:

  Msb      7    6    5    4    3    2    1    0
  Reg 3   A13   1    1    1    1    1    1    1
  Reg 10   0    0    0    0    0   A16  A15  A14

 De kleurentabel bestaat opnieuw uit drie (vier dus) blokken
 en is per blok als volgt opgebouwd:

 &00
   aanvullen tot acht
 &00

 Waarbij de eerste nibble de kleur van de als 1 gedefini◊erde
 patronen bepaald en de laagste nibble de kleur van de als 0
 gedefini◊erde. Dit kun je per lijn wijzigen.

## NAAM TABEL

 Nu hebben we de kleuren en de patronen, dan volgt tot slot
 nog de naam tabel. De plaats van deze tabel in het geheugen
 kun je bepalen met register 2.

  Msb      7    6    5    4    3    2    1    0
  Reg 2    0   A16  A15  A14  A13  A12  A11  A10

 In deze tabel bepaal je op welke plaats je gedefini◊erde
 patronen terecht moeten komen. Positie nul in de tabel komt
 overeen moet coordinaat (0,0). Positie ÇÇn komt overeen moet
 (8,0) enz. In deze bytes zet je het patroon nummer. Je hoeft
 je geen zorgen te maken om de kleurentabel, want deze is
 automatisch gekoppeld aan de patroontabel. Uiteraard bestaat
 deze tabel ook weer uit drie (vier) delen.

 Zo, nu hebben we alle data van screen4 besproken, laten we
 eens kijken hoe het in de praktijk werkt door middel van ÇÇn
 van de meest aansprekende voorbeelden: Space Manbow.

 Als we Space Manbow opstarten moeten we even door wat scherm
 5 plaatjes heen en komen we in het prachtige speelscherm.

 Wat zien wij nu:

 Een stilstaande bovenkant (screen 5, line interrupt)
 Een sterren scroll (screen4)
 Een scrollend middenveld (Screen 4)
 Een rap scrollend benedenveld (Screen 4)
 Een lading sprites.

 Geweldig!

 De sterretjes worden wel heel simpel gedaan, door voortdurend
 het patroon te wijzigen. Een simpele RLCA instructie doet
 hier wonderen. Het middenveld wordt gescrolled door de
 naamtabel voortdurend te verplaatsen en tot slot de onderste
 lijn wordt bewerkt door gewoon twee keer zo snel de naam
 tabel door te schuiven.

 Zoals je al altijd gezien had, is het resultaat verbluffend,
 het spel lijkt eerder van de SuperNes te komen, dan van een
 simpele MSX.
 Aangezien Konami een stel designers in dienst had die een
 lange staat van dienst op de MSX 1 achter de rug hadden, is
 er goed gebruik gemaakt van de mogelijkheden van Screen 4.
 Als je goed kijkt, valt je namelijk op dat er echt gespeeld
 is met de twee kleuren per 8x8 blokje. Het is vergelijkbaar
 met het ontwerpen van sprites.
 Ook in de hogere levels is leuk ingehaakt op de mogelijkheden
 van pattern screens. Zo noem ik de laserstralen van het boss
 monster in het laatste level. Als je dat met een line
 commando in screen 5 zou willen doen, zou dat heel erg traag
 worden. Op schermpje vier edit je een patroontje en met wat
 gespeel in de naamtabel heb je schermvullende actie.

 Een ander leuk voorbeeld van Screen 4 is FireHawk. Het intro,
 met de naar achter vliegende letters is ook op scherm vier
 gemaakt. Waarom? Omdat scherm vier snel is en het weinig
 geheugen in beslag neemt. Ook het hele spel loopt in Screen
 4
 en scrollt met stappen van 8 bij 8.

 Waarom gebruikt dan niemand scherm 4? Simpel:

 ? Je moet een heel goede tekenaar zijn
 ? Je moet een heel goed tekenprogramma hebben
 ? Je moet heel goede dataveld editors hebben
 ? Je moet iemand met een MSX1 verleden zijn

 Aangezien er bijna niemand aan deze eisen voldoet, maakt dan
 ook niemand iets op Screen 4. Alleen Jan van Valburg heeft
 ooit twee Screen 4 spellen gemaakt, Gianna Sisters en
 Retaliator. Oh ja, Çn ANMA natuurlijk met Troxx.

 Dus, als je ooit nog een Space Manbow wilt maken: leer teke-
 nen met beperkingen en gebruik allerlei truukjes om de
 mensen te laten geloven dat ze naar een Super Nes zitten te
 kijken.

 Succes


Jan-Willem van Helden

 P.S. Ik probeer op de volgende FD nog iets leuks te doen met
 Screen 4. Let op!