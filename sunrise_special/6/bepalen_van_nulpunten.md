                   B E P A L E N   V A N   N U L P U N T E N


          In  de serie  numerieke wiskunde  op de Special is deze keer
          het bepalen  van een  nulpunt van  een niet-lineaire functie
          aan de beurt (de methodes kunnen uiteraard ook voor lineaire
          functies  worden  gebruikt,  maar dat  is onzinnig  omdat de
          oplossing daarvan gewoon kan worden uitgerekend).

          f(x) is een functie van R naar R en we zijn op zoek naar een
          x  waarvoor  geldt f(x)  = 0.  Er kunnen  uiteraard meerdere
          nulpunten zijn,  de hier  besproken algoritmes  geven echter
          slechts  ��n nulpunt.  Indien je  alle nulpunten wilt vinden
          moet het algoritme dus meerdere keren worden aangeroepen met
          een  ander  startinterval.  We  gaan  er van  uit dat  f een
          continue functie is.


                                B I S E C T I E

          Het meest  voor de  hand liggende  algoritme om dit probleem
          aan  te pakken  is bisectie. Deze methode is gebaseerd op de
          tussenwaardestelling:

          Zij J een interval, en f: J --> R een continue functie. Neem
          x1  en y1  uit J,  zo dat  x1 <  y1, zij c een waarde tussen
          f(x1) en f(y1). Dan bestaat er een t zo dat x1 <= t <= y1 en
          f(t)=c.

          Het bewijs  laat ik  hier achterwege,  voor het bewijs wordt
          overigens een techniek gebruikt die sterk lijkt op bisectie!
          Het komt erop neer dat als bijvoorbeeld f(1)=3 en f(2)=5, en
          f(x) is continu, dat f(x) dan ergens tussen 1 en 2 de waarde
          4 aan moet nemen.

          Je  begint met  een gesloten  interval [a,b]  waarvoor geldt
          f(a)f(b) <  0. Volgens  de tussenwaardestelling  ligt er dan
          minstens  ��n nulpunt  in [a,b].  De tekens van f(a) en f(b)
          zijn immers verschillend als f(a)f(b) < 0.

          Vervolgens bereken je c als het midden van het interval, dus
          c := (a+b)/2. Nu zijn er drie gevallen te onderscheiden:

          f(a)f(c) < 0: het nulpunt ligt in [a,c]
          f(a)f(c) > 0: het nulpunt ligt in [c,b]
          f(a)f(c) = 0: c is het nulpunt

          In de eerste twee gevallen neem je [a,c] respectievelijk als
          het nieuwe  interval en  ga je  weer verder,  anders ben  je
          klaar.  Aangezien  de  computer niet  met oneinige  precisie
          werkt kan het optreden dat dat nooit voorkomt, daarom is het
          nodig  om een  stopcriterium in te bouwen, we be�indigen het
          algoritme ook  als de lengte van het interval kleiner is dan
          een  bepaalde tolerantie (een heel klein getal, bijvoorbeeld
          0.000000000001).

          Ik  heb  dit  algoritme geprogrammeerd  in het  onderstaande
          BASIC programma dat ook op de disk te vinden is. Probeer het
          eens  uit met  als startinterval [0.5,1]. Je kunt natuurlijk
          ook  een  andere  functie  proberen.  Het  programma zal  je
          waarschuwen indien je een verkeerd startinterval kiest.


          100 ' BISECTIE.BAS
          110 ' Zoek de oplossing van het probleem f(x) = 0
          120 ' door gebruik te maken van de bisectiemethode
          130 ' Door Stefan Boer
          140 ' Sunrise Special #6
          150 ' (c) Stichting Sunrise 1994
          160 '
          170 ' Zet hier de functie
          180 '
          190 DEFFN F(X)=(X+1)*(X+1)*EXP(X*X-2)-1
          200 '
          210 CLS:PRINT"Bisectie":PRINT
          220 INPUT"a = ";A
          230 INPUT"b = ";B
          240 IF FNF(A)*FNF(B) >= 0 THEN PRINT"Kies een ander
              interval!":GOTO 220
          250 PRINT"a";TAB(20);"b";TAB(40);"c";TAB(60);"f(c)"
          260 C=(A+B)/2
          270 PRINT A;TAB(20);B;TAB(40);C;TAB(60);FNF(C)
          280 IF ABS(A-B)<1E-12 THEN 310
          290 Y=FNF(A)*FNF(C)
          300 IF Y<0 THEN B=C:GOTO 260 ELSE IF Y>0 THEN A=C:GOTO 260
          310 PRINT:PRINT"Nulpunt:";C


                          F A L S E   P O S I T I O N

          Bij  bisectie maak  je alleen gebruik van het teken van f(a)
          en  f(b),  niet van  de waarde.  Je gebruikt  dus niet  alle
          informatie. Door  dit wel  te doen  kun je in minder stappen
          het nulpunt bepalen. Je neemt als c het snijpunt met de x-as
          van  de lijn  die de  punten (a,f(a)) en (b,f(b)) met elkaar
          verbindt. Dit algoritme heet false position.

          Ik zal  nu de  afleiding van  de formule  voor dit algoritme
          laten zien. De parametrische voorstelling van de rechte lijn
          is:

                              y - f(a) = m (x - a)

          waarbij m de richtingsco�ffici�nt van de lijn is:

                                    f(b) - f(a)
                                m = -----------
                                      b  -  a

          Het  invullen hiervan  in de  bovenste vergelijking en y = 0
          (we willen  immers het  snijpunt met  de x-as hebben) levert
          op:

                                  a f(b) -  b f(a)
                              c = ----------------
                                    f(b) - f(a)

          Vervolgens   doe   je  hetzelfde   als  bij   bisectie.  Het
          stopcriterium van  bisectie is hier echter ongeschikt, omdat
          bij sommige functies de lengte van het interval niet kleiner
          kan  worden dan  een bepaalde  waarde (teken  maar eens  een
          plaatje van de functie (1/x) - 1 met het interval [0,5]). We
          kiezen daarom  het stopcriterium  |c(i+1) -  c(i)| < epsilon
          waarbij  c(i+1) de nieuwe c is en c(i) de oude c. Epsilon is
          de tolerantie.

          Ik heb ook dit algoritme in BASIC geprogrammeerd. Als je het
          met dezelfde functie en hetzelfde startinterval probeert zul
          je  zien  dat  het  in veel  minder stappen  convergeert dan
          bisectie,  het  gaat  echter  niet  veel  sneller omdat  het
          berekenen van  c meer  tijd kost. Bij de meeste functies zal
          false position minder stappen nodig hebben dan bisectie.


          100 ' FALSEPOS.BAS
          110 ' Zoek de oplossing van het probleem f(x) = 0
          120 ' door gebruik te maken van de false position methode
          130 ' Door Stefan Boer
          140 ' Sunrise Special #6
          150 ' (c) Stichting Sunrise 1994
          160 '
          170 ' Zet hier de functie
          180 '
          190 DEFFN F(X)=X*X+LOG(X)
          200 '
          210 CLS:PRINT"False position":PRINT
          220 INPUT"a = ";A
          230 INPUT"b = ";B
          240 PRINT:PRINT"a";TAB(20);"b";TAB(40);"c";TAB(60);"f(c)"
          250 CC=C:C=(A*FNF(B)-B*FNF(A))/(FNF(B)-FNF(A))
          260 PRINT A;TAB(20);B;TAB(40);C;TAB(60);FNF(C)
          270 IF ABS(CC-C)<1E-12 THEN 300
          280 Y=FNF(C)*FNF(A)
          290 IF Y<0 THEN B=C:GOTO 250 ELSE A=C:GOTO 250
          300 PRINT:PRINT"Nulpunt:";C


                         Z O N D E R   I N T E R V A L

          Dit  waren twee  algoritmes die  werken met een interval. Er
          zijn drie  algoritmes die  met ��n  punt werken:  vast punt,
          Newton-Raphson   en  secant.  De  eerste  twee  zijn  minder
          geschikt om  in BASIC  te programmeren  en de  secantmethode
          voegt  niets nieuws  toe. Ik zal dat hieronder per algoritme
          uitleggen.


                               V A S T   P U N T

          Bij het  vast punt algoritme herschrijf je het probleem f(x)
          =  0 in x = g(x). Vervolgens reken je net zo lang een nieuwe
          x uit totdat het verschil met de vorige x kleiner is dan een
          bepaalde tolerantie.  Deze methode  is niet  in algemene zin
          voor  de  computer  te  programmeren  omdat de  computer het
          probleem niet kan herschrijven. Deze methode is sowieso niet
          handig omdat de herschrijving aan de voorwaarde moet voldoen
          dat  g'(x) < 1 op een bepaald interval. Indien g'(x) > 1 dan
          zal het divergeren.


                          N E W T O N - R A P H S O N

          Je neemt  de raaklijn aan de functie in het punt (x,f(x)) en
          je  nieuwe benadering  is het  snijpunt van deze lijn met de
          x-as. Dit  herhaal je totdat de nieuwe benadering minder dan
          een bepaalde tolerantie verschilt van de vorige.

          Deze methode  heeft twee nadelen: je hebt de afgeleide nodig
          om  de  raaklijn  te  bepalen  en  het  gaat  mis  als f'(x)
          toevallig  0 wordt.  Dan is de raaklijn horizontaal en is er
          geen snijpunt met de x-as.


                                  S E C A N T

          Deze methode is gelijk aan de Newton-Raphson methode, alleen
          schat je  de raaklijn  als een  rechte lijn  door het vorige
          punt  en het huidige punt. Hoewel de afleiding anders is, is
          het resultaat hetzelfde als de false position methode.


                               T E N S L O T T E

          De  volgende  keer  ga  ik  het  waarschijnlijk hebben  over
          numeriek  integreren.   Ook  daar   zijn  een  paar  handige
          algoritmes   voor  die  eenvoudig  in  BASIC  kunnen  worden
          geprogrammeerd. Tot dan!

                                                          Stefan Boer
