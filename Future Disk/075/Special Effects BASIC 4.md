Speciale dank gaat uit naar J.B.Vonk.                           

                                                                

                   SPECIAL EFFECTS BASIC #4                     
                   ------------------------                     

 Welkom bij het vierde deel van Special Effects. Deze keer      
 (nog) een deel van sprites, de logische operaties , de uitleg  
 van PAC-IT , een pacman-kloon en tot slot nog het DEF FN ,     
 ON INTERVAL en INSTR commando , een aantal losse flodders die  
 handig zijn bij het programmeren in BASIC ....                 



                        SPRITES (4)                             
                        +-+-+-+-+-+                             

 Deze keer een korte SPRITES-cursus. De volgende (en tevens     
 laatste) SPRITES zal een aantal truuks laten zien met          
 sprites.                                                       
 Deze keer wordt alleen de 3 kleurige sprite uit de doeken      
 gedaan en nog een paar aardigheidjes met het color sprite$     
 commando.                                                      

 Jazeker. Ook driekleurige sprites zijn mogelijk terwijl er     
 maar 2 sprites over elkaar worden gezet. Waar de 2 sprites     
 elkaar overlappen, worden deze ge-ORd (zie LOGISCHE OPERATIES  
 , hieronder). Zo kan deze 3e kleur worden berekend. U kunt     
 dan zelf de gewenste kleur geven met COLOR=. (zie SEB #1).     

 Maar hoe wordt dit dan gedaan ??                               

 De zesde bit van de colorsprite$-byte wordt geset.             
 Deze byte ziet er als volgt uit                                

 | 7 | 6 | 5 | 4 | 3 | 2 | 1 | 0 |                              
 ---------------------------------                              
 |VPb|ORb|IGb|nvt|CLR|CLR|CLR|CLR|                              


 VPb (bit 7) : als deze bit geset is , wordt de sprite 32       
 pixels naar links verplaatst.                                  

 ORb (bit 6) : als deze bit geset is en twee sprites            
 overlappen, zal er een OR-operatie tussen deze twee kleuren    
 plaatsvinden en deze kleur wordt op het scherm afgebeeld.      

 IGb (bit 5) : de sprite-overlapping wordt genegeerd.           

 Bit 4 heeft geen functie.                                      

 CLR (bit 0 t/m bit 3) : deze bits geven de spritekleur aan.    


 Als bijvoorbeeld de spritelijn-kleur van sprite I 12 moet      
 zijn en van sprite II 10 en ze moeten ge-ORd worden, dan       
 moet bit 6 geset zijn :                                        

 SPRITE I :  &b01001100 'bit 6 geset en kleur 12 (&b1100)       

 SPRITE II : &b01001010 'bit 6 geset en kleur 10 (&h1010)       

 In een list :                                                  

 10 COLOR 15,0,0 : SCREEN 5,0                                   
 20 A$=CHR$(&HFF)+CHR$(&HFF)+CHR$(&HFF)+CHR$(&HFF)              
 30 SPRITE$(0)=A$+A$                                            
 40 SPRITE$(1)=A$+A$                                            
 50 COLOR SPRITE$(0)=CHR$(&B01001100)                           
 60 COLOR SPRITE$(1)=CHR$(&B01001010)                           
 70 PUT SPRITE 0,(100,100),,0                                   
 80 PUT SPRITE 1,(104,100),,1                                   
 90 GOTO 90                                                     

 De sprites worden ten opzichte van elkaar 4 pixels             
 verschoven. Ze overlappen elkaar dus van (104,100)-(108,100)   
 Daar wordt de kleur dus :                                      

 1100                                                           
 1010 or                                                        
 -------                                                        
 1110 'color 14 !!!                                             


 LET OP !! Voor iedere sprite-lijn is een colorsprite$-bit,     
 dus kunt u in totaal maar liefst 32 kleuren in 2 sprites       
 gebruiken.                                                     

 Het aardige programmaatje SPRITES.BAS laat alle besproken      
 mogelijkheden nog eens zien.                                   



                       LOGISCHE OPERATIES                       
                       +-+-+-+-+-+-+-+-+-                       

 Een logische operatie is een bewerking van een                 
 variable ,copy,enz.                                            

                           VARIABLE                             
                           --------                             

 De "hoofd" logische operaties zijn de AND,OR,XOR.              

 AND   X Y  XY        OR   X Y  XY         XOR   X Y  XY        

       0 0  0              0 0  0                0 0  0         
       1 0  0              1 0  1                1 0  1         
       0 1  0              0 1  1                0 1  1         
       1 1  1              1 1  1                1 1  0         

 U ziet het is dus eigenlijk een bewerking van BITs. Maar als   
 u nu bv. een in A staande waarde wilt ANDen dan typt U         

 PRINT A AND 0                                                  
  0                                                             

 Er komt nu altijd nul uit , want als U met 0 AND dan komt er   
 altijd 0 uit (alleen als X en Y 1 zijn dan komt er 1 uit.)     

 Nu zijn hier hele leuke dingen mee te doen , in BASIC zouden   
 er nog wel andere commando's nodig kunnen zijn bv. als X=0 en  
 Y=0 dan XY=1 . Daarom volgt hieronder nog een lijst(je) met    
 aanvullende (afgeleid van AND,OR en XOR) logische operaties.   

 NOT    X Y  XY      EQV    X Y  XY      IMP   X Y XY           

        0 -  1              0 0  1             0 0 1            
        1 -  0              1 0  0             1 0 0            
        - -                 0 1  0             0 1 1            
        - -                 1 1  1             1 1 1            

 De NOT functie is handig bij het kijken of een variable 0 is.  
 Van deze logische operatie kan het resultaat bekeken worden    
 met :                                                          

 PRINT NOT A                                                    

 De EQV is de omgekeerde XOR functie (EQV = Exclusive Or        
 Negative , XOR = eXlisive OR) , zoals de naam het al zegt.     

 En dan het IMP commando. Zelf heb ik het nog nooit gebruikt    
 en ik denk dat deze logische operatie dan ook volstrekt        
 zinloos is.                                                    

                            COPY,LINE                           
                            ---------                           

 Er zijn ook logische operaties m.b.t. het copie�ren en het     
 tekenen van een lijn. In SEB #1 is de operatie TPSET aan bod   
 gekomen. Hieronder volgt een lijst van alle logische           
 operaties bij een COPY :                                       

 AND : Voert een AND operatie op de kleuren uit (zie VARIABLE)  

 OR : Voert een OR operatie op de kleuren uit (zie VARIABLE)    

 XOR : Voert een XOR operatie op de kleuren uit (zie VARIABLE)  

 PRESET : Voert een INVERSE (NOT) uit op de kleuren. Iedere     
          bit die geset is (1) wordt 0 en iedere bit die 0 is   
          wordt 1.                                              

 PSET : De normale copy.                                        

 Ook bestaan de bovenstaande commando's met een T ervoor        
 (TAND,TOR,TXOR,TPRESET,TPSET). Dit betekend dat color 0 als    
 doorzichtig wordt beschouwd (en dat de achtergrond dus te      
 zien is).                                                      

 Een voorbeeldje :                                              

 10 COLOR 0,0 : SCREEN 5 : CIRCLE (25,25),25,10                 
 20 LINE (100,100)-(200,200),2,BF : GOTO 140                    
 30 COPY(0,0)-(50,50) TO (125,125),0,AND : RETURN               
 40 COPY(0,0)-(50,50) TO (125,125),0,TAND : RETURN              
 50 COPY(0,0)-(50,50) TO (125,125),0,XOR : RETURN               
 60 COPY(0,0)-(50,50) TO (125,125),0,TXOR : RETURN              
 70 COPY(0,0)-(50,50) TO (125,125),0,OR : RETURN                
 80 COPY(0,0)-(50,50) TO (125,125),0,TOR : RETURN               
 90 COPY(0,0)-(50,50) TO (125,125),0,PRESET : RETURN            
 100 COPY(0,0)-(50,50) TO (125,125),0,TPRESET : RETURN          
 110 COPY(0,0)-(50,50) TO (125,125),0,PSET : RETURN             
 120 COPY(0,0)-(50,50) TO (125,125),0,TPSET : RETURN            
 130 'HOOFDLUS , SPRING IEDERE KEER NAAR ANDERE COPY.           
 140 D=D+1 : A$=INPUT$(1)                                       
 150 ON D GOSUB 30,40,50,60,70,80,100,110,120                   
 160 GOTO 160                                                   

 Met dit programma kunt u alle verschillende logische           
 operaties zien. Bekijk bij iedere operatie de kleuren maar     
 eens. Dit is een zeer beknopt programma. In de file            
 "COPYLOG.BAS" kunt U een uitgebreider programma zien.          

 U kunt deze logische operaties ook gebruiken bij het LINE      
 commando. Zie daarvoor "LINELOG.BAS". Kijk ook gerust eens in  
 de LIST. U kunt hier veel van leren.                           

 Genoeg over de logische operaties. U zult ook in andere        
 programma's deze logische operaties zien en zelfs in ML wordt  
 een deel ervan gebruikt. Dit komt doordat de logische          
 operaties erg snel zijn. Experimenteer gerust , Uw computer    
 heeft eindeloos gedult.                                        


 Deze tekst is een beetje lang , zodat hij in 2 delen           
 gesplitst moest worden.                                        

 Lees verder bij het 2e deel .....                              
                                                                
            Special Effects Basic #4 (vervolg)                  
            ----------------------------------                  



                           PAC-IT                               
                           ++++++                               

 Al een paar keer werd het aangekondigd , maar nu beginnen we   
 dan toch echt met een spel , PAC-IT. Het is een kloon van het  
 overbekende spel PACMAN. Toch is zo'n simpel uitziend spel     
 nog ingewikkeld. In dit deel gaan we niet verder dan de        
 opbouw van het speelscherm en een deel van de hoofdlus         
 (lopen, happen en stoppen voor een muur.)                      
 In de volgende SEBs komt de rest van het spel aan bod.         
 Maar voordat u begint te lezen zou het handig zijn de          
 listing bij de hand te hebben. Druk dan nu op reset , laad     
 de file PACIT.BAS,zet de printer aan , druk op de J , laad     
 daarna de file MAGAZINE.LDR en lees verder ...                 

                           VRAAG 1                              
                           +++++++                              

 De eerste vraag is het probleem van "welke screen ?". De       
 screenmode wordt bij dit spel screen 5.                        
 In deze screen kan er namelijk van een groot aantal            
 commando's gebruik worden gemaakt zoals SET PAGE,POINT,        
 COPY enz.                                                      

 Om bij het begin te beginnen , gaan we eerst eens aan de       
 grafische aspecten werken. Hieronder wordt verstaan : de       
 sprites,de achergrond,de scorelijst enz. Dit is dan ook het    
 begin van de listing. Meteen daarna komt de hoofdlus. Dat is   
 in dit geval de besturing en het happen.                       

                         DE GRAPHICS                            
                         +++++++++++                            

 De sprite(s) van PACCIE is getekend met de SPRITE-EDITOR van   
 futuredisk #6. Deze worden met het programaatje SPRITE.LDR     
 (ook van FD #6) in het V-ram ingeladen.                        
 De scorelijst-met-cijfers- tekening is getekend in (volgens    
 mij) het beste screen 5-tekenprogramma : DD-graph.             
 Deze tekening (PACIT.DAT) is , zoals de extensie zegt , een    
 DAT-file (inladen m.b.v. COPY "<naam>" TO (A,B),PG).           
 De co�rdinaten zijn zelf opgemeten en daarmee wordt alles op   
 de goede plaats gezet.                                         

                         DE BESTURING                           
                         ++++++++++++                           

 De besturing van PAC-IT is iets anders dan de besturing van    
 CUJOMU (die er niet op FUTUREDISK 7 stond, maar nu wel van     
 de partij is). PACCIE moet nl. altijd doorlopen (totdat hij    
 tegen een muur loopt). Daarom staan in variablen X en Y        
 respectivelijk de X en de Y STAP (!). De co�rdinaten van de    
 sprite zijn de variablen A en B. Deze worden dus iedere keer   
 met X resp. Y opgeteld. Deze X en Y stap worden weer bepaald   
 door de variable K. Hierin staat hoe groot de X en Y stap      
 moet zijn. Als u K dus verandert, zal de stap ook              
 veranderen.                                                    

 Ook het spritenummer wordt bij iedere verandering van          
 richting gegeven. Dit zou ook via een ingewikkelde formule     
 kunnen, maar dit is makelijker.                                

                          MUUR ???                              
                          ++++++++                              

 POINT .. Het is al eerder gezegd, maar het POINT-commando      
 komt toch altijd weer aan bod. Ook bij PAC-IT neemt het een    
 grote plaats in nl. bij het "zien" van een muur. Er moet dan   
 gestopt worden. Haalt u de POINT-regels maar eens uit het      
 programma en alles zal in de soep lopen.                       

 Bij iedere beweging van de sprite wordt er gekeken of er nog   
 niet tegen een muur aangebotst is. Op alle 4 de hoeken van     
 de sprite (A,B/A+8,B/A,B+8/A+8,B+8) wordt gekeken of er geen   
 puntje (POINT !!) van kleur 12 zit (de kleur van de muur).     
 Is dit wel het geval , dan wordt de A en B met de X en de Y    
 stap afgetrokken, met als gevolg dat de sprite stil blijft     
 staan.                                                         

                         HAPPEN                                 
                         ++++++                                 

 Ook bij het HAPPEN van de witte blokjes, speelt het POINT-     
 commando de hoofdrol. Er wordt nl. iedere keer gekeken of      
 de sprite op kleur 15 staat (de kleur van de blokjes). Zo      
 ja, dan wordt er een zwart blokje overheen gezet, zodat het    
 lijkt alsof PACCIE het blokje heeft opgegeten.                 

 De volgende keer krijgt PACCIE zijn tegenstanders te zien.     



                 DEF FN , ON INTERVAL en INSTR                  
                 +-+-+-+-+-+-+-+-+-+-+-+-+-+-+                  

 Tot slot nog enkele handige BASIC commando's.                  

 DEF FNA (X,Y)=<vergelijking> Met dit commando kunt U een       
                             eigen vergelijking laten oplossen  
 Het bijbehorende commando waarmee U de vergelijking kunt       
 laten werken is                                                

 A=FNA   In A komt dan de uitgerekende waarden. Bv.             

 10 'Voorbeeld DEF FNA                                          
 20 '                                                           
 30 DEF FNA (X,Y) =(X*X)+Y                                      
 40 FOR X=0 TO 4                                                
 50 A=FNA(X,1) : PRINT ,A; : NEXT X                             
 60 END                                                         

 Na F5 te hebben gedrukt zal er dit verschijnen :               

  1         2         5         10        17                    

 Want 0*0+1 = 1                                                 
      1*1+1 = 2                                                 
      2*2+1 = 5                                                 
      3*3+1 = 10                                                
      4*4+1 = 17                                                

 P.S.    I.P.V de A in DEF FNA kunt u ook een andere variable   
         gebruiken als U meerdere vergelijkingen wilt           
         invoeren.                                              
         De A in .=FNA moet dan natuurlijk ook worden           
         vervangen.                                             



 ON INTERVAL=<ID> GOSUB <REGELNUMMER>                           

 Dit commando springt om de ID interrupts naar <REGELNUMMER>.   
 Bij 1 interrupt (letterlijk onderbreeking) wordt om de 1/50    
 of 1/60 seconde naar een bepaalt adres gesprongen. Staat hier  
 een z.g.n. RET dan gebeurt er niets, andersdan wordt naar de   
 gedefinieerde plaats (regelnummer) gesprongen. (zie ook de     
 ML cursus #7).                                                 
 Bij dit commando horen nog 3 commando's nl. :                  

 INTERVAL ON : Het commando wordt in werking gebracht (na       
               iedere ID interrupts wordt er naar het regel-    
               nummer gesprongen.)                              

 INTERVAL OFF : Het commando ON INTERVAL wordt gestopt.         

 INTERVAL STOP : Het commando ON INTERVAL wordt genegeerd.      
                 Als U nu weer INTERVAL ON wordt gedaan dan     
                 zal er verder worden gegaan waar is            
                 genegeerd.                                     

 Dit commando is zeer geschikt om een regelmatige klok op te    
 laten lopen , zie het voorbeeld hieronder :                    

 10 'VB. van interval klokje                                    
 20 '                                                           
 30 TIJD=300                                                    
 40 ON INTERVAL=60 GOSUB 80                                     
 50 INTERVAL ON                                                 
 60 A$=INKEY$ : IF A$<>CHR$(13) THEN 60 ELSE END                
 70 'INTERVAL                                                   
 80 LOCATE 0,0 : PRINT TIJD : TIJD=TIJD-1                       
 90 IF TIJD<0 THEN END ELSE RETURN                              


 Tot slot nog het INSTR commando :                              

 A=INSTR(B,A$,B$) : In A komt te staan het hoeveelste           
                    karakter van A$ gelijk is met het           
                    eerste karakter van B$. Er wordt gezocht    
                    vanaf plaats B. Als B niet wordt ingevuld   
                    (A=INSTR(A$,B$)) dan wordt vanaf het        
                    eerste karakter in A$ gezocht.              

 Bijvoorbeeld :                                                 

 PRINT INSTR("ABCD","A")                                        
  1                                                             

 PRINT INSTR(2,"ABCD","A")                                      
  0                                                             

 PRINT INSTR("ABCD","E")                                        
  0                                                             

 10 CLS : PRINT "WILT U EEN GETAL INVOEREN ? (J/N)"             
 20 A$=INKEY$: IF INSTR("JjNn",A$)>0 THEN 30 ELSE 20            
 30 A=INT((INSTR("JjNn",A$)+1)/2) : IF A=1 THEN PRINT "JA"      
 40 IF A=2 THEN PRINT "NEE"                                     
 50 END                                                         

 Er wordt dus gekeken of A$ in "JjNn" zit. Zo ja , dan staat    
 geeft de INSTR :                                               
 als A$ = "J" ; 1 , A=INT((1+1)/2)=1                            
 als A$ = "j" ; 2 , A=INT((2+1)/2)=1 (INT(1.5)=1)               
 als A$ = "N" ; 3 , A=INT((3+1)/2)=2                            
 als A$ = "n" ; 4 , A=INT((4+1)/2)=2 (INT(2.5)=2)               

 De toepassing van INSTR is vooral handig, als er heel veel     
 mogelijkheden zijn. M.b.v. ON (INSTR(A$,B$)) GOSUB, kan dan    
 naar de subroutines van iedere letter gegaan worden.           




 Dit was het weer voor deze keer. De volgende keer zoals        
 gezegd de laatste SPRITES en we gaan eens kijken hoe je de     
 computer kunt laten denken !!!                                 

 De basic-files zijn ingepakt. Je kunt ze uitpakken met         
 het bekende programma PMEXT.com. Ga dus eerst naar MSX-Dos     
 en typ:        PMEXT A:BASICC.PMA B:*.*                        
 Pmext staat op deze FutureDisk!                                

 Tot de volgende keer .                                         

                                            TOM WAUBEN          

 P.S.  Heeft U nog vragen over deze cursus of zit u gewoon met  
       een probleem over BASIC , bel dan gerust naar :          

       046-758100 (na 15.00 u.)                                 

 P.P.S  Lees ook de EXTRA SEB op deze disk. (deze gaat          
        over de VDP-registers).                                 
                                                                


                  SPECIAL EFFECTS BASIC #4+                     
                  -------------------------                     

    EXTRA *** EXTRA *** EXTRA *** EXTRA *** EXTRA *** EXTRA     


 Jazeker !! Deze keer een EXTRA SEB, om eens wat meer           
 tegemoet te komen aan de wensen om BASIC wat algemener te      
 nemen, immers BASIC heeft veel meer mogelijkheden. Daarom      
 deze EXTRA rubriek.                                            

 Deze keer worden een aantal VDP-registers uitgediepd, met      
 name de registers 13 en 14.                                    


                           VDP ...                              
                           +++++++                              

 In BASIC bestaan 1 commando om in de VDP te schrijven of te    
 lezen ,nl.                                                     

 VDP(<A>)=<w>                                                   

 Hierbij is <A> het registernummer en <w> de <w>aarde. Bij      
 deze VDP-registers worden altijd een aantal bits geset, om     
 de gewenste uitwerking te krijgen.                             

 De VDP (letterlijk Video Display Processor) bepaalt het        
 uiterlijk van het scherm. De registers hebben dus allemaal     
 (direct of indirect) invloed op het scherm. Hieronder ziet u   
 een lijst van de belangrijkste VDP-registers in SCREEN 0.      
 Alleen register 24 is voor screen 5 t/m 12.                    
 Alle VDP-registers bespreken zou alleen maar langdradig        
 worden.                                                        


 VDP(7) : Bevat de (1e) voorgrond en achtergrond kleur.         
          De eerste 4 bits bevatten de voorgrondkleur en de     
          laatste 4 de achtergrondkleur.                        

          Bijvoorbeeld :                                        

          VDP(7)=&b00011111 '= COLOR 1,15 (&b0001=1,&b1111=     
                               15)                              


 VDP(9) : De vierde bit van register 9 bepaalt of sprites wel   
          of niet afgebeeld worden op het scherm. Als deze      
          geset is, worden ze wel afgebeeld , anders niet.      


 VDP(10) : Hiermee kan de frequentie worden bepaald (50 of 60   
           Hz.). Als bit 2 geset is dan is de frequentie 50     
           Hz., anders 60 Hz.                                   

           Bijvoorbeeld :                                       

           VDP(10)=&b00000000 'dit is 60 Hz.                    

           VDP(10)=&b00000010 'en dit 50 Hz.                    


 VDP(13) : Dit is ��n van de leukste VDP-registers, samen met   
           VDP(14).                                             
           Het bepaald de tweede voor- en achtergrond kleur     
           van het scherm.                                      

           Bijvoorbeeld :                                       

           VDP(13)=&b00101110 '2e voorgrond = 2 (&b0010) en     
                               2e achtergrond =14 (&b1110)      


 VDP(14) :Bepaald hoe lang voor- en achtergrond 1 en hoelang    
          voor- en achtergrond 2 te zien zijn.                  

          Deze 2e kleur in screen 0 is alleen te gebruiken      
          als BLOKKEN. Over iedere positie van het scherm       
          kan een blokje worden gezet. Voor ieder blokje        
          staat een bit in V-ram. Deze reeks bits lopen         
          van adres &h800 to &h905.                             

          Het is dus mogelijk om 4 kleuren in screen 0 te       
          krijgen. Dit is erg handig bij allerlei               
          toepassingen, zoals een menu-balkje (zie SEBMENU op   
          deze disk).                                           

          Nog een handige functie bij VDP(14) is dat er zelf    
          bepaald kan worden hoe lang de 1e voor- en achter-    
          grond en hoe lang de 2e voor- en achtergrond wordt    
          laten zien. Er kan dus een knipper effect worden      
          gecre�rd. De eerste nibble (de eerste 4 bits) geeft   
          aan hoelang de 2e kleuren en de tweede nibble geeft   
          aan hoeland de 1e kleuren op het scherm staan. *      

          Dus bij VDP(14)=&b11110000 zullen altijd de tweede    
          kleuren op het scherm staan.                          

          Leuk is dan het knipperen altijd blijft doorgaan,     
          ook onder het laden. (zie het programma EXTRA.BAS)    


          Bijvoorbeeld :                                        

          10 SCREEN 0                                           
          20 FOR I = &H800 TO &H905 : VPOKE I,0 : NEXTI 'WIS    
             ALLE BITS                                          
          30 VPOKE &H800,&B1010101                              
          40 VDP(13)=&HF4 'VOORGROND 15, ACHTERGROND 4          
          50 VDP(14)=&H55 'KNIPPER EVEN LANG 5x 2e KLEUREN,     
                           5x  1e KLEUREN                       
          60 GOTO 60                                            

  * Met "1e kleuren" worden bedoeld de standaard kleuren        
    Met "2e kleuren" worden de kleuren gezet met VDP(13)        
    bedoeld.                                                    


 VDP(19) : Dit register is identiek aan SET ADJUST (X,Y).       
           De Y is bij dit register de eerste nibble, de X      
           de tweede. Het verschil met SET ADJUST is dat        
           VDP(19) niet (!) weggesaved wordt in de klokchip.    
           De ADJUST kan dus veranderd worden, zonder dat de    
           standaard instelling wordt "aangetast".              

           Bijvoorbeeld :                                       

           VDP(19)=&hF1 'komt overeen met : SET ADJUST(1,0)     

           VDP(19)=&h27 'komt overeen met : SET ADJUST(-1,-7)   


 VDP(24) : Tenslotte register nummer 24. De werking van dit     
           register is het beste uit te leggen met een          
           voorbeeldje, nl. DD-graph.                           
           Als er uit een optie wordt gekozen, scrollt het      
           hele beeld soepel naar onder.                        
           Dit is ook de werking van VDP(24). Het laat het      
           beeld in zijn geheel verticaal scrollen. Ook         
           hiervan staat een leuk voorbeeld in dezelfde file    
           (EXTRA.BAS).                                         

 Ook van de andere registers zijn voorbeelden. Al deze          
 voorbeelden staan in 1 file : EXTRA.BAS , op te starten in     
 het SEBMENU.LDR .                                              

 In de volgende SEB EXTRA zal het toetsenbord met z'n           
 functies worden uitgediept.                                    


                                             TOM WAUBEN   
