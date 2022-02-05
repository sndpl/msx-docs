                            M E M M A N

                  I N   T H E O R I E   E N   P R A K T I J K


                               I N L E I D I N G

          Onder  MSX-DOS  1  laat  het  geheugenbeheer,  met  name bij
          aanwezigheid van een memory mapper, veel te wensen over. Met
          de  komst van  MSX-DOS 2 is dat sterk verbeterd, daar zitten
          immers routines  in om een pagina uit de memory mapper op te
          roepen  en weer vrij te geven. Volgens de deskundigen zit er
          echter   een   bug  in   deze  routines,   waardoor  eenmaal
          opgevraagde pagina's  nooit meer  vrijgegeven kunnen worden,
          tenzij de machine wordt gereset.

          Zo is MEMory MANager geboren. De programmeurs van MST hadden
          de behoefte aan een stel goed werkende routines, waarmee het
          gehele geheugen  benut kan  worden. Ze gingen echter verder.
          Meerdere  memory mappers,  externe geheugen modules (16 & 64
          kB)  en  TSR's. Welnu  over dit  laatste onderwerp  gaat dit
          artikel.


                         W A T   I S   E E N   T S R ?

          Gewoonlijk  zal  een  programma  na uitvoering  niet in  het
          geheugen achterblijven. Programma's die dat wel doen, worden
          aangeduid met de afkorting TSR: Terminate and Stay Resident.
          Voorbeelden van  dergelijke programma's  zijn printerbuffers
          en   RAMdisks.  Maar  ook  andere  toepassingen,  zoals  een
          rekenmachine  of  een kalender  die met  een toetscombinatie
          opgeroepen kan worden, zijn denkbaar.

          In het verleden zijn TSR's voor de MSX een tamelijk zeldzaam
          verschijnsel  geweest.  Het  probleem  was namelijk  dat het
          geheugen dat  de TSR  gebruikt, ook  door andere programma's
          gebruikt  kan worden.  Er zijn  in een standaard MSX machine
          geen  mogelijkheden  om een  stuk geheugen  voor een  TSR te
          reserveren. Dit  probleem wordt  door MemMan  uit de  wereld
          geholpen.  MemMan beheert  het geheugen en zorgt er voor dat
          er geen geheugenconflicten optreden.

          Dankzij MemMan  is het  mogelijk meerdere  TSR's tegelijk in
          het  geheugen te  hebben, waarbij  elke TSR  maximaal 16  kB
          groot kan  zijn. Op  de standaard  MSX is het laden van meer
          dan  ��n TSR al lastig en alleen mogelijk als de TSR niet al
          te groot  is. Met  de invoering  MemMan krijgt de MSX betere
          TSR  mogelijkheden dan  de alom  gewaardeerde PC.  Bovendien
          doen ze niet onder voor de 'desktop accesoires' zoals die op
          de Macintosh en de Atari ST gebruikt worden.

          Dit  hoofdstuk is  regelrecht overgenomen uit de handleiding
          van MemMan, zoals die is geschreven door het MST.


                          N U   D E   P R A K T I J K

          Dat klinkt  natuurlijk mooi,  maar werkt  het in de praktijk
          ook zo soepel. Ik hoop in de rest van dit artikel een aantal
          problemen  boven water  te halen,  waaruit blijkt dat MemMan
          heel  leuk  is, maar  nog niet  af. Daarentegen  wil ik  wel
          iedereen blijven  aanmoedigen om MemMan te blijven (of gaan)
          gebruiken, want werken zonder MemMan is helemaal een gruwel.


                              P R O B L E E M   1

          Een hoop  bestaande programma's werken niet samen met MemMan
          en  profiteren dus  niet van  het bereikbaar worden van meer
          geheugen. Bovendien  werken veel  programma's niet  eens als
          MemMan actief is.

          Het eerste gedeelte van het probleem is lastig op te lossen.
          Het  betekent dat  �f het programma herschreven moet worden,
          �f er  moet een  alternatief programma  gebruikt worden, dat
          wel met MemMan samenwerkt.

          Het  tweede gedeelte  van het  probleem is  veel leuker. Ook
          hier herken je weer twee probleemgroepen.

          De eerste  groep zijn  de vastlopers.  Als je je bedenkt dat
          MemMan  binnen  de  grenzen  van  de  MSX  standaard  werkt,
          betekent  dit dat  de vastlopende  programma's dat  dus niet
          doen. Programma's  die het  gehele systeem voor zich opeisen
          en  denken dat  ze ermee  mogen doen  wat ze  willen. Helaas
          bestaan ze.

          De  tweede   groep  is   het  absolute   toppunt  voor   m'n
          lachspieren.  Het programma  geeft kort na het opstarten een
          'Not  enough  memory'  error  en  het  programma stopt.  Het
          probleem ligt hier in het TPA geheugen.

          Wat is  TPA geheugen?  Dat is de 64 kB die voor de processor
          direct  zichtbaar is.  De rest  van het geheugen is, net als
          deze 64  kB, onderverdeeld in blokken van 16 kB. Door nu ��n
          van  die blokken uit het TPA geheugen te verwisselen met een
          blok uit  de memory mapper, ziet de processor weer een ander
          stukje data of programma. Om dit in goede banen te leiden is
          MemMan ontwikkeld. Maar om goed te kunnen functioneren heeft
          MemMan  permanent  een  stukje  geheugen  nodig,  zoals  ook
          MSXDOS(2).SYS   en  COMMAND(2).COM   zich  in  dit  geheugen
          bevinden, maar  ook de stack e.d.. Wat er nu nog overblijft,
          is  voor  een  in te  laden programma  en z'n  te declareren
          variabelen.  Door MemMan  is dat  vrije TPA geheugen nu zo'n
          1,5  kB  kleiner geworden.  Voor sommige  programma's genoeg
          voor de eerder genoemde error.

          Dit  laatste  is  alleen  te  verhelpen  door het  programma
          opnieuw te  compileren, met een verkleind vrij TPA geheugen.
          Hiervoor heb je dan wel de source van het programma nodig en
          dat  is vaak een probleem. Als het om een commercieel pakket
          gaat,  is   die  source   simpelweg  onbereikbaar   voor  de
          eindgebruiker. Het bedrijf wenst niet meer aan het programma
          te  sleutelen (veel voorkomend commentaar nu MSX commercieel
          niet meer  interessant is)  en wil  alleen voor grof geld de
          source verkopen. Als het om een PD of shareware pakket gaat,
          zou  je de  schrijver ervan  kunnen benaderen met het eerder
          genoemde  verzoek.  Sommigen  zullen  er  gehoor  aan  geven
          anderen niet.  Persoonlijk vind ik dat elke programmeur hier
          vooraf  rekening mee  moet houden  en dus  standaard met een
          verkleind TPA geheugen moet compileren.

          Dit probleem  is dus  niet de schuld van de programmeurs van
          MST.  Programma's welke  voor het MemMan tijdperk geschreven
          zijn, kunnen moeilijk verweten worden dat ze niet met MemMan
          samenwerken. Maar  de overige moeilijkheden van dit probleem
          zijn simpel te wijten aan het niet naleven van de standaard,
          of  het  niet  wensen  rekening  te houden  met MemMan.  Die
          programmeurs  welke in  deze categorie thuis horen, zijn wat
          mij betreft amateurs.

          Nvdr.  Er is  natuurlijk ook  nog een categorie: programma's
          waarvoor het samenwerken met MemMan onzin is. Bijv. spellen,
          demo's en ook deze Special! Al zou de printerbuffer wel leuk
          zijn bij het uitprinten.


                              P R O B L E E M   2

          TSR's zijn  vaak op te roepen d.m.v. een toetscombinatie. In
          diverse  programma's  wordt  ook  gebruik  gemaakt  van  een
          toetscombinatie. Wanneer er een TSR actief is welke dezelfde
          toetscombinatie  gebruikt als  een bepaalde  functie uit het
          lopende programma,  heb je  een probleem.  Welke van de twee
          wint  nu het  gevecht, het  programma of  de TSR.  Oplossing
          hiervoor is simpel. Toetscombinaties zijn voor TSR's. In een
          normaal programma  is het  gebruik van  toetscombinaties dus
          uit den boze.

          Ook hier geldt hetzelfde als bij probleem 1. Programma's van
          voor  het MemMan  en TSR  tijdperk, kunnen dit niet verweten
          worden. Maar  programma's die ontwikkeld zijn toen MemMan en
          TSR's  wel reeds  bestonden, hadden  er rekening  mee moeten
          houden. De  programmeurs van  deze programma's zijn dus hier
          ego�stisch bezig geweest.


                              P R O B L E E M   3

          TSR's  die elkaar  in de haren vliegen, of de combinatie van
          programma  en  TSR die  elkaar niet  liggen. Oorzaak:  de al
          eerder  genoemde  toetscombinaties,  maar  ook  het  actieve
          SCREEN speelt  een rol.  Dit laatste  punt heb  ik lang over
          nagedacht,  maar ik  denk dat  hier, in tegenstelling tot de
          andere  moeilijkheden,   een  taak   is  weggelegd  voor  de
          programmeurs van MemMan.

          Sommigen  van jullie kennen ongetwijfeld de TSR Micro Music.
          Deze TSR zet na een bepaalde toetscombinatie een menu op het
          scherm.  Welnu  de  programmeur  heeft  hier  geen  rekening
          gehouden met het actieve screen en gaat er simpel vanuit dat
          SCREEN  0  actief  is.  Gelukkig is  hij niet  de enige  TSR
          programmeur  die  deze fout  maakt en  het is  dus ook  geen
          persoonlijke aanval  op de programmeur van Micro Music. Deze
          TSR wordt alleen als voorbeeld genomen.

          Maar  ga eens  in de positie van de programmeur staan. Eerst
          uitzoeken  welk  SCREEN  actief  is, vervolgens  kijken welk
          gedeelte je  daarvan wil  gebruiken, dan  de aanwezige  info
          ergens  opslaan en de nieuwe info plaatsen. Bij het verlaten
          van de  TSR, moet  dan de omgekeerde weg gevolgd worden. Als
          je  dan ook nog eens toevallig in een modem programma zit en
          het scherm  is constant aan verandering onderhevig, dan zijn
          de rapen helemaal gaar.

          Het is dus veel eenvoudiger om te zeggen dat zo'n TSR alleen
          werkzaam is  in SCREEN  0. Sommige  programmeurs kunnen  dit
          zelfs niet eens goed (Micro Music laat bij een actieve jANSI
          een  paar gekleurde  blokjes zien boven in het scherm), maar
          dat is een ander verhaal. MST zou hier kunnen helpen.

          Wanneer er  in MemMan  een routine  zit, die  voor TSR's een
          universele   manier  aanbiedt  voor  het  plaatsen  van  een
          karakter op  een op te geven positie op het scherm, ongeacht
          welk  SCREEN actief is en de oude informatie van die positie
          bewaart,  zijn  we  al  een  hele  stap  verder.  Nu kan  de
          programmeur van  een TSR  simpel tegen MemMan zeggen: ik wil
          dit  karakter op  die positie.  Wanneer MemMan  dan bijhoudt
          welke  posities   gebruikt  worden   door  de  TSR,  kan  de
          programmeur  van de  TSR voor  het verlaten  van de TSR deze
          posities weer herstellen door aan MemMan de opdracht daartoe
          te geven.

          De  ideale   oplossing  voor   bijna  alle  problemen.  Veel
          programma's  wijzigen niets  op het  scherm, wanneer een TSR
          actief is. Voor die gevallen dat zowel een programma als een
          TSR  actief   zijn,  die   beiden  het   scherm  geheel   of
          gedeeltelijk wijzigen, wordt het praktisch onmogelijk om dit
          af  te  vangen.  Die  programma's  die intensief  met MemMan
          samenwerken,  zouden  aan  MemMan de  door de  TSR gebruikte
          posities  kunnen opvragen  en deze  dan niet gebruiken, maar
          dat kan  soms onoplosbare  problemen met  zich mee  brengen.
          Denk hierbij alleen maar aan de pulldown menu's e.d..

          Dus  de  hier  voorgestelde uitbreiding  van MemMan  is niet
          alles  dekkend,  maar  is een  hele grote  stap in  de goede
          richting.


                               C O N C L U S I E

          Wat is het leven met MemMan en speciaal hiervoor ontwikkelde
          programma's   toch   een   genot.   Helaas   laten   diverse
          programmeurs  nog wel eens een steekje vallen. Wanneer ze de
          inhoud van  dit artikel  nu eens  als wet gaan beschouwen en
          wanneer  MST de  voorgestelde uitbreiding van MemMan nu eens
          bewerkstelligt, zijn we bijna waar we zijn moeten:

                  Een systeem wat op een bijna optimale manier gebruik
                  maakt van alle mogelijkheden die het systeem bied.

          Tot  slot nog ��n extra opmerking voor de programmeurs. Wees
          nu eens  niet zo  eigenwijs door  er van  uit te gaan dat de
          aangesloten diskdrives A: en B: heten, maar vraag dit gewoon
          aan het systeem. Harddisk gebruikers hebben A: en B: vaak in
          gebruik  als een  partitie van  de harddisk en de diskdrives
          heten dan  (in mijn  geval) F:  en G:.  Maar het gebruik van
          vele kopieerprogramma's is hierdoor niet meer mogelijk.

                                                       Henk Hoogvliet
