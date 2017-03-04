MACHINETAAL CURSUS DEEL 9                     
-------------------------------                  

Na  de vorige  keer mooi  de BASIC  cursus gevolgd te hebben,   
gaan we  nu mooi zelf aan de slag met oersaaie, maar broodno-   
dige, mcode.                                                    
Deze  keer behandelen we de MSX2 SUB-ROM en de schuif- en ro-   
teerfuncties.                                                   

 SCHUIF- EN ROTEERFUNCTIES                     
-------------------------------                  
De  schuif-  en  roteerfuncties  zijn bedoeld  om binnen  een   
register de bits te verschuiven.                                
Schuifinstructies schuiven  alle bits 1 naar rechts of 1 naar   
links,  roteerfuncties  doen  dit ook, maar roteren de inhoud   
van de laagste bit in de hoogste, of andersom.                  
We bespreken d'er een rits (allemaal dus !!):                   

*       RLCA    (Rotate Left Circular Accumulator)              

Roteert  de inhoud van register A 1 bit naar links. De inhoud   
van bit 7 komt in de Carry vlag en in bit 0.                    
Je kunt  dus snel  de inhoud  van de hoogste bit van A testen   
met deze instructie en dan op de carry te testen.               

*       RLA     (Rotate Left Accumulator)                       

Doet  bijna hetzelfde  als een  RLCA, maar  de inhoud  van de   
Carry bit komt nu in bit 0.                                     

*       RRCA    (Rotate Right Circular Accumulator)             

Zie RLCA,  maar i.p.v.  naar links  te schuiven  schuift deze   
instructie naar rechts en komt bit 0 nu in de Carry en in bit   
7.                                                              

*       RRA     (Rotate Right Accumulator)                      

Zie  RLA, maar  i.p.v. naar  links te  schuiven schuift  deze   
instructie naar  rechts en  de inhoud  van bit  0 komt  in de   
Carry. De Carry gaat naar bit 7.                                

Tot  zover de instructies die alleen op register A betrekking   
hebben. Nu  de rest  van de instructies die je op alle 8 bits   
registers te gebruiken zijn.                                    

*       RLC r   (Rotate Left Circular)                          

Deze instructie doet hetzelfde als de instructie RLCA. Nu kun   
je dus in alle 8 bits registers schuiven.                       
Ook kun je deze instructie gebruiken in de volgende formaten:   
RLC     (IX+d)                                          
RLC     (IY+d)                                          
RLC     (HL)                                            
Nu  schuif je dus met de inhoud van geheugenlocaties waarheen   
dat 16 bits register verwijst.                                  

*       RRC r   (Rotate Right Circular)                         

Deze instructie doet hetzelfde als de instructie RRCA.          
Ook deze instructie werkt weer zo:                              
RRC     (IX+d)                                          
RRC     (IY+d)                                          
RRC     (HL)                                            

*       RL m    (Rotate Left)                                   

Deze instructie  doet hetzelfde als RLA. Werkt op alle 8 bits   
registers.                                                      

*       RR m    (Rotate Right)                                  

Deze instructie  doet hetzelfde als RRA. Werkt op alle 8 bits   
registers.                                                      


*       SLA m   (Shift Left Arithmetic)                         

Dit is de eerste schuifinstructie.                              
Alle bits worden 1 plaats naar links geschoven, waarbij bit 7   
in de Carry wordt geschoven en in bit 0 komt een 0.             

*       SRA m   (Shift Right Arithmetic)                        

Alle  bits gaan  1 naar  rechts. Bit 0 komt in de Carry en de   
inhoud van bit 7 verandert niet.                                

*       SRL m   (Shift Right Logical)                           

Hetzelfde als SRA, maar in bit 7 komt een 0.                    

*       RLD     (Rotate Left Digit)                             

Dit  is een  verplaatsing van  nibbles (groupies  van 4 bits)   
tussen register  A en  de inhoud  van HL,  (HL). De  onderste   
nibble  van A  (bit 0-3) komt in de onderste nibble van (HL).   
De onderste  nibble van (HL) komt in de bovenste. De bovenste   
nibble van (HL) komt in de onderste nibble van A (jaja).        
*       RRD      (Rotate Right Digit)                           

Ook  dit is  een verplaatsing  van nibbles  tussen A en (HL).   
De onderste  nibble van  A gaat  naar de  bovenste van  (HL).   
De  bovenste  van  (HL)  gaat  naar  de  onderste  van  (HL).   
En  de  onderste  van  (HL)  gaat  naar  de  onderste  van  A   
(jajajaja).                                                     

Omdat  al deze  instructies moeilijk te begrijpen zijn zonder   
begleidende schema's,  staat er  op deze  disk een .PTC file.   
Deze kun je in Dynamic Publisher inladen.                       
Tenminste  ......... als  het oppermonster  'm niet gewist of   
gecrunched heeft (RED. det is toch waardeloze zeik !!).         

        HET SUB ROM                            
       -------------                           

Aarg,  er zit  nog meer ROM in je MSX 2!!! Je dacht dat je met  
de BIOS  plus BASIC alles had, kom je bedrogen uit. De schele   
makers  van de  MSX 2  vonden dat  ze te weinig plaats hadden   
voor typische  MSX 2  routines. Daarom  werd er  nog een  16K   
groot  ROM gemaakt  dat ergens  in 1  van de  slots geplaatst   
werd. Omdat  het slot  systeem nogal  moeilijk is,  hebben de   
programmeurs van ASCII op adres #15F een BIOS routine gemaakt   
waarmee je makkelijk het Subrom kunt aansturen.                 
Deze routine werkt als volgt:                                   
In register IX plaats je het adres van de Subrom routine, die   
je wilt gebruiken. Een paar voorbeelden:                        

GRTPRT: EQU     #89                                             
LD      A,5                                             
CALL    #5F                                             
LD      A,"A"                                           
LD      IX,GRTPRT                                       
CALL    #15F                                            
RET                                                     

Deze routine print een teken op de grafische schermen 5 tot 8   
en verwacht  dit teken  in register  A. Vergelijk in BASIC de   
instructie: OPEN "GRP:" AS#1: PRINT #1,"A".                     

SETPLT: EQU     #14D                                            
LD      A,5                                             
CALL    #5F                                             
LD      D,15                                            
LD      A,%11110000                                     
LD      E,0                                             
LD      IX,SETPLT                                       
CALL    #15F                                            
RET                                                     

Deze  routine verandert  een kleur van je palet. In A moet je   
in  de  bovenste nibble  de roodintensiteit  zetten en  in de   
laagste nibble de blauwintensiteit. In E zet je in de laagste   
nibble  de  groenintensiteit. In  D staat  het te  veranderen   
colornummer. In BASIC is dit: COLOR=(15,7,0,0).                 

De routine  NEWPAD op  #1AD kan  de muis aansturen. Dit werkt   
ongeveer hetzelfde als in basic. Als je de BASIC begrijpt kun   
je   het   onderstaande   machinetaalprogrammaatje  ook   wel   
ontcijferen.                                                    

NEWPAD: EQU     #1AD                                            
LD      A,12                                            
LD      IX,NEWPAD       ;Schakel muis aan.              
CALL    #15F                                            
LD      A,13            ;Lees X-offset uit              
LD      IX,NEWPAD       ;(optellen bij oude X-co.)      
CALL    #15F                                            
PUSH    AF              ;bewaar X-offset                
LD      A,14            ;Lees Y-offset uit              
LD      IX,NEWPAD       ;(optellen bij oude Y-co.)      
CALL    #15F                                            
LD      B,A             ;Y-offset in B.                 
POP     AF                                              

Voor  een  geheel  overzicht  van het  Subrom kun  je contact   
opnemen met de programmeerlijn.                                 

Eigenlijk  hadden we nog een mooie grote listing willen laten   
zien, waarin veelvuldig gebruikt wordt gemaakt van de schuif-   
en roteer  instructies. Maar  des te  langer je  er over gaat   
nadenken,  des te  moeilijker wordt  het om  iets zinnings te   
verzinnen. Toch nog een klein voorbeeldje:                      

LD      A,#F0           ;HOOGSTE NIBBLE VOL             
RRCA                    ;IN A:  %01111000               
RRCA                    ;IN A:  %00111100               
RRCA                    ;IN A:  %00011110               
RRCA                    ;IN A:  %00001111               
AND     %00001111       ;BOVENSTE NIBBLE WISSEN         

Soms moet  je gaan  rekenen met de bovenste nibble. Nu is dat   
flink  lastig. Daarom  is het handig om de bovenste nibble in   
de onderste  te schuiven.  De AND  op het  einde is eigenlijk   
niet  nodig, maar als er al data in de onderste nibble staat,   
wordt deze doorgeschoven in de bovenste bits.                   

Door  een  foutje werkte  het programma  SPRITES vorige  keer   
niet. Dit  is verholpen  en dan hopen dat in de rebount alles   
goed gaat.                                                      

Veel programmeer plezier en een prettige vakantie,      

                       Ruud Gelissen,          
               Jan Willem van Helden  
