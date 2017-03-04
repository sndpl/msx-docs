                   SPECIAL EFFECTS BASIC # 2                    
                  ---------------------------                   
                                                                
 Welkom bij alweer de tweede aflevering van SPECIAL EFFECTS.    
 Ik wil het in deze aflevering gaan hebben over sprites en het  
 POINT commando. Op het einde zal ik ook nog twee basic scroll  
 routines bespreken.                                            
                                                                
                           SPRITES                              
                           -------                              
                                                                
 Je kunt sprites in delen in twee soorten : 8 x 8  matrix       
                                           16 x 16 matrix       
                                                                
                          8 x 8                                 
                                                                
 De 8 x 8 sprites worden in 8 horizontale stukken verdeeld.     
 Ieder stuk is eigenlijk een binair getal. Een 1 stelt een      
 puntje voor , een 0 een lege plaats.Dus :                      
                                                                
                00011000                                        
                00111100                                        
                01111110                                        
                11111111                                        
                00011000                                        
                00011000                                        
                00011000                                        
                00011000                                        
                                                                
 Je hebt nu dus een pijl.Om de Sprite in het videoram te        
 zetten , gebruik je :                                          
                                                                
 SPRITE$(A)=CHR$(&B00011000)+CHR$(&B00111100)+CHR$(&B01111110)  
 +CHR$(&B11111111)+CHR$(&B00011000)+CHR$(&B00011000)+           
 CHR$(&B00011000)+CHR$(&B00011000)                              
                                                                
 De bovenstaande pijl staat nu in het videoram.                 
 Alleen is deze scrijfwijze nogal lang. Het handigste bij       
 sprites is on het HEXADECIMALE talstelsel te gebruiken.        
 Met het (bekende) commando PRINT HEX$(&B00000000) kun je alle  
 codes omzetten naar hexadecimaal.                              
 De A bij SPRITE$(A)=-- is het SPRITENUMMER.                    
 Dit is belangrijk bij het op het scherm zetten van een         
 sprite!!                                                       
                                                                
 Het is wel een tijdrovende en omslachtige manier om zo         
 sprites te maken. Veel handiger is het als je een sprite       
 editor gebruikt. Zo'n sprite editor staat op deze disk onder   
 de naam SPRITEDT.LDR                                           
                                                                
                             16 x 16                            
                                                                
 De 16 x 16 sprites zijn iets moeilijker te maken dan de 8 x 8  
 sprites. Je neemt hier namelijk vier 8 x 8 sprites.            
 Dus een 16 x 16 sprite is :                                    
                             A$ C$                              
                             B$ D$                              
                                                                
 A$ is de eerste 8 x 8 sprite en het kwart deel links boven     
 in. Als je nu net zo als bij de 8 x 8 sprites de codes in het  
 videoram wilt zetten (en je hebt de data in A$,B$,C$,D$) dan   
 doe je :                                                       
                                                                
               SPRITE$(A)=A$+B$+C$+D$                           
                                                                
 Als je bijvoorbeeld 4 pijlen (zie 8 x 8) in 1 spritenummer     
 wilt hebben doe je A$=<data van pijl , zie 8 x 8>              
                                                                
                    B$=A$ : C$=A$ : D$=A$                       
                    SPRITE$(0)=a$+B$+C$+D$                      
                                                                
 In spritenummer 0 staan dus nu de 4 pijlen.                    
                                                                
 LET OP !! JE KUNT DE SPRITES ALLEEN MAAR IN HET VIDEORAM       
 ZETTEN ALS JE EERST SCREEN 1 OF HOGER HEBT INGESTELD.          
                                                                
                         SPRITE OP SCHERM                       
                                                                
 Als je de bovenstaande pijlen in spritenummer 0 op het scherm  
 wilt zetten ,doe je :                                          
                                                                
 10 screen5,2                                                   
 20 A$=<data pijl>                                              
 30 b$=a$ : c$=a$ : d$ = a$                                     
 40 sprite$(0)=a$+b$+c$+d$                                      
 50 put sprite 0,(100,100),15,0                                 
 60 goto 60                                                     
                                                                
 Regel 10 : de ,2 betekend dat je 16 sprites op het scherm      
 kunt zetten (16 sprites = 16 x 16 sprites). Er zijn ook nog    
 andere mogelijkheden :                                         
 ,0 : 8  sprites   Normale grootte                              
 ,1 : 8  sprites   Dubbele grootte                              
 ,2 : 16 sprites   Normale grootte                              
 ,3 : 16 sprites   Dubbele grootte                              
                                                                
 Regel 50 :   PUT SPRITE A,(B,C),D,E                            
                                                                
 A is het raster,waar de sprite op staat. Meestal is het        
 raster gelijk aan het spritenummer. Soms, als je bijvoorbeeld  
 een lopend poppetje in een sprite wilt zetten, gebruik je 1    
 raster en meerdere spritenummers.                              
 B is de X co�rdinaat, C is de Y co�rdinaat, D is de kleur en   
 E is het spritenummer.                                         
                                                                
                   TWEE SPRITES OVER ELKAAR                     
                                                                
 Ik kan me voorstellen dat je een sprite met 1 kleur erg saai   
 vindt. De mogelijkheid bestaat echter dat je twee sprites      
 over elkaar kunt zetten , zonder dat dit gaat knipperen of     
 zo. Ook kun je gebruik maken van het commando                  
                                                                
                 COLOR SPRITE$(A)=<DATA KLEUR>                  
                                                                
 Als je nu bijvoorbeeld de 16 sprite pijl hebt en je wilt dat   
 de bovenste vier regels respectivelijk kleur 1-4 hebben en de  
 onderste vier regels kleur 5-8 en er tussenin kleur 9.         
 De color sprite$ is een RASTER !!!                             
                                                                
 10 screen 5,2                                                  
 20 sprite$(0)=A$+B$+C$+D$                                      
 30 E$=chr$(1)+chr$(2)+chr$(3)+chr$(4)+chr$(0)+chr$(0)+         
 +chr$(0)+chr$(0)                                               
 40 F$=chr$(0)+chr$(0)+chr$(0)+chr$(0)+chr$(5)+chr$(6)+         
 +chr$(7)+chr$(8)                                               
 50 color sprite$(2)=E$+F$                                      
 60 putsprite 2,(100,100),9,0                                   
 70 goto 70                                                     
                                                                
 Dat achter color sprite$ maar twee strings komen heeft te      
 maken met dat je met 16 horizontale lijnen werkt. E$ geeft     
 dus de kleuren van de 8 bovenste regels en F$ van de 8         
 onderste.                                                      
 Je kunt met deze kennis bv. een lopend poppetje maken. In het  
 programma "WALK.BAS" zie je hoe.                               
                                                                
                            POINT                               
                            -----                               
                                                                
 Nu iets heel anders. De volgende keer wordt er een besproken   
 hoe je een spel kunt maken. Een (zeer) belangrijk commando     
 bij en spel is :                                               
                                                                
                        A=POINT(B,C)                            
                                                                
 In A komt te staan : de kleur van de pixel met co�rdinate B    
 en C.                                                          
 Als je dus de kleur van pixel 100,100 wilt weten in screen 5   
 doe je :                                                       
                                                                
 10 screen5                                                     
 20 a=point(100,100)                                            
 30 screen0                                                     
 40 print a                                                     
                                                                
 Dit commando is ook te gebruiken in een IF..THEN lus. Als je   
 bijvoorbeeld een sprite hebt die niet verder mag dan een lijn  
 van kleur 2, dan doe je :                                      
                                                                
                     IF POINT(A,B)=2 then <regel nummer>        
                                                                
 A en B zijn de co�rdinaten van de sprite.                      
                                                                
 Dit is heel handig bij het maken van spelletjes. Als je een    
 poppetje hebt dat niet door een wit muurtje mag (kleur 15)     
 dan kun je dat met POINT maken :                               
                                                                
 10 color 15,0,0 : screen 5,2                                   
 20 sprite$(0)=<data poppetje>                                  
 30 a=250 : line (0,0)-(10,211),15,bf                           
 40 put sprite 0,(a,100),10,0                                   
 50 a=a-1 : if point (a,100)=15 then a=250                      
 60 goto 40                                                     
                                                                
 In dit geval begint de sprite weer op 250,100 als de witte     
 balk is aangeraakt. Er zijn heel veel mogeijkheden met dit     
 commando en bij bijna ieder programma wordt er gebruik van     
 gemaakt.                                                       
                                                                
                           SCROLL                               
                           ------                               
                                                                
 En dan nu iets heel anders.Twee scroll-routines in BASIC. De   
 �ne met hetzelfde principe als de Machinetaal scroll , de      
 andere met een wat meer geschikt principe voor basic.          
                                                                
                           SCROLL 1                             
                                                                
 Bij deze scroll wordt een deel van een karakter uit een strig  
 gecopi�rd naar page 2. In page 2 wordt de scroll opgebouwd.    
 Dan wordt uit page 2 de tekst gcopi�erd en dan wordt weer een  
 stukje karakter naar page 2 gecopi�erd. Enz,enz. (Zie voor     
 het copi�ren van karakter de vorige SPECIAL EFFECTS)           
                                                                
 Er zitten een paar nadelen aan deze scroll :                   
                                                                
 1 als je grote letters pakt en je copi�ert dan steeds maar 1   
   lijntje dan gaat de scroll veel te langzaam.                 
                                                                
 2 als je meerdere lijnen copi�ert,krijg je een blokkerig       
   effect,als er een vertraging in staat en als je er geen      
   vertraging in zet, krijg je een soort knipper effect. Deze   
   scroll staat op deze disk onder de naam "SCROLL.BAS"         
                                                                
                                                                
                           SCROLL 2                             
                                                                
 Bij deze scroll wordt de tekst alvast in de pages gezet. Als   
 je dan als het ware de pages "afgescant" en ieder stukje naar  
 page 0 copieert krijg je een goede scrol. Een voorbeeld van    
 zo'n soort scroll is een demootje,genaamd "DOOLHOF.BAS". De    
 computer gaat met een vierkantje (sprite) door het             
 doolhof. Het gehele doolhof staat in page 1. De computer       
 copieert steeds een stukje uit page 1 naar page 0.             
                                                                
 De volgende keer meer truuks met sprites en een eerste         
 spelletje. Ik hoop dat de BASIC-kennis weer iets uitgebreid    
 is.                                                            
                                                                
                                                  Tom Wauben    

