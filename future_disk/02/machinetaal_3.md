                                                               
        Machinetaal cursus deel III                             
                                                                
 Welkom bij alweer het derde deel van deze cursus.              
 Zoals vorige keer beloofd beginnen we dit keer met de INC en   
 DEC commandos.                                                 
                                                                
 Eerst een klein programmatje:                                  
                                                                
        ORG    0A000H                                           
        LD     A,0                                              
        INC    A                                                
        LD     B,255                                            
        DEC    B                                                
        RET                                                     
                                                                
 Wat  doet dit programaatje nu ? We zullen het even van regel   
 tot regel nalopen.                                             
                                                                
 ORG    0A000H                                                  
                                                                
 Dit is  een assembler  instructie die de assembler zegt waar   
 in   het  geheugen  het  programma  moet  beginnen.  In  dit   
 voorbeeld dus op adres 0A000H                                  
                                                                
 LD     A,0                                                     
                                                                
 Dit is  een laadinstructie  die we in de vorige cursussen al   
 behandeld hebben. Het register A wordt geladen met de inhoud   
 0.                                                             
                                                                
 INC    A                                                       
                                                                
 Aha, dit is dan eindelijk wat nieuws.                          
 INC  wil  niets  meer zeggen  dan verhoog  (van het  Engelse   
 INCrease).  Oftewel,  het  register  A  moet met  1 verhoogd   
 worden. Nu staat er dus in register A het getal 1.             
                                                                
 LD     B,255                                                   
                                                                
 Laad  het register B met 255 (in B staan dus nu alle bits op   
 1).                                                            
                                                                
 DEC    B                                                       
                                                                
 Ook weer een nieuwe instructie.                                
 DEC is  het tegenovergestelde  van INC.  DEC verlaagt dus de   
 inhoud van een register (van het Engelse DECrease). Nu staat   
 dus in register B het getal 254.                               
                                                                
 RET                                                            
                                                                
 Een instructie die wil zeggen dat het programma beeindigd is   
 en dat kan worden teruggekeerd naar het hoofdprogramma.        
                                                                
 De  instructies INC  en DEC kunnen op vele manieren gebruikt   
 worden en  kent daarom  een paar vormen. Deze zullen we effe   
 noemen:                                                        
 (het  nu volgende  verhaal geldt  voor zowel INC als DEC, in   
 het voorbeeld zullen we ons aan INC houden)                    
                                                                
 INC    m                                                       
                                                                
 Verhoogt de door  m  te specificeren operand met 1.            
                                                                
 m  kan zijn: r,(HL),(IX+d) of (IY+d)                           
                                                                
 r  staat voor alle achtbits registers, dus A,B,C,D,E,H,L       
                                                                
 (HL)  betekent  dat de  inhoud van  het adres  waar HL  naar   
 verwijst met 1 verhoogd moet worden.                           
                                                                
 (IX+d)  en (IY+d)  willen zeggen, waar de inhoud van IX plus   
 de relatieve verplaatsing d met 1 verhoogd moet worden.        
                                                                
 INC    ss                                                      
                                                                
 verhoogt het door ss gespecificeerde registerpaar met 1        
                                                                
 ss  kan zijn: HL,DE,BC,IY,IX of SP                             
                                                                
 Als je  dus een  INC op  een 16-bits  getal geeft, wordt ook   
 hier de inhoud met 1 verhoogt.                                 
                                                                
                                                                
 In  het  bovenstaande  verhaaltje  stonden  een paar  termen   
 waarvan  je nog niets snapt (zoals (HL) of (IX+d)), maar die   
 zullen in de volgende cursussen nog wel worden uitgelegd.      
                                                                
 Je ziet  INC en  DEC zijn niet zo'n moeilijke instructies en   
 door  er een  beetje mee te spelen weet je al gauw hoe je ze   
 moet gebruiken.                                                
                                                                
 Ook vertelden  we in  de vorige  cursus, dat  we de JP en de   
 CALL zouden behandelen. Aangezien belofte schuld maakt,        
                                                                
                                                                
 JP en CALL-groep.                                              
                                                                
 JP  en CALL zijn eigenlijk het beste te vergelijken met GOTO   
 en GOSUB  in BASIC.  Een CALL  wordt, net zoals een GOSUB in   
 BASIC, afgesloten met een RETurn.                              
 Uit  een JumP  kun je,  eveneens als bij GOTO in BASIC, niet   
 met een RETurn of zoiets dergelijks terugkeren.                
 Het is dus al zoals de namen het zeggen:                       
 een JumP staat voor spring (dus niet meer terug)               
 een CALL staat voor roep aan (en keer nog ooit eens terug).    
                                                                
 Om het  allemaal een  beetje te  verduidelijken, dan  nu een   
 klein listingkje.                                              
                                                                
        ORG    0A000H                                           
                                                                
 ST:                                                            
        LD      B,0                                             
        CALL    HEGEGA                                          
        RET                                                     
                                                                
 HEGEGA:                                                        
        INC     B                                               
        RET                                                     
                                                                
 Wat doet dit programma nu:                                     
                                                                
 ORG    0A000H                                                  
                                                                
 zie boven.                                                     
                                                                
 ST:                                                            
                                                                
 Dit is een label (zie vorige cursus)                           
                                                                
 LD     B,0                                                     
                                                                
 Laad het register B met 0                                      
                                                                
 CALL   HEGEGA                                                  
                                                                
 Roep de subroutine HEGEGA aan.                                 
                                                                
 RET                                                            
                                                                
 Keer terug naar hoofdprogramma (bv BASIC)                      
                                                                
 HEGEGA:                                                        
                                                                
 Label subroutine.                                              
                                                                
 INC    B                                                       
                                                                
 Verhoog register B met 1.                                      
                                                                
 RET                                                            
                                                                
 Keer  terug naar  hoofdprogramma (in  dit geval  dus naar de   
 instructie na de CALL).                                        
                                                                
 Dit programmaatje  heeft dus  niet meer  gedaan dan  B met 1   
 verhoogd   (Lees  nog  maar  eens  na  dankzij  onze  nieuwe   
 textroutine, ook KUBIE is eens zo begonnen).                   
 We zullen  maar niet  uitgaan leggen  wat er  hardware-matig   
 gebeurt  als er  een CALL of een JumP wordt uitgevoerd, want   
 dat zou helemaal onbegrijpelijk worden.                        
                                                                
 In de volgende cursus zullen we een beetje meer MSX gerichte   
 machinetaal gaan behandelen, want het wordt nu toch wel heel   
 saai.                                                          
                                                                
 Veel Succes namens:                                            
                                                                
 Ruud Gelissen en                                               
 Jan-Willem van Helden.                                         
                                                                
                --------Boekenlijst---------                    
 ZAKBOEKJE Z-80                                                 
                                                                
        J.B. Vonk -- Kluwer technische boeken -- (c) 1985       
                                                                
 MACHINETAAL Z-80                                               
        J.B. Vonk/E.J.J. Doppenberg -- Kluwer Tech. Boeken --   
        (c) 1987                                                
                                                                
                --------Woordenlijst--------                    
                                                                
 ADRES/GEHEUGENADRES/GEHEUGENPLAATS:                            
                                                                
        Een plaats in het geheugen van de MSX.                  
                                                                
 ASSEMBLER:                                                     
                                                                
        Een programma dat het assembleren uitvoert.             
                                                                
 ASSEMBLEREN:                                                   
                                                                
        Het omzetten van machinetaal naar MCODE.                
        Voorbeelden van machinetaal zijn de listingen in deze   
        cursus.                                                 
                                                                
 BIT:                                                           
                                                                
        Een waarde die 0 of 1 kan zijn.                         
        (zie vorige cursussen)                                  
                                                                
 BYTE:                                                          
                                                                
        Een groepje van 8-BITS.                                 
        (zie vorige cursussen)                                  
                                                                
 LABEL:                                                         
                                                                
        Een naam voor een geheugenplaats, vergelijkbaar         
        met een regelnummer in BASIC.                           
                                                                
 MCODE:                                                         
                                                                
        Dit is de taal waarin de machinetaal commando's worden  
        omgezet en die de computer kan begrijpen en uitvoeren.  
        (zie vorige cursussen)                                  
                                                                
 REGISTER:                                                      
                                                                
        Een register is te vergelijken met een variable         
        in BASIC.                                               
        (zie vorige cursussen)                                  
                                                                
        Voor vragen zie de Helplines.                           
                                          
