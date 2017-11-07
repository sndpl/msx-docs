                        SPECIAL EFFECTS BASIC # 3               
                        +++++++++++++++++++++++++               
                                                                
 Alweer de derde aflevering van SEB. Deze keer leert U een      
 uitgebreid tekenprogramma te maken. De besturing, pages enz    
 worden besproken . Maar eerst het tweede deel van SPRITES.     
                                                                
                                                                
                               SPRITES #2                       
                               ----------                       
                                                                
 Zoals de vorige keer bescheven zijn sprites (ltrl. GEESTEN)    
 handig voor gebruik in eigen programma's. Om een sprite in 't  
 geheugen te zetten, is er in BASIC het commando SPRITE$(A)=-   
 (zie vorige cursus). Er is ook een andere manier:              
 De data wordt in een DATA file meteen in het sprite geheugen   
 ingelezen. Om dit te begrijpen eerst nog een stukje theorie    
 over het V-ram.                                                
 In het V-ram heeft iedere screen zijn eigen geheugen. Zo is er 
 er geheugen voor karakters, kleuren etc. Ook voor sprites is   
 er in iedere screen een stukje gereserveerd. Een klein         
 overzichtje zal dit duidelijk maken :                          
                                                                
 KARAKTER POSITIE TABEL (geeft de karakterposities op beeld     
 weer)                                                          
 KARAKTERVORM TABEL (vorm van de karakters)                     
                                                                
 KLEUREN TABEL (bepaalt kleur die elke beeldpositie heeft)      
                                                                
 PALET TABEL (bevat alle R-G-B tinten van iedere kleur)         
                                                                
 SPRITE KLEUREN TABEL (bevat de kleuren van COLOR SPRITE)       
                                                                
 SPRITE ATTRIBUTE TABEL (bevat info. over sprite: grootte ,     
 positie enz.)                                                  
 SPRITE VORM TABEL (bevat vorm van sprites)                     
                                                                
 Sommige screens hebben ��n tabel niet (bv. screen 0 heeft      
 geen SPRITE vorm tabel). Iedere tabel gebruikt een stukje      
 V-ram. Het is mogelijk hierin te schijven. We kunnen dus een   
 sprite meteen in het geheugen poken.                           
 Het poken in het V-ram gaat met het bevel:                     
                                                                
 VPOKE A,B : in A staat het V-ram adres en in B de waarde.      
                                                                
 Om nu in het V-ram een sprite te zetten, moet je dus het       
 adres weten van de SPRITE-VORM TABEL in de gewenste screen.    
 Een sprite die je aan het begin van de tabel poked heeft       
 spritenummer 0, de tweede nr. 1 enz.                           
 Het volgende programmaatje zet een sprite die de DATA in de    
 DATA-regels heeft staan in het V-ram (screen 5)                
                                                                
 10 SCREEN 5,0 : 'een 8 sprite                                  
 20 FOR I=0 TO 7 : '8 DATA'S                                    
 30 READ A : 'LEES WAARDE IN A                                  
 40 VPOKE &H7800+I,A:'HET EERSTE ADRES VAN DE SPRITE-VORM TABEL 
 IS &H7800. DE SPRITE KOMT DUS HELEMAAL IN HET BEGIN TE STAAN   
 50 NEXT I                                                      
 60 'OPROEPEN SPRITE (=NORMAAL)                                 
 70 PUT SPRITE 0,(128,106),15,0                                 
 80 GOTO 80                                                     
 90 'DATA REGEL SPRITE (GESTREEPT VLAKJE)                       
 100 DATA 255,0,255,0,255,0,255,0                               
                                                                
 Deze manier is vooral handig bij veel sprites. Schrijf alle    
 data in het gewone RAM, save het weg en laad dit in het V-     
 ram op de juiste plaats. Dit komt nog in een volgende cursus   
 bod.                                                           
                                                                
 Dit zelfde vehaaltje geldt voor COLOR SPRITE.                  
                                                                
 Genoeg over sprites. We gaan nu een tekenprogrammaatje maken.  
                                                                
                            SEB-GRAPH                           
                            ---------                           
                                                                
 Om een eigen programma te maken zullen we eerst de besturing   
 d.m.v. de randapparatuur (toetsenbord,joystick enz.)           
 bespreken. De besturing van toetsenbord en joystick is het-    
 zelfde. Er zijn nl. 8 richtingen en een rust plaats. In een    
 schema ziet dat er zo uit :                                    
                               1                                
                           8       2                            
                         7     0     3                          
                           6       4                            
                               5                                
                                                                
 Richting 5 is bv. dat er naar onder is gedrukt op het toetsen  
 bord of joystick.                                              
 Zoals in het bovenstaande schema geeft de computer de          
 richtingen weer m.b.v. het commando :                          
                                                                
 A = STICK(B) : A komt de waarde te staan (0-8 , zie            
                bovenstaande schema) en in B staat de poort     
                waarvan moet worden gelezen in B kan staan:     
                0 : Toetsenbord                                 
                1 : Joystick poort 1                            
                2 : Joystick poort 2                            
                Let op! A is altijd een variable , B is         
                een constante.                                  
                                                                
 M.b.v. deze kennis kun je bv. een menuutje maken in screen 0.  
 Kijk maar in de onderstaande listing ;                         
                                                                
 10 SCREEN 0 : WIDTH 80 : KEY OFF :'INSTELLINGEN                
 20 CLS : FOR I = 1 TO 5 : READ A$ : LOCATE 30,I:PRINT A$       
 'Zie onderstaande informatie                                   
 30 NEXT I                                                      
 40 IF STRIG(0) OR STRIG (1) OR STRIG (3) THEN 110              
 50 A=STICK(0):IF A=0 THEN A=STICK(1) : IF A=0 THEN             
 A = STICK (2) : IFA=0 THEN GOTO 40                             
 60 IF A=1 THEN Y=Y-1 : IF Y<1 THEN Y=5                         
 70 IF A=5 THEN Y=Y+1 : IF Y>5 THEN Y=1                         
 80 LOCATE 28,YO : PRINT " " : LOCATE 39,YO : PRINT " "         
 90 LOCATE 28,Y : PRINT ">" : LOCATE 39,Y :PRINT "<"            
 100 YO=Y : GOTO40                                              
 110 IF Y = 5 THEN END ELSE RESTORE 130 : FOR I=1 TO Y :        
 READ A$ : NEXT I                                               
 120 RUN A$+"LDR"                                               
 130 DATA MENU                                                  
 140 DATA MAGAZINE                                              
 150 DATA DEMO                                                  
 160 DATA AUTOEXEC                                              
 170 DATA STOP                                                  
                                                                
 Een eerste langere listing. Nog even wat uitleg :              
                                                                
 Regel 20 : READ A$ : in a$ komt de data te staan die achter    
            commando DATA staat. Wordt er nog een READ A$       
            gedaan dan springt de computer automatisch naar de  
            sting achter de komma of de string achter een       
            nieuwe DATA-regel (in dit geval).                   
            RESTORE in regel 110 geeft aan waar de DATA moet    
            worden gelezen.(in dit geval regel 130)             
                                                                
 Regel 40 : hier staat een nieuw commando :                     
                                                                
 IF STRIG (A) THEN <B> : Dit commando gaat naar regel <B> als   
                         er een knop/spatie van toetesenbord ,  
                         joystick of muis wordt ingedrukt.      
 In A staat :                                                   
                                                                
 0 spatiebalk                                                   
 1 joystick / muis  poort 1 , knop 1                            
 2 joystick / muis  poort 1 , knop 2                            
 3 joystick / muis  poort 2 , knop 1                            
 4 joystick / muis  poort 2 , knop 2                            
                                                                
 In dit geval wordt er naar regel 110 gesprongen als er spatie  
 of knop 1 van joyst./muis in poort 1 of 2 wordt gedrukt.       
                                                                
 (Dit programmaatje staat op deze disk onder de naam            
  "SEBMENU.LDR")                                                
                                                                
            het lezen van de besturing van de muis              
            --------------------------------------              
 Om de muis te lezen is er ook een appart commando :            
                                                                
 PAD (A) : A kan 18 waardes hebben. Hier worden alleen de       
 waardes voor de muis in poort 1/2 besproken.                   
                                                                
 MUIS in poort 1 : eerst de altijd PAD (12) doen. Dit is om     
                   een de MUIS-DATA te "openen". PAD (13)       
                   geeft de X vershuiving en PAD (14) de        
                   Y vescuiving.                                
 MUIS in poort 2 : hetzelfde verhaal alleen met resp. PAD (16)  
                   PAD (17) en PAD (18)                         
                                                                
 In het programma CUJOMU.LDR kunt u alle besproken besturings-  
 vormen in een programma zien. U kunt dit programma ook voor    
 eigen gebruik gebruiken. Dit programmaatje vormt de basis      
 voor het tekenprogrammaatje.                                   
                                                                
 Om te kiezen tussen de verschillende opties (lijn tekenen      
 enz.) maken we bebruik van IKONEN. Deze tekeningetjes worden   
 ingeladen in PAGE 1. (zie SEB #1). In page 2 wordt de oude     
 tekening bewaard.                                              
                                                                
 Het enige commando wat we nu nog niet kennen om het            
 tekenprogramma af te maken is :                                
                                                                
 ON A GOSUB <B> , <C> ,<ENZ> : dit commando spring naar de sub  
                               routine die begint op regel B    
 als A 1 is , sprint naar regel <C> als A 2 is enz.             
                                                                
 Een voorbeeldje :                                              
                                                                
 10 D=STICK (0) : IF D=0 THEN10                                 
 20 ON D GOSUB 40,50,60,70,80,90,100,110                        
 30 GOTO 10                                                     
 40 PRINT "BOVEN" : RETURN                                      
 50 PRINT "RECHTS-BOVEN" : RETURN                               
 60 PRINT "RECHTS" : RETURN                                     
 70 PRINT "RECHTS-ONDER" : RETURN                               
 80 PRINT "ONDER" : RETURN                                      
 90 PRINT "LINKS-ONDER" : RETURN                                
 100 PRINT "LINKS" : RETURN                                     
 110 PRINT "LINKS-BOVEN" : RETURN                               
                                                                
 Deze listing gaat dus naar regel 40 als D=1(boven).            
                                                                
 Nu kunnen we dus SEB-GRAPH maken. Op deze disk staat het       
 tekenprogrammaatje onder de naam SEB-GRPH. Bekijk gerust de    
 list eens. Daar leert U veel van.                              
                                                                
 Tot slot nog een denkspelletje waarin veel van het             
 bovenstaande wordt gebruikt. Start het maar eens op (DENKSPEL  
 .LDR) en speel. Als het goed is kunt U zoiets nu ook maken.    
                                                                
                                                                
 Veel theorie deze keer, maar U ziet : je kunt er leuke         
 dingen mee maken. Volgende keer proberen we eens een echt      
 spelletje te maken. Is bij U al de programmeer drift           
 toegeslagen , stuur dan eens een eigengemaakt programmaatje    
 op naar het bekende FUTURE-DISK adres (zie helplines).         
 Heb je nog vragen , bel dan 046-758100 (na 16.00 U. Tom).      
                                                                
                                                  Tom Wauben   
