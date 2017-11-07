                                                                
                  ---- Mcode cursus #12 ----                    
                                                                
                ����                     ����                   
                ����                     ����                   
                ����                     ����                   
                ����                     ����                   
                                                                
                                                                
 De vorige keer geen Mcodecursus, omdat Ruud en ik even een     
 weekje waren skieen en dus hele andere dingen aan onze kop     
 hadden dan een mcodecursus.(NvdR: en thuis met twee gebroken   
 benen op bed lagen)                                            
                                                                
 Deze keer behandel ik de PSGregisters 14 en 15.                
                                                                
 Deze 2 registers van de PSG worden op de MSX gebruikt voor     
 het uitlezen van de beide spelingangen, in de volksmond        
 beter bekend als de joystick-poorten.                          
                                                                
 De aansluiting voor de spelingangen zijn 2 vrouwelijke (Ja,    
 de MSX is dus een zij!!) 9-pins AMP-aansluitingen en zijn      
 als volgt ingedeeld:                                           
                                                                
             *************************************              
             *                                   *              
             *    ***   ***   ***   ***   ***    *              
             *   * 1 * * 2 * * 3 * * 4 * * 5 *   *              
              *   ***   ***   ***   ***   ***   *               
              *                                 *               
              *     ***    ***    ***    ***    *               
              *    * 6 *  * 7 *  * 8 *  * 9 *   *               
               *    ***    ***    ***    ***   *                
               *                               *                
                *******************************                 
                                                                
 Pin nr.        Naam Signaal    Richting                        
                                                                
    1           FWD             IN                              
    2           BACK            IN                              
    3           LEFT            IN                              
    4           RIGHT           IN                              
    5           +5 V            ---                             
    6           TRG 1           IN/UIT                          
    7           TRG 2           IN/UIT                          
    8           OUTPUT          UIT                             
    9           GND             ---                             
                                                                
 PSG register 14 kan alleen gelezen worden. De bits van dit     
 register hebben de volgende betekenis:                         
                                                                
 Bit 7  Cassete-ingangsignaal                                   
 Bit 6  ????                                                    
 Bit 5  invoersignaal pin 7                                     
 Bit 4  invoersignaal pin 6                                     
 Bit 3  invoersignaal pin 4                                     
 Bit 2  invoersignaal pin 3                                     
 Bit 1  invoersignaal pin 2                                     
 Bit 0  invoersignaal pin 1                                     
                                                                
 PSG register 15 kun je zowel lezen als schrijven. Voor de      
 betekenis van de bits geldt:                                   
                                                                
 Bit 7  ????                                                    
 Bit 6  keuze spelingang 1 of 2 (1=ingang 2)                    
 Bit 5  uitvoersignaal pin 8    ingang 2                        
 Bit 4  uitvoersignaal pin 8    ingang 1                        
 Bit 3  uitvoersignaal pin 7    ingang 2                        
 Bit 2  uitvoersignaal pin 6    ingang 2                        
 Bit 1  uitvoersignaal pin 7    ingang 1                        
 Bit 0  uitvoersignaal pin 6    ingang 1                        
                                                                
                                                                
 Deze registers kun je gewoon gebruiken als normale PSG         
 registers en je kan dus gewoon via poort #A0 het nummer van    
 het register schrijven (14 of 15) en dan met poort #A1 een     
 waarde inschrijven of met poort #A2 de waarde van het          
 register lezen.                                                
                                                                
 Wat hebben we nu aan deze wetenschap ???                       
                                                                
 Via deze 2 spelingangen kunnen we joysticks (genotsknotsen),   
 muizen, touchpads, lichtpistolen etc. etc. op de MSX           
 aansluiten. Ook kan er zo een verbinding tussen 2              
 MSXcomputers tot stand worden gebracht, je kunt immers         
 lezen en schrijven (in beperkte mate) via deze poorten. Er     
 bestaat zelfs een boek dat "Kreatief met Spelingangen"         
 omgaat: Electronicaprojecten voor MSX-Computers. Dit boekje    
 ligt nog voor geen tientje bij de meeste "de Slegte"-          
 boekhandels. Wie weet brengt dit nog iemand op het idee om     
 een SNES-scope via de spelingangen te besturen.                
                                                                
 Tot slot nog een listinkje voor de Codefreaks:                 
                                                                
                                                                
        ORG     #C000                                           
                                                                
        DB      #FE                                             
        DW      ST,EN,ST                                        
                                                                
 ;PSG Lees                                                      
 ;Leest de inhoud van ingang 1 en 2                             
                                                                
 ST:                                                            
        DI                                                      
        LD      A,15                                            
        OUT     (#A0),A                                         
        LD      A,%00000000                                     
        OUT     (#A1),A ; Ingang 1 aanzetten                    
        LD      A,14                                            
        OUT     (#A0),A                                         
        IN      A,(#A2) ; Ingang 1 lezen                        
        LD      (STCK1),A                                       
        LD      A,15                                            
        OUT     (#A0),A                                         
        LD      A,%01000000                                     
        OUT     (#A1),A ; Ingang 2 aanzetten                    
        LD      A,14                                            
        OUT     (#A0),A                                         
        IN      A,(#A2) ; Ingang 2 lezen                        
        LD      (STCK2),A                                       
        EI                                                      
        RET                                                     
 STCK1: DB      0                                               
 STCK2: DB      0                                               
 EN:    END                                                     
                                                                
 Wil je echt nog wat meer over deze registers te weten komen    
 dan raad ik je aan om eens de BIOSroutines #D5,#D8 en #DB te   
 disassemblen. Dit zijn namelijk respectievelijk de             
 Joystickuitlees, de Joystickknoppenuitlees en de muisroutine   
 van de MSX.                                                    
                                                                
 Mijn grote dank gaat uit aan A.Rensink voor de informatie      
 over dit onderwerp.                                            
                                                                
                                                                
                        Veel programmeerplezier van:            
                                                                
                                Jan-Willem van Helden           
                                en      Ruud Gelissen           
                                                                
 P.S. Ja, sorry dat dit zo'n korte cursus is, maar Hegega       
 moet Bet Your Life nog afmaken en dus staat de programmeer-    
 cursus effe op een wat lager pitje .....      
