����           Welkom bij het 11de deel van            ����     
����                                                   ����     
����                de MACHINETAALCURSUS               ����     
����                                                   ����     
                                                                
    ------------------------------------------------------      
                                                                
 Vorige keer hebben we o.a. de OUT/IN instructies behandeld en  
 beloofden we al om in dit deel uitvoerig op de PSG registers   
 in te gaan.                                                    
 Misschien lijkt dat overbodig, omdat de gevorderde BASIC pro-  
 grammeur dit waarschijnlijk al lang beheerst, maar in          
 machinetaal is het allemaal veel beter uit te leggen.          
 BASIC is op het punt van de SOUND intructies namelijk erg      
 machinetaal geori�nteerd.(dus zonder het te weten zat je ook   
 vroeger al, je MSX bijna rechtstreeks te besturen d.m.v.       
 SOUND).                                                        
                                                                
                      De PSG registers.                         
                                                                
 De PSG (Programmeble Sound Generator) kent 16 verschillende    
 registers, waarvan de eerste 14 dienen voor het genereren van  
 geluid.                                                        
                                                                
 R0 en R1 dienen om de frequentie (lees:toonhoogte) te bepalen  
 van het eerste kanaal. Een frequentie kan bestaan uit 12 Bits  
 en dus varieren tussen de 0 en 4095 (2 tot de 12de-1).         
 De onderste 8 bits van deze frequentie moeten in register 0,   
 de bovenste 4 bits in register 1 gezet worden.                 
                                                                
 Dat kan gebeuren met de volgende instructies:                  
                                                                
 WRTPSG:      EQU     #93               ; BIOS                  
                                                                
              XOR     A                 ; A=0, weet je nog?     
              LD      E,%xxxxxxxx       ; Laagste deel freq.    
              CALL    WRTPSG                                    
              LD      A,1               ; register 1            
              LD      E,%xxxx           ; Hoogste deel freq.    
              CALL    WRTPSG                                    
                                                                
 R1 en R2 moet je gebruiken om de frequentie van kanaal B       
 te veranderen. R3 en R4 zijn dan natuurlijk voor kanaal C.     
                                                                
 Met de registers 2, 4 en 6 wordt dus de grove toon "geset",    
 met de registers 0,1 en 3 de fijne frequentieverschillen.      
                                                                
 Maar hoe speel ik nu een C op octaaf 4?                        
 Daarvoor kun je het beste de volgende tabel gebruiken:         
                                                                
                   1   2   3   4   5   6   7   8  OCTAAF        
       -----------------------------------------------          
             C  - D5D 6AF 357 1AC D6  6B  35  1B                
             C# - C9C 64E 327 194 CA  65  32  19                
             D  - BE7 5F4 2FA 17D BE  5F  30  18                
             D# - B3C 59E 2CF 168 B4  5A  2D  16                
             E  - A9B 54E 2A7 153 AA  55  2A  15                
             F  - A02 501 281 140 A0  50  28  14                
             F# - 973 4BA 25D 12E 97  4C  26  13                
             G  - 8EB 476 23B 11D 8F  47  24  12                
             G# - 86B 436 21B 10D 87  43  22  11                
             A  - 7F2 3F9 1FD FE  7F  40  20  10                
             A# - 780 3C0 1E0 F0  78  3C  1E   F                
             B  - 714 38A 1C5 E5  71  39  1C   E                
       ------------------------------------------------         
          NOOT                                                  
                                                                
 Voor een C op octaaf 4 zet je dus in register 0: &H0AC         
                                en in register 1: &H001         
                                                                
 Nu we een frequentie kunnen bepalen is het ten eerste van      
 belang dat de toon een volume toegewezen krijgt, hiervoor      
 dienen de registers R8 t/m R10.                                
 Slechts de 5 onderste bits zijn van belang.                    
                                                                
 Als de 5de bit (=&b000x0000) 0 is, geven de 4 onderste bits    
 het volume aan, dat daarmee kan varieren tussen 0 en 15.       
 Is de 5de bit 1 dan worden alle andere bits verwaarloosd en    
 wordt het volume bepaald door de zogenaamde Envelope Cycle.    
                                                                
                                                                
                      De Envelope Cycle.                        
                                                                
 Met behulp hiervan kan men een toon een bepaald                
 "aanzwel- en uitsterfpatroon" meegeven.                        
                                                                
 In R13 wordt bepaald hoe dit patroon eruit ziet. Daarbij zijn  
 alleen de onderste 4 bits van belang.                          
                                                                
 Men onderscheidt:                                              
                                                                
 0-3: uitstervende toon (volume daalt)                          
 4-7: aanzwellende toon (volume stijgt)                         
 8  : uitstervende toon, die als hij helemaal weg is weer       
      opnieuw start (zaagtand vorm)                             
 9  : idem 0-3                                                  
 10 : respectievelijk uitstervende- en aanzwellende toon        
      (na volume 15 gaat het volume dalen tot 0 en weer tot 15  
      aanzwellen)                                               
 11 : toon sterft uit en gaat daarna op maximum volume.         
 12 : aanzwellende toon, die als hij helemaal hard is weer      
      opnieuw start (zaagtand vorm)                             
 13 : zwelt aan tot maximum en blijft zo hard.                  
 14 : idem 10, maar start met aanzwellen i.p.v. met uitsterven. 
 15 : idem 4-7                                                  
                                                                
 Als we een patroon hebben gekozen, kunnen we ook nog bepalen   
 hoe lang de "aanzwel/uitsterfgolf" duurt m.b.v R11 en R12.     
                                                                
 Alle 16 bits mogen gebruikt worden; hoe hoger het 16 bits      
 getal hoe langer de golf duurt.                                
 De laagste byte moet je in R11 zetten en de hoogste byte in    
 R12. Ook hier geldt dus: R12 regelt de grove- ,R11 de fijne    
 afstelling..                                                   
                                                                
 Onthoudt goed dat het vullen van de registers R11 t/m R13      
 geen nut heeft als in een van de volume registers              
 (R8 t/m R10) geen 5de bit geset is.(16-31 staat.)              
                                                                
 Voor alle kanalen wordt dezelfde ENVELOPE gebruikt !!          
                                                                
                                                                
 Op de PSG kun je niet alleen tonen genereren, ook het maken    
 van ruis is mogelijk.                                          
 In de 5 laagste bits van R6 wordt de ruisfrequentie weer-      
 gegeven. Deze kan dus varieren tussen 0 en 31, waarbij 31 de   
 hoogste "ruistoon" is. De ruisfrequentie wordt normaal         
 gesproken aan alle 3 de kanalen meegegeven. M.b.v. register 7  
