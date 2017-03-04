       M S X 2 / 2 +   V D P   C U R S U S   ( 4  )
                                                               
          
          Het laatste  commado dat ik de vorige keer heb behandeld was 
          HMMM. Voordat we verdergaan, eerst nog even iets anders.
          
          
                              V E R S N E L L E N 
          
          Er zijn  twee methodes  om het  uitvoeren van  de commando's 
          door de VDP te versnellen.
          
          1) Zet de sprites uit
             Als bit 1 van R#8 gelijk wordt gemaakt aan 1, kan de tijd 
             die normaal voor het weergeven van sprites wordt gebruikt 
             nu gebruikt worden voor het uitvoeren van commando's. Dat 
             gaat daardoor nu sneller.
          
          2) Zet het scherm uit
             Als bit 6 van R#1 gelijk wordt gemaakt aan 0, kan de tijd 
             die normaal voor  het  weergeven  van  het  scherm  wordt 
             gebruikt  nu  gebruikt  worden  voor  het  uitvoeren  van 
             commando's. Dat gaat daardoor nu sneller.
          
             U kunt dit bijvoorbeeld gebruiken bij flip-screens (wordt 
             veel bij MSX2 spellen gebruikt, er is geen scrolling maar 
             als je het scherm uitloopt wordt het even  zwart,  waarna 
             het volgende scherm wordt getoond). Je  kunt  het  scherm 
             namelijk ook zwart maken door PAGE 1 zwart  te  maken  en 
             die te tonen. De schermopbouw gaat echter sneller  als  u 
             bit 6 van R#1 gebruikt.
          
          Ik ga nu verder met het overzicht van alle commando's.
          
          
                                    H M M V 
          
                          High-speed move VDP to VRAM
          
          Dit  commando vult  een bepaalde  rechthoek met een bepaalde 
          kleur. Het doet dus hetzelfde als LINE,BF in BASIC.
          
          High speed betekent geen logische bewerking en transport per 
          byte. De  uitvoering is  daarom veel sneller. Het nadeel van 
          transport  per byte  is dat in SCREEN 5 en 7 (G4 en G6) de X 
          coîrdinaat een  veelvoud moet  zijn van  2, en  in SCREEN  6 
          (G5), de X coîrdinaat een veelvoud moet zijn van 4.
          
          De volgende registers hebben betekenis bij HMMV:
          
          NX: grootte van rechthoek in X richting   R#40, R#41
          NY: grootte van rechthoek in Y richting   R#42, R#43
          DX: X coîrdinaat                          R#36, R#37
          DY: Y coîrdinaat                          R#38, R#39
          DIX, DIY: richting                        R#45
          CLR: kleur                                R#44
          
          Nadat   bovenstaande  data   naar  de  juiste  registers  is 
          geschreven kan het commando worden uitgevoerd door de waarde 
          &B11000000 naar R#46 te schrijven. CE geeft aan of de VDP al 
          klaar is (=0) of niet (=1).
          
          Let op:  R#44 bevat  niet het  kleurnummer, maar de byte die 
          geschreven  moet worden.  Bij SCREEN  5, 6  en 7  kun je dus 
          verticale strepen  maken. Zie voor de opbouw van de byte het 
          commando HMMC in het vorige deel.
          
          
                                    L M M C 
          
                            Logical move CPU to VRAM
          
          Dit  commando verplaatst data van de microprocessor naar het 
          VRAM naar  een bepaalde  rechthoek. De  data wordt per pixel 
          verstuurd,   er  kunnen   dus  logische  bewerkingen  worden 
          toegepast. Er zijn ook geen beperkingen op de x coîrdinaten.
          
          De volgende  registers moeten  eerst met  de juiste  waarden 
          worden gevuld:
          
          R#36, R#37: DX        X coîrdinaat bestemming
          R#38, R#39: DY        Y coîrdinaat bestemming
          R#40, R#41: NX        grootte in X-richting
          R#42, R#43: NY        grootte in Y-richting
          R#44      : data      de juiste data
          R#45      : DIX, DIY  de juiste richting
          
          Daarna kunt  u het  commando laten uitvoeren door &B1011---- 
          naar R#46 te schrijven. Op de plaats van ---- kunt u de code 
          van  de gewenste  logische operatie  schrijven (0000 is geen 
          logische operatie).
          
          De data wordt nu per pixel verzonden. Dat gaat als volgt:
          
              MSB  7   6   5   4   3   2   1   0  LSB
          R#44     -   -   -   -   C3  C2  C1  C0     G4 en G6
          R#44     -   -   -   -   -   -   C1  C0     G5
          R#44     C7  C6  C5  C4  C3  C2  C1  C0     G7 en hoger
          
          Dit commando is de langzamere versie van HMMC. Er kunnen  nu 
          echter wel logische bewerkingen worden gebruikt. Ook is elke 
          X coîrdinaat mogelijk. Ik geef ook weer het werkschema:
          
          1 Stel de commando registers in
          2 Schrijf &B1011---- naar R#46 (logische bewerking op ----)
          3 Lees S#2
          4 Als CE gelijk is aan 0, dan is het klaar
          5 Als TR gelijk is aan 0, dan is het verzenden nog niet
            klaar, ga terug naar 3
          6 Anders kan nu de data naar R#44 worden geschreven
          7 Ga verder bij 3
          
          
                                    L M C M 
          
                            Logical move VRAM to CPU
          
          Dit commando  verplaatst data van een bepaalde rechthoek per 
          pixel  naar  de  CPU.  Het  is  onzin  hierbij een  logische 
          bewerking te gebruiken.
          
          De volgende registers moeten worden ingevuld:
          
          R#32, R#33: SX        Bron X coîrdinaat
          R#34, R#35: SY        Bron Y coîrdinaat
          R#40, R#41: NX        Grootte van rechthoek in X richting
          R#42, R#43: NY        Grootte van rechthoek in Y richting
          R#45      : DIY, DIX  Richting
          
          Het commando wordt uitgevoerd door de waarde &B10100000 naar 
          R#46 te schrijven. De data kan dan uit S#7  worden  gelezen. 
          Denk er daarbij om dat de data verschilt per screenmode. Het 
          overzicht is hetzelfde  als  de  data  die  bij  het  vorige 
          commando naar CLR moest worden geschreven. Ik geef u voor de 
          duidelijkheid wederom een werkschema:
          
          1 Lees S#7
          2 Schrijf de juiste waardes naar de commando registers
          3 Schrijf &B10100000 naar R#46
          4 Lees S#2
          5 Als TR=0 dan transport nog niet klaar, ga naar 4
          6 Lees S#7 en doe met de waarde wat u wilt
          7 Als CE gelijk is aan 0 is het commando klaar
          8 Ga anders naar 4
          
          Let op:
          TR  moet  op  0  worden  gezet  voordat  het commando  wordt 
          uitgevoerd.  Daarom moeten  eerst S#7 worden gelezen. Ook al 
          is de  data naar S#7 geschreven en TR=1, zal daarna toch het 
          commando  worden afgemaakt  en zal  CE gelijk worden gemaakt 
          aan 0.
          
          
                                    L M M M 
          
                           Logical move VRAM to VRAM
          
          Het  LMMM  commando verplaatst  de data  van het  aangegeven 
          rechthoekige stuk naar een ander rechthoekig stuk. Omdat dit 
          gaat  in  eenheden  van  pixels, kunt  u logische  operaties 
          gebruiken. Ook dit is een zeer veel gebruikt commando.
          
          De volgende registers moeten  eerst  van  de  juiste  waarde 
          worden voorzien:
          
          R#32, R#33: SX        Bron X coîrdinaat
          R#34, R#35: SY        Bron Y coîrdinaat
          R#36, R#37: DX        Doel X coîrdinaat
          R#38, R#39: DY        Doel Y coîrdinaat
          R#40, R#41: NX        Grootte X richting
          R#42, R#43: NY        Grootte Y richting
          R#45      : DIX, DIY  Richting
          
          Schrijf &B1001---- naar R#46 om het commando uit te  voeren. 
          U kunt  op  de  streepjes  de  gewenste  logische  bewerking 
          invullen.
          
          Dit commando wordt door MSX BASIC 2.0 en hoger gebruikt voor 
          COPY (SX,SY)-STEP(NX,NY) TO (DX,DY),,<logische bewerking>.
          
          
                                    L M M V 
          
                            Logical move VDP to VRAM
          
          Dit commando doet hetzelfde als HMMV, met het  verschil  dat 
          er nu wel logische bewerkingen kunnen  worden  toegepast  en 
          dat het transport nu met pixels in plaats van bytes gebeurt. 
          De X coîrdinaat kan dus elke waarde hebben. Het gaat  daarom 
          ook langzamer.
          
          Dit commando wordt in MSX BASIC 2.0 en hoger  gebruikt  voor 
          LINE(DX,DY)-STEP(NX,NY),CLR,BF,<logische bewerking>.
          
          Voor de werking verwijs ik dus naar HMMV, er moet nu  echter 
          &B1000---- naar R#46 worden geschreven,  en  op  de  laagste 
          vier bits de logische bewerking. Van de X coîrdinaat  worden 
          alle bits behouden.
          
          
                                    L I N E 
          
          De  naam zegt  het al, met dit commando kunt u de VDP lijnen 
          laten trekken.  Die lijn  die wordt  getekend is  de schuine 
          lijn  van een  rechthoekige driehoek.  (Dat is  een driehoek 
          waarvan  twee  zijden loodrecht  op elkaar  staan.) De  twee 
          rechthoekszijden worden  aangegeven als  afstanden vanaf ÇÇn 
          punt. (Dat is het punt waar de rechte hoek zit.)
          
          De volgende registers  moet  u  beschrijven  met  de  juiste 
          waardes:
          
          R#36, R#37: DX        X coîrdinaat
          R#38, R#39: DY        Y coîrdinaat
          R#40, R#41: MJ        grootte van de langste zijde (NX)
          R#42, R#43: MI        grootte van de kortste zijde (NY)
          R#45      : DIX       richting van de rechthoekszijden
                                vanaf het punt (DX,DY)
                                0: rechts  1: links
                    : DIY       0: omlaag  1: omhoog
                    : MAJ       0: lange zijde is X
                                1: lange zijde is Y
          R#44      : C         Kleur (G4,G6 0-15; G5 0-3; G7 0-255)
          
          Schrijf &B0111----  naar R#46  om de  lijn te trekken. Op de 
          streepjes kunt u een eventuele logische bewerking invullen.
          
          Dit is  erg ingewikkeld. Zoals al eerder aangekondigd zullen 
          in  latere delen van de VDP cursus vele toepassingen van het 
          geleerde  behandeld  worden, voorzien  van zeer  uitgebreide 
          uitleg.  Daar  zal  ook  zeker een  routine voor  lijnen bij 
          zitten. Voor de duidelijkheid geef ik u nu nog hoe het er in 
          BASIC uit zou zien:
          
          LINE (DX+MAJ,DY)-(DX,DY-MIN),C,,<logische operatie>.
          Hierbij zijn DIX, DIY en MAJ gelijk aan 0.
          
          Zoals gewoonlijk is CE van S#2 gelijk aan 1 als het commando 
          wordt uitgevoerd.
          
          
                                    S R C H 
          
          Het SRCH commando zoekt (search = zoeken) naar een pixel met 
          een bepaalde  kleur naar  de linker-  of rechterkant van een 
          bepaald  startpunt. Dit commando wordt bijvoorbeeld gebruikt 
          bij het PAINT commando van BASIC.
          
          De volgende registers worden door SRCH gebruikt:
          
          R#32, R#33: SX        X coîrdinaat startpunt
          R#34, R#35: SY        Y coîrdinaat startpunt
          R#44        C         te zoeken kleur
          R#45      : DIX       Geeft zoekrichting aan (0 = rechts)
                    : EQ        Geeft aan wanneer de VDP moet stoppen
                                met zoeken. 1 = stop als een pixel met 
                                de juiste kleur is gevonden, 0 = stop
                                als een pixel met een andere kleur is 
                                gevonden.
          
          Schrijf de waarde &B01100000 naar R#46 om  het  commando  te 
          starten. De statusregisters worden als volgt beãnvloed:
          
          S#2       : BD        Bit 4 wordt 1 als kleur gevonden
                    : CE        Bit 0 wordt 0 als commando klaar
          S#8       : BX        X coîrdinaat van plaats waar gevonden
          S#9       : BX        Bit 0 bevat bit 8 van X coîrdinaat
                                Overige bits bevatten de waarde 1
                                (bit 8 wordt alleen gebruikt bij G5
                                 en G6)
          
          Werkschema:
          
          1) Schrijf de juiste waardes naar de commandoregisters
          2) Schrijf &B01100000 naar R#46
          3) Lees S#2
          4) Als CE=1 dan 3, commando nog niet klaar
          5) Als BD=0 dan klaar, kleur niet gevonden
          6) Lees S#8 en S#9
          7) Klaar
          
          
                                    P S E T 
          
          Het PSET commando doet hetzelfde  als  PSET(X,Y),C,<logische 
          operatie> in Basic. Het zet een  pixel  in  de  kleur  C  op 
          positie (X,Y), daarbij wordt eventueel de logische bewerking 
          uitgevoerd met de pixel die er al stond.
          
          De coîrdinaten moeten in DX en DY staan, R#36-R#39. De kleur 
          kunt  u  in  R#44  zetten.  De  volgende  kleurnummers  zijn 
          mogelijk:
          
          Mode:              Kleurbereik:
          -------------------------------
          SCREEN 5, G4       0-15
          SCREEN 6, G5       0-3
          SCREEN 7, G6       0-15
          SCREEN 8, G7       0-255
          
          Schrijf  de waarde &B0101---- naar R#46 om het PSET commando 
          uit  te  voeren.  Op  ----  kunt  u  eventueel  een logische 
          operatie invullen.
          
          
                                   P O I N T 
          
          Dit commando doet het tegenovergestelde van PSET. Het  leest 
          namelijk de kleur van een bepaald pixel. In BASIC hebben  we 
          daarvoor het commando POINT(X,Y).
          
          De coîrdinaten  moeten nu  in SX  en SY staan, R#32-R#35. De 
          kleur  komt in  S#7 te  staan. Daarbij  zijn dezelfde kleur- 
          nummers mogelijk als bij PSET.
          
          Schrijf de waarde &B01000000 naar R#46 om het POINT commando 
          uit te  voeren. Het CE bit van S#2 zal tijdens het uitvoeren 
          van het commando de waarde 1 krijgen. Als het klaar is wordt 
          CE weer op 0 gezet. Dan kan S#7 worden uitgelezen.
          
          
                         N A   E E N   C O M M A N D O 
          
          Dit was het einde van het overzicht van alle commando's. Tot 
          slot nog  een overzicht  van de beãnvloeding van de commando 
          registers  door de verschillende commando's. Ofwel: hoe zien 
          de commandoregisters eruit als een commando is uitgevoerd.
          
          Het  is namelijk niet nodig om alle registers steeds opnieuw 
          in te  vullen. Sommigen  behouden hun  waarde. U  kunt in de 
          tabel  zien welke  dat zijn.  In de  praktijk zullen meestal 
          toch alle commandoregisters worden beschreven.
          
          Uitleg bij de tabel:
          
          - Onveranderd
          * Coîrdinaat aan het einde van commando (of kleurcode)
          # De waarde die het register had toen het einde van het
            scherm werd bereikt
          
                   SX   DY   DX   DY   NX   NY   CLR  CMRH CMRL ARG
          ------------------------------------------------------------ 
          HMMC     -    -    -    *    -    #    -    0    -    -
          YMMM     -    *    -    *    -    #    -    0    -    -
          HMMM     -    *    -    *    -    #    -    0    -    -
          HMMV     -    -    -    *    -    #    -    0    -    -
          LMMC     -    -    -    *    -    #    -    0    -    -
          LMCM     -    *    -    -    -    #    *    0    -    -
          LMMM     -    *    -    *    -    #    -    0    -    -
          LMMV     -    -    -    *    -    #    -    0    -    -
          LINE     -    -    -    *    -    -    -    0    -    -
          SRCH     -    -    -    -    -    -    -    0    -    -
          PSET     -    -    -    -    -    -    -    0    -    -
          POINT    -    -    -    -    -    -    *    0    -    -
          ------------------------------------------------------------ 
          
          N.B. De eindwaardes van SY, DY  en  NY  (SY*,  DY*  en  NYB) 
          moeten als volgt worden omgerekend:
          
          (DIY = 0)     SY* = SY + N   DY* = DY + N
          (DIY = 1)     SY* = SY - N   DY* = DY - N
          
          NYB = NY - N
          
          Bij het LINE commando moet u nog 1 van N aftrekken  als  MAJ 
          gelijk is aan 0.
          
          
                                T O T   S L O T 
          
          Met  deze  theorie  kunt  u  nu  alle  VDP  commando's  zelf 
          toepassen.  Ik  heb  bij  deze herhaling  van de  VDP cursus 
          echter  nog   een  extra   tekst  geschreven,  waarin  wordt 
          uitgelegd  hoe je  de commando's  het beste naar de VDP kunt 
          sturen, met een aantal voorbeelden. Dit staat in het submenu 
          onder de naam VDP CURSUS ML.
          
          De volgende keer zijn de statusregisters aan de beurt. Nadat 
          ook de  sprites en  de opslag  van de scherminhoud behandeld 
          zijn zullen we een aantal zeer leuke toepassingen doornemen. 
          Veel succes en tot de volgende keer!
          
                                                          Stefan Boer
          