    2+ SCHERMEN IN BASIC.


                             ����
                             ����
                             ����
                             ����


 Rond 1988 dacht men bij ASCII, jongens we zijn 't zat, d'r
 moeten meer kleuren in die MSX2. Men is eens goed gaan
 nadenken, en dat ging die sukkels daar bij ASCII niet erg
 goed af. Want wat hebben die eikels toen besloten: Wel meer
 kleurtjes, maar niet meer VRAM, en ja, dan krijg je gezeik.
 Als de heren bij ASCII de volgende MSX serie (2+) nou gewoon
 512KVram hadden gegeven was er niets aan de hand geweest,
 maar dat wou niet volgens ASCII. Maar goed, bij ASCII zijn
 ze toen gaan nadenken hoe ze zo veel mogelijk kleuren in zo
 weinig mogelijk VRAM konden krijgen en dat werd dus het YJK-
 systeem. Toen ze alles af hadden hadden de heren bij ASCII
 zelf ook wel door dat 't niets was en hebben toen ook maar
 besloten het geplande MSX3 systeem -met Z800!- en dus de
 nieuwe V9958 te laten voor wat het was en hebben toen maar
 eerst even de 2+ uit gebracht die nog wel een Z80 had maar
 dus wel de nieuwe V9958. Verder dan die V9958 is het nooit
 gekomen, het V9978 idee werd in een vroeg stadium al
 afgeblazen wegens te hoge kosten en te weinig potentiele
 kopers. -ik had toen nog geen MSX- en de V9990 werd ook een
 flop doordat het een poging was om meer computersoorten
 tegelijk te bereiken. Op de MSX moeten wij het dus doen met
 een zeer slome en in efficiente V9958. Dat er weinig mogelijk
 is met de V9958 is wel duidelijk. Digi's worden perfect en
 daar houdt 't wel zo'n beetje mee op. Toch valt 't allemaal
 nog wel een beetje mee. Als je maar weet wat je doet.

          COLORSPILL
 
 Screen 2 is al net zo onbruikbaak als screen 12. Hetzelfde
 verhaal geldt hier namelijk als bij Screen 12. Te veel
 kleuren in te weinig geheugen. Toch zijn heel behoorlijke
 resultaten te behalen op screen 2. Kijk maar naar de hele
 Nemesis serie en klonen die Konami heeft uitgebracht. Als
 jij een spel als Nemesis III speelt op je MSX2 werkt het toch
 allemaal echt in screen 2. Ook al zijn de kleuren iets
 aangepast. Ik ben er echt van overtuigd dat dit ook kan in
 screen 12. Maar dan moeten er wel eerst fatsoenlijke
 tekenprogramma's komen voor dit scherm.

         DE 'LOGICA'

 Om te begrijpen hoe de V9958 te werk gaat moeten we eerst even
 kijken hoe alle data in 't Vram wordt gezet.

  Bit:  |  7  |  6  |  5  |  4  |  3  |  2  |  1  |  0  |
 --------------------------------------------------------
 Byte 1 |  H  |  H  |  H  |  H  |  H  |  G  |  G  |  G  |
 Byte 2 |  H  |  H  |  H  |  H  |  H  |  B  |  G  |  G  |
 Byte 3 |  H  |  H  |  H  |  H  |  H  |  R  |  R  |  R  |
 Byte 4 |  H  |  H  |  H  |  H  |  H  |  B  |  R  |  R  |
 --------------------------------------------------------

 Hierbij staat de H voor helderheid, de R, de G en de B voor
 respectievelijk R G en B. Een voorbeeldje. Iedereen klaagt
 altijd over de kleur blauw die zo verdomd moeilijk op 't
 scherm is te krijgen. Nou, als je weet wat je doet valt dat
 wel mee.

 VPOKE &H0000,&B00000000 : VPOKE &H0001,&B00000100
 VPOKE &H0002,&B00000000 : VPOKE &H0003,&H00000100

 De bits die voor blauw staan gewoon op 'aan' zetten en
 klaar! Lastig is dus wel dat je altijd binnen een groepje
 van 4 bytes moet blijven. En dat is dus 't hele probleem van
 Screen 12. Verder is er niets aan de hand. De Helderheid kun
 je gewoon vrij per pixel instellen zonder dat er allerlei enge
 dingen gebeuren..
 Dat betekent dus dat er 32 helderheden per pixel kunnen
 worden ingesteld en ik denk dat dat echt een sterk punt is
 van Screen 12!! Als tekenprogramma's nou eens gewoon gebruik
 maakten van de uit Screen5 bekende balkjes voor RGB en dan
 nou eens gewoon die kleur in de 32 helderheden zou afbeelden
 werd 't leven van de tekenaar op screen 12 al een heel stuk
 makkelijker. Denk ik tenminste!! Goed, met deze gegevens
 kunnen we al een heel eind verder, maar waarom werkt
 Screen 10 dan wel goed in basic? Nou Screen10 bestaat gewoon
 niet. Als dat alles niet een heel stuk duidelijker maakt
 weet ik 't ook niet meer! Nee, Screen 10 bestaat aleen in
 basic, de basic interpreter gaat met screen 10, dat
 eigenlijk gewoon Screen 11 is, zodanig om dat alle
 instructies dezelfde zijn als die uit Screen 8, zij het dat
 de kleur en van 0 tot 15 lopen. Hoe werkt dit dan?

  Bit: |  7  |  6  |  5  |  4  |  3  |  2  |  1  |  0  |
 ------+-----+-----+-----+-----+-----+-----+-----+-----+
 Byte 1|  x  |  x  |  x  |  x  |  S  |  G  |  G  |  G  |
 Byte 2|  x  |  x  |  x  |  x  |  S  |  B  |  G  |  G  |
 Byte 3|  x  |  x  |  x  |  x  |  S  |  R  |  R  |  R  |
 Byte 4|  x  |  x  |  x  |  x  |  S  |  B  |  R  |  R  |
 ------+-----+-----+-----+-----+-----+-----+-----+-----+

 De x is hier variabel. Dit komt door de S in het geheel.
 Die S is namelijk een Switch. Staat deze 'uit' dan geldt de
 normale verwerking van de bits en zijn alle x bits de
 helderheid.. Staat zo'n bit echter 'aan' dan schakelt
 'screen5' in en worden de xbits de 4bits kleurcodes die we
 van screen5 kennen. De basis kleur van de vier pixels blijft
 hetzelfde zodat zowel dat 16 paletkleur en als de 65536
 Screen10 kleuren kunnen worden afgebeeld in de Screen10 mode.
 Omdat dus een bit wordt gebruikt voor de Switch gaat dus wel
 de helft van de helderheden verloren aangezien er nog maar
 16 helderheden kunnen worden gemaakt. Een plaatje van
 Screen12 naar 11 converteren is dus geen probleem aangezien
 je alleen bit3 maar 'uit' hoeft te zetten. Dit kan gewoon
 met een Box in 'kleur' &B 11110111 waar vervolgens een AND
 op los moet worden gelaten waardoor bit3 altijd nul wordt
 en de andere bitjes onaangetast blijven.
 Op de disk staat een basiclisting die 256*212=54272 kleuren
 op 't scherm tovert. Meer was op zich 't probleem niet zij
 het dat er niet meer pixels beschikbaar zijn in screen 12 en
 ik de pest heb aan interlacing. Maar hoe ga ik dan te werk?
 Simpel, je begint door met een simpel rekentruukje alle
 mogelijke (4096) basis kleuren op het scherm te toveren en
 die vervolgens per basiskleur naar 32 pixel te copien. Je
 hebt namelijk van elke basiskleur 32 pixels nodig om daar
 later je H bits op aan te passen om de 32 helderheden te
 krijgen. Als dan het hele scherm vol staat met van alle
 basiskleuren 32 pixels kun je je helderheden er overheen
 XORen zonder dat je de basis kleurbits aantast. Hiervoor
 gebruik ik een truuk die eigenlijk niet mag maar goed,
 niemand heeft het door. Dat kleuren aantal is natuurlijk
 theoretisch want 't komt ongetwijfeld wel op een kleiner
 aantal uit.

 &B11111101,&B11111000,&B11111000,&B11111000

 is natuurlijk 't zelfde als

 &B11111011,&B11111000,&B11111000,&B11111000

 Maar dat zal een ieder duidelijk zijn, als je twee 'groen'
 bitjes aan zet maakt 't natuurlijk niet uit welke twee dat
 zijn..ook al denkt mijn programma daar anders over. Waarom
 ASCII ook zegt dat er maar 19000 nogwat kleuren zijn is mij
 ook een raadsel. Van screen 5 zei men toch ook dat er een
 palet uit 512 verschillende kleuren mogelijk was? R3 G3 B3 is
 bij mij 't zelfde als B3 G3 R3 of niet soms? Nou weet ik ook
 wel dat 't verschil tussen helderheid 31 en 30 niet te zien
 is en dat als twee bits 'groen' aan staan het niet uit maakt
 welke dat zijn maar laten we gewoon zeggen dat screen12
 2 tot de 12e * 32 = 131072 kleuren heeft zodat we tegen al
 dat Super Nintendo geweld op kunnen met ons 2plus-je. Tegen
 PCers zeggen we dan gewoon weer 131072 basiskleuren in
 16 helderheden zodat hun 1,6 miljoen ook weer met bijna
 500000 is over schreden en dan kunnen we er weer tegenaan met
 ons MSXje. Ik hoop dat ik met deze korte uitleg 't een en
 ander duidelijk heb gemaakt en HOO WACHT!!! voor dat ik
 het vergeet, of dat programmaatje van mij werkt weet ik eigen-
 lijk niet... Ik heb namelijk een monochrome monitor hangen aan
 m'n MSX-Turbo-R (wat ben ik ook een stakker.) zodat alle kleur
 en zo'n beetje 't zelfde lijken en of ze dus ook echt verschil
 lend zijn weet ik niet. Niet boos worden als 't niet zo is oke
 boyz!? Oh ja, ik was aan het afsluiten dus daar ga ik dan maar
 weer mee verder. Ik hoopte geloof ik dat ik jullie weer 't een
 en ander heb duidelijk gemaakt en dat was 't dan wel voor deze
 keer want 3 basic cursussen op een nummer is wel echt genoeg..

 Tot de volgende keer dan maar!!

 Tobias "God, i'm perfect!" Keizer
