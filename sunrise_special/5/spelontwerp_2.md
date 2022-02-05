                          S P E L O N T W E R P   2 ? 
                                                       
          
          Ongeveer een  week na het uitbrengen van een Sunrise Special 
          breekt  bij ons  thuis de  hel los.  De brievenbus heeft dan 
          plotseling last  van chronische indigestie. Zozeer zelfs dat 
          de  postbode speciaal  met de wagen langs komt om de fanmail 
          in bulk  af te  leveren. Nog  dramatischer zijn de taferelen 
          bij  onze onrechtmatig  gedigitaliseerde hoofdredacteur. Bij 
          hem  wordt   de  fanmail  op  dergelijke  dagen  op  pallets 
          aangeleverd.  Dit nadat een dozijn postbodes hernia's hebben 
          opgelopen bij  het afleveren  van postzakken lofbetuigingen. 
          Bij  deze wil  ik (ook  namens Kasper) al mijn fans bedanken 
          voor de  vage bijschriften op mijn girorekening alsmede voor 
          de    overvloedige   huwelijksaanzoeken??    [Nvdr.   Zoveel 
          vrouwelijke of (XOR) homofiele MSX'ers zijn er toch niet?]
          
          U ziet,  enig cynisme  is ons  niet vreemd.  Zelden of nooit 
          komt   er  ook  maar  ��n  letter  commentaar  (positief  of 
          negatief)   op   de   door   ons   gegenereerde   technische 
          computerromans. Je  hebt meer  kans dat je met je ogen dicht 
          intelligent  leven in  de ruimte ontdekt dan dat er ook maar 
          een vleugje van een reactie waar te nemen is na de produktie 
          van een SRS.
          
          Schertste daarom ook mijn verbazing toen ik een poos geleden 
          plotseling wel  enthousiaste mensen  op mijn  stoep vond die 
          graag  wilden dat  ik meer  over een  bepaald onderwerp  zou 
          schrijven. Het  ging daarbij over een vermeende serie die ik 
          zou gaan maken met als thema: "het ontwerpen van een spel".
          
          Dat onderwerp had ik in een opwelling eens bedacht, en onder 
          het  motto 'It  seemed like  a good idea at the time' ben ik 
          toen als  een bezetene  aan een  20 delige  cursus begonnen. 
          Veel verder dan deel 1 ben ik echter nooit gekomen. De reden 
          daarvoor  was dat  alhoewel ik  me wel  een voorstelling kon 
          maken over  hoe je  zoiets aanpakt  ik er  nog geen serieuze 
          praktijkervaring  in had.  Feitelijk stond  ik jullie dus de 
          les te  leren over  een onderwerp  waar ik zelf niets van af 
          wist.  Zelfs ik begrijp dat zoiets voorbestemd is om fout te 
          gaan en  daarom heb  ik toen besloten om met die onzin op te 
          houden.
          
          Heden  doet zich  echter de situatie voor dat die praktische 
          kennis wel aanwezig is. Door de dramatische reakties op mijn 
          eerste  poging  leek  het  me  toch  een  goed  idee om  het 
          verdronken  kalf   uit  de   gedempte  put   te  graven.  In 
          tegenstelling    tot    de    vorige   keer    zullen   deze 
          softwaretechnische  verhandelingen zich  in eerste instantie 
          niet expliciet  op het  ontwerpen van een spel richten, maar 
          meer   op  randvoorwaarden   die  van  belang  zijn  bij  de 
          totstandkoming van  een groot software projekt. Indien ik de 
          maanden  die zullen  komen niet  aan een  terminale situatie 
          onderworpen zal  worden beloof  ik plechtig om mijn uiterste 
          best  te  doen  om  een  aantal  samenhangende  artikelen te 
          schrijven.
          
          
             W A T   G A A N   W E   D O E N   E N   W A A R O M ? 
          
          Wat ik  wil doen  is het behandelen van een aantal problemen 
          die  bij ELK  groot software  projekt aan de orde komen. Het 
          beste  voorbeeld  daarvan  is  bijvoorbeeld het  beheren (en 
          beheersen) van  data. Theoretisch  is het  zeer eenvoudig om 
          een  geheugen van  willekeurige grootte tot de nok te vullen 
          met relevante  data, maar praktisch kleven daar wat haken en 
          ogen  aan. Hoe  onthoud je  waar welke gegevens staan en hoe 
          zorg je  er voor  dat deze  gegevens elkaar  niet in  de weg 
          zitten. Ook moet je er voor zorgen dat de uiteenlopende data 
          op  het juiste moment op de juiste plaats aanwezig zijn, wat 
          dan  ook  nog met  conversieslagen gepaard  gaat (zoals  het 
          decrunchen van grafische data).
          
          Hopelijk zie  je al  dat de  te behandelen problematiek niet 
          van  toepassing is  op het  gemiddelde BASIC programma, maar 
          eerder  op  het soort  programma dat  minimaal 128  kB nodig 
          heeft om  te kunnen  werken. Waarom zou zo'n groot programma 
          meer   problemen  veroorzaken   dan  het   gemiddelde  BASIC 
          programma? Dat  heeft te  maken met het feit dat de door het 
          systeem  in  ROM  aangeboden  routines  voor  dataverwerking 
          plotseling  niet  meer  voldoende blijken.  In BASIC  kun je 
          vrolijk van legio voorgebakken commando's gebruik maken, die 
          dan  in assembly  plotseling niet  meer eenvoudig voorhanden 
          zijn.
          
          Samengevat  kun je  stellen dat  het nodig  is om  een eigen 
          'systeem' van  eigen voorgebakken  routines te  maken om een 
          groot  project in  assembly (of  elke andere  taal die  niet 
          BASIC heet)  te kunnen realiseren. Het door BASIC aangeboden 
          systeem is simpelweg niet langer bruikbaar.
          
          
          De (voorlopig) te behandelen onderwerpen zijn:
          
          - Opbouw van het eigen systeem
            o een standaard filesysteem en kennismaking met een aantal
              relevante programmeerstrategie�n.
            o programmeer- en ontwikkelomgevingen
              * keuze van en eisen aan de omgeving
              * mogelijkheden van een omgeving
              * algemene opbouw
            o geheugen management
            o interrupt management
            o opbouw van het geheugen
            o debugging
          
          Bij   de    behandeling   van    deze   onderwerpen   zullen 
          waarschijnlijk  geen  programma's  gegeven worden.  De reden 
          hiervoor  is dat  praktisch blijkt dat een te beperkt aantal 
          mensen dat als nuttig beschouwd.
          
          Mochten er nu mensen zijn die denken:  "Leuk,  maar  ik  zou
          graag ook wat over onderwerp  'X'  willen  weten",  dan  kan
          hij/zij dergelijke wensen bij de redactie  deponeren  waarna
          ik ze zal behandelen.
          
          De eerlijkheid  gebied me  te zeggen  dat we (= Fuzzy Logic) 
          reeds  beschikken over  een dergelijk  systeem. Dit  systeem 
          (F-KERNEL genaamd)  heeft een  aantal mogelijkheden  die het 
          uitermate  geschikt maakt om bijv. een RPG mee te maken. Het 
          systeem gaat  zo ver  dat op de BDOS na ALLE funkties van de 
          MSX-BIOS  zijn overgenomen.  Ik vermeld het maar even om aan 
          te  tonen  dat  ik  niet weer  uit mijn  nek sta  te zwammen 
          (alhoewel?)  [Nvdr.  Who  cares?  Nek-zwammen  kan  ook heel 
          interessant zijn!].
          
          
                                T O T   S L O T 
          
          H�t grote voordeel van zo'n eigen systeem is dat het (indien
          goed en doordacht ontworpen) herbruikbaar is voor van  alles
          en nog wat. Zodra je je eerste spel af hebt met zo'n systeem
          is het volgende  een  makkie  omdat  je  al  een  scala  van
          handige, snelle, ge�ntegreerde en bij de  gebruiker  bekende
          routines  hebt  die  veel  'dom'  werk  uit  handen   nemen.
          Natuurlijk  gaat  het  niet  alleen  om  spellen,  maar  ook
          utility's, muziekprogramma's en noem ze maar op  kunnen  met
          dergelijke  systemen  gemaakt  worden.   De   routines   die
          onderdeel maken  van  dat  systeem  moet  je  zien  als  een
          aanvulling op het  Operating  System  van  je  computer.  De
          routines die er in komen zijn de routines die JIJ  makkelijk
          in het gebruik vindt en die je vaak gebruikt. Samengevat kan
          je zeggen  dat een  vast eigen  systeem ervoor  zorgt dat je 
          voor elk project niet opnieuw het wiel hoeft uit te vinden.
          
          De ge�nteresseerden  kunnen hun  hart nu ophalen door deel 1 
          aan  een kritisch  onderzoek te  onderwerpen dat ook op deze 
          disk aanwezig  is. Bedenk echter wel dat geen commentaar van 
          de  buitenwereld  voor  mij betekent  dat er  blijkbaar geen 
          animo  voor  dit  onderwerp is.  Ik ben  voor veel  zaken te 
          porren,  maar lullen  tegen spreekwoordelijke  muren is daar 
          niet  een  onderdeel van.  (Gek toch  dat de  Special zoveel 
          leden heeft,  maar dat  die nooit wat van zich laten horen.) 
          Wat  ik persoonlijk erg leuk zou vinden is als bv. de makers 
          van  D.A.S.S.,  Giana Sisters,  Pumpkin Adventure  of andere 
          professionele spellen of andere soorten programma's ook eens 
          iets over  de door  hun gehanteerde  systemen zouden  willen 
          melden.
          
          Voor nu, veel plezier en keep on MSX'ing
          
                                                     Alex van der Wal
