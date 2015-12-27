S E T   S C R O L L   I N   B A S I C



          I N L E I D I N G

Op een  MSX2+ kan  (in BASIC) m.b.v. het SET SCROLL commando
een  vrijwel vloeiende  scroll beweging  krijgen. Omdat niet
iedereen zelf  een vloeiende  scroll in  BASIC of  in ML kan
programmeren,   leg  ik  in  deze  tekst  uit  hoe  je  heel
gemakkelijk het SET SCROLL commando kunt gebruiken.


         S E T   S C R O L L

Het  SET SCROLL  commando kan je in alle schermen gebruiken,
behalve in SCREEN 0. Bij gebruik van het SET SCROLL commando
in SCREEN  0 gaat het naar boven scrollen niet hetzelfde als
in  de grafische  schermen. Bij  deze tekstschermen  gaat de
tekst of  eventueel tekening  niet omhoog  maar scrollt  met
rijen  van acht  hoog, (dat  is dus het formaat van de ASCII
karakterset), daar wil ik mee zeggen dat alles wat boven aan
het blok  van acht  hoog verdwijnt, er aan de onderkant weer
bij komt.

In  de grafische  schermen scrollt  het hele  beeld wel goed
maar krijg  je het sprites-blok te zien. Daar heb ik wel een
truukje voor maar dat komt later.


            G E B R U I K

Het gebruik is eigenlijk heel gemakkelijk, je hoeft namelijk
alleen  het  commando  plus  een  getal voor  de horizontale
scrolling  en een  getal voor  de verticale  scrolling in te
voeren.  B.v.  Set  scroll  2,4  betekent  dat  je  2 pixels
horizontaal en 4 pixels verticaal scrollt.

Overigens  mag  je  geen  negatieve  (dus  min)  getallen in
voeren.  Dat  is  ook niet  nodig want  als je  in grafische
schermen  SET  SCROLL  0,0  in  voert geeft  dat het  zelfde
resultaat  als SET  SCROLL 256,0 (voor SCREEN 6 en 7 dus SET
SCROLL 512,0),  alleen is  het scherm bij SET SCROLL 256 (of
512),0 een keer helemaal rond geweest. Zo kan je dus ook met
een  lus optellen van 0 tot 255 (of 511) en ook aftellen van
255 (of  511) tot  0. Dat  laatste geeft dus een scroll naar
links en de eerste naar
rechts. Zo werkt dat ook met het verticaal scrollen.


 V D P ( 1 9  )  E N   V D P ( 2 4  )

Het set  scroll commando  komt overeen  met VDP(19)  (of set
adjust)  en VDP(24).  VDP(19) geeft  namelijk een  soort van
horizontale scroll net als set scroll X,0. Hier staat X voor
een getal.  Er is  wel een groot nadeel aan VDP(19) namelijk
dat  de getallen  die je in kunt voeren van -7 tot +8 lopen.
En met  VDP(19) kan  je een  horizontale EN verticale scroll
maken.  Dan nu  VDP(24), dit  komt volkomen  overeen met het
getal  dat  je  in  voert  bij  set  scroll  voor  verticaal
scrollen. Je  kunt ook  getallen van  0 tot  255 invoeren en
zelfs  het spritesblok  komt ook  mee. Hieronder  heb ik een
klein tabelletje  neergezet waarin  je kunt zien waarmee SET
SCROLL  overeenkomt,  zoals  je ziet  is verticaal  scrollen
hetzelfde als VDP(24).

     HORIZONTAAL  -- VERTICAAL
-------------------------------------------------
|SET SCROLL | GETAL 0 TOT 255 | GETAL 0 TOT 255 |
|-----------|-----------------|-----------------|
|   VDP(19) | GETAL -7 TOT +8 | GETAL -7 TOT +8 |
|-----------|-----------------|-----------------|
|   VDP(24) |     N.V.T.      | GETAL 0 TOT 255 |
-------------------------------------------------


          V O O R B E E L D

Dan  nu maar even een klein voorbeeld programaatje. Met de '
geef ik  wat kommentaar.  Bedenk wel  dat dit  programmaatje
ALLEEN voor MSX2+ bedoeld is.

; Dit programma is alleen voor grafische schermen 5 en 8

10 COLOR 15,0,0:SCREEN X ' VUL HIER VOOR X IN 5 OF 8
20 SET PAGE 1,1:CLS:COPY (0,0)-(256,64) TO (0,212)
30 VDP(9)=VDP(9) OR 2     'LAAT DE SPRITES NIET ZIEN
40 OPEN "GRP:" FOR OUTPUT AS #1
50 PSET (200,200):PRINT #1,"MSX 2+"
55 ' _TURBO ON                    'GEBRUIK ALLEEN MET _BC
60 X1 = 1:Y1 = 1
70 X2 = X2+X1:IF X2 > 245 THEN X1 = -1 'X2>245 ? JA,DAN -1
80 Y2 = Y2+Y1:IF Y2 > 200 THEN Y1 = -1 'Y2>200 ? JA,DAN -1
90 IF X2<15 THEN X1 = 1                'X2<15  ? JA,DAN +1
100 IF Y2<50 THEN Y1 = 1               'Y2<50  ? JA,DAN +1
110 SET SCROLL X2,Y2                   'SCROLLEN
120 IF INKEY$="" THEN GOTO 70 ELSE END

Je  kan eventueel  nog _BC  gebruiken en dan in regel 55 het
rem (')  teken weghalen. Als je het programma RUNt zie je de
tekst MSX 2+ over het beeld schuiven, de tekst gaat ook door
het  beeld (dus links eruit en rechts erin). Maar er is geen
sprites blok te bekennen.

Veel  plezier met  het SET  SCROLL commando  en misschien de
volgende keer een complete tekstzoek routine met scroll.

                                   Bart Schouten
