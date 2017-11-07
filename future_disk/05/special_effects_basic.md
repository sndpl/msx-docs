                        SPECIAL EFFECTS BASIC                  
                         -------- # 1 --------                  
                                                                
 Dit keer nog een soort cursus, niet over het gewone huis,tuin  
 en keuken BASIC, maar over het BASIC met wat verdere           
 bedoelingen: spellen, demo's, gebruikersprogramma's, enz.      
 enz.                                                           
                                                                
 De naam special effects klinkt nog al proffesioneel. Het is    
 dus niet een cursus over de comando's PRINT, CLS, SCREEN ..    
 enz. Ik wil het dan ook in deze eerste aflevering hebben over  
 het zelf maken van letters (en natuurlijk gebruiken in eigen   
 programma's) en kleuren.                                       
                                                                
                                LETTERS                         
                                -------                         
                                                                
 Om de letters op het beeld te printen kun je gebruik maken     
 van :                                                          
                                                                
 SCREEN 5 : OPEN"GRP:"AS#1 : PRINT#1,"<tekst>"                  
                                                                
 Dit is een standaard comando dat bijna iedereen kent. Maar     
 als je nu zelf letters wilt gaan maken, in een tekenprogramma  
 of gewoon in basic, is dat een stuk moeilijker op het beeld    
 te printen. Je hebt hierbij een paar comando's nodig :         
                                                                
 A=LEN(A$)       : In A komt de lengte van A$ te staan.         
                   Bijvoorbeeld : A$="FUTURE" dan is A 7.       
                                                                
 A$=MID$(B$,A,B) : In A$ komt te staan : de lengte B, A         
                   posities ver, van B$ .                       
                   Bijvoorbeeld: B$="FUTUREDISK 5",A=7,B=4      
                   dan staat in A$ "DISK".                      
                                                                
 A=ASC(A$)       : In A komt de ASCII waarde van A$ te staan.   
                   Als de lengte van A$ (LEN(A$)) > ��n,        
                   dan komt in A de ASCII waarde van de eerste  
                   letter te staan.Dus als A$="FUTUREDISK" is   
                   dan staat in A 70 (de ASCII waarde van "F")  
                                                                
 SET PAGE A,A    : Wisselt van pages.In A staat het             
                   PAGENUMMER.(geldt alleen voor screen 5 en    
                   hoger.)                                      
                                                                
 COPY(A,B)-(C,D) : Met dit comando copi�er je van A,B tot C,D   
 ,G TO (E,F),H,AC  uit page G  naar E,F uit page H.             
                   i.p.v AC (Ander Commando) moet je :          
                                                                
        PSET,PRESET,XOR,OR,AND,TPSET,TPRESET,TXOR,TOR,TAND      
                                                                
                   zetten. Wat al deze commando's betekenen,    
                   ga ik nu niet uitleggen.(Kijk maar eens in   
                   het boek dat bij de computer zit onder het   
                   hoofdstuk LOGISCHE OPERATIES).               
                   De functie van de z.g.n LOGISCHE OPERATIES   
                   achter een COPY invoer is dat je iets over   
                   elkaar kunt copi�ren zonder dat hetgene      
                   waar je overheen copi�ert weg gaat.          
                                                                
 Nu maar eens een programma :                                   
                                                                
 10 color 15,0,0:screen5       :'maak altijd de achtergrond 0!  
 20 set page 1,1: close: open"grp:"as #1                        
 30 preset (0,0):print #1, "ABCDEFGHIJKLMNOPQRSTUVWXYZ"         
 40 set page 0,0                                                
 50 a$="FUTUREDISK ISSUE VIJF"                                  
 60 for a = 1 to len(a$)                                        
 70 b$= mid$ (a$,a,1)                                           
 80 b= asc (b$)                                                 
 90 b=b-65 : b=b*8                                              
 100 copy(b,0)-(b+7,8),1 to (c,d),0                             
 110 c=c+8 : next a                                             
 120 goto 120                                                   
                                                                
 Regel 90: De ASCII waarde in B is altijd groter dan 64 (65 =   
 ASCII waarde "A") De "A"-"Z" in page 1 staan op positie 0,0.   
 Als je dus de letter A uit page 1 wilt copi�ren, moet B met    
 65 worden afgetrokken.Maar als je "B" wilt copi�ren, krijg     
 je de positie 1,0 , terwil de "B" op 8,0 staat.Er moet dus     
 met 8 vermenigvuldigd worden.                                  
                                                                
 Regel 110 : Als je van C een constante zou maken, zouden       
 alle letters uit A$ op een plaats dus overelkaar gecopi�erd    
 worden.Dit is niet de bedoeling dus moet C iedere keer met 8   
 (de lengte van een letter) worden opgeteld.                    
                                                                
 De letters uit dit programma zijn standaard. Je kunt zelf ook  
 letters tekenen in een tekenprogramma. Je kunt ook andere      
 afmetingen gebruiken , dan is een kleine wijziging nodig.      
 Maar als echte BASIC programmeur wil je zelf letters in BASIC  
 maken.                                                         
 We gaan nu dikke letters met 7 verschillende kleuren maken.    
 Het principe berust hierop: zet 7 keer de letters A-Z onder    
 elkaar, steeds met een andere kleur.Als je nu de tweede        
 lijn van de tweede regel op de tweede lijn van de eerste       
 regel copi�ert en de derde lijn van de derde regel op de       
 derde lijn van de eerste regel copi�ert ENZ.ENZ., krijg je     
 in de eerste regel 7 verschillende kleuren.                    
 Als je ze nu deze eerste regel dikker wilt gaan maken,         
 gebruik je het bovenstaand beschreven commando                 
 COPY(A,B)-(C,D),G TO (E,F),H,TPSET (in dit geval TPSET,        
 want dit "achtervoegsel" zorgt dat je iets over elkaar heen    
 kunt zetten, zonder dat hetgene waar je overheen copi�ert      
 weg gaat.) Dit is ideaal, want we willen de eerste regel       
 over de eerste regel heenzetten, ��n positie verder.(Dit is    
 het principe van letters dikmaken.) We doen dus :              
                                                                
             COPY(0,0)-(254,8),1 TO (1,0),1,TPSET               
                                                                
 In een programaatje wordt dit alles :                          
                                                                
 10 color 1,0,0 : screen 5                                      
 20 set page 1,1 : open"grp:" as #1                             
 30 fora=0to7:preset (0,a*8):?#1,"ABCDEFGHIJKLMNOPQRSTUVWXYZ"   
 40 color a+2:next a                                            
 50 fora=0to7:copy(0,a*8+a)-(255,a*8+a)to(0,a)                  
 60 next a                                                      
 70 'letters in regel 1 hebben 7 kleuren                        
 80 copy(0,0)-(254,8) to (1,0),,tpset                           
 90 'letters in regel 1 dikker gemaakt                          
 100 'laad nu vorige programma en doe dan SCREEN5 : GOTO 40     
                                                                
 Als U nu denkt van "Moet ik dat nu allemaal gaan intypen" dan  
 heeft U het mis. Op deze diskette staat het programma          
 BASICKS1.LDR . Als u dit runt, ziet U alle voorbeelden die     
 hier besproken worden.                                         
                                                                
                          KLEUREN                               
                          -------                               
 Als U nu de kleuren van de letters niet mooi vindt, kunt U     
 die veranderen met :                                           
                                                                
 COLOR=(A,B,C,D)  : A = kleurnummer (0-15)                      
                    B = tinten rood  (0-7)                      
                    C = tinten groen (0-7)                      
                    D = tinten blauw (0-7)                      
                                                                
 Als je bijvoorbeeld bij de vorige programma de kleuren 1-7     
 hebt gebruikt en je wilt ze laten vari�ren van donker naar     
 licht rood (bijvoorbeeld) dan doe je dus :                     
                                                                
          FOR A = 1 TO 7 : COLOR = (A,A,0,0) : NEXT A           
                                                                
 (de kleur en de kleur tint zijn het zelfde dus kun je twee     
 keer A gebruiken want : color 1 heeft 1 tint rood , color 2    
 heeft 2 tinten rood enz.)                                      
                                                                
 Dit is wel leuk maar er zijn veel leukere dingen te doen.Ik    
 zal 3 programa's bespreken.                                    
                                                                
                          ZEEPBEL                               
                          -------                               
 In het programma worden er met het commando RND gewerkt.       
                                                                
 A=INT(RND(B)*C) : In A komt : Random van B (Random is          
                   willekeurig) Als B dus 1 bevat krijg je      
                   allemaal willekeurige getallen van 0 tot 1   
                   In C komt een getal te staan dat aangeeft    
                   hoeveel getallen er mogelijk zijn. INT       
                   betekend gewoon afronden op het              
                   dichtsbijliggende natuurlijke getal.         
                   Als B=1 en C=10 dan staat in A een           
                   willekeurig getal van 1 tot en met 10.       
                                                                
 In ZEEPBEL worden 15 cirkels getekend op met RND bepaalde      
 co�rdinaten met RND bepaalde tussenruimte.Iedere cirkel heeft  
 een kleur van 1-15 (0 = achtergrond). De kleinste cirkel       
 heeft kleur 1, de �nerkleinste kleur 2 enz.                    
 Als je nu alle kleuren 0,0,0 maakt (zwart en gelijk aan        
 achtergrond) dan zie je dus niets. Als je nu kleur 1 wit       
 (7,7,7) maakt en de rest zwart, dan kleur 2 wit en de rest     
 zwart , dan kleur 3 wit en de rest zwart enz. enz., dan        
 zie je dus dat er als het ware een cirkel naar voren komt.Als  
 je nu van kleur 15 naar kleur 1 springt dan lijkt het net als  
 of de cirkel kapot springt (het ZEEPBEL effect).               
 (ZEEPBEL en de twee volgende programma's staan ook in          
 BASICKC1.LDR)                                                  
                                                                
                     FUTUREDISK INTROLOGO                       
                     --------------------                       
                                                                
 Dit programmaate bootst de intro van (deze) FUTUREDISK na.     
 De intro met het rollende licht wel te heten. De tekening      
 staat in een .DAT file en deze laden we in in PAGE 1.          
 We maken nu alle kleuren (0 uitgezonderd) donkerblauw (0,0,1)  
 Dit kan door :                                                 
               FOR A=1 TO 15 : COLOR =(A,0,0,2) : NEXT A        
                                                                
 Dan wordt de tekening uit PAGE 1 gecopi�rt naar PAGE 0 en      
 wordt net als bij ZEEPBEL eerst kleur 1 wit gemaakt en kleur   
 2-15 donkerblauw enz.Doordat BASIC in dit programma nog al     
 snel functioneerd krijg je een soort Machine Taal effect.      
                                                                
                                                                
                          COUNT                                 
                          -----                                 
                                                                
 In het laatste programma COUNT wordt afgeteld van 9 tot 0.     
 We laden ook hier weer een .DAT file in. Alle kleuren worden   
 zwart gemaakt en het aftellen begint.Als U spatie drukt ,      
 komt het 2e deel van COUNT. Nog een keer wordt er afgeteld ,   
 nu in digitale cijfes.                                         
                                                                
 Zo zie je maar weer dat je met een simpel commando als COLOR   
 =(A,B,C,D) leuke programaatjes kunt maken.Ik hoop ook dat ik   
 de ANTI-BASICers een beetje heb kunnen overtuigen dat BASIC    
 niet alleen iets is voor domme zielepootjes. Grote spellen     
 zullen inderdaad niet snel genoeg werken in basic (zonder      
 R800), hier heb je toch Machinetaal voor nodig.                
                                                                
 In de volgende SPECIAL EFFECTS BASIC zal ik aandacht besteden  
 aan SPRITES en er zullen ook weer de nodige programma's bij    
 zitten. Ik hoop dat het leerzaam was. Volgende keer meer!      
                                                                
                                                   Tom Wauben   
