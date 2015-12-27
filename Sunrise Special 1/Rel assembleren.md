R E L O C A T A B L E   A S S E M B L E R E N


Eerst  even  dit:  ik  ga bij  dit stukje  uit van  de GEN80
assembler en de L80 linker. Omdat deze programma's onder DOS
draaien, gaat het hier dus ook automatisch over '.COM'-files
als resultaat.

Naast GEN80 kun je bijvoorbeeld ook met M80 en RMAC reloca-
table assembleren.


    N O R M A A L   A S S E M B L E R E N

Veel mensen die wel eens iets in machinetaal doen, schrijven
hun  listing  als  een  lange  lijst  met  instructies.  Het
assembleren kan, vooral bij de wat grotere programma's, best
lang duren.  De assembler moet immers die hele lap tekst van
een schijf lezen.

Na  het testen  kom je  er nog wel eens achter dat iets niet
helemaal goed  werkt, en  je gaat  aan het  foutzoeken... Na
lang  zwoegen is  de plaats  waar de fout zich bevindt waar-
schijnlijk  gevonden.   Verandering  aanbrengen  en  opnieuw
assembleren.

De assembler  moet nu  die hele  lap tekst  (ook de foutloze
delen) opnieuw verwerken. Dit kost weer een hoop tijd.

Dit  proces herhaalt  zich tot het moment dat de programmeur
denkt dat alle fouten uit het programma zijn verwijderd.

Doordat  iedere keer  dat je  de listing assembleert de hele
programmatekst  moet  worden  verwerkt  kan dit,  vooral bij
lange listings, enorm veel tijd kosten.


R E L O C A T A B L E   A S S E M B L E R E N

Maar  het kan ook  anders, en dat  anders  heet 'relocatable'
assembleren.

Hierbij   maak   je   gebruik   van   korte  deelprogramma's
waarin  allerlei subroutines  staan. Die  losse delen kun je
dan apart van elkaar assembleren en later aan elkaar plakken
met een linker (dit is een apart verkrijgbaar programma).

In die losse delen  zit de grote kracht  van het relocatable
assembleren. Als je bij het foutzoeken een fout ontdekt, dan
hoef  je  niet weer  die hele  lijst door  te spitten  en te
assembleren,  maar  slechts een  klein stukje  van het  hele
programma. Omdat  je nu  maar een stukje van het geheel door
de  assembler haalt, bespaar je veel tijd (de andere modules
zijn immers  niet veranderd, dus die hoef je nu dan ook niet
opnieuw te assembleren).

Ook  kun je  met deze  modulaire opzet modules die je eerder
hebt geschreven  opnieuw gebruiken  in je  volgende program-
ma's.  Je  kunt  zo dus  een bibliotheek  van kant  en klare
modules  opbouwen.


              W E R K W I J Z E

Bij  relocatable  assembleren ga je er  van uit dat een lang
programma op  te splitsen valt in kortere delen, die als het
even kan ook nog min of meer iets met elkaar te maken hebben
(een soort van bibliotheek-bestanden dus).

Als  je een  zo'n module geschreven hebt, dan kun je hem as-
sembleren.

In de relocatable mode gaat een assembler heel anders om met
adressen. Hij  ziet de  adressen nu niet als een echt getal,
maar  als een  offset. De  echte adressen  worden pas  later
ingevuld.

Je  krijgt nu  ook geen '.COM'-files, maar '.REL'-files, wat
staat voor 'relocatable'.

Na  het schrijven  van de  modules en het assembleren ervan,
kun  je  alles  aan  elkaar gaan  knopen, het  'linken'. Dit
linken  gebeurt  met een  apart programma,  de linker  (bij-
voorbeeld LINK.COM of L80.COM).


             H E T   L I N K E N

Bij dit 'linken' geef je op welke modules je aan elkaar wilt
koppelen.  Daarna  geef  je  de  naam van  het uiteindelijke
programma op et voila! Je hebt je programma....

Met l80 gaat dit linken als volgt:

A>l80 module1,module2,module3,prognaam/n/e

De  linker laadt nu de bestanden module1.rel, module2.rel en
module3.rel van  disk. Ze  komen ook  in die volgorde in het
uiteindelijke  programma te  staan, dus  je kunt niet zomaar
bij een  module beginnen!  In dit geval begint de programma-
uitvoer dus met module1.

Hierna worden  deze modules 'gelinkt'. Dit linken wil zoveel
zeggen  als 'zet  de genoemde bestanden achter elkaar en vul
waar dat  nodig is  de juiste adressen in'. Bij het invullen
van  adressen worden al de  'offset' adressen vervangen door
absolute adressen.

Na het linken wordt het gevormde programma op disk gezet. In
het voorbeeld is die naam 'prognaam.com'. Als er '/n' achter
achter  een naam staat, gaat de linker er van uit dat dat de
naam van het programma moet worden.

De '/e'  wil zeggen  dat de  linker na  het wegschrijven van
prognaam.com moet stoppen, zodat je weer in DOS zit.


          H E T   R E S U L T A A T

Nu  heb   je  een  programma  op  disk  staan  met  de  naam
prognaam.com.  Het testen  kan beginnen.  Dit testen bestaat
meestal uit  het opstarten  van dat programma en proberen of
je je geesteskindje vast kunt laten lopen.

Als  er  nu  een  fout  wordt gevonden  die bijvoorbeeld  in
module2  zit, dan  hoef je  alleen module2  te veranderen en
opnieuw te  assembleren. Bij  het linken moet je (uiteraard)
wel weer alle modules opgeven.

Het  is trouwens handig als je de assembleeren link-opdracht
in een  batchfile zet.  Dat kan veel tikwerk schelen (en het
werkt wat vriendelijker).

Zo'n batch ziet er dan bijvoorbeeld zo uit:

gen80 %1
l80 init,commando,diversen,edit,drawscr,getconv,monitor/n/e


   A S S S E M B L E R O P D R A C H T E N

Een  assembler die in een relocatable mode kan worden gezet,
kent een aantal speciaal op die mode gerichte opdrachten.

Zo heb  je bij  GEN80 o.a. de directives external en public.
Er zijn er nog wel meer die met de relocatable mode te maken
hebben, maar  dat staat  allemaal in  de handleiding  van je
assembler.

Je kunt GEN80 laten weten dat  je in  relocatable mode  wilt
assembleren door op de eerste regel van je programma * R+ te
zetten.

Het  kan natuurlijk  ook door  gen80ins op te starten en die
relmode vast  in het  programma te zetten. Als je nu iets in
de  absolute mode wilt assembleren, dan kun je dat doen door
op de eerste regel * R- te zetten.


              V O O R B E E L D

Op deze disk staan als voorbeeld 3 modules (.GEN) en hun ge-
assembleerde vorm (.REL en .SYM).
De listings bevatten volop commentaar, dus bekijk ze eens...

Ik heb ook het gelinkte programma bijgevoegd (RELADEMO.COM).
Het  programma geeft  een piepje,  wacht op een toetsdruk en
drukt als  laatste 20  keer het ingedrukte teken af. Op zich
niet  echt nuttig,   maar het  gaat dan  eigenlijk ook om de
listings.


                                         Rudy Oppers
