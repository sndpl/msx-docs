              M I D I :   E E N   T A A L   I N   B E W E G I N G 
                                                                   
          
          MSX  is in de ban van MIDI. Alweer heel wat programma's zijn 
          intussen op de markt verschenen, welke gebruik maken van die 
          rare ronde  aansluitingen, die  zoveel lijken  op de  aloude 
          audio-aansluitingen. Maar wat is MIDI nu eigenlijk? Dat deze 
          vraag  niet zo  gemakkelijk te beantwoorden is als ze op het 
          eerste gezicht lijkt, wil ik in dit verhaal duidelijk maken.
          
          MIDI is  namelijk heel  wat meer  dan een methode om data te 
          sturen  naar een  keyboard, module  of synthesizer. Hoevelen 
          van u wisten bijvoorbeeld dat MIDI ook bedoeld is om special 
          effects zoals  vuurwerk en  rookmachines e.d. aan te sturen? 
          Om  dat  allemaal  te  kunnen  is MIDI  in eerste  instantie 
          weliswaar  niet ontwikkeld, maar is ze wel uitgegroeid. MIDI 
          is een volwaardige taal geworden. Compleet met grammatica en 
          vocabulaire. Zelfs  compleet met  de bij  elke taal  horende 
          uitzonderingen op de regels.
          
          En  zoals bij  elke taal is het ook bij MIDI noodzakelijk om 
          de grammatica  te leren  vooraleer men  de taal kan spreken. 
          Gebeurt  dit  niet  en  ga  je proberen  de taal  te spreken 
          voordat  je de grammatica kent, dan zal je instrument je wel 
          VERSTAAN  maar  BEGRIJPEN  is  een  andere  zaak.  Misschien 
          herinneren enkelen  van u  nog het lachwekkende Duits van de 
          Belgische  keeper Jean-Marie Pfaff, toen-ie in Duitsland bij 
          een Duitse ploeg voetbalde. Dat ging zoiets als volgt:
          
          "Ja, die  penalty. Ich  zag al  an sein anlaufen, dattie der 
          bal  rechts van  mij wollte  gaan schiessen, dus war het f�r 
          mich auch nicht zo moeilijk die bal te stopfen..."
          
          Dat  dit geen  Duits is ziet elke Nederlander. Maar dat toch 
          zo ongeveer elke Duitser begreep wat Jean-Marie wilde zeggen 
          ligt  toch  minder voor  de hand.  Voor MIDI  geldt ongeveer 
          hetzelfde. Zolang de foutjes tegen de grammatica nog niet t� 
          erg zijn,  zal elk  instrument begrijpen wat u bedoelt, maar 
          wilt  u een  echt 'gesprek'  met uw  instrument aangaan, dan 
          zult  u  toch  de  grammatica  (en  de  vocabulaire  van  uw 
          instrument) enigszins onder de knie moeten hebben. Bovendien 
          is de  vocabulaire niet  bij elk  instrument gelijk! Evenals 
          niet  elk persoon het woord 'ambivalentie' kent [Nvdr. Eh?], 
          terwijl het  toch een  goed Nederlands woord is, zo kent ook 
          niet  ieder instrument  alle MIDI-codes. Kijk dus niet al te 
          gek  op,  wanneer uw  instrument niet  begrijpt wat  u zegt, 
          terwijl u uzelf netjes aan de MIDI-regels houdt...
          
          
                           M I D I   A L S   T A A L 
          
          Tot zover  de (lange) inleiding. Want als we het hebben over 
          de  grammatica van  MIDI, dan hoort daar ook een uitleg bij, 
          nietwaar? Want, hoe 'spreek' je nu foutloos MIDI?
          
          Ten eerste  bestaat MIDI alleen maar uit getallen. En wel de 
          getallen 0 t/m 255. De hele MIDI-vocabulaire bestaat dus uit 
          precies 256 'woorden'.
          
          Ten  tweede dien  je een  juiste zinsopbouw  te volgen.  Net 
          zoals  bij  een  echte  taal.  Laten  we  eens  het volgende 
          Nederlandse zinnetje bekijken:
          
            Ik ga fietsen
          
          De woorden  "ga" en  "fietsen" zeggen  iets over de "ik". Ze 
          verschaffen  als het  ware data bij het woordje "ik". Zonder 
          die data betekent het zinnetje niets. Dit geldt voor MIDI in 
          exact dezelfde mate. Elke MIDI-zin dient te beginnen met die 
          informatie, waarover  je iets  wilt zeggen.  In MIDI-termen: 
          Elke  zin dient te beginnen met een status-byte gevolgd door 
          een aantal  databytes. Maar hoe herken je nu een statusbyte? 
          Dat  hebben  de  heren  MIDI-uitvinders  iets  intelligenter 
          gedaan  dan  de  heren  Nederlandse-taal-uitvinders.  In  de 
          Nederlandse  taal  is  alleen door  ervaring of  studeren te 
          leren welke woorden nu statuswoorden zijn. In MIDI hebben ze 
          het simpeler gemaakt: Elk getal dat groter is dan 127 is een 
          statusbyte. Is het getal dus 127 of kleiner, dan betreft het 
          een databyte.
          
          Elke MIDI-zin dient dus te beginnen met een getal dat groter 
          is dan  127. Deze  statusbytes vertellen  het instrument wat 
          het moet gaan doen. De databytes daarna vertellen HOE het te 
          doen.
          
          De  statusbytes zijn  bovendien nog  ingedeeld in 8 groepjes 
          van 16 getallen, corresponderend met het aantal MIDI-kanalen 
          dat  MIDI  kan aansturen.  Dit is  eenvoudiger zichtbaar  te 
          maken wanneer  de getallen  hexadecimaal worden weergegeven: 
          Een statusbyte begint dan namelijk met een getal tussen 8 en 
          F  en eindigt met een getal tussen 0 en F. Dit laatste getal 
          geeft het  MIDI-kanaal aan (0=kanaal 1 t/m F=kanaal 16).
          
          Het  eerste getal  vertelt MIDI  wat te  gaan doen  en welke 
          databytes te  verwachten. Net als in een gewone taal: Na een 
          woordje  'je' verwacht  je een  werkwoordsvorm behorend  bij 
          'je', ook  al zou  deze werkwoordsvorm  er hetzelfde uitzien 
          als een andere werkwoordsvorm. Bij een zinnetje als 'Je gaat 
          fietsen'  is het  tweede woord  exact gelijk  aan het tweede 
          woord uit  de zin  'Hij gaat  fietsen', terwijl de betekenis 
          (puur grammaticaal gezien) niet gelijk is. De eerste keer is 
          het een werkwoordsvorm behorend bij 'je', de tweede keer een 
          werkwoordsvorm  behorend bij  'hij'. Maak van beide woordjes 
          'gaat' maar  eens een  verbuiging van  het werkwoord 'zijn'. 
          Het  eerste 'gaat' wordt 'bent' het tweede 'gaat' 'is'. Voor 
          MIDI geldt  dit ook. De databytes zijn voor alle statusbytes 
          gelijk  van  vorm  (nl.  0  t/m  127)  maar  verschillen  in 
          betekenis  afhankelijk van  het (hexadecimale) getal waarmee 
          een statusbyte begint.
          
          
                                N O T E   O F F 
          
          Als  een  statusbyte  begint  met een  8h (dit  zijn dus  de 
          statusbytes 80h  t/m 8Fh)  betekent dit voor MIDI dat er een 
          toon  UITgezet moet  worden. Er behoren nu twee databytes te 
          volgen, die  aangeven welke noot uitgezet dient te worden en 
          hoe snel de toon moet uitsterven. Deze tweede databyte wordt 
          bij  lange  na  niet door  alle instrumenten  begrepen, maar 
          weglaten is NIET toegestaan. Evenmin als de zin 'Ik voel mij 
          ambivalent' duidelijk wordt als het woord 'ambivalent' wordt 
          weggelaten,  is het  voor MIDI  duidelijk wanneer  de tweede 
          databyte wordt  weggelaten. Het  grote verschil  is dat MIDI 
          handelt naar wat er WEL begrepen wordt en dus de toon uitzet 
          met  een gemiddelde  snelheid (terwijl misschien bedoeld was 
          om de toon extra snel uit te zetten).
          
          
                                 N O T E   O N 
          
          De statusbytes  beginnend met een 9h (dus 90h t/m 9Fh) laten 
          een  toon  AANzetten.  Wederom  worden  er  door  MIDI  twee 
          databytes  verlangd, waarbij  de eerste  aangeeft welke noot 
          aan te  zetten en  de tweede  de velocity  (i.e. de hardheid 
          waarmee de toon aangeslagen dient te worden). Dit statusbyte 
          behoort  tot  de  standaard-vocabulaire van  elk instrument; 
          ieder instrument begrijpt het.
          
          Er geldt  ook nog  een uitzondering bij deze statusbyte, die 
          eigenlijk  meer regel  dan uitzondering is. Wanneer een noot 
          aangezet wordt met een velocity (dus tweede databyte) van 0, 
          wat  dus  feitelijk  betekent  dat een  noot wordt  aangezet 
          ZONDER  deze   aan  te   slaan,  wordt  daarmee  een  eerder 
          aangezette  noot  UITgezet!  Deze uitzondering  is bij  alle 
          instrumenten  bekend  en  wordt zelfs  meer gebruikt  dan de 
          offici�le methode van uitzetten.
          
          Wanneer uw  instrument u  dus vertelt een noot aan te zetten 
          met  een  velocity  van 0,  bedoelt hij  niets meer  dan een 
          vorige noot (met uiteraard eenzelfde eerste databyte) UIT te 
          zetten.   Tja,  MIDI   is  zelfs   qua  uitzonderingen   een 
          volwaardige taal!
          
          
                  P O L Y P H O N I C   K E Y P R E S S U R E 
          
          Begint een  statusbyte met een Ah, dan wordt daarmee de zgn. 
          polyfonische  toetsdruk (in  Engels polyphonic  keypressure) 
          geregeld. Een  moeilijk woord  en een  even moeilijk begrip, 
          waarvoor  we  bij  de uitleg  zelfs naar  niet-elektronische 
          instrumenten  moeten kijken.  Eenieder, die ooit een (goede) 
          uitvoering  van  het  muziekstuk  'Il Silenzio'  heeft mogen 
          aanhoren, heeft  kunnen horen  dat de trompet de lange noten 
          aan  het einde  laat 'trillen'.  [Nvdr. De  functie MOD  van 
          MoonBlaster doet  hetzelfde.] Nu wilden de muziekmakers, dat 
          dergelijke  effecten ook  via MIDI  aan te  sturen waren. De 
          MIDI-makers, niet al te beroerd, voerden dus de polyfonische 
          toetsdruk in.  Wanneer bij  een (duurder)  instrument na het 
          aanslaan  van een  toets nog  eens extra op de toets gedrukt 
          wordt (bij  de instrumenten spreekt men van aftertouch), kan 
          men  een extra effect aanroepen. Dit extra effect is meestal 
          het trillen  van de toon, maar in tegenstelling tot wat veel 
          mensen  menen, is  dit NIET  standaard! Er  bestaan ook  wel 
          instrumenten,  die   de  toonKLANK   veranderen  wanneer  er 
          'door'gedrukt    wordt,   bijvoorbeeld    sommige   duurdere 
          synthesizers.
          
          In  MIDI  werkt  de polyfonische  toetsdruk alsvolgt:  Eerst 
          uiteraard  de  statusbyte  (A0h t/m  AFh) gevolgd  door twee 
          databytes.  De  eerste  databyte  geeft  aan welke  noot het 
          effect krijgt, de tweede databyte de hoeveelheid effect.
          
          PAS OP: De noot  hoeft niet aan te staan! Een leuk resultaat 
          is  te krijgen  door een  bepaalde noot  dit effect te geven 
          ZONDER dat  deze eerst  is aangezet.  Elke volgende keer dat 
          deze  noot wordt  aangezet krijgt de noot nu automatisch dit 
          effect. Op  deze manier  kun je bijvoorbeeld een valse piano 
          imiteren.  Die toets die vals moet klinken geef je polyfonic 
          keypressure  en  elke  volgende  keer dat  deze toets  wordt 
          aangeslagen, zal  het geluid  'gek' doen  (het nut  van deze 
          truuk is vrij beperkt, maar ach, het is er ��n...).
          
          
                          C O N T R O L   C H A N G E 
          
          Begint een statusbyte met een Bh (dus het hexadecimale getal 
          B,  voor alle  duidelijkheid...) dan betreft het een Control 
          change.   Dit   statusbyte   is   zonder  twijfel   ��n  der 
          krachtigsten van  MIDI. Alle  effecten, die  niet een  eigen 
          statusbyte  hebben, maar wel door verschillende instrumenten 
          moeten worden  begrepen, hebben  hier hun basis gevonden.
          
          De  Control  change is  een lijst  van effecten  (in Engels: 
          Controls), die  gebruikt kunnen worden om uw conversatie met 
          uw  instrument  levendig  te  maken.  De twee  databytes die 
          het    statusbyte   (dus   B0h   t/m   BFh)   volgen   geven 
          achtereenvolgens het  soort effect  en de  'zwaarte' van dat 
          effect   aan.  Daar   deze  MIDI-boodschap  (in  MIDI-jargon 
          MIDI-event genoemd) zo enorm krachtig is, ben ik van zins in 
          een volgend artikel hierop verder in te gaan. Voor nu kan ik 
          dus alleen maar zeggen: Als uw MIDI-kennis nog nul is, blijf 
          dan in  's hemelsnaam  van deze boodschap af. De kans dat uw 
          keyboard  iets  verstaat,  dat  u  absoluut  niet  wilt,  is 
          huizenhoog aanwezig.
          
          
                          P R O G R A M   C H A N G E 
          
          Hier  belanden we op de grens van de grammatica en steken we 
          de brug  over naar  de vocabulaire.  Maar eerst  nog even de 
          grammatica:  Het statusbyte (C0h t/m CFh) wordt gevolgd door 
          slechts ��n  databyte. Dit  databyte is  het MIDI-equivalent 
          van  de vocabulaire.  Met dit  databyte geef  je je keyboard 
          namelijk te kennen WAT voor geluid deze dient te maken.
          
          Maar  pas op!  Tot voor kort had elke producent (en vaak ook 
          elk instrument)  zijn eigen vocabulaire. Dit was zo ontstaan 
          omdat  van  oorsprong  de  program  change  bedoeld  was  om 
          veranderingen  aan te brengen in het geluid van synthesizers 
          zonder  geheugen.   Toen  echter   de  instrumentmakers   de 
          beschikking  kregen  over  geheugen,  zodat  een hoeveelheid 
          standaard-geluiden  vooraf al  in het  instrument opgeslagen 
          konden worden,  werd dit MIDI-event meer en meer gebruikt om 
          een  geluid  (afhankelijk  van  producent  patch, programma, 
          timbre of preset genoemd) te kiezen.
          
          Maar  daar  iedere  producent zijn  eigen voorkeur  had voor 
          geluiden,  was de lijst met geluiden ook op ieder instrument 
          anders. Een  standaard was op dit gebied dus (tot voor kort) 
          ver  te zoeken. De situatie was vergelijkbaar met talen, die 
          wel dezelfde  grammatica kenden  (dus zinsopbouw), maar heel 
          andere woorden gebruikten.
          
          Echter  zoals al twee keer kort opgemerkt, is deze toren van 
          Babel langzaam  maar zeker  aan het  verdwijnen. Een  aantal 
          jaren   geleden   kwamen   de   grootste   producenten   van 
          instrumenten  namelijk overeen om in het vervolg elke waarde 
          van het  databyte eenzelfde  instrument te laten selecteren. 
          Dit  betekent geenszins,  zoals zo  velen menen,  dat nu ook 
          alle  keyboards  van willekeurig  welke producent  hetzelfde 
          KLINKEN! Het betekent alleen dat wanneer je als databyte een 
          0  geeft  alle instrumenten  voortaan een  piano-klank laten 
          horen.  Het  verschil in  klankopwekking is  niet veranderd, 
          zodat een  piano op  een Yamaha-instrument  nog steeds  heel 
          anders  (beter...?) klinkt  dan op een Casio. Deze standaard 
          wordt de GM-standaard genoemd, wat staat voor GENERAL MIDI.
          
          Voor  Roland  ging deze  standaardisatie nog  lang niet  ver 
          genoeg  en  daarom  voerden  zij  de  GS-standaard  in.  Een 
          GS-instrument is 'upwards compatibel' met een GM-instrument, 
          net  zoals  bij  de  MSX-systemen  het  MSX2-systeem upwards 
          compatibel was  met het  MSX1-systeem (met die uitzondering, 
          dat bij MIDI de compatibiliteit WEL 100% is) en een minister 
          wel   begrijpt  wat   een  bouwvakker   zegt,  maar   zelden 
          omgekeerd...
          
          Op deze  twee systemen  kom ik een volgende keer terug, daar 
          dit een bron is van veel, heel veel misverstanden!
          
          
                        C H A N N E L   P R E S S U R E 
          
          Een  statusbyte  beginnend  met  een  Dh  regelt de  channel 
          pressure,  een  eenvoudig  alternatief  voor  de  polyphonic 
          keypressure. Toen  MIDI ontstond  dacht men  voor aftertouch 
          (zie  polyphonic  keypressure)  twee alternatieven  nodig te 
          hebben; een duur (de polyphonic keypressure) en een goedkoop 
          (de   channel   pressure).   Dit   vanwege   het  feit   dat 
          drukreceptoren onder toetsen een dure aangelegenheid was (en 
          is!)  en  dus  niet alle  producenten �lke  toets een  eigen 
          receptor  wilden geven.  Helaas is  het momenteel zo, dat de 
          polyphonische  aftertouch   in  enorm   weinig  instrumenten 
          ingebouwd  is en praktisch alle producenten voor de goedkope 
          variant hebben gekozen.
          
          Wat  MIDI-betreft  is  het  verschil  tussen  de  polyphonic 
          keypressure en  de channel pressure opvallend klein, nl. ��n 
          databyte. Waar de polyphonic keypressure twee databytes (��n 
          voor  noot en  ��n voor  hoeveelheid) kent,  kent de channel 
          pressure maar  ��n databyte,  nl. de hoeveelheid. Het effect 
          geldt  daarna  voor  alle  toetsen  (of  noten  op hetzelfde 
          MIDI-kanaal,   vandaar  de   naam  'channel   pressure'  dat 
          'kanaaldruk' betekent).  Voor de compleetheid dien ik nog op 
          te  merken, dat  de polyphonic  keypressure door maar weinig 
          instrumenten wordt begrepen (m.u.v. de GS-instrumenten, waar 
          het  standaard  is  dat  ze  polyphonic  keypressure  kunnen 
          ontvangen, maar - tot nu toe - geen enkele het uitzendt).
          
          
             - Deel 2 van deze tekst kunt u in het submenu vinden -
          
          
          
          
             - Deel 1 van deze tekst kunt u in het submenu vinden -
          
          
                       P I T C H   B E N D   C H A N G E 
          
          Nu word  ik melancholiek. De Pitch-bender... Wat mij betreft 
          HET  effect,  dat  de  mooiste  resultaten kan  hebben, maar 
          tegelijkertijd  een liedje volledig kan afbreken. Mensen die 
          ECHT goed  met pitch bend kunnen werken, zijn zeldzaam. Maar 
          vooruit,  niet  gedroomd,  uitleggen! De  pitch bend  change 
          heeft  als statusbyte de getallen E0h t/m EFh (dus beginnend 
          met  een  Eh). Na  dit statusbyte  dienen twee  databytes te 
          volgen. De  eerste voor de fijnafstemming (LSB) en de tweede 
          voor   de  grofafstemming   (MSB).  Eigenlijk  dienen  beide 
          databytes als  ��n 14-bits  getal gezien  te worden, waarbij 
          het tweede databyte voorop staat. Het bereik is in dit geval 
          dan  ook niet  van 0  t/m 127, maar van -8192 t/m +8191! Het 
          nulpunt ligt  bij 64 en 0 (dus eerste databyte 64, en tweede 
          databyte 0).
          
          Het  resultaat  van  deze  MIDI-boodschap  is alles  behalve 
          standaard!   Bij  Yamaha  bijvoorbeeld  doet  een  volledige 
          uitslag  (beide  databytes  127)  de  toon  een  vol  oktaaf 
          verhogen, terwijl  bij Roland  slechts 2 halve tonen! Hierin 
          komt  bij  de  GM-standaard  geen  verbetering, maar  bij de 
          GS-standaard  van  Roland  wel. Bij  de GS-standaard  is het 
          namelijk  mogelijk via  de control changes genaamd NRPN (Non 
          Registered Parameter Number) de 'Pitch bend sensitivity' (in 
          Nederlands de 'gevoeligheid van de pitch bender (sorry, geen 
          Nederlands alternatief  voorhanden)' genaamd)  te veranderen 
          van  0 halve  noten tot  24 halve noten (= twee oktaven). De 
          uitleg  ziet   u  terug   in  het  artikel  over  GS/GM,  de 
          MIDI-getallen  geef  ik  u  hier,  zodat  diegenen  die  een 
          GS-instrument bezitten kunnen gaan experimenteren.
          
          PAS  OP: c  = het gewenste MIDI-kanaal (standaard staat elke 
          upper-part van  een GS-instrument op kanaal 4, dus c=3) en x 
          is  de  hoeveelheid  halve  noten,  die  met  de pitch  bend 
          maximaal wordt veranderd:
          
          Bc 101 0 Bc 100 0 Bc 6 x
          
          De  statusbytes (steeds  Bc) zijn  hexadecimaal! Wilt u deze 
          code bijvoorbeeld  met behulp  van een  Music Module naar uw 
          keyboard  sturen  op  kanaal  4,  dan  laadt  u  onderstaand 
          programma  (MIDI.BAS) in waarbij u op de plaats van de c het 
          gewenste MIDI-kanaal  invult en  op de  plaats van  de x het 
          aantal halve noten:
          
          10 out 0,3 : out 0,21 ' = initialisatie MIDI van de MusMod
          20 out 1,&HBc : out 1,101 : out 1,0
          30 out 1,&HBc : out 1,100 : out 1,0
          40 out 1,&HBc : out 1,6 : out 1,x
          
          Gaat  u ooit  zelf met behulp van MIDI muziek maken en in uw 
          muziekstuk  met  pitch bend  werken, gebruik  dan ALTIJD  de 
          pitch bend  sensitivity (ook  al herkent  uw instrument deze 
          niet)  en vergeet  niet aan  het eind van uw liedje de pitch 
          bend sensitivity ALTIJD weer terug te zetten op 2 (dit geldt 
          trouwens  voor  alle veranderingen  die u  in uw  muziekstuk 
          maakt: zet ze aan het eind weer terug)!
          
          
                         S Y S T E M   M E S S A G E S 
          
          De  system  messages hebben  als overeenkomst  dat ze  allen 
          beginnen met een Fh en geen databytes nodig hebben. Voor het 
          overige  zijn  deze  statusbytes  het zwarte  schaap van  de 
          MIDI-familie: Ze  zijn de  uitzondering op  zo ongeveer elke 
          MIDI-regel:
          
          Het  tweede  (en  uiteraard  tevens  laatste)  getal  van de 
          statusbytes heeft GEEN betrekking op MIDI-kanalen. De system 
          messages hebben altijd betrekking op het hele instrument.
          
          De  system real  time messages mogen NOOIT in een muziekstuk 
          worden verwerkt  en zijn  louter bedoeld  om de communicatie 
          tussen instrumenten te bevorderen.
          
          Voor  meer informatie verwijs ik u naar een volgend artikel, 
          daar over dit onderwerp hele boeken zijn volgeschreven!
          
          
                          R U N N I N G   S T A T U S 
          
          Dan tot  slot nog een opmerking over de zgn. running status. 
          Net  zoals in  onze taal bij een opsomming datgene waar iets 
          over gezegd  wordt mag  worden weggelaten,  mag dit  ook bij 
          MIDI. Een voorbeeld:
          
          'De  fiets is groen, rijdt prettig, heeft een stalen frame.' 
          is  hetzelfde  als:  'De  fiets  is  groen,  de  fiets rijdt 
          prettig, de fiets heeft een stalen frame.'
          
          In  MIDI   mag  dit  ook.  In  plaats  van  steeds  dezelfde 
          statusbyte  te moeten  herhalen, hoeft deze slechts ��n keer 
          (in het  begin uiteraard;  een MIDI-zin dient nu eenmaal met 
          een  statusbyte te  beginnen) genoemd te worden. Mocht u dus 
          ooit denken  bij een  conversatie met uw instrument 'Tjonge, 
          wat  gebruikt-ie toch  veel databytes', dan is uw instrument 
          dus  bezig  met  een  opsomming  en  dient u  elke keer  het 
          statusbyte erbij te denken.
          
          Van belang is dus ook het aantal databytes dat normaliter op 
          een gegeven statusbyte volgt:
          
          C0h 1 2 3 4 5 6 7 8 9 10
          
          betreft 10 MIDI-boodschappen, terwijl
          
          B0h 1 2 3 4 5 6 7 8 9 10
          
          er slechts  5 betreffen!  Mocht u dit niet meteen begrijpen, 
          denkt  u er  dan aan dat C0h slechts ��n databyte behoeft en 
          B0h twee!
          
          
                                T O T   S L O T 
          
          In dit  artikel heb  ik geenszins  beoogd volledig  te zijn. 
          Daarvoor  is  dit  artikel  ook  in  eerste  instantie  niet 
          bedoeld. Ik heb alleen maar een basis willen geven om verder 
          te  gaan met  MIDI en  u er op attent willen maken, dat MIDI 
          een TAAL  is en  ook als  zodanig behandeld  zou behoren  te 
          worden. En net zoals niet elk mens zal begrijpen wat ik hier 
          schrijf, zal ook niet elk instrument hetzelfde begrijpen van 
          een  MIDI-boodschap. Dat  hoort nu  eenmaal bij  MIDI en  is 
          eigenlijk ook het grootste voordeel van MIDI.
          
          Daarom  blijf ik  altijd weer  sceptisch staan tegenover het 
          gebruik van  zgn. drivers  om alle instrumenten hetzelfde te 
          laten  begrijpen.  Naast  het  feit  dat  u  ten  eerste  uw 
          instrument   te  kort   doet  (elk   instrument  heeft  zijn 
          specifieke voordelen,  die niet  met drivers te ondersteunen 
          zijn) is het ten tweede onmogelijk om echt alle instrumenten 
          hetzelfde te laten begrijpen.
          
          Als  voorbeeld voor  deze stelling  wijs ik alleen maar naar 
          het pitch  bend probleem, zoals ik dat behandeld heb bij het 
          verhaal  over de  pitch bend  change. Het  is toch  ook niet 
          mogelijk om  elk mens  over de  hele wereld dezelfde taal te 
          laten  spreken! En  mocht dit ooit tot de mogelijkheden gaan 
          behoren, dan  zullen er  direct weer  per streek verschillen 
          ontstaan. In Nederland hebben we talloze woorden om neerslag 
          aan  te  duiden  (regen, motregen,  hagel, sneeuw,  stortbui 
          enz.),  terwijl  in  de  woestijn  aan dergelijke  begrippen 
          totaal  geen  behoefte  is! Daar  hebben ze  weer veel  meer 
          behoefte aan woorden die bruin aangeven...
          
          Mocht  u naar  aanleiding van  dit artikel nog vragen hebben 
          over  MIDI,  dan  verwijs  ik  u  graag  naar  een boek  dat 
          geschreven is  door Christian  Braut, getiteld "Het complete 
          MIDIboek  - Theorie  en praktijk"  en wordt  uitgegeven door 
          Sybex. Loop  eens binnen  bij uw plaatselijke bibliotheek en 
          blader  het eens  door of  ga eens kijken bij uw boekhandel, 
          waarbij ik  eerlijkheidshalve wel  dien op te merken dat het 
          boek niet goedkoop is!
          
                                                      Ruud van Gestel
