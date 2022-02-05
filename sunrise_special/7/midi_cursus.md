                             M I D I   C U R S U S 
                                                    
          
                         C O N T R O L   C H A N G E S 
          
          Na  de vorige  keer een verhaal te hebben afgestoken over de 
          opbouw van  MIDI-commando's wil ik het deze keer eens hebben 
          over de werkwoorden in de MIDI-taal: de CONTROL CHANGES.
          
          Zoals  de  vorige  keer  al  opgemerkt (en  ongetwijfeld nog 
          bekend...) begint een Control Change hexadecimaal met een B. 
          Daarachter   komt,  om  het  byte  volledig  te  maken,  een 
          hexadecimaal getal  tussen 0  en F om het MIDI-kanaal aan te 
          wijzen, waarbij (zoals bekend) 0 kanaal 1 is en F kanaal 16. 
          Na  dit statusbyte  (want het begint met een getal groter of 
          gelijk aan  8) volgen  twee databytes.  Het eerste  databyte 
          geeft  de te  veranderen controller aan, de tweede de waarde 
          die de controller krijgt.
          
          Ok�, tot dusver de herhaling; zijn er nog vragen?
          
          "Ja, wat is een controller?"
          
          Euh ja,  goede vraag,  maar niet zo goed te beantwoorden. Ik 
          kan  simpelweg een  cirkelredenering toepassen en zeggen dat 
          een controller datgene is, dat met behulp van Control Change 
          is te  veranderen, maar  dat is  taalkundig niet  erg netjes 
          (maar wel makkelijk, nietwaar heren politici?).
          
          Laat  ik  als  antwoord  op  die vraag  teruggrijpen op  het 
          ontstaan van MIDI:
          
          Door   de  indeling   van  MIDI   in  status-  en  databytes 
          (respectievelijk die  bytes groter  dan of gelijk aan 128 en 
          kleiner  dan 128)  en de indeling van de statusbytes in vier 
          bits voor status en vier bits voor MIDI-kanaal bleef er voor 
          MIDI niet bijster veel meer over om te regelen. De vier bits 
          die de  status aangeven  (en dus ook nog MOETEN beginnen met 
          een  1, daar  anders het  getal kleiner  is dan  128) kunnen 
          slechts 8  verschillende statussen aanduiden. Niet echt veel 
          om  alle nuanceverschillen  in muziek  mee aan  te sturen... 
          Daarom  ontstond   er  de  Control  Change,  als  een  soort 
          werkwoord   voor  de  MIDI-taal:  Alle  parameters,  die  te 
          veranderen    zijn    met   behulp    van   MIDI    in   een 
          keyboard/synthesizer  en  geen eigen  Status hebben,  worden 
          aangestuurd met  de Control  Change. In  totaal zijn dus met 
          behulp  van  Control  Change  128  verschillende  parameters 
          (oftewel  controllers) aan  te sturen (databytes 00 t/m 7F). 
          Tot zover  nog volledig duidelijk, hoop ik, en dus volgen nu 
          de uitzonderingen:
          
          De  controllers (waarvan  u dus nog steeds niet weet wat het 
          zijn;  geduld,  het  wordt  u  wel  duidelijk. Blijf  rustig 
          doorlezen)  zijn   onderverdeeld  in  Continue  controllers, 
          Switch  controllers,  geregistreerde  en niet-geregistreerde 
          controllers. 
          
          De   Continue  controllers  zijn  wederom  onderverdeeld  in 
          14-bits  controllers  (dus  twee  databytes, later  meer) en 
          7-bits (oftewel 1 data-byte) controllers.
          
          De indeling in databytes is alsvolgt:
          
          00h t/m 1Fh: 14-bits Continue controllers (MSB)
          20h t/m 3Fh: 14-bits Continue controllers (LSB)
          40h t/m 45h: Switch-controllers
          46h t/m 5Fh: 7-bits Continue controllers
          60h t/m 61h: data plus en data min (zie verder)
          62h t/m 65h: geregistreerde/niet-geregistreerde controllers
          66h tot 77h: nog niet gedefinieerd
          78h t/m 7Fh: Modusboodschappen (zie verder)
          
          
                   14-BITS CONTINUE CONTROLLERS (MSB EN LSB)
          
          Als het eerste databyte na de statusbyte een getal is tussen 
          00h  en  3Fh, betreft  het die  controllers, die  een waarde 
          kunnen bevatten  tussen 0  en 16384. De databytes tussen 00h 
          en  1Fh geven  de grove afstemming; die tussen 20h en 3Fh de 
          fijnafstemming.  Controller  01h  en  21h zijn  dus DEZELFDE 
          controller, waarbij 01h de grove afstemming regelt en 21h de 
          fijnafstemming! V��r  het ontstaan van de GM en GS-standaard 
          waren  slechts  13  van de  32 14-bits  Continue controllers 
          gedefinieerd:
          
          
                     0 1 H / 2 1 H :   M O D U L A T I O N 
          
          Het  doel van  de modulation  is om  een geluid expressiever 
          weer te  geven. Hoe  dat gebeurt  is aan  de fabrikant om te 
          beslissen!  De meest  gebruikte modulations zijn de vibratie 
          (voor diegenen,  die niet  weten wat dat is: trilling...) en 
          de tremolo (variatie in de amplitude van een geluid).
          
          
                 0 2 H / 2 2 H :   B R E A T H   C O N T R O L 
          
          Omdat de  DX7-synthesizer van  Yamaha het  eerste instrument 
          wat   met  een   standaard  in  MIDI,  zijn  veel  van  zijn 
          controllers  overgenomen.  E�n van  deze controllers  was de 
          Breath Control.  Voor diegene,  die nog  nooit iemand  bezig 
          heeft gezien met de Breath Controller (en ik denk dat dat de 
          meesten  zijn, daar  deze controller  eigenlijk alleen  door 
          Yamaha ondersteund  wordt en  dan nog maar in beperkte mate. 
          Dadelijk   wordt   wel   duidelijk  waarom...):   De  Breath 
          Controller  is  een  plastic  mondstuk,  waarin  je  blaast. 
          Naarmate  je harder  blaast, wordt  de Breath Control hoger. 
          Het grote  nadeel van  deze controller is de mate van blazen 
          die   nodig   is   om  de   maximale  uitslag   te  krijgen: 
          Keyboard-spelers   die   rood   aanlopen  waren   echt  geen 
          zeldzaamheid,  vooral niet  als men  roker was en/of net een 
          weinig (of wat meer) bier achterover had geslagen...
          
          
                0 4 H / 2 4 H :   F O O T   C O N T R O L L E R 
          
          Speciaal voor  rokers en  drankorgels (en al de anderen, die 
          niet  voor  gek  wilden  staan  met  een  buis in  hun mond) 
          ontstond  de Foot  controller. Deze doet exact hetzelfde als 
          de  Breath  controller,  maar  wordt  (hoera!)  met de  voet 
          bediend.
          
          
                0 5 H / 2 5 H :   P O R T A M E N T O   T I M E 
          
          Deze controller regelt de snelheid van het portamento.
          
          "Euh, pardon? Wat is een portamento?"
          
          Jij  weer?  Nou  vooruit:  Portamento  is  een  geleidelijke 
          overgang  tussen  twee  noten.  Denk  bijvoorbeeld  aan  een 
          violist, die  zijn vinger over een snaar laat glijden of aan 
          Brian  May  op  overdrive-gitaar.  Hoe  hoger de  waarde hoe 
          sneller naar de volgende noot wordt 'toegelopen'. 
          
          (P.S. Er  is ook  nog een  Portamento Switch-controller: Pas 
          als deze aan staat, werkt het portamento...)
          
          
                     0 6 H / 2 6 H :   D A T A   E N T R Y 
          
          Er  zijn ook  controllers, die  geen ruimte meer hebben voor 
          databytes, doordat  ze voor  het aanduiden van de controller 
          al beide databytes nodig hebben. Een voorbeeld hiervan is de 
          verderop   behandelde  geregistreerde   en  ongeregistreerde 
          controllers.  Om   dergelijke  controllers  toch  te  kunnen 
          veranderen  bestaat er de Data entry. De waarde die met deze 
          controller wordt gegeven wordt gebruikt om de LAATST gekozen 
          controller (geregistreerd of ongeregistreerd) te veranderen. 
          Heeft u  nog geen  controller gekozen gehad, heeft deze Data 
          entry ook geen effect...
          
          
                    0 7 H / 2 7 H :   M A I N   V O L U M E 
          
          Met  behulp van  deze controller  wordt het  volume van  ��n 
          MIDI-kanaal veranderd.  Deze controller is verreweg de meest 
          gebruikte,  daar dit  de enige  standaard-methode is  om het 
          volume van een keyboard/synthesizer te veranderen.
          
          
                        0 8 H / 2 8 H :   B A L A N C E 
          
          Verwar  deze  controller  niet  met  controller  10h/30h! De 
          balance regelt  NIET de  van audio-systemen bekende balance. 
          Daarvoor  dient  controller  10h/30h! De  Balance regelt  de 
          verhouding  tussen  twee  geluiden,  wanneer  een  voice  is 
          samengesteld   uit  twee  geluiden.  Deze  controller  wordt 
          vrijwel alleen  gebruikt bij  synthesizers en  dan nog  niet 
          vaak! 
          
          
                            0 A H / 2 A H :   P A N 
          
          Deze   controller   be�nvloedt  het   stereobeeld   van  ��n 
          MIDI-kanaal.   Wanneer de waarde 0 wordt meegestuurd bevindt 
          het  geluid  zich uiterst  links. Naarmate  de waarde  hoger 
          wordt, verplaatst  het geluid  zich naar  rechts, totdat  de 
          maximale waarde  is gegeven  en het  geluid zich dus uiterst 
          rechts bevindt.
          
          
                     0 B H / 2 B H :   E X P R E S S I O N 
          
          Deze  controller bepaalt  het volumebeeld ter verfijning van 
          controller  07h/27h   (Main  volume).  Voor  die  oplettende 
          mensen,  die  dus  nu  vragen  of het  volume dus  in totaal 
          28-bits   geregeld  wordt   (controller  07h/27h=14-bits   + 
          controller  0Bh/2Bh=14-bits)  moet ik  antwoorden: Officieel 
          wel, maar  verreweg de  meeste instrumenten (sterker nog: ik 
          ben  nog nooit  anders tegengekomen) herkennen alleen de MSB 
          van alle  controllers. En  om dan  toch het  volume wel  met 
          behulp van 14-bits te regelen is de Expression ontstaan.
          
          
                      10h/30h T/M 13h/33h: GENERAL PURPOSE
          
          Deze  controllers zijn  speciaal in het leven geroepen om de 
          diverse fabrikanten  een mogelijkheid  te geven  om toch van 
          elkaar   af  te  wijken.  Deze  controllers  zijn  dus  NIET 
          standaard;  elke  fabrikant heeft  ze met  eigen controllers 
          ingevuld. Om  weer terug te grijpen naar de vergelijking met 
          taal: Dit is het dialect van MIDI.
          
          
                      S W I T C H - C O N T R O L L E R S 
          
          Dit  zijn er  in totaal  5. Deze  controllers kennen slechts 
          twee standen:  Aan en uit. Maar omdat MIDI alleen met 7-bits 
          databytes  kan werken  is 00h  t/m 3Fh  'uit' en 40h t/m 7Fh 
          'aan'.  Persoonlijk  vind ik  dat ze  deze switches  best op 
          bit-niveau hadden  kunnen zetten;  dus 1  controller, die  7 
          switches  kan bedienen met het ene databyte van 7 bits! Maar 
          helaas kozen  de MIDI-ontwerpers  voor deze  ruimte-vretende 
          oplossing.
          
          Zoals gezegd zijn de waarden 00h t/m 3Fh gelijk aan 'uit' en 
          40h t/m  7Fh gelijk  aan 'aan',  maar dat  heeft niet iedere 
          fabrikant  begrepen. Verreweg het veiligst is het dus alleen 
          de  waarden   00h  (voor  'uit')  en  7Fh  (voor  'aan')  te 
          gebruiken. 
          
          De 5 switches in willekeurige volgorde zijn:
          
          
                       4 0 H :   D A M P E R   P E D A L 
          
          Dit  is  het  sustain-pedaal  van  een  piano.  Wanneer deze 
          controller  aan staat,  wordt de  toon niet meer uitgezet en 
          blijft dus eeuwig (in principe, tenminste) doorklinken. 
          
          
                         4 1 H :   P O R T A M E N T O 
          
          Inmiddels weet dus iedereen wat portamento is en kan ik hier 
          volstaan  met  de mededeling,  dat deze  controller dus  het 
          portamento aan- en uitzet.
          
          
                          4 2 H :   S O S T E N U T O 
          
          Sostenuto  is in  principe gelijk aan het Damper pedaal, met 
          dat verschil,  dat het  Damper-(oftewel sustain-)pedaal  ook 
          die  tonen,  die  later  aangezet  worden  aanhoudt  en  het 
          sostenuto-pedaal alleen die tonen, die al aanstonden voordat 
          dit pedaal (controller) wordt aangezet, blijft aanhouden.
          
          
                         4 3 H :   S O F T   P E D A L 
          
          Aangezien  een goede  piano twee  pedalen heeft (en een hele 
          goede zelfs  drie, maar dat terzijde) regelt deze controller 
          het  tweede pedaal,  het soft-pedaal.  Dit pedaal  dempt het 
          geluid, zoals  eenieder die  ooit achter  een piano  gezeten 
          heeft  (en natuurlijk ook gespeeld; alleen zitten levert bij 
          een  piano  weinig  geluid  op, uitzonderingen  daargelaten) 
          weet.  Voor  die  twee  overblijvers:  Ik  kan hier  wel een 
          uitgebreid verhaal  gaan ophangen  over de  werking van  dit 
          pedaal,  maar verder dan de opmerking, dat het geluid minder 
          scherp  klinkt,  kom ik  toch niet.  Mocht u  zich nog  geen 
          indruk kunnen  maken van  dit effect,  dan resteren  er twee 
          mogelijkheden:   1)  achter   een  piano   gaan  zitten   en 
          uitproberen; of 2) (altijd de beste oplossing; ook voor alle 
          andere controllers) uitproberen via MIDI!
          
          
                              4 5 H :   H O L D 2 
          
          Deze  controller  dupliceert  het sustain-pedaal  wanneer er 
          tegelijk  twee functies  gebruikt moeten  worden om een noot 
          aan  te   houden.  Persoonlijk   ben  ik   nog  maar  weinig 
          instrumenten tegengekomen, die deze controller herkennen.
          
            - De tekst wordt vervolgd in de volgende submenu-optie -
          
          
          
                      - Dit is het vervolg van de tekst -
          
                             M I D I   C U R S U S 
                                                    
          
             7 - B I T S   C O N T I N U E   C O N T R O L L E R S 
          
          Dit  zijn de  continue controllers,  die niet via MSB en LSB 
          geregeld worden  (hoewel ik  al eerder opgemerkt heb, dat ik 
          persoonlijk nog geen instrument ben tegengekomen, dat de LSB 
          van   die  14-bits  continue  controllers  herkent...)  maar 
          slechts 1  controller en  dus 1 databyte genoeg vinden. Deze 
          zijn:
          
          
                50h T/M 53h: GENERAL PURPOSE CONTROLLERS 5 T/M 8
          
          Naast de 14-bits controllers, die per fabrikant verschillen, 
          zijn  er   ook  4  7-bits  controllers,  die  per  fabrikant 
          verschillen.  Elke fabrikant mag deze controllers naar eigen 
          goeddunken invullen; tussen verschillende instrumenten hoeft 
          dus geen  herkenning op  te treden, wanneer deze controllers 
          gebruikt   worden.   Misverstanden   zijn   echter   wel  te 
          verwachten:  wanneer de  ene fabrikant met controller 50h de 
          felheid  van   zijn  display   regelt  (het   is  maar   een 
          voorbeeld...)  en een  ander met  controller 50h  de leslie, 
          kunnen er  nogal leuke  effecten ontstaan, wanneer deze twee 
          instrumenten  doorgekoppeld zijn!  Denk in vergelijking maar 
          eens  aan   een  rasechte   Amsterdammer,  die  afzakt  naar 
          Maastricht:  Wat-ie al verstaat (dat zal wel niet veel zijn) 
          verstaat-ie waarschijnlijk nog verkeerd ook!
          
          
                    5 B H   T / M   5 F H :   E F F E C T S 
          
          Met   deze    5   controllers    worden   5    verschillende 
          effectprocessoren aangestuurd; deze zijn achtereenvolgens:
          
          - 5Bh: External effects depth (meestal een soort echo)
          - 5Ch: Tremolo depth (om de mate van tremolo te regelen)
          - 5Dh: Chorus depth (om de mate van chorus te regelen)
          - 5Eh: Celeste (oftewel detune) depth
          - 5Fh: Phaser depth (om het geluid 'uit fase' te zetten)
          
          Voor  die enkeling,  die nu  het idee  krijgt, dat ik weinig 
          informatie over  dit onderwerp  geef: Deze  effecten zijn zo 
          apart,  dat  ze  uitleggen  eigenlijk  afbreuk  doet  aan de 
          duidelijkheid;  je moet  ze horen  om ze  te begrijpen... En 
          bovendien  zijn  ze  slechts  sporadisch gebruikt,  dus echt 
          belangrijk zijn  ze niet  (voordat ik  de GS-kenners over me 
          heen  krijg: Ja,  op controllers  5Bh en  5Dh kom  ik in het 
          GS-verhaal nog apart terug!).
          
          
                  60h EN 61h: DATA INCREMENT EN DATA DECREMENT
          
          Via controller  06h/26h was  het mogelijk  data te versturen 
          naar aparte controllers. Welnu met de controllers 60h en 61h 
          is  het mogelijk deze data respectievelijk met 1 te verhogen 
          of te  verlagen. Net  zoals bij controller 06h/26h al gezegd 
          hebben deze controllers voornamelijk invloed op de:
          
          
                 62h T/M 65h: ON- EN GEREGISTREERDE CONTROLLERS
          
          Omdat  de MIDI-ontwerpers  een hekel  hadden aan  een eindig 
          aantal  controllers,  verzon men  de geregistreerde  (64h en 
          65h) en  de ongeregistreerde  (62h en  63h) controllers. Het 
          verschil  tussen  deze  twee  is  weer  heel  eenvoudig:  De 
          geregistreerde zijn standaard, de ongeregistreerde zijn weer 
          per  fabrikant anders.  Verder zijn  ze volstrekt  hetzelfde 
          opgebouwd:
          
          De  tweede  databytes  geven  nu  geen  waarde aan  maar een 
          volgende reeks  controllers, waarbij de hoogste van elk paar 
          (bij  de geregistreerde dus 65h, bij de ongeregistreerde dus 
          63h)  de  MSB  regelt  en  de  laagste  van  elk  paar  (64h 
          respectievelijk 62h) de LSB. Het aantal controllers wordt in 
          ��n klap  uitgebreid met 16384 nieuwe controllers! Bovendien 
          worden   bij  deze   zgn.  RPN   (geregistreerde)  en   NRPN 
          (niet-geregistreerde) w�l zowel de MSB als de LSB verwacht!
          
          De waarde van de gekozen controller wordt achteraf veranderd 
          met  behulp  van  de  DATA-controllers (06h  en 26h  voor de 
          absolute invoer en 60h en 61h voor de relatieve invoer).
          
          Een voorbeeld, voor diegenen die naar duidelijkheid snakken:
          
          Om NRPN-controller MSB=00h/LSB=16h de waarde 54h te geven op 
          MIDI-kanaal  14, dient  u alsvolgt de volgende bytes naar uw 
          instrument te sturen:
          
          BDh (B=Control Change; D=kanaal 14)
          63h (MSB van het NRPN eerst)
          00h (waarde van het MSB)
          
          BDh
          62h (LSB van het NRPN)
          16h (waarde van het LSB)
          
          BDh
          06h (DATA-invoer)
          54h (waarde van wijziging van NRPN 00h/16h)
          
          Zou de  waarde nu een 14-bits getal geweest moeten zijn, dan 
          zou  ook Controller 26h gebruikt hebben moeten worden en zou 
          het MIDI-event er alsvolgt uitgezien hebben:
          
          BDh 63h 00h BDh 62h 16h BDh 06h 04h BDh 26h 54h
          
          Voor  diegenen  die de  04h niet  thuis kunnen  brengen: Dit 
          slaat nergens  op en  is slechts  bedoeld als  voorbeeld. De 
          waarde die met dit laatste voorbeeld wordt uitgezonden, is:
          
          binair: &B00001001010100
          hexadecimaal: &H0454h
          decimaal: 1108
          
          Welke  controller er nu veranderd is, kan ik natuurlijk niet 
          zeggen, daar  dit (in  tegenstelling tot de RPN-controllers) 
          bij  de NRPN-controllers  bij elk  instrument anders is. Het 
          betreft hier  ook maar  een voorbeeld van het gebruik van de 
          NRPN- en RPN-controllers.
          
          De  RPN-controllers zijn  dus wel  geregistreerd en voor die 
          grapjassen,  die  nu denken:  "Ha, laat  hem maar  eens alle 
          16384 controllers  behandelen", kan  ik antwoorden:  Wie het 
          laatst  lacht, lacht het best, want momenteel zijn er van de 
          16384  geregistreerde  controllers slechts  3 geregistreerd! 
          Deze zijn achtereenvolgens:
          
          MSB 00h/LSB 00h: PITCHBEND SENSITIVITY
          
          Met  behulp van deze RPN-controller kan men aangeven hoeveel 
          noten een  toon moet  wijzigen bij een volledige uitslag van 
          het  Pitch-bend-wiel. Het MSB van  de data  (06h) geeft  het 
          aantal halve  noten aan,  het LSB  het aantal honderdste van 
          een noot.
          
          MSB 00h/LSB 01h: FINE TUNING
          
          Hiermee  kunt  u uw  instrument heel  fijnschalig afstemmen: 
          Wanneer  zowel  MSB  als  LSB van  de waarde  00h is,  is uw 
          instrument 1 volledige halve noot verlaagd; is de MSB en LSB 
          daarentegen allebei 7Fh, dan is uw instrument een halve noot 
          verhoogd. Het gouden midden ligt bij MSB 40h en LSB 00h. Met 
          behulp van  deze controller  kunt u  dus in  8192 stappen uw 
          instrument  een halve  toon verlagen  en in  8191 stappen uw 
          instrument een halve toon verhogen!
          
          MSB 00h/LSB 02h: COARSE TUNING
          
          Gelijk aan FINE TUNING maar veel grofschaliger: Nu kunt u uw 
          instrument 64  halve noten  verlagen en verhogen! Het midden 
          ligt uiteraard bij MSB=40h en LSB=00h.
          
          
                       M O D U S B O O D S C H A P P E N 
          
          Poeh,  poeh, we  zijn er bijna: Als laatste onderdeel van de 
          Control   changes   worden   nog   even  snel   de  7   zgn. 
          modusboodschappen behandeld: Deze zijn achtereenvolgens:
          
          
                   R E S E T   A L L   C O N T R O L L E R S 
          
          Wanneer de volgende MIDI-event wordt uitgezonden:
          
          Bxh 79h 00h
          
          Worden  op het  gewenste MIDI-kanaal  (vul voor  x een getal 
          tussen 0h en Fh in) in principe alle controllers van 00h t/m 
          78h, de  Pitchbend en  de Aftertouch  weer teruggezet op hun 
          beginwaarde.  Ik  zeg  duidelijk  in  principe,  want  in de 
          praktijk  worden over het algemeen maar een paar controllers 
          teruggezet.  De   fabrikant  bepaalt   namelijk  zelf  welke 
          controllers  gereset worden  en naar welke waarde ze gereset 
          worden...
          
          
                           L O C A L   C O N T R O L 
          
          Met de volgende MIDI-event:
          
          Bxh 7Ah 00h
          
          Wordt  de local control afgezet. Wanneer als tweede databyte 
          in plaats  van een 00h een 7Fh wordt uitgezonden, wordt deze 
          aangezet. Duidelijk toch...
          
          "Uhm, ben ik weer... Wat is Local Control eigenlijk?"
          
          Local  Control  regelt de  verbinding tussen  toetsenbord en 
          toongenerator. Wanneer  Local Control  aanstaat, wordt  elke 
          toets,  die op  het toetsenbord wordt aangeslagen �n naar de 
          MIDI-out �n naar de toongenerator gestuurd, zodat er (als er 
          op  de  MIDI-out  ook  een instrument  aangesloten is)  twee 
          geluiden klinken.  Staat nu  de Local  Control af, dan is de 
          verbinding  tussen toetsenbord  en toongenerator  verbroken. 
          Elke  toets,  die nu  aangeslagen wordt,  wordt w�l  naar de 
          MIDI-out gestuurd,  maar wordt niet meer door het instrument 
          gespeeld. Daarentegen worden wel alle noten, die via MIDI-in 
          binnenkomen  gewoon gespeeld.  Het is  alsof uw  keyboard of 
          synthesizer is  gesplitst in  een apart  commandotoetsenbord 
          (in  Engels: Masterkeyboard) en een toongenerator. Wilt u in 
          dat geval  toch geluid  krijgen uit uw keyboard/synthesizer, 
          zult u de MIDI-out met de MIDI-in moeten verbinden! (PAS OP: 
          Staat  Local Control aan, dan wordt dit geintje door diverse 
          fabrikanten ten strengste afgeraden!)
          
          
                           A L L   N O T E S   O F F 
          
          Deze MIDI-boodschap  (Bxh 7Bh  00h) zet  alle tonen, die nog 
          aanstaan  op het  betreffende kanaal in ��n keer uit. Alleen 
          wanneer  het   sustain-pedaal  aan  staat  (controller  40h) 
          behoren de tonen aan te blijven staan. In de praktijk blijkt 
          dit  echter niet zo te zijn. Sommige fabrikanten houden zich 
          er netjes aan, anderen zetten ook dan alle noten uit.
          
          
               O M N I   O N / O F F ;   P O L Y   E N   M O N O 
          
          De volgende MIDI-events:
          
          Bxh 7Ch 00h (Omni mode off)
          Bxh 7Dh 00h (Omni mode on)
          Bxh 7Eh 00h (Mono mode on)
          Bxh 7Fh 00h (Poly mode on)
          
          hebben   invloed   op  de   diverse  MIDI-modi   (enkelvoud: 
          MIDI-modus). Als  enige overeenkomst  zetten ze allemaal ook 
          alle  noten uit.  Voor de  rest zijn deze MIDI-modi hopeloos 
          verouderd; vrijwel  geen enkel  hedendaags produkt maakt nog 
          gebruik  hiervan. Dat wil geenszins zeggen, dat (zoals ik al 
          eens meegemaakt heb) deze MIDI-events als vervanging voor de 
          gewone ALL NOTES OFF gebruikt mogen worden!
          
          Voor de ge�nteresseerden:
          MIDI kent 4 modi:
          
          1) Omni on, poly
          Het instrument  accepteert alle voice-boodschappen over welk 
          kanaal  dan  ook  en zal  alle tonen  aanzetten (tenzij  het 
          instrument een polyfonie-grens heeft).
          
          2) Omni on, mono
          Het  instrument accepteert alle voice-boodschappen over welk 
          kanaal dan  ook, maar  zal slechts  ��n toon  tegelijk laten 
          horen (speciaal voor oude, monofone synthesizers).
          
          3) Omni off, poly
          Het instrument accepteert alleen die voice-boodschappen, die 
          via hetzelfde kanaal komen als de OMNI OFF boodschap. Verder 
          zullen  wel alle  tonen, die  via dat kanaal worden gestuurd 
          gespeeld worden (tenzij er een polyfonie-grens is).
          
          4) Omni off, mono
          Alleen  die voice-boodschappen  die via  het, met behulp van 
          OMNI  OFF,  geselecteerde  kanaal  verstuurd worden,  worden 
          gespeeld, maar  nooit meer  dan ��n  tegelijk. Deze modus is 
          speciaal  bedoeld  voor  MIDI-gitaar-spelers. Elke  snaar op 
          zo'n  gitaar  correspondeert  met  een  MIDI-kanaal. Op  die 
          manier  kan de  gitaar-speler met behulp van zijn 6 snaren 6 
          verschillende instrumenten aansturen.
          
          
                                    S L O T 
          
          Poeh,  we  zijn  dan  toch  bij  het einde  van dit  verhaal 
          gekomen.  Zoals  u  wellicht  is  opgevallen,  valt over  de 
          Control Changes veel, heel veel te vertellen. Desondanks heb 
          ik alles  slechts summier  behandeld (enkele  uitzonderingen 
          daargelaten).  Voor diegenen, die willen gaan experimenteren 
          staat op deze diskette een BASIC-programma, dat via de Music 
          Module van  Philips laat  horen wat  het effect  is van elke 
          besproken  controller.  Gebeurt  er  op  een gegeven  moment 
          niets,  dan ondersteunt  uw instrument deze controller niet. 
          RUN't  u  het programma  en er  gebeurt helemaal  niets, dan 
          dient u een Music Module, MIDI-kabels of een MIDI-instrument 
          te kopen  en/of deze  naar behoren  met elkaar  te verbinden 
          (MIDI-out naar MIDI-in). 
          
          Ik wens u nog veel MIDI-plezier en tot een volgende keer...
          
                                                      Ruud van Gestel
