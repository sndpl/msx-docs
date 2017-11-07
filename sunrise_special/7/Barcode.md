          
                                B A R C O D E S 
                                                 
          
          Dit tekstje  gaat over barcodes en het lezen daarvan. Het is 
          ook voor degenen die geen barcode reader hebben geschikt.
          
          Voor   onze  MSX   computers  zijn   heel  wat  interessante 
          cartridges uitgebracht,  zoals de cartridge met sensoren uit 
          Japan,  de robotarm van Spectravideo, de Sanyo Lightpen (als 
          je die  hebt, bel  me dan  eens!) en  de barcode  reader van 
          Philips.  Aangezien ik  de laatste in mijn bezit heb, ben ik 
          er wat  mee gaan rommelen, waarvan dit artikel het resultaat 
          is.
          
          Ik  ben niet  de eerste die dat doet; in MCM 47 staat er een 
          artikeltje  over,   maar  toen   ik  mijn   MCM-stapel  eens 
          uitmestte,  bleek  het  dat nr.  47 slechts  schitterde door 
          afwezigheid... Ik moet het dus allemaal zelf uitzoeken...
          
          
                              D E   W E R K I N G 
          
          Allereerst heb ik de reader maar eens opengeschroefd. Al het 
          werk wordt gedaan door een speciaal barcode IC. De gebruiker 
          hoeft  slechts de  pen over  de barcode  te halen  en het IC 
          verwerkt het  signaal en  slaat de barcode op in een buffer. 
          De communicatie met de MSX verloopt via I/O poort &H18, maar 
          door  middel van  een jumper kan ook I/O poort &HB8 gebruikt 
          worden. We  zullen dus eerst moeten weten via welke poort de 
          communicatie verloopt. Een handige oplossing hiervoor is het 
          volgende  BASIC programma,  dat ook  op disk  staat onder de 
          naam BAR-ADR.BAS:
          
          10 P=&H18:GOSUB 40:P=&HB8:GOSUB 40:P=0
          20 PRINT "Poort: ";HEX$(P)
          30 END
          40 IF PEEK(P)=255 THEN RETURN ELSE RETURN 20
          
          N.B. Als P=0 dan is er geen barcode reader aanwezig.
          
          Vervolgens moeten we een (BASIC-)programma hebben om de code 
          ook daadwerkelijk uit te kunnen lezen. Dit staat ook op disk 
          onder de naam BAR-READ.BAS.
          
          Als we de pen over een barcode halen, dan zal de code in een 
          buffer  worden opgeslagen. Dat betekent dat er geen commando 
          nodig is  om het lezen te starten. Ook tijdens een programma 
          kan  je dus  een barcode inlezen, omdat de hardware het hele 
          zaakje afhandelt.  De code  blijft in de buffer staan totdat 
          die  uitgelezen wordt  of totdat  een nieuwe barocde gelezen 
          wordt. We  hoeven dus  alleen de  buffer uit te lezen via de 
          I/O  poort die  ingesteld is. In het vervolg van het artikel 
          ga ik  voor het  gemak uit  van I/O poort &H18. Bit 7 is het 
          status  bit. 0 betekent dat de barcode gelezen kan worden en 
          1 dat het einde van de barcode bereikt is.
          
          We hoeven alleen iets te lezen als de code gereed staat, dus 
          als bit  7 laag  is. Dat  wil zeggen dat het getal op de I/O 
          poort  kleiner dan  128 is. Daarom wordt de eerste regel van 
          het uitleesprogramma:
          
          10 B=INP(&H18):IF B>127 THEN 10
          
          Als dat  niet zo is, dan is de waarde van belang en moet dus 
          bewaard  worden.  Uiteindelijk  is het  de bedoeling  dan de 
          string  B$ de  code bevat.  We zetten  de waarde  van de I/O 
          poort dus in B$:
          
          20 B$=CHR$(B)
          
          Vervolgens  kunnen  we  de rest  van de  code uitlezen,  dus 
          totdat  het eind  van de code bereikt is en bit 7 hoog wordt 
          (en de waarde van de I/O poort dus groter dan 127 wordt):
          
          30 B=INP(&H18):B$=B$+CHR$(B AND 127): IF B<128 THEN 30
          
          Merk op  dat de  waarde waarbij  bit 7  geset wordt  nog wel 
          meetelt!  Je moet  dus eerst de waarde opslaan en DAARNA pas 
          kijken of  die groter dan 127 wordt (en dus de laatste was). 
          De  AND 127  dient daar  voor, want een AND 127 maakt immers 
          altijd bit  7 laag  en zorgt  er dus voor dat ook de laatste 
          waarde juist is.
          
          Vervolgens geven we een BEEP ten teken dat de code ingelezen 
          is en we printen de string op het scherm.
           
          40 BEEP:PRINT B$  
          
          De  code bevat  eerst een letter. Dit geeft het type barcode 
          aan. Er  zijn namelijk verschillende manieren om een barcode 
          te  maken. Leuk  om te weten, veel heb je er niet aan. In de 
          meeste gevallen  (zeker bij supermarktprodukten) zal het een 
          D  zijn. Wat  echter wel interessant is aan die letter is of 
          het een  hoofd- of  kleine letter  is, of anders gezegd : of 
          bit 5 van de eerste waarde van de code geset is of niet. Bit 
          5  geset betekent  namelijk dat  de barcode  van rechts naar 
          links is  gelezen en  Bit 5  gereset (laag)  betekent dat de 
          barcode van links naar rechts is gelezen.
          
          
                  D E   O P B O U W   V A N   E E N   C O D E 
          
          Nu het  mogelijk is om een barcode in te lezen, gaan we eens 
          kijken  hoe die  opgebouwd is.  Om de code optisch te kunnen 
          lezen, wordt  elk getal weergegeven door 2 witte en 2 zwarte 
          strepen, elk met een lengte varierend tussen 1 en 4 modulen. 
          Hoe  de  code  precies  gecodeerd  is,  heb  ik niet  kunnen 
          achterhalen.
           
          De  cijfers  zelf  hebben  ook  nog  een  betekenis,  als ze 
          tenminste aan  de internationale  norm voldoen.  Dat zal bij 
          supermarkt  produkten zeker  het geval  zijn. Soms gebruiken 
          bedrijven  etc.  eigen  (interne)  streepjescodes. Een  mooi 
          voorbeeld is de handleiding van mijn CM11342 RGB-monitor van 
          Philips, daarin  worden niet alleen cijfers maar ook letters 
          gebrukt;  het typenummer  staat in  de streepjescode.  In de 
          Times  staat  een  voorbeeld,  DEMO2,  van  zo'n  afwijkende 
          streepjescode. Deze is afkomstig van een zending van Philips 
          en bevat mijn adres. Dit soort codes is dus niet standaard.
          
          Binnen de standaard kunnen we 3 types onderscheiden:
          
          1. De code voor boeken
          
          Deze beginnen  met 97 en de rest is ISBN-nummer. Het laatste 
          cijfer is een controlecijfer, meer hierover bij 2.
          
          2. De code voor produkten per stuk (met een vaste prijs)
          
          Deze zijn  op allerlei  etenswaren te vinden. De eerste twee 
          cijfers  bevatten de landcode, bijvoorbeeld 87 is Nederland. 
          De   volgende   5   cijfers   bevatten   de   producentcode, 
          bijvoorbeeld 10400  is Albert  Heijn Huismerk, de volgende 5 
          cijfers bevatten het artikelnummer, en het laatste cijfer is 
          een  controlecijfer. Als je dit cijfer optelt bij de som van 
          de even  cijfers (excl.  controlegetal) en 3 maal de som van 
          de  oneven cijfers (excl. controlegetal) precies een tiental 
          opleveren.
          
          Om het  een en  ander te verduidelijken volgt een voorbeeld. 
          We gebruiken de in de Times afgebeelde code, DEMO1:
          
          2263182000062
           
          Dat shrijven we voor het gemak even als :
          
          22 63182 00006 2
          
          Het controle  cijfer is  zoals je  ziet 2.  Controle of  het 
          klopt:
          
          Som v.d. even cijfers : 2+2+6+8+2+6         =  26
          3 keer de som v.d. oneven cijfers : 3*(3+1) =  12
          Het controlegetal                           =   2  +
          ----------------------------------------------------
                                                         40
          
          En dat  is een  tiental! Het  komt ook  voor dat een bepaald 
          artikel  1 code heeft voor producent/artikel. De code is dan 
          5 tekens korter.
          
          
          3. De artikelen per gewicht
          
          Van bepaalde produkten is de prijs niet van te voren bekend, 
          zoals  groente  en  fruit,  kaas,  brood,  vlees/vleeswaren, 
          zelfbedieningssnoepgoed etc.  Op de  bon verschijnt dan vaak 
          een streepjescode. Deze heeft als landcode een cijfer tussen 
          20  en  29.  de volgende  5 cijfers  geven het  product aan, 
          vervolgens   5  cijfers  voor  de  prijs  en  tenslotte  het 
          controlecijfer.
          
          Als voorbeeld een code van Nasivlees voor � 4.36:
          
          2221001004363
          
          22   : Geeft aan  dat het  om een  artikel uit de groep "per 
                 gewicht" gaat
          21001: Nasivlees
          00436: De prijs (in centen)
          3    : Controlecijfer
          
          
                             D E   S O F T W A R E 
          
          Al  het bovenstaande is verwerkt in een programma, dat op de 
          disk staat  onder de  naam BARCODE.BAS. Het is geschikt voor 
          alle MSX'en en het werkt op SCREEN 0 met WIDTH80 of WIDTH40. 
          Dit programma is in BASIC, maar voor assembler of een andere 
          programmeertaal geldt natuurlijk precies hetzelfde.
          
          Dit  programma  herkent de  hierboven genoemde  soorten. Het 
          programma  gebruikt  de  datafile  BARCODE.DAT. Als  het een 
          barcode  herkent,  dan  wordt  de informatie  op het  scherm 
          gezet, anders  wordt je  gevraagd de  gegevens in  te typen. 
          Landcodes  en  producentcodes  die al  ooit ingegeven  zijn, 
          worden  herkend, dus  van sommige  dingen hoef je alleen het 
          artikel in te geven.
          
          De besturing werkt geheel met de reader. Daarvoor zijn er in 
          de  Times  5  besturingsstreepjescodes  afgedrukt.  Dit zijn 
          gewoon    afgeknipte   bonnetjes    uit   de    Albert-Heijn 
          groenteweegschaal, maar ze worden door het programma herkend 
          als zijnde  een besturingscode.  In theorie  is het mogelijk 
          dat  groente of  fruit van  Albert Heijn  als besturingscode 
          wordt gezien, maar in de praktijk is dat vrijwel onmogelijk, 
          omdat ik  een gewichtje van 10 gram gebruikt heb en speciale 
          soorten groente/fruit. Want wie koopt er nou 10 gram appels?
          
          Als  eerste, voor  het opstarten  van de software, moet je 2 
          keer over  een streepjescode  heengaan. Als  je de  computer 
          aanzet,  staat er namelijk rommel in de buffer van de reader 
          en als  je een  code leest  is dat  weg. Daarna  kun je  het 
          programma starten met RUN"BARCODE.BAS".
          
          Op  disk  staat  een  voorbeeldfile  met een  aantal bekende 
          produkten  erin, zodat je niet alles hoeft in te typen en er 
          ook in  het eerste  begin al wat herkend wordt. Hierbij moet 
          je  denken aan Albert Heijn en Melkunie melk, Pickwick Thee, 
          DE  Koffie  etc.  Het  is  ook  mogelijk om  zonder file  te 
          beginnen, u  kunt BARCODE.DAT  dus ook wissen. Het programma 
          maakt dan een nieuwe aan zodra je iets ingeeft.
          
          In  het begin  van het programma moet u een barcode ingeven. 
          Op dat  moment zijn  ook de keuzes STOP en HELP mogelijk. In 
          de  rest van  het programma wordt aangegeven welke keuzes je 
          kunt maken.
          
          Het  programma  is  nog  niet  volmaakt,  zo  kunt  je niets 
          verbeteren  en  ook niets  wissen. Als  je zin  hebt om  het 
          programma uit te breiden, ga je gang! Ik zou het leuk vinden 
          als je  het ons  opstuurde, zodat het op de volgende Special 
          geplaatst kan worden. Ook andere toepassingen met behulp van 
          de  barcode reader  zijn welkom.  Stuur deze  dan naar  mijn 
          adres en niet naar de postbus.
          
                                                       Rob Augusteijn
