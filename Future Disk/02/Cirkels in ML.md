Cirkels in ML                                                   
-------------                                                   
                                                                
Cirkel = Zuiver Ronde Kring (Prisma Woordenboek p.61)           
                                                                
                                                                
Vele  wiskundige  genie�n  hebben  hun  hoofd   gebroken   (niet
letterlijk natuurlijk) over het  fenomeen  cirkel.  Tegenwoordig
doen we niet meer zo lastig over cirkels.  Je  neemt  gewoon  je
rekenketel en berekent een  aantal  sinussen  en  cosinussen  en
voila, een cirkel.                                              
                                                                
Op een computer gaat het net zo. Je neemt  basic,  initialiseerd
een scherm en typt "CIRCLE (100,100),100,15". En  wonderwel,  er
wordt een cirkel op het scherm afgebeeld.                       
                                                                
Vraag: HOE DOE IK DIT IN ML.                                    
                                                                
Antwoord:                                                       
                                                                
Om antwoord te geven op de vraag zou ik natuurlijk kunnen zeggen
dat je de computer moet gebruiken om een aantal sinus en cosinus
waardes te berekenen en  deze  vervolgens  af  te  beelden.  Hoe
bereken ik echter  sinussen  en  cosinussen  in  ML.  Natuurlijk
springen de wiskundigen onder ons een meter boven hun stoel uit,
want    nu    kunnen    ze    gaan    zitten    kwijlen     over
machtreeksontwikkelingen  (ja,  zo   smaakt   het   ook).   Alle
wiskundigen hoeven nu dus niet meer verder te  lezen,  want  zij
kunnen beginnen en h��l v��l geluk.                             
                                                                
Zo, nu zijn alleen de simpele zieltjes  overgebleven  (waaronder
ik) en kunnen wij het een stuk gemakkelijker aanpakken.         
                                                                
Waar was ik gebleven, oh ja, cirkels. Om de simpelere  oplossing
uit te leggen ga ik beginnen met te zeggen dat ik eigenlijk geen
hele cirkels ga berekenen. Niet gelijk schrikken,  je  zult  het
verschil niet zien hoor. Met simpeler bedoel  ik,  dat  ik  maar
1/8ste deel van  een  cirkel  ga  berekenen  en  door  een  paar
spiegelingen de  rest  laat  tekenen.  Verder  ga  ik  ook  niet
oneindig veel punten berekenen (een cirkel  bestaat  theoretisch
uit oneindig veel punten), want dat is te veel werk. Nee, ik  ga
gebruik maken van de  maximale  resolutie  van  de  computer  en
daardoor gaat het een stuk sneller.                             
                                                                
Om te beginnen laat ik een "flowchart" zien  van  de  (overigens
niet zelf verzonnen) oplossing:                                 
                                                                
         +-------------------+                                  
         | x=0               |                                  
         | y=R (straal)      |                                  
         | A=R (hulp teller) |                                  
         +---------+---------+                                  
                   |                                            
+------------------+                                            
|                  |                                            
|                  +                                            
|                 / \                                           
|                /   \             Nee                          
|               < A<R >----------------+                        
|                \   /                 |                        
|                 \ /                  |                        
|                  +                   |                        
|                  | Ja                |                        
|        +---------+---------+         |                        
|        | y=y-1             |         |                        
|        | A=A+y+y           |         |                        
|        +---------+---------+         |                        
|                  |                   |                        
|                  +-------------------+                        
|                  |                                            
|        +---------+---------+                                  
|        | Plaats 8 pixels   |                                  
|        +---------+---------+                                  
|                  |                                            
|        +---------+---------+                                  
|        | A=A-x-x-1         |                                  
|        | x=x+1             |                                  
|        +---------+---------+                                  
|                  |                                            
|                  +                                            
|                 / \                                           
| Nee            /   \                                          
+---------------< x>y >                                         
                 \   /                                          
                  \ /                                           
                   +                                            
                   | Ja                                         
         +---------+---------+                                  
         | Klaar is Klara    |                                  
         +-------------------+                                  
                                                                
Als we dit kunnen omzetten in ML dan zijn we  klaar.  Natuurlijk
wil iedereen weten  hoe  de  oplosing  werkt.  Ik  zal  niet  de
wiskundige afleiding geven, maar het  op  zijn  "boerenfluitjes"
vertellen.                                                      
                                                                
We beginnen  bovenaan  de  cirkel.  Vervolgens  trekken  we  een
horizontale lijn, en wel zolang tot er een punt buiten de cirkel
valt (dit wordt berekend met Pythagoras). Vervolgens gaan we  de
lijn een pixel lager tekenen en begint alles van  voor  af  aan.
Als dat niet simpel is en het werkt ook nog eens. Oh ja, voor de
grapjassen die na drie dagen nog aan het tekenen  zijn:  Stoppen
als x groter dan y is. Verder  moet  er  rekening  mee  gehouden
worden dat het middelpunt van de cirkel ook de oorsprong van het
assenstelsel is (dat krijg je met wiskundige beschouwingen).    
                                                                
En dan nu..... Geen cirkels  natuurlijk,  dat  wil  zeggen  niet
hier, maar wel op de FUTUREDISK zelf. Er staan  3  files  op  de
FUTUREDISK die met cirkels te maken hebben.                     
                                                                
(1) Deze tekstfile (als je de FUTUREDISK tekstroutine  zit,  dan
kun je nu op de P drukken om de tekstfile te laten Printen).    
                                                                
(2) De GEN file CIRCLE.  Deze  GEN  file  kun  je  in  je  eigen
programma's opnemen  als  CALL  routine.  De  gebruiksaanwijzing
staat in de GEN file.                                           
                                                                
(3) Een kleine demo met cirkels, die  vanuit  het  software-menu
opgestart kan worden.                                           
                                                                
                                                                
Nog een paar opmerkingen:                                       
                                                                
(a) Nu iedereen  weet  hoe  zij/hij  (de  dames  niet  vergeten)
cirkels moet maken in ML verwachten wij (ik  in  het  bijzonder)
van de FUTUREDISK  binnen  enkele  weken  stapels  diskettes  te
ontvangen. Op deze diskettes staan dan  uiteraard  een  heleboel
demo's waar cirkels worden getekend. Wat we  ook  ontvangen,  we
zullen het zeker in een van de volgende nummers plaatsen.       
                                                                
(b) De routine zoals ze in  de  GEN  file  staat  is  een  trage
routine. Dit  komt,  omdat  in  de  routine  op  het  zogenaamde
klipping (buiten beeld vallen van pixels)  gecontroleerd  wordt.
Als je deze controle weg laat, dan wordt de routine  aanzienlijk
sneller.                                                        
                                                                
(c) De credits voor het bedenken van deze oplossing gaan naar:  
                                                                
Roland Waclawek                                                 
  Kreisgenerator                                                
    c't 9/86 p.122                                              
                                                                
George Sutty & Steve Blair                                      
  Programmer's guide to the EGA/VGA                             
    Brady Books, Simon & Schuster, New York, 1988               
                                                                
J�rgen Petsch                                                   
  Kopierbahnhof VGA-RAM                                         
    c't 4/90 p.292                                              
                                                                
Foley, van Dam, Feiner & Hughes                                 
  Computer graphics: Principles and Practice                    
    Addison-Wesley, 1990                                        
                                                                
J�rgen Petsch                                                   
  Turbo-Circle: Schnelle Kreise in Turbo-Pascal!                
    c't 10/91 p.172                                             
                                                                
                                                                
                                                                
Voor vragen en/of opmerkingen bel:                              
                                                                
         043-437778                                             
                                                                
                                                                
                                                                
                             Groetjes,                          
                                                                
                                       Jeroen "KUBIE" Smael     
                                                                
                                                                
                                                                
                                           C YA @ !!!!!         
