                 M S X 2 / 2 +   V D P   C U R S U S   ( 1 1  )
                                                                
          
                              P A L E T S P L I T 
          
          We   gaan  deze   keer  een  voorbeeld  van  een  zogenaamde 
          paletsplit behandelen.  Dit is  een screensplit, waarbij het 
          palet wordt veranderd bij de screensplit. Hierdoor kunnen er 
          dan bijvoorbeeld 32 kleuren tegelijk op het scherm in SCREEN 
          5. Eerst maar even wat theorie.
          
          
                         P A L E T   A A N S T U R E N 
          
          De VDP heeft 16 paletregisters, die worden aangeduid met P#0 
          t/m  P#15.  Deze  paletregisters kunnen  niet direct  worden 
          beschreven,  en  het  is  helemaal  niet mogelijk  om palet- 
          registers uit te lezen.
          
          De paletregisters  kunnen alleen indirect worden beschreven, 
          en  wel door  eerst het  palet te selecteren door het nummer 
          naar R#16  te schrijven en vervolgens de paletdata naar Port 
          #2 te sturen (I/O poort &H9A).
          
          De  paletdata bestaat uit twee bytes, waarvan slechts 9 bits 
          van belang  zijn. Drie  bits voor rood, drie bits voor blauw 
          en  drie bits  voor groen.  De twee bytes met paletdata zijn 
          als volgt opgebouwd:
          
          MSB   7   6   5   4   3   2   1   0   LSB
                0   ----R----   0   ----B----             1e byte
                0   0   0   0   0   ----G----             2e byte
          
          Let  op, want  de volgorde is niet R,G,B zoals bij de COLOR= 
          instructie  in  BASIC maar  R,B,G. De  handigste notatie  om 
          paletbytes weer  te geven  is door  de eerste byte in hex te 
          zetten en de tweede gewoon in decimaal, dus zo:
          
          &HRB,G
          
          Waarbij  R, B  en G  zoals we  gewend zijn lopen van 0-7. De 
          kleur met R=3, B=7 en G=1 wordt dus:
          
          &H37,1
          
          Tot slot  geef ik  nog even  een voorbeeld  van de  complete 
          procedure  die moet  worden gevolgd  om de  BASIC instructie 
          COLOR=(13,7,2,5) in ML te maken:
          
                  DI
                  LD      A,13
                  OUT     (&H99),A
                  LD      A,16+128
                  OUT     (&H99),A        ; selecteer P#13
                  EI
                  LD      A,&H75          ; R=7, B=5
                  OUT     (&H9A),A
                  LD      A,2             ; G=2
                  OUT     (&H9A),A
          
          De  DI  en  EI  instructies zijn  nodig omdat  de interrupts 
          altijd  uit  moeten  staan  als  we  naar  een  VDP register 
          schrijven. Bij  het schrijven  van de  paletregisters is dit 
          alleen  nodig  als  er  op  de  interrupt  ook  iets  met de 
          paletregisters  zou kunnen  worden gedaan. In dat geval moet 
          de EI verhuizen naar het einde van de routine.
          
          Tot  slot  moeten we  nog weten  dat de  paletpointer (R#16) 
          automatisch wordt  verhoogd na  het ontvangen van twee palet 
          bytes  van Port  #2 en  daarbij na P#15 weer verder gaat bij 
          P#0. Als  we bij  bovenstaand voorbeeld dus ook nog kleur 14 
          zouden   willen  veranderen,  kunnen  we  volstaan  met  het 
          schrijven van de twee paletbytes naar &H9A.
          
          
                             S C R E E N S P L I T 
          
          De theorie over het palet is nu in voldoende mate behandeld, 
          maar  voor  een  paletsplit  hebben we  ook een  screensplit 
          nodig. Ik prefereer de zogenaamde "polling screensplit". Dit 
          zegt  u  waarschijnlijk  weinig,  tenzij  u  de  tekst  over 
          Interrupt  Service  Routines  op  Sunrise  Special  #2  hebt 
          gelezen. Daar wordt namelijk uitgelegd wat polling is.
          
          Er zijn  twee manieren  om een screensplit te detecteren. De 
          eerste  manier  is  door  de  VDP de  opdracht te  geven een 
          interrupt  te genereren zodra hij bij een bepaalde beeldlijn 
          is bij de schermopbouw. De computer springt bij de interrupt 
          naar  adres  &H38  en  komt zo  vanzelf bij  de hook  &HFD9A 
          terecht. Als je die hook afbuigt met je eigen routine kun je 
          de screensplit verder afhandelen.
          
          Er is echter een veel simpeler manier, die bovendien sneller 
          reageert op  de screensplit,  en dat is polling. Bij polling 
          wacht  je  gewoon  totdat  de  VDP  bij  de  juiste  lijn is 
          aangeland  met de  schermopbouw. Als  je dit een beetje slim 
          uitkient  neemt  het ook  nog eens  minder processortijd  in 
          beslag  dan  de  interruptmethode.  Bij de  interruptmethode 
          worden namelijk alle registers op de stack gePUSHt, en wordt 
          er flink  wat heen  en weer  gesprongen voordat  de computer 
          eindelijk bij jouw screensplitroutine is. Bij polling is dat 
          allemaal niet nodig.
          
          
                 S C R E E N S P L I T   M E T   P O L L I N G 
          
          Hoe  werkt nu  zo'n screensplit met polling. Vrij eenvoudig. 
          Je moet  het programma  zo programmeren dat je per interrupt 
          werkt. Aan het begin zet je een HALT neer. Een HALT wacht op 
          een  interrupt. Dit  is normaal gesproken altijd de 50 of 60 
          Hz interrupt,  die optreedt  als de  VDP met de schermopbouw 
          bij  lijn 212  is aangeland.  HALT laat  die interrupt eerst 
          uitvoeren,  en   daarna  wordt  er  verder  gegaan  met  het 
          hoofdprogramma.
          
          Heb  je  bijvoorbeeld de  replayroutine van  MoonBlaster aan 
          &HFD9F hangen,  dan wordt de muziek dus eerst afgespeeld. Na 
          de  HALT  doe  je  de  scherminstellingen voor  het bovenste 
          gedeelte van het scherm. In dit geval is dat dus het naar de 
          VDP sturen van het palet voor het bovenste gedeelte.
          
          Vervolgens  ga  je  andere  dingen  doen  (bijvoorbeeld  het 
          toetsenbord uitlezen,  berekeningen doen,  etc.), en je mikt 
          het  ongeveer zo uit dat de computer daarmee klaar is als de 
          VDP met de beeldopbouw bij de screensplit is aangeland.
          
          Nu zet je de lijn voor de screensplit in R#19. De VDP zal nu 
          bit 0  van S#1  zetten zodra  hij bij die lijn is. Je checkt 
          dus  bit 0  van S#1  net zolang totdat het op 1 springt. Dan 
          stuur je  het palet  voor de  onderste helft  van het scherm 
          naar  de VDP. Vervolgens heb je nog tijd om andere dingen te 
          doen. Als je er maar voor zorgt dat de computer weer op tijd 
          bij de HALT is.
          
          
                   M E E R D E R E   S C R E E N S P L I T S 
          
          Het  grote   voordeel  van  deze  methode  is  dat  meerdere 
          screensplits  geen enkel  probleem zijn. Gewoon nog een keer 
          het screensplitroutinetje  erin zetten,  alleen dan  met een 
          andere lijn.
          
          Bij  de interruptmethode wordt dit toch ingewikkelder, omdat 
          de computer  bij elke interrupt (ook de 50/60 Hz interrupt!) 
          naar  &HFD9A springt,  zodat je  in die  routine eerst  moet 
          uitzoeken welke screensplit het eigenlijk is.
          
          Genoeg  theorie, we gaan gauw verder met het voorbeeld. Hier 
          komt   de   assemblerlisting,  zoals   gewoonlijk  rijkelijk 
          voorzien van uitleg.
          
          
          ; PALSPLIT.ASM
          ; Voorbeeld paletsplit: 32 kleuren op het scherm
          ; VDP Cursus Sunrise Magazine #6
          ; Stefan Boer 18/01/93
          ; (c) Ectry 1993
          
          LINE:   EQU   106
          
                  ORG   &HD000
                  DI
                  XOR   A
                  OUT   (&H99),A
                  LD    A,16+128
                  OUT   (&H99),A        ; P#0 selecteren
          
          
          Hier wordt  P#0 geselecteerd. Dit hoeven we slechts ��n keer 
          te  doen, omdat  we toch  elke keer  alle 16  paletregisters 
          beschrijven. Na afloop staat de paletpointer dan immers toch 
          weer op 0. Hierna begint de hoofdlus:
          
          
          UPPER:  EI
                  XOR   A
                  CALL  &HD8            ; spatiebalk testen
                  RET   NZ
          
          
          Door de  routine GTTRIG  op adres  &HD8 aan te roepen met de 
          waarde  0  in  het A  register vragen  we de  status van  de 
          spatiebalk  op.  Als  de  spatiebalk  is ingedrukt  wordt de 
          routine be�indigd.
          
          
                  HALT                  ; wacht op interrupt
          
          
          Hier   wordt  op   de  interrupt  gewacht,  dit  is  dus  de 
          'screensplit' op lijn 212.
          
          
                  DI
                  LD    HL,PALET1       ; palet voor bovenste helft
                  LD    BC,&H209A       ; 32 bytes naar poort &H9A
                  OTIR                  ; stuur palet naar VDP
          
          
          De  interrupts  worden  uitgezet, al  is dat  eigenlijk niet 
          nodig omdat er in de praktijk toch geen interrupts optreden. 
          Vervolgens  wordt het  palet met een OTIR instructie naar de 
          VDP gestuurd. Omdat we in dit voorbeeld verder toch niets te 
          doen hebben gaan we gelijk verder met de screensplitroutine.
          
          
                  LD    A,LINE
                  OUT   (&H99),A
                  LD    A,19+128
                  OUT   (&H99),A        ; lijn voor screensplit
          
          
          Hier wordt de lijn voor de screensplit naar R#19 geschreven. 
          LINE is  aan het  begin van  de listing  met een  EQU op 106 
          gezet,  dat is  het midden  van het  scherm (want dat is 212 
          lijnen).
          
          
                  LD    A,1
                  OUT   (&H99),A
                  LD    A,15+128
                  OUT   (&H99),A        ; S#1 selecteren
                  NOP
                  NOP                   ; geef VDP tijd om data klaar
                                        ; te zetten
          LOWER:  IN    A,(&H99)        ; lees S#1
                  BIT   0,A             ; test op screensplit
                  JP    Z,LOWER         ; lijn nog niet bereikt
          
          
          We selecteren eerst S#1, zodat we die kunnen lezen door Port 
          #1 (poort  &H99) te  lezen. De  twee NOPjes zijn nodig om de 
          VDP  de kans  te geven  de juiste  data op  &H99 te  zetten. 
          Vervolgens wachten  we tot  bit 0  van S#1 op 1 springt. Dan 
          weten we dat de VDP bij lijn 106 is aangeland.
          
          
                  LD    HL,PALET2       ; palet voor onderste helft
                  LD    BC,&H209A       ; 32 bytes naar poort &H9A
                  OTIR                  ; stuur palet naar VDP
          
          
          We  sturen op  de inmiddels  bekende wijze het palet naar de 
          VDP.
          
          
                  XOR   A
                  OUT   (&H99),A
                  LD    A,15+128
                  OUT   (&H99),A        ; weer S#0 voor BIOS
          
          
          Dit  is  belangrijk,  voor  de  BIOS  moet  S#0  altijd zijn 
          geselecteerd. Om  het BIOS  zo snel mogelijk te houden wordt 
          er namelijk simpelweg IN A,(&H99) gebruikt om S#0 te lezen.
          
          
                  JP    UPPER
          
          
          En  tot slot  springen we weer naar de bovenste screensplit, 
          waar eerst  nog even  op de spatie wordt gecheckt. Nu nog de 
          paletten invullen:
          
          
          ; palet voor bovenste helft van scherm
          
          PALET1: DB    &H00,0,&H11,1,&H22,2,&H33,3
                  DB    &H44,4,&H55,5,&H66,6,&H77,7
                  DB    &H10,0,&H20,0,&H30,0,&H40,0
                  DB    &H50,0,&H60,0,&H70,0,&H77,0
          
          ; palet voor onderste helft van scherm
          
          PALET2: DB    &H00,0,&H01,0,&H02,0,&H03,0
                  DB    &H04,0,&H05,0,&H06,0,&H07,0
                  DB    &H00,1,&H00,2,&H00,3,&H00,4
                  DB    &H00,5,&H00,6,&H00,7,&H07,7
          
          
          De  hier gegeven data is natuurlijk volslagen willekeurig, u 
          kunt hier  zelf invullen  wat u  wilt. Deze  assmblerlisting 
          staat  als PALSPLIT.ASC  op de diskette, geassembleerd onder 
          de naam PALSPLIT.BIN.
          
          
                               V O O R B E E L D 
          
          In PALSPLIT.PMA zit het voorbeeldprogramma PALSPLIT.BAS, dit 
          programma  tekent 2  maal 15 verticale balken in SCREEN 5 om 
          de  routine  uit  te  testen.  Er  zijn dan  dus 31  kleuren 
          tegelijk  op  het  scherm  (30 balken  + achtergrondkleur  = 
          zwart).
          
          
                        N E T T E   P A L E T S P L I T 
          
          U  zult  bij  het  voorbeeld  helemaal niets  merken van  de 
          paletsplit. Dit komt omdat de split ergens bij lijn 106 zit, 
          en  daar zit alleen kleur 0. Ik heb kleur 0 express in beide 
          paletten hetzelfde  gehouden, zodat de overgang niet te zien 
          is.
          
          Kijk maar eens wat er gebeurt als u kleur 0 wel verschillend 
          maakt,  dan kun  je duidelijk de screensplit zien zitten. Je 
          ziet ook dat hij trilt.
          
          Bij andere  screensplits kun  je de  split wegwerken door op 
          een  bit van  een statusregister te checken dat aangeeft dat 
          de VDP  begint met  het opbouwen van de lijn. Omdat dat niet 
          precies  klopt  moet  er  dan  ook nog  een wachtlusje  bij. 
          Waarschijnlijk  kom ik  daar in  een later  deel van  de VDP 
          cursus nog wel eens op terug.
          
          Bij een  paletsplit is  de hier  gebruikte oplossing  echter 
          beter,  het kost  vrijwel geen  moeite en  je ziet  de split 
          absoluut niet.  Zorg dus  dat kleuren  die in  het overloop- 
          gebied  voorkomen in  beide paletten hetzelfde zijn, dan zie 
          je niets van de paletsplit.
          
          
                             5 1 2   K L E U R E N 
          
          Ik denk  dat het  mogelijk is om met behulp van deze routine 
          alle  512 kleuren tegelijkertijd op het scherm te krijgen in 
          SCREEN 5,  door gewoon  het screensplitstuk een flink aantal 
          keren  achter elkaar  te zetten  en voldoende  verschillende 
          paletten toe te voegen. Ik heb het zelf nog niet geprobeerd, 
          misschien een andere keer.
          
          Veel succes  met het  experimenteren met deze routine en tot 
          de volgende keer.
          
                                                          Stefan Boer
