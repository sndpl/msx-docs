                              C I R C L E 
                                               
          
          Met  het CIRCLE  commando in BASIC kunt u heel wat meer doen 
          dan simpele  cirkeltjes tekenen.  In dit  artikel zal  ik de 
          volledige  syntax  behandelen  en  twee  eenvoudige listings 
          bespreken die gebruik maken van het CIRCLE commando.
          
          
                                  S Y N T A X 
          
          De volledige offici�le syntax van het CIRCLE commando luidt:
          
          CIRCLE  [STEP] (x co�rdinaat middelpunt,y co�rdinaat middel- 
          punt), straal, [kleur], [beginhoek], [eindhoek], [hoogte/
          breedte verhouding]
          
          x en y co�rdinaten middelpunt tussen -32768 en +32767
          straal tussen -32768 en +32767
          kleur afhankelijk van SCREEN: 0-15  in SCREEN 2,3,4,5,7,10
                                        0-3   in SCREEN 6
                                        0-255 in SCREEN 8,11,12
          
          Dit was  al wel bekend neem ik aan. Voordat ik verder ga met 
          de  beginhoek en  eindhoek, moet ik eerst even uitleggen wat 
          radialen zijn.
          
          
                                R A D I A L E N 
          
          Iedereen kent  het getal  pi wel, 3.1415926... Pi is precies 
          de  omtrek van  een cirkel  met diameter 1. Stel je hebt een 
          molen  waarvan  de  wieken  precies  1  meter lang  zijn, de 
          diameter van de wieken samen is dan dus 2 meter en de omtrek 
          van de  cirkel die de wieken 'in de lucht tekenen' is gelijk 
          aan  2pi.  Als  een wiek  een heel  rondje draait,  legt het 
          uiterste puntje dus een afstand van precies 2pi af.
          
          In  plaats van  dat je  zegt dat hij 360 graden is gedraaid, 
          kun je  ook spreken  van een  draaiing van 2pi radialen. 180 
          graden  komt overeen  met pi, 90 graden met 1/2pi, 45 graden 
          met 1/4pi, etc. In het algemeen geldt de volgende formule:
          
          <hoek in rad> = <hoek in graden> * pi / 180
          
          Bij wis-  en natuurkunde  worden hoeken  meestal in radialen 
          uitgedrukt,  en de  meeste rekenmachines  met goniometrische 
          functies  (sin,  cos,  tan)  kunnen  behalve  graden ook  in 
          radialen werken.
          
          We  hebben  deze  kennis  nodig,  omdat de  beginhoek en  de 
          eindhoek  bij het  CIRCLE commando in radialen moeten worden 
          opgegeven. Oh  ja, iets  wat je ook nog nodig hebt is dat de 
          stand  van de  kleine wijzer om 3 uur overeen komt met 0, om 
          12 uur met 1/2 pi, om 9 uur met pi en om 6 uur met 1.5 pi.
          
          
                      B E G I N -   E N   E I N D H O E K 
          
          Je hoeft met het CIRCLE commando niet per se een hele cirkel 
          te tekenen, ook een gedeelte is mogelijk. Hiervoor gebruiken 
          we  de  begin-  en  eindhoeken. Voor  beiden kan  een waarde 
          tussen 0 en 2pi worden ingevuld.
          
          Je kunt  in BASIC  pi overigens  zeer eenvoudig met een voor 
          BASIC maximale nauwkeurigheid verkrijgen met
          
          PI=4*ATN(1)
          
          (de ArcTaNgens van 1 is immers gelijk aan 1/4 pi)
          
          We  kunnen dus  bijvoorbeeld alleen het stuk "van twaalf tot 
          drie uur" tekenen met:
          
          CIRCLE (100,100),50,,0,PI/2
          
          
                                 S T R A L E N 
          
          Het CIRCLE  commando kan  echter nog meer! Door een minteken 
          voor de hoek te zetten, zal hij namelijk het uiteinde van de 
          cirkelboog  verbinden met het middelpunt door een lijn (zo'n 
          lijn heet dan een straal). Zo kunnen we dus een soort pacman 
          met bek naar rechts tekenen met:
          
          CIRCLE (100,100),50,,-PI/4,-7*PI/4
          
          En door  voor begin- en eindhoek hetzelfde getal op te geven 
          krijgen  we alleen  nog maar  de lijn! Dit gebruiken we voor 
          een eenvoudig  programma om  cirkeldiagrammen oftewel taart- 
          diagrammen te tekenen.
          
          
                            T A A R T D I A G R A M 
          
          100 ' TAART.BAS
          110 ' Taartdiagram met CIRCLE commando
          120 ' Door Stefan Boer
          130 ' Sunrise Special #4
          140 ' (c) Stichting Sunrise 1993
          150 '
          160 KEYOFF:SCREEN0:WIDTH80:COLOR15,0,0
          170 DIMX(29)
          180 PRINT"Taartdiagram":PRINT
          190 PRINT"Voer de gegevens in, 0 om af te sluiten:":PRINT
          200 INPUTX(N):IFX(N)<>0ANDN<29THENT=T+X(N):N=N+1:GOTO200
          210 PI=4*ATN(1):H=-2*PI:N=N-1
          220 SCREEN5
          230 CIRCLE(128,106),100
          240 FORI=0TON
          250 CIRCLE(128,106),100,,H,H
          260 H=H+X(I)*2*PI/T
          270 NEXT
          280 GOTO280
          
          Een overzicht van de gebruikte variabelen:
          
          X(0)...X(29)    de waardes bij 'part' 0 t/m 29
          N               aantal parten
          T               totale waarde van parten
          PI              3.1415926...
          H               hoek
          I               lusteller
          
          De  werking is  vrij eenvoudig. We beginnen bij een hoek van 
          -2pi en tellen vervolgens telkens
                         
                          2PI
                     X(I) ---
                           T
          
          bij de hoek op, dat is namelijk precies de hoek van part I.
          
          
                          H O O G T E / B R E E D T E 
          
          Normaal gesproken  tekent de  computer een cirkel waarvan de 
          hoogte  en de  breedte twee keer zoveel pixels tellen als de 
          straal.  Zo'n   cirkel  is  echter  (zeker  in  SCREEN  6/7) 
          nauwelijks  rond te  noemen, en  bovendien wil je soms juist 
          een  ovaal  hebben.  Daarvoor  is de  laatste parameter,  de 
          hoogte/breedte  verhouding.  Hiervoor  kan  een  willekeurig 
          positief getal worden ingevuld.
          
          Als dit getal kleiner is dan 1, dan wordt de hoogte met deze 
          verhouding vermenigvuldigd.  Is het  groter dan 1, dan wordt 
          de breedte door de verhouding gedeeld.
          
          Bij 50 Hz kunt u de volgende waardes gebruiken om cirkels te 
          krijgen die mooi rond zijn:
          
          SCREEN          hoogte/breedte verhouding
          
          2,3,4           1.4
          5,8,10,11,12    1.36
          6,7             0.68
          
          
                     C I R K E L   I N   R E C H T H O E K 
          
          We  kunnen  de  hoogte/breedte  verhouding gebruiken  om een 
          routine te programmeren die een cirkel tekent die precies in 
          een  rechthoek  past.  Hierbij  moet  wel  even goed  worden 
          nagedacht,  want  je  doet  heel snel  iets verkeerdom.  Het 
          uiteindelijke resultaat is onderstaande listing.
          
          100 ' CIRKEL.BAS
          110 ' Tekent cirkel precies in rechthoek
          120 ' Door Stefan Boer
          130 ' Sunrise Special #4
          140 ' (c) Stichting Sunrise 1993
          150 '
          160 KEYOFF:SCREEN0:WIDTH80:COLOR15,0,0
          170 PRINT"Cirkel in rechthoek":PRINT
          180 INPUT"X1 ";X1
          190 INPUT"Y1 ";Y1
          200 INPUT"X2 ";X2
          210 INPUT"Y2 ";Y2
          220 SCREEN5
          230 LINE(X1,Y1)-(X2,Y2),8,B
          240 XS=ABS(X1-X2):YS=ABS(Y1-Y2)
          250 XC=(X1+X2)/2:YC=(Y1+Y2)/2
          260 V=YS/XS:IFV<1THENR=ABS(XC-X1)ELSER=ABS(YC-Y1)
          270 CIRCLE(XC,YC),R,,,,V
          280 GOTO280
          
          De gebruikte variabelen weer op een rij:
          
          X1,Y1   coordinaten van hoekpunt rechthoek
          X2,Y2   coordinaten van andere hoekpunt
          XS      breedte van rechthoek
          YS      hoogte van rechthoek
          XC,YC   coordinaten van middelpunt
          V       hoogte/breedte verhouding
          R       straal
          
          
                               T E N S L O T T E 
          
          Er  kunnen  met  cirkels zeer  mooie figuren  op het  scherm 
          worden  getoverd, maar  dat laat  ik graag  aan uw  fantasie 
          over. De  theorie kan  nu in  ieder geval  het probleem niet 
          meer zijn.
          
                                                          Stefan Boer
    