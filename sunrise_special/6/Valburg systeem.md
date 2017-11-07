                         V A L B U R G - S Y S T E E M


          Het  begon allemaal  toen ik  las dat  Alex v.d.  Wal Gianna
          Sisters professioneel  noemde, ik  voelde me meteen geroepen
          om  ook eens  iets te  schrijven over  mijn aanpak van grote
          projecten. Maar we beginnen bij het begin.

                           G I A N A   S I S T E R S

          Giana Sisters  is eigenlijk  niet erg professioneel opgezet.
          Ik ben begonnen in WBASS2 met de scroll-routine. Dat was een
          heel  elementair iets  dat ik af moest hebben voordat er met
          iets   anders    kon   worden    begonnen.   Daarna   kwamen
          achtereenvolgens erbij: de sprite routines, besturing van de
          speler  en  botsen  van  de  speler  tegen  de  achtergrond,
          vijanden,   interactie  van   vijanden  met  de  speler,  en
          vervolgens werden  alle onderdelen  uitgewerkt tot  het spel
          dat het nu is.

          Ik  heb natuurlijk ook een editor gemaakt om de velden in te
          ontwerpen, en  een converter om de SCREEN 4 karakters aan te
          maken.  Langzamerhand bleek  dat de  source te groot werd om
          nog in WBASS2 mee te werken. Ik moest toen tegen mijn zin in
          overschakelen  op  GEN80.  Dus  eerst  naar  DOS,  de  tekst
          editten, GEN80,  naar BASIC,  uitproberen, werkt  niet, naar
          DOS, etc.. Achteraf gezien vrij omslachtig.

          Op een gegeven moment heb je dan de speelroutine zo'n beetje
          wel  af en moet je bezig gaan met een programma eromheen dat
          highscores doet,  stages inlaadt,  etc.. Ik heb er toen voor
          gekozen  om daar  een los programma van te maken dat de main
          in  kon  laden.  Geen  kernel  dus,  geen  eigen  BIOS, niks
          daarvan.  Wel  werd  de  BIOS  bijna  geheel gepasseerd.  Ik
          gebruikte hem  alleen nog voor schermwisselingen en joystick
          poort uitlezen.

          Het  resultaat mag  er zijn. Een echte Mario-kloon waar heel
          MSX'end Nederland  zo op  zat te  wachten. Maar veel kritiek
          kreeg ik wel. Het was grafisch niet mooi genoeg, ik had niet
          genoeg  fade-in en  fade-outs erin zitten en Loek van Kooten
          gaf nog  kritiek op  het uit Friesland afkomstig zijn van de
          club.  ("gemene valletjes  bedacht door  de makers, maar die
          komen dan ook maar uit Friesland")


                    T O T A A L   A N D E R E   A A N P A K

          Er is  nogal wat  veranderd na  dat tweede  spel. Ik  zal nu
          eindelijk  to the  point komen  en vertellen hoe ik vind dat
          het echt moet, en waarom.

          Allereerst ben  ik me  vreselijk gaan afzetten tegen WBASS2.
          Door  het gemis  van MACRO's,  relocatable files,  een goede
          INCLUDE functie,  door het te kleine werkgeheugen en doordat
          het onder BASIC werkt. Snel gaat het wel, en voor beginnende
          ML  programmeurs of  demomakers is  het helemaal  zo gek nog
          niet.

          Programma's  maken  gaat  het  beste  onder DOS.  Liefst met
          ramdisk  of HD  en met een tR of 7 MHz als het even kan. DOS
          heeft het  voordeel dat  er 2 keer zoveel ruimte beschikbaar
          is  als onder  BASIC. Natuurlijk  kan je  onder BASIC ook de
          interpreter wegschakelen en de BIOS weghalen, maar onder DOS
          is dat  al gebeurd,  dus waarvoor  al die  moeite? En als je
          toch   de  BIOS   wilt  gebruiken  dan  doe  je  gewoon  een
          interslotcall. Over het algemeen zal je de functies die snel
          moeten zijn toch al wel zelf hebben gemaakt.

          Ook draaien  programma's die  je onder  DOS hebt gemaakt ook
          meteen goed vanuit de bootsector. Gewoon het BDOS jump adres
          naar  05H copieren  en starten maar. Alle RAM pagina's staan
          al  voorgeschakeld.  Of rename  je de  .COM file  liever als
          COMMAND.COM? MSXDOS.SYS erbij en dan is het meteen klaar.

          Dan  het idee  van een  eigen kernel.  Het idee  is leuk  en
          klinkt heel  professioneel en interessant, maar eigenlijk is
          het  alleen handig  bij diskmagazines.  Daar heb je nl. vaak
          heel  veel  programma's met  allemaal dezelfde  routines. In
          andere gevallen  komen de  routines maar een keer voor en is
          het   alleen   maar   onhandig   om   al   die   overtollige
          niet-gebruikte routines in pagina 0 mee te sjouwen.

          Daarom beveel  ik ook  aan om  libraries te maken met daarin
          alle  routines die je anders in een BIOS/kernel zou stoppen.
          Het enorme  voordeel hiervan  is dat precies de routines die
          jij in je programma aanroept uit de library worden gevist en
          aan  je programma worden geplakt! Zo gebruik je dus niets te
          veel aan  geheugen en  hoef je  ook geen kernel in te laden!
          Voor  mij is dat nu al de ideale manier en volgens mij weten
          een heleboel  mensen niet  dat een  linker dat kan. Lees het
          voorgaande   dan  nog  maar  eens  goed  want  het  is  echt
          superhandig.  Een   linker  is  een  zeer  vernuftig  stukje
          software dat modulair programmeren tot kinderspel maakt.

          Dan  komen  we  nu  bij  het  probleem van  het filesysteem.
          Eigenlijk  heb  ik  dat  hiervoor  al  opgelost. Gewoon  een
          library  functie  die  een  file  op  een bepaald  adres kan
          inladen. Als het nu op sector moet komen te staan vervang je
          de library  functie door  een sector lader met erbij gelinkt
          de  data met  de plaats van de files. Aan het programma zelf
          hoeft niets te worden veranderd, gewoon opnieuw linken en je
          programma laadt op sectorniveau.


               P L A A T J E S   E N   M U Z I E K   L I N K E N

          In  plaats van  inladen zou je natuurlijk ook de plaatjes en
          muziek in je programma kunnen stoppen. Alleen zit je dan met
          het probleem  waar? Je  kan het  natuurlijk heel precies aan
          het einde van je programma zetten, maar als je programma dan
          wat  groter  wordt  moet  je  het  adres  weer aanpassen  en
          bovendien  dat  hele  plaatjes  er  weer  aan  CONCATten  of
          misschien  zelfs met de hand eraan plakken als je geen DOS 2
          hebt.

          Je  raadt  al wat  de oplossing  is... linken.  maak van  de
          muziekfile of  grafische file een .REL file. Eraan linken en
          de  linker plakt  alles precies  op de  juiste plaatsen  aan
          elkaar  en  vult  daarna  de  juiste  adressen  in  in  jouw
          programma. Handiger kunnen we het niet maken.

          Misschien denk je nu wel "te mooi om waar te zijn, dat duurt
          veel  te lang  en bovendien  is het alleen maar theorie. Hij
          gebruikt zelf vast WBASS2". Helemaal mis! Het linken gebeurt
          razendsnel. Over het algemeen doet de linker er nooit langer
          over dan  30 seconde,  en dan  zoekt ie dus library's uit en
          plakt  ie plaatjes  aan je programma en alles. Het tweede is
          ook niet  waar want  zelf heb  ik (samen met Jouke Dijkstra,
          die  mag wel  eens genoemd  worden) alles  al in de praktijk
          gebracht. Ik heb zelf al een programma gemaakt waarin in een
          file alle  muziek, plaatjes  etc. erbij  zitten en dan staat
          het ook nog een op sector en wordt ingeladen vanuit de boot.
          (zonder MSXDOS.SYS)


                   J A ,   I K   W I L   O O K   L I N K E N

          Dat  kan, join  the club.  Als je  interesse hebt in hoe dit
          alles  in  de  praktijk  in  zijn  werk  gaat,  of  je  hebt
          belangstelling voor  de library's en programma's waarmee dit
          alles  moet worden  bewerkstelligd dan  hoef je  alleen maar
          even  contact  op te  nemen met  mij via  post, telefoon  of
          echomail, uploads/downloads alleen via Piramide BBS.

                                                      Jan van Valburg
