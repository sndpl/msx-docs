                       C   C U R S U S   D E E L   6 - 1 
                                                          
          
          
                            1 .   I N L E I D I N G 
          
          Dit deel  is bedoeld  als afsluiting  van de cursus. Denk nu 
          niet  dat iemand  door slechts  deze zes tekstfiles te lezen 
          een volleerde C programmeur zal zijn, maar het is een basis. 
          Op  een  tweetal  zaken  na  (en  die  worden  direkt hierna 
          besproken)  is   zo  ongeveer   de  hele  taal  C  de  revue 
          gepasseerd.  Maar kennis van de taal is niet voldoende om er 
          goede programma's  in te  kunnen schrijven,  ervaring is ook 
          van belang, en we kunnen de kunst ook gedeeltelijk afkijken, 
          gewoon door bestaande C-sources te bestuderen.
          
          Maar  aangenomen dat  we een  project van  enige omvang in C 
          willen uitvoeren  - ik  mag er toch wel van uitgaan dat deze 
          files  niet alleen  gelezen worden omdat 't plot zo spannend 
          is? - hoe kunnen we dan zoiets het beste aanpakken? Dat komt 
          in hoofdstuk 3 ter sprake.
          
          De hoofdstukken  4 gaat over het debuggen van C-programma's, 
          en  hoofdstuk 5 behandelt de ontwikkeling van een vrij groot 
          voorbeeldprogramma. Deze laatste twee hoofdstukken worden in 
          de volgende Sunrise Special gepubliceerd.
          
          
                  2 .   N O G   T W E E   S T A T E M E N T S 
          
          Al enige  tijd terug  heeft iemand  mij erop  gewezen dat ik 
          (foei   toch!)  een   tweetal  statements  ben  vergeten  te 
          bespreken. Dat  klopt inderdaad,  maar tot  mijn verdediging 
          wil  ik  dan  wel  aanvoeren  dat  deze  twee statements  in 
          principe   geen  extra   mogelijkheden  aan  ons  repertoire 
          toevoegen.
          
          
                   2 . 1   H E T   F O R - S T A T E M E N T 
          
          Iedere programmeertaal  schijnt een  for-statement te moeten 
          hebben,  en C is daar geen uitzondering op. Maar in C is het 
          niet nodig  hem te  gebruiken als  simpele teller, zoals zal 
          blijken. De syntax is als volgt:
          
             for (<exp1>; <exp2>; <exp3>) <statement>
          
          Heel  obscuur. Maar  het wordt  wel duidelijker  als we  het 
          equivalente while-statement laten zien:
          
             <exp1>; 
             while (<exp2>) { 
               <statement> 
               <exp3>; }
          
          Toch is er een klein verschil: een continue-statement brengt 
          ons bij <exp3>, en niet naar het begin van de while lus. Een 
          break-statement daarentegen  doet in  beide gevallen precies 
          hetzelfde.
          
          De  meest  gebruikelijke  manier  om  een  for-statement  te 
          gebruiken is als volgt:
          
             for (cntr = 0; cntr < 60; cntr++) printf("%d ", cntr);
          
          Hier  wordt  het  statement inderdaad  als simpel  tellertje 
          gebruikt,  en  de  getallen 0  t/m 59  worden op  het scherm 
          gezet.  (De variabele  'cntr' is  trouwens na afloop 60, hou 
          daar rekening mee!)
          
          We hoeven  overigens niet  alledrie de  uitdrukkingen in het 
          for-statement  te  gebruiken:  we mogen  ze eventueel  zelfs 
          alledrie   weglaten,  maar  als  we  de  middelste  weglaten 
          ontstaat een  eindeloze lus,  die we  alleen met een 'break' 
          kunnen  verlaten. Sommige programmeurs geven de voorkeur aan 
          een
          
               for(;;)
          
          boven een
          
               while(TRUE)
          
          als ze  zo'n eindeloze  lus willen,  maar dat is een kwestie 
          van persoonlijke stijl en smaak.
          
          
                2 . 2   H E T   S W I T C H   S T A T E M E N T 
          
          Hoewel  de functie  van een switch-statement in principe ook 
          door het  gebruik van  if-statements kan worden gesimuleerd, 
          is  het toch erg nuttig, en dan met name omdat het programma 
          er  een  stuk  overzichtelijker  van  kan  worden. Het  komt 
          ongeveer  overeen  met het  CASE-statement in  PASCAL of  in 
          mindere mate  met de  ON..GOTO in BASIC. Helaas is de syntax 
          een beetje lastig, maar het went snel genoeg:
          
             switch(<exp>) <switch-block>
          
          En de syntax van het switch-block:
          
             { <case-label> <statements> 
               <case-label> <statements> 
               ... 
             [default: <statements>] }
          
          Voor  <statements> lees:  een willekeurig  aantal (ook nul!) 
          statements. De  ... geeft  aan dat  een switch-block uit een 
          willekeurig  aantal case-labels  en bijbehorende  statements 
          kan bestaat.  Het default-label  is optioneel,  zoals aan de 
          vierkante haken is te zien.
          
          Case-labels hebben de vorm: case <constante>:
          
          Ok,  dat is  dus een  switch-statement, maar  wat doet 't nu 
          precies?  Wel,  eigenlijk is  het heel  eenvoudig: vanaf  de 
          'switch' wordt  gesprongen naar het statement volgend op het 
          case-label waarvan de <constante> overeen komt met de waarde 
          van  <exp>. Indien zo'n label niet bestaat wordt er naar het 
          statement volgend  op 'default:' gesprongen, en als dat niet 
          bestaat  wordt  geen  enkel  statement  in het  switch-block 
          uitgevoerd,   en  gaat  het  programma  gewoon  verder.  Een 
          voorbeeldje, snel, voor de hersens overbelast raken:
          
               switch(getch()) { 
               case 'j': 
               case 'J': printf("U drukte op de 'J'\n"); 
                         break;
          
               case 'n': 
               case 'N': printf("U drukte op de 'N'\n"); 
                         break;
          
               default:  printf("U heeft 't niet begrepen...\n"); }
          
          Met  getch()  halen  we  een  karakter  van het  toetsenbord 
          binnen, en afhankelijk van welke dat is springen we naar een 
          van de case-labels. Is het een 'j' dan vervolgen we na 'case 
          'j':', en  vanaf daar  vervolgt het  programma zijn  weg, en 
          komt  dan  de  printf-functie  tegen.  De  case-labels  zijn 
          uiteraard geen statements, ze dienen slechts om een bepaalde 
          plek  in het  switch-block aan te geven. De breakstatements, 
          dat  ons  het  switch-block  doen  verlaten,  zijn  dan  ook 
          essentieel, anders zou er komen te staan:
          
               U drukte op de 'J' 
               U drukte op de 'N' 
               U heeft 't niet begrepen...
          
          Dat  levert   wel  weer   wat  hoognodige   inspiratie  voor 
          Sinterklaasgedichten,  de komende  december, maar verder heb 
          je er  niet veel aan. Dit is een belangrijk verschil met een 
          taal  als  PASCAL,  waar in  een CASE-statement  slechts die 
          statements  uitvoert die  bij het betreffende label horen. C 
          is wat  dat betreft  - trouw aan z'n reputatie - moeilijker, 
          maar  flexibeler, en daar hebben we in bovenstaand voorbeeld 
          gebruik van  gemaakt om  de invoer  niet in  hoofdletters te 
          hoeven omzetten.
          
          Misschien  ten overvloede:  we mogen  natuurlijk ook switch- 
          statements maken  met int's of unsigned's in de 'switch', en 
          de constanten in de case-labels mogen elke vorm hebben: alle 
          constanten  worden intern  geconverteerd naar hetzelfde type 
          als de uitdrukking in de 'switch'.
          
          
               3 .   G R O T E R E   P R O J E K T E N   I N   C 
          
          Wie  vol  goede  moed  een  programmaatje  in  C  begint  te 
          schrijven, en  er vervolgens  hier en daar wat functies bij- 
          maakt  merkt al  gauw tot  zijn grote  schrik dat  de source 
          sneller groeit  dan het  financieringstekort op de Russische 
          [Nvdr. Eh... is het niet GOSsische?] begroting, en bovendien 
          al  even ondoorzichtig wordt. Ai, daar zitten we nu met onze 
          goede voornemens:  het is  dan wel niet zo'n zooitje als het 
          gemiddelde  BASIC-programma (eh,  we hebben  onze variabelen 
          tenminste wel  fatsoenlijke namen  gegeven, niet?)  maar een 
          hoop  van onze  functies beginnen al een paar honderd regels 
          lang te  worden, omdat we in die functies steeds weer nieuwe 
          taken  hebben ondergebracht. En ondertussen doet de compiler 
          er twintig  minuten over  om het programma te compileren, en 
          we  hebben onze  B: drive  al afgekoppeld  omdat de compiler 
          anders te weinig geheugen heeft...
          
          Klinkt dat  bekend in  de oren?  Nou ja, we trappen er alle- 
          maal  wel eens  in, het begint allemaal zo klein, zo simpel. 
          Misschien  is  het wel  tijd om  een paar  gouden regels  op 
          papier te zetten:
          
          
                   P R O G R A M M E E R   M O D U L A I R ! 
          
          Dit  is   het  krachtigste  hulpmiddel  om  grote  projecten 
          overzichtelijk  te houden. In tegenstelling tot bijv. PASCAL 
          mag  het   programma  namelijk  in  verschillende,  kleinere 
          sources  gesplitst  worden,  die  apart  gecompileerd  mogen 
          worden.  Pas in de laatste stap wordt dan alles samengevoegd 
          door onze linker.
          
          Hiermee  worden  we op  verschillende manieren  geholpen. Zo 
          gaat  het  compileren  van  kleine  sources  natuurlijk veel 
          sneller  dan  grote,  en  de compiler  zal veel  minder snel 
          geheugenproblemen  krijgen.  Het  dwingt  ons  bovendien  de 
          functies  in  logische,  bij  elkaar  behorende  groepen  te 
          verdelen. Hierdoor wordt de logica (of het gebrek eraan) van 
          het programma  veel duidelijker  zichtbaar. Als  we erg veel 
          moeite  hebben om  een logische  opdeling te  maken zijn  de 
          functies  waarschijnlijk  te  groot,  en  hebben  elk teveel 
          taken.
          
          
                           H O U   H E T   K L E I N 
          
          Zijn onze  functies te  groot en  meer dan  50 regels  is in 
          feite al te groot, en ongeveer 25 regels (een scherm vol) is 
          waarschijnlijk  beter  dan  moeten  we  bovenstaand principe 
          gebruiken  om  onze  functies  te  vereenvoudigen. In  feite 
          moeten  we in  een enkele  zin kunnen zeggen wat een functie 
          precies doet,  en die  zin moeten we dan als commentaarregel 
          voor de functie zetten, zoals:
          
             /* Haal de default-configuratie uit DEFAULT.CNF */
          
          
                                   K I S S   
          
          Een acroniem: Keep It Simple and Stupid. Je kan dit zien als 
          een  uitbreiding van voorgaande regel, en waarschuwt ons het 
          allemaal  niet  te  mooi  te  willen  doen. Een  simpele (en 
          eenvoudig  te  begrijpen)  routine  laat zich  gemakkelijker 
          aanpassen als in de loop van het programmeren de eisen eraan 
          iets  veranderen,  en  zal  minder snel  voor subtiele  bugs 
          zorgen. Als het programma klaar is en werkt kunnen we altijd 
          nog hier  en daar  de zaak optimaliseren. (De ervaring leert 
          echter dat we tegen die tijd allang blij zijn dat 't af is!) 
          Performance-freaks  wil ik  hier graag  wijzen op de 90%-10% 
          regel, een  vuistregel die  erop neerkomt  dat een programma 
          90% van de tijd doorbrengt in slechts 10% van de code. Enige 
          prestatieverbetering  is alleen dan te verwachten als we die 
          10% aanpakken.  Voor 90% van het programma blijft dan gewoon 
          de KISS-regel van toepassing.
          
          
                     I N F O R M A T I O N - H I D I N G   
          
          Een principe wat vooral de betrouwbaarheid van een programma 
          ten  goede zal  komen is "information hiding". Hiermee wordt 
          bedoeld dat  we een  taakverdeling kiezen waardoor (groepen) 
          functies  zoveel mogelijk onafhankelijk van elkaar opereren. 
          Zo  is  een  MODEM-functie  natuurlijk het  gemakkelijkst te 
          gebruiken  als   we  er   eenvoudig  karakters  naar  kunnen 
          schrijven,  of ervan kunnen ontvangen zonder dat we ons druk 
          hoeven maken  hoe dat  verder gebeurt.  Het eerste wat we in 
          zo'n  geval doen is de MODEM-functie van een buffer voorzien 
          voor  inkomende  en  uitgaande  data:  hierdoor  wordt  deze 
          functie  veel  makkelijker  in  het gebruik,  omdat we  geen 
          rekening meer  hoeven houden met de precieze tijd waarop een 
          karakter   arriveert.   We   "verbergen"   dus   de   exacte 
          ontvangsttijd (die is meestal toch onbelangrijk).
          
          Maar  we kunnen buffers niet onbeperkt groot maken: helemaal 
          kunnen we  dus de  tijdfactor niet  uitschakelen. We  zouden 
          echter  kunnen overwegen om de MODEM-functie automatisch een 
          XOFF te laten sturen als de buffer - zeg - tweederde vol is, 
          en een  XON al  hij weer  voor maar  eenderde vol is. In dat 
          geval  hebben  we  dus  de  fysieke manier  van communiceren 
          helemaal afgeschermd van de rest van het programma, die naar 
          believen de informatie kan krijgen en verwerken.
          
          Dit  voorbeeld  maakt  duidelijk  hoe  we een  taakverdeling 
          kunnen  kiezen die maakt dat functies elkaar zoveel mogelijk 
          als een  simpele "black-box" zien, die zich niet druk hoeven 
          te  maken  hoe  de anderen  precies werken.  Maar natuurlijk 
          mogen we geen belangrijke informatie verbergen: bovenstaande 
          MODEM-functie zou geen genade kunnen vinden in de "ogen" van 
          een  ZMODEM-protocolfunctie, omdat  in protocollen de timing 
          wel degelijk  van belang is. Maar in dat geval kunnen we wel 
          weer   een  andere   taakverdeling  vinden   door  bijv.  de 
          MODEM-functie de  data alvast  per frame te laten ontvangen: 
          dan  wordt  de  timing voor  de rest  van het  protocol weer 
          minder kritisch!
          
          Bovenstaand  voorbeeld is  natuurlijk niet meer dan dat: een 
          voorbeeld, en  probeer het  onderliggende principe  in ieder 
          geval  daarvan  los  te  zien.  Het is  in ieder  geval goed 
          gebruik om het aantal parameters van een functie te beperken 
          tot  de essentiele  (bij voorkeur  vier of minder) en zo min 
          mogelijk van  systeemvariabelen gebruik te maken (tenzij die 
          binnen  het  programma  vrij constant  zijn, zoals  bijv. de 
          screen-mode).
          
          
                             E N   T O T   S L O T 
          
          Hou het N E T J E S   E N   O V E R Z I C H T E L I J K !!
          
          Een  paar  tips:  laat  de regels  netjes inspringen  binnen 
          lussen  en  if..else statements  en wees  consequent hierin. 
          Ikzelf vind  het prettigst  om twee  spaties in te springen, 
          maar  sommigen vinden vier, of zelfs meer, overzichtelijker. 
          Een kwestie van smaak.
          
          Indien er  veel regels staan tussen het begin van een lus en 
          het  eind ervan  werkt het  vaak verduidelijkend  om in  een 
          stukje commentaar aan te geven waar het einde van de lus is, 
          zo ongeveer:
          
            while (ptr != NULL && *ptr != '\0') { 
              <een hoop regels> 
            } /* while (ptr !=... */
          
          Gebruik voldoende  commentaar, maar overdrijf niet. Geef wel 
          altijd  (kort) aan  wat een functie geacht wordt te doen, en 
          geef bij  belangrijke (voornamelijk  globale) variabelen aan 
          waar  ze voor  staan als  dit al  niet uit de naam duidelijk 
          wordt.
          
          Probeer  een systeem aan te leggen in de keuze van namen. De 
          namen 'tmp' of 'temp', of zij die ermee beginnen of eindigen 
          zijn  bij   mij  altijd  tijdelijke  variabelen,  en  andere 
          veelgebruikte  (delen van) namen zijn 'src' (source = bron), 
          'dst' (destination = bestemming), 'count' of 'cnt' (aantal), 
          'siz'  of  'size'  (grootte),  'ptr' (pointer),  'x' en  'y' 
          (coordinaten)  en   'buf'  (buffer).  Combinaties  zijn  ook 
          mogelijk,  bv. 'bufsize', waaruit direkt blijkt dat het hier 
          de grootte van een buffer betreft.
          
          Met  '#define'  gedefinieerde  constanten  horen  altijd  in 
          HOOFDLETTERS.  Dit   is  algemeen   gebruikelijk  onder   de 
          C-programmeurs,  die het meestal toch maar zelden met elkaar 
          over iets eens zijn.
          
          En tot  slot, als  je echt  een flink  project aan  de broek 
          hebt:  maak je  aantekeningen in  een LOGBOEK (blocnote, oud 
          schrift, etc.)  zodat het gemakkelijker wordt zelfs na lange 
          tijd nog eens op het project terug te komen.
          
                                                         Robert Amesz
