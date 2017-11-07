---- MACHINETAAL CURSUS DEEL 10-1 ----              










Eindelijk heeft de nummering van de FutureDisk dan toch de     
machinetaalcursus bijgehaald. Maar ja, wat wil je als de       
hoofdredacteur compleet gestoord(NvdR. Komt dat misschien      
niet door jou, dat ik zo gestoord ben, Jantje?) is en vanaf    
nummer 6 steeds vreemdere nummeringen gaat hanteren.           

Genoeg gekanker...                                             

Daar we al bijna de complete instructie set van de Z80 hebben  
behandeld is de machinetaalcursus eigenlijk dood en zou je     
nu moeten kunnen programmeren in mcode.                        
Het leek ons nu interessant om meer achtergrond informatie     
over de verschillende microchips in je MSX te geven.           
Daarom beginnen we dan ook met de bespreking van de            

---- Programmable Sound Generator ----               

De Programmable Sound Generator (kortweg PSG) is de            
standaard MSX geluidschip. Deze kun je dan ook in iedere MSX   
aantreffen. Het is een AY-3-8910 (of verwante) microchip en    
wordt in het MSX-systeem gebruikt voor de BEEP en muzikale     
ondersteuning. Ons peesgeetje kan de volgende dingen:          

* Via 3 toongeneratoren 4096 verschillende frequenties         
weergeven op 16 volumeschalen.                                 
* Met een Envelope generator kunnen instrumenten benaderd      
worden.                                                        
* Met een Noise generator (Ruis!!!) kunnen drums, explosies    
en golven gecre erd worden.                                    

Dit klinkt misschien allemaal spectaculair en je vraagt je     
misschien af waarom al die PSG melodietjes zo "slecht"         
klinken.                                                       
Het probleem is dat de PSG de envelope en noise generators     
niet op meerdere kanalen kan toepassen. Je kunt maar 1         
kanaal op een viool doen lijken, een ander op een drum, maar   
er blijft er 1 het monotone PSG geluid voortbrengen dat je     
hoort als je in BASIC een PLAY opdracht geeft.                 
Toch mag je niet klagen. Bedenk dat deze chip (lang) voor      
1983 ontwikkeld werd en toen zeer goed was (wel eens een       
VIC20 horen dreunen??) en dat spellenmakers als Konami,        
Compile, T&E soft en weet ik wie, er toch zeer goede muziek    
uit voortbrachten. Muziek van de MicroCabin, die zowel PSG     
als FM-Pac gebruikt, klinkt soms zelfs als MIDI muziek.        

Nu ongeveer de mogelijkheden van de PSG zijn opgesomt kunnen   
we eens gaan kijken naar de meer programmatische kant (wat     
een woord!!) oftewel HOE KRIJG IK D'R GELUID UIT ????          

De communicatie tussen de Z80 en de PSG verloopt via de I/O    
poorten #A0 t/m #A2.                                           

I/O poorten ??? Oke, bijna alles hadden we behandeld:          

Input/Output poorten zijn de verbindingen van de Z80 met z'n   
hulpchips zoals de diskdrive, VDP, PSG etc.                    
Je kunt je dit het beste voorstellen als mannetjes die met     
touwtjes aan elkaar zitten. Als het belangrijkste mannetje     
(de Z80 dus) een ruk aan het touw geeft (een stroompje         
stuurt) gaat het mannetje dat aan het touw vastzit aan de      
hand van de sterkte van de ruk een handeling uitvoeren.        

De Z80 heeft 256 van deze touwtjes (0 t/m 255) en kan          
daarmee de hele MSX naar z'n hand zetten (een soort            
dictatortje dus)(NvdR. Zoiets als mij?). Niet alle poorten     
zijn bezet, een aantal is nog gereserveerd voor toekomstige    
uitbreidingen.                                                 

Je kunt de I/O poorten aansturen met de volgende Z80           
instructies:                                                   

     ---- Output instructies ----                   


OUT     (n),A                                           

*      Deze instructie stuurt de inhoud van A naar poort n.    
n kan een waarde hebben van 0 tot 255.                  

OUT     (C),r                                           

*      Deze instructie stuurt de inhoud van register r naar    
de poort waarheen het C-register verwijst               

OTDR    (OuTput Decrement Repeat)                       

*      Stuurt de inhoud van het adres waar HL naar verwijst    
naar de poort waar register C heen verwijst. HL wordt   
daarna met 1 verlaagd. Het register B, dat als teller   
gebruikt wordt, wordt met 1 verminderd. Zolang B niet   
nul is wordt de instructie herhaald.                    

OTIR    (OuTput Increment Repeat)                       

*      Doet hetzelfde als OTDR alleen wordt het HL register    
nu verhoogd i.p.v. verlaagd.                            

OUTD    (OUTput Decrement)                              

*      Doet weer hetzelfde als OTDR alleen wordt de            
instructie nu niet herhaald totdat B gelijk aan nul     
is. Register B wordt gewoon verlaagd.                   

OUTI    (OUTput Increment)                              

*      Gelijk aan OUTD, alleen wordt HL verhoogd.              


     ---- Input instructies ----                    


IN      A,(n)                                           

*      Omgekeerde van OUT (n),A. De Z80 vraagt nu een waarde   
van het randapparaat. In register A komt nu de waarde   
van de met n gespecificeerde poort.                     

IN      r,(C)                                           

*      Register r wordt geladen met de waarde van de poort     
waar register C naar verwijst.                          

INDR    (INput Decrement Repeat)                        

*      Tegenovergestelde van OTDR.                             

INIR    (INput Increment Repeat)                        

*      Tegenovergestelde van OTIR                              

IND     (INput Decrement)                               

*      Tegenovergestelde van OUTD                              

INI     (INput Increment)                               

*      Tegenovergestelde van OUTI                              


Gewapend met deze informatie zijn we klaar om de I/O poorten   
voor de PSG te bestormen.                                      
(Maar dat doen we in tekst 2)   

---- MACHINETAAL CURSUS DEEL 10-2 ----             

(Ok�, maak de borst maar nat,we moeten de PSG bestormen!)      









In poort #A0 moet je het registernummer zetten dat je wilt     
onderzoeken. Met poort #A2 kun je de inhoud vervolgens         
lezen en met poort #A1 kun je de inhoud veranderen.            

Huh ?? Registers ?? De PSG ???                                 

Ja, ook hulpchips kunnen registers hebben net zoals de Z80.    
Een hulpchip is namelijk een eigen processor. De Z80 kan       
alleen via een paar registers commando's geven aan een         
hulpchip. Als je de PSG een toon wilt laten voort brengen      
moet de Z80 eerst de commando's voor die toon doorsturen,      
maar daarna neemt de PSG het over en blijft zo'n toon          
klinken totdat het Z80 het commando voor "uit" geeft.          

In de volgende cursus zullen we dieper ingaan op de            
registers van de PSG en hun functie. We sluiten dit keer af    
met een explosie geluidje in machinetaal... luistert en        
huivert.                                                       
;EXPLOSIE.GEN                                                  
;(C) Michiel Visser                                            

   DB      #FE             ;Header voor GEN80      
   DW      ST,EN,ST        ;Bij WBASS2 overbodig   

   ORG     #C000                                   

ST:                                                     
   XOR     A               ;a=0                    
   LD      E,0                                     
   CALL    WRTPSG                                  
   LD      A,1                                     
   LD      E,5                                     
   CALL    WRTPSG                                  
   LD      A,2                                     
   LD      E,0                                     
   CALL    WRTPSG                                  
   LD      A,3                                     
   LD      E,13                                    
   CALL    WRTPSG                                  
   LD      A,4                                     
   LD      E,255                                   
   CALL    WRTPSG                                  
   LD      A,5                                     
   LD      E,15                                    
   CALL    WRTPSG                                  
   LD      A,6                                     
   LD      E,30                                    
   CALL    WRTPSG                                  
   LD      A,7                                     
   LD      E,0                                     
   CALL    WRTPSG                                  
   LD      A,8                                     
   LD      E,16                                    
   CALL    WRTPSG                                  
   LD      A,9                                     
   LD      E,16                                    
   CALL    WRTPSG                                  
   LD      A,10                                    
   LD      E,16                                    
   CALL    WRTPSG                                  
   LD      A,11                                    
   LD      E,0                                     
   CALL    WRTPSG                                  
   LD      A,12                                    
   LD      E,5                                     
   CALL    WRTPSG                                  
   LD      A,13                                    
   LD      E,0                                     
   CALL    WRTPSG                                  

   LD      B,255                                   
WACHT:                          ;even wachten           
   PUSH    BC                                      
   POP     BC                                      
   DJNZ    WACHT                                   

   LD      A,12                                    
   LD      E,56                                    
   CALL    WRTPSG                                  
   LD      A,13                                    
   LD      E,0                                     
WRTPSG:                                                 
   OUT     (#A0),A         ;Register               
   LD      A,E                                     
   OUT     (#A1),A         ;Data Write !!          
   RET                                             
EN:                                                     
   END                                             

Misschien is het je al opgevallen dat de data veel             
overeenkomst vertoont met de BASIC opdracht SOUND. In weze     
doet SOUND niets anders dan het weg"outen" van de waardes      
naar de PSG. Zo zie je maar hoe machinetaal georienteerd de    
BASIC wel niet is !!                                           

Overigens is de routine WRTPSG rechtstreeks uit de BIOS        
gehaald. We hebben ze even uitgetikt om het OUT commando te    
verduidelijken.                                                

   Rest ons nog de ASCII en Michiel Visser te      
   bedanken voor hun medewerking.                  

           Veel programmeerplezier van:            

                   Jan Willem van Helden           
                   en      Ruud Gelissen    
                   
