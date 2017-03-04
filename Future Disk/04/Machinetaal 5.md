                                                                
 Machinetaal cursus deel 5                                      
                                                                
 Na alle mooie informatie in de vorige afleveringen             
 gaven we jullie in de vorige aflevering (deel 4) de            
 eerste grote listing. Gelukkig klopt onze programmeer          
 cursus tot hier toe nog helemaal want we hebben nog geen       
 boze reacties via onze programmeer-lijn gehad.                 
                                                                
 Overigens excuses voor de nogal snelle behandeling van         
 EQU en CP, de schrijver van het stukje (JWVH) had niet         
 gecontroleerd of alle gebruikte commando's in de listing       
 behandeld waren. Daar Ruud Gelissen het stuk pas op de         
 clubdag in Elsloo te zien kreeg (de deadline dus) is er        
 toen snel nog enige tekst toegevoegd. We hopen dat dit         
 in de toekomst niet meer voorkomt (red.).                      
                                                                
 Vanwege bovenstaand verhaal behandelen we EQU en CP nog        
 een keer, maar nu wat uitgebreider.                            
                                                                
 EQU:                                                           
                                                                
 EQU is niet de nieuwe Europese munteenheid, maar de            
 betekenis is wel bijna hetzelfde. In het Engels betekent       
 het namelijk ,"maak gelijk aan" (EQUate). Voor de munten       
 betekent dat dus dat ze gelijk gemaakt worden aan een          
 standaard, voor de computer betekent dat, dat het              
 ervoor staande LABEL overal in de listing vervangen            
 wordt door het achter de EQU staande cijfer.                   
                                                                
 Voorbeeld:                                                     
                                                                
 KOEN:  EQU     6                                               
 DOLS:  EQU     3                                               
                                                                
        LD      A,KOEN                                          
        LD      B,DOLS                                          
        ADD     A,B                                             
        RET                                                     
                                                                
 KOEN wordt nu overal vervangen door 6,                         
 DOLS wordt nu overal vervangen door 3, dus in het              
 geheugen zal staan:                                            
                                                                
        #3E,#6          ; LD      A,6                           
        #05,#3          ; LD      B,3                           
        #80             ; ADD     A,B                           
        #C9             ; RET                                   
                                                                
 Je ziet, EQU is geen commando en wordt dus niet in het         
 geheugen geplaatst.                                            
                                                                
 P.S. het teken "#" is hetzelfde als "&H" en betekent           
 dus dat de hexadecimale notatie wordt gebruikt.                
 Sommige assemblers maken gebruik van het "#".                  
                                                                
 CP:                                                            
                                                                
 CP staat voor het Engelse ComPare, wat vergelijk               
 betekent. Het vergelijkt de inhoud van de Accumulator          
 (register A) met die van de waarde die achter de               
 instructie staat en zet aan de hand van de uitkomst de         
 vlaggen. CP 251 trekt dus 251 van A af maar zet de             
 uitkomst niet terug in A. Als A nu gelijk aan 251 is,          
 is de Z vlag geset ( 1 dus ) en de C vlag niet ( ** ).         
 Is A kleiner dan 251, dan is de C vlag geset, de Z vlag        
 niet. Is A groter dan 251, is de Z vlag gereset en de          
 C vlag ook.                                                    
                                                                
 ** :   De C vlag of CARRY vlag wordt dus geset als een         
        berekening een "foutje" opleverde.                      
        b.v. LD    A,100        of: LD     B,255                
             SUB   110              INC    B                    
                                                                
         of: LD    A,#10 (=16)  ect.                            
             CP    17                                           
 (voor een overzicht van de vlaggen zie FUTURE-DISK #0          
 (public domain)).                                              
                                                                
 Nu het complete overzicht van CP:                              
                                                                
 CP     s                                                       
                                                                
 Trekt de door s te specificeren operand af van de inhoud       
 van de accumulator. Het resultaat wordt genegeerd maar         
 beinvloed wel de vlaggen.                                      
                                                                
 s kan zijn: r,n,(HL),(IX+d) of (IY+d)                          
                                                                
 r:                                                             
 De accumulator wordt vergeleken met een van de registers       
 of met zichzelf.                                               
 r kan dus zijn:        A,B,C,D,E,H of L                        
                                                                
 n:                                                             
 Een getal varieerende van 0 to 255.                            
                                                                
 (HL):                                                          
 Met de inhoud waarheen het adres HL verwijst.                  
                                                                
 (IX+d):                                                        
 Met de inhoud waarheen het adres IX plus verplaatsing d        
 verwijst.                                                      
                                                                
 (IY+d):                                                        
 Met de inhoud waarheen het adres IY plus verplaatsing d        
 verwijst.                                                      
                                                                
 Oke, dit stukje is gedeeltelijk overgeschreven uit het         
 zakboekje Z80 Machinetaal door J.B.Vonk, omdat het             
 hierin het duidelijkst en goed beschreven staat. Dit           
 even voordat andere instanties hierover problemen gaan         
 maken !                                                        
                                                                
                                                                
 Al eerder zijn de POINTER register genoemt maar nog            
 niet uitvoerig besproken.                                      
                                                                
 IX en IY                                                       
                                                                
 IX en IY zijn bijna gewone 16-bits registers als DE en         
 BC, ware het niet dat zij nog een speciale functie             
 hebben. Je kunt bijvoorbeeld register A laden met              
 (IX+10). Nu komt in A de inhoud van geheugenplaats 10 na       
 het adres waar IX naar verwijst. Het klinkt moeilijk,          
 maar kijk eens naar dit voorbeeldje.                           
                                                                
        LD     IX,TRASH                                         
        LD     A,(IX+0)                                         
 TRASH:                                                         
        DB      0                                               
        DB      1                                               
        DB      2                                               
        DB      3                                               
                                                                
 Nu komt in A dus 0 te staan. Vervangen we nu LD A,(IX+0)       
 door LD A,(IX+1), dan komt er in A 1 te staan. IX wordt        
 daarom altijd aan geduid met de verplaatsing d (IX+d).         
 d kan een waarde van 0 to 255 aannemen.                        
                                                                
 Dit gehele verhaal geldt ook voor IY.                          
                                                                
 Ook zagen we een aantal keer (HL). Dit is te vergelijken       
 met (IX+0).                                                    
                                                                
 Met deze instructies kunnen we ook INCreasments en             
 DECreasments uitvoeren. INC (IX+143) is dus gewoon             
 mogelijk. Dit opent veel nieuwe mogelijkheden voor             
 professionele programmeurs (Databases,spreadsheets etc.)       
 Voor de demo en spelletjes programmeur zijn ze niet zo         
 interessant vanwege hun traagheid. Het INCreasen               
 (taalverloedering) van A duurt 4 T-cycli (computertijds        
 eenheid). INC (HL) duurt 11 T-cycli en INC (IX+d) duurt        
 maar liefst 23 T-cycli (bijna het 6 dubbele van INC A).        
 Dit wil natuurlijk niet zeggen dat je ze niet moet             
 gebruiken. Soms zijn ze onmisbaar, zoals in muziek             
 routines, want wil je het anders schrijven, kost het je        
 veel en veel meer geheugen. Zo zie je maar, vaak is            
 programmeren een kwestie van kiezen tussen snelheid of         
 geheugen besparing.                                            
                                                                
 P.S. traag moet hier eigenlijk tussen "aanhalings-             
 tekens"  staan,  een  computer  commando  is nooit  echt       
 traag.                                                         
                                                                
 Een T-cycli is 1 puls van de ingebouwde klok van de            
 processor: Op de Z80 is dat 4 Mhz (afgerond).                  
 1 T-cycli duurt dus 1/4*10e6 = 2,5*10e-7 = 250 nanosec.        
                                                                
 Op de Turbo R draait het interne klokje met een snelheid       
 van 28 Mhz. Hier duurt 1 T-cycli dus 1/28*10e6 =               
 3,5*10e-8 = 35 nanosec. (dat is snel !!!, dat lijkt ...        
 wel !!!). Maar dit is allemaal een beetje al te                
 technisch, want zodadelijk beginnen we over zaken die          
 wij niet snappen... (zoals interne struckturen of              
 zoiets).                                                       
                                                                
 De Z80 kent ook nog de logische instructies.                   
 Voor de vergevorderde BASIC programmeur zijn ze                
 misschien al bekend, maar voor het gros van de MSX-ers         
 is het Latijn (of Chinees voor de Latinisten).                 
                                                                
 Het zijn:      OR,XOR en AND.                                  
                                                                
 De officieele notatie is OR s, waarbij s hetzelfde kan         
 zijn als bij CP (zie boven).                                   
                                                                
 OR B voert een OR uit op A met de inhoud van B en zet          
 het resultaat in A. De logische functies vergelijken 2         
 getallen binair bit voor bit met het volgende resultaat.       
                                                                
        A       0101 0101                                       
        s       1001 0111                                       
                                                                
        A OR s  1101 0111                                       
 De OR(of) functie maakt een bit 1 als in A of in s er 1        
 staat of als ze beide 1 zijn.                                  
 Voor AND(en) geldt dat beide bits 1 moeten zijn wil de         
 uitkomst 1 zijn.                                               
                                                                
        A       0101 1110                                       
        s       1001 0110                                       
                                                                
        A AND s 0001 0110                                       
                                                                
 De XOR (eXclusive OR) doet hetzelfde als OR, maar niet         
 als beide bits 1 zijn.                                         
                                                                
        A       1001 0101                                       
        s       0110 1110                                       
                                                                
        A XOR s 1001 1011                                       
                                                                
 Met deze instructies kun je bits testen, of setten.            
 Let op:        XOR     A geeft 0 (kijk maar na!)               
                XOR     255 Inverteer A                         
                                                                
 Voor iedereen met Natuurkunde in zijn pakket of op             
 school, dit allemaal gaat ook behandeld worden als             
 onderdeel van fysische informatica. Door er nu al mee om       
 te gaan is het dan gemakkelijker te begrijpen.                 
                                                                
 Om bits te setten of resetten zijn er ook de                   
 instructies:                                                   
                                                                
 SET en RES                                                     
                                                                
 SET b,m                                                        
                                                                
 Set het door b te specificeren bit van de door m te            
 specificeren operand.                                          
 m kan zijn: r,(HL),(IX+d) of (IY+d).                           
                                                                
        SET     1,A                                             
                                                                
 Bit 1 van register A staat nu op 1.                            
                                                                
 Bij RES geldt natuurlijk het omgekeerde. De bits worden        
 nu op 0 gezet.                                                 
                                                                
 Nog een listinkje to slot:                                     
                                                                
        ORG     #A000                                           
                                                                
        XOR     A               ; in A staat 0                  
        LD      B,A             ; in B ook                      
 LOOP:                                                          
        DEC     A               ; A wordt verlaagd (*)          
        INC     B               ; B wordt verhoogd              
        JP      NZ,LOOP         ; in A en B staat 0             
        SET     3,A             ; in A staat %0000 1000         
        SET     5,B             ; in B staat %0010 0000         
        OR      B               ; in A staat %0010 1000         
        LD      C,A             ; in C staat %0010 1000         
        LD      A,B             ; in A staat %0010 0000         
        AND     C               ; A AND C                       
        LD      (GENIC),A       ;in ZOOI komt %0010 0000        
        RET                     ; terug naar ??                 
 GENIC:                                                         
        DB      0                                               
        END                                                     
                                                                
 Opmerking: # en % staan voor resp. Hexadecimaale en            
 Binaire getallen.                                              
                                                                
 *: Als nul verlaagd wordt met 1 wordt het 255 (de              
    routine voert dus 256 x deze loop uit).                     
                                                                
 De listing slaat natuurlijk nergens op, maar we hopen          
 dat je de logische functies hierdoor een beetje beter          
 begrijpt.                                                      
                                                                
 Volgende keer wordt het misschien eens tijd voor een           
 scroll of zoiets, want nu zijn alle basis begrippen            
 besproken.                                                     
                                                                
        Veel succes en plezier namens:                          
                                        Ruud Gelissen           
                                        Jan Willem van          
                                        Helden.                 
                                                                
 Met dank aan:                                                  
                Zakboekje Z80           J.B. Vonk               
                Machinetaal Z80         J.B. Vonk en            
                                        E.J.J. Doppenberg       
                                                                
 
