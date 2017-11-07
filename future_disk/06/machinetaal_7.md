                                                                
 Machinetaal Cursus Deel 7                                      
                                                                
 Hoera, 1 jaar FutureDisk. Dat betekent ook 1 jaar de           
 machinetaal cursus. We hopen dat je er iets aan gehad          
 hebt en nu toch al een beetje gevorderd bent.                  
                                                                
 Deze aflevering bekijken we eens de interrupt hook.            
                                                                
 In BASIC bestaat het commando "ON INTERVAL GOSUB". Dit         
 commando zorgt ervoor dat er om een bepaalde tijd naar         
 een programma stukje gesprongen wordt en het hoofd-            
 programma onderbroken wordt. Na afhandeling van de             
 routine wordt het hoofdprogramma weer aangesproken             
 totdat er weer een INTERVAL optreed.                           
                                                                
 Nu in machinetaal is het ook niet anders. Alleen bestaan       
 er hier geen commando's voor. We geven even een                
 voorbeeld.                                                     
                                                                
 Stel je wilt een scroll die voortdurend doorgaat (De           
 PLUTO demo) of een muziekroutine die regelmatig speelt.        
 Door nu op #FD9F een JP (jouw routine) te zetten kun je        
 er zeker van zijn dat om de 50/60 seconden (hangt af van       
 je interruptfrequentie) deze routine wordt aangegeven.         
                                                                
 Een voorbeeldje om een routine op interrupt te zetten:         
                                                                
        LD      A,#C3           ;de JumP instruktie             
        LD      HL,CODE         ;jouw routine adres             
        DI                      ;zet de interrupt uit !         
        LD      (#FD9F),A       ;plaats op H_TIMI               
        LD      (#FDA0),HL      ;plaats op H_TIMI+1             
        EI                      ;Enable Interrupt               
                                                                
 Na de EI zal je programma bij elke interrupt naar je           
 routine springen (het adres CODE).                             
                                                                
 Ook bestaat de H_KEYI, #FD9A nog. Hook #FD9A wordt veel        
 vaker aangeroepen dan H_TIMI (precieze frequentie              
 onbekend). Deze hook moet je dan ook gebruiken voor            
 screensplits etc. Voor muziekroutines, scrolls e.d.            
 gebruik je gewoon H_TIMI.                                      
                                                                
 Deze beide hooks zijn allebei uit te schakelen door het        
 gebruik van DI (Disable Interrupt) en in te schakelen          
 door EI (Enable Interrupt).                                    
 Zodra de MSX een interrupt krijgt (de Z80 zorgt hiervoor       
 ) springt de computer naar adres #38. Normaal staat hier       
 BIOS en worden dus H_KEYI en H_TIMI afgehandeld.               
 Gevorderde programmeurs schakelen echter de BIOS weg           
 (ANMA bijvoorbeeld) en plaatsen op #38 hun eigen               
 interrupt afhandeling. Omdat RAM sneller is dan ROM zijn       
 er nu enkele mogelijkheden meer want de ROM afhandeling        
 is nogal erg traag. Maar dit soort truuks zijn alleen          
 nodig als je een megademo met vele screensplits,               
 line-interrupts etc. wilt maken. Maar dat is voor de           
 gevorderde.                                                    
                                                                
 Nu dacht men eind jaren '70 bij Zilog inc. dat er              
 behoefte was aan meerdere interrupt modes namelijk:            
                                                                
 Interrupt Mode 0 (IM0)                                         
 Na een interrupt-aanvraag wacht de Z80 tot het                 
 randapparaat een instructie op de databus plaatst. Deze        
 instruktie wordt uitgevoerd. Een CALL of RST geeft een         
 sprong naar een subroutine die met RETI moet worden            
 afgesloten.                                                    
 Deze interrupt mode heb ik nog nooit gebruikt en ik            
 twijfel dan ook sterk of die ook wel in het MSX systeem        
 ondersteund wordt. Wie het weet mag bellen.                    
                                                                
 Interrupt Mode 1 (IM1)                                         
 De Z80 geeft een RST #38 (call #38). Dit is dus de             
 normale interrupt mode waarop de MSX standaard staat.          
                                                                
 Interrupt Mode 2 (IM2)                                         
 (er staat in m'n boekje een lap tekst van 10 regels met        
 onbegrijpelijke taal. Aangezien ik deze Interrupt Mode         
 ook nog nooit heb zien worden toegepast plaats ik dit          
 verhaal niet. Voor geinterreseerden: Koop een boek over        
 de Z80).                                                       
                                                                
 Al deze modes zijn te schakelen met de commando's IM0,         
 IM1 en IM2.                                                    
                                                                
 Ook genereert de Z80 nog een nietmaskeerbare interrupt         
 (NMI). Een interne flipflop wordt gereset bij de               
 aanvraag van een NMI. Na elke instructie wordt deze            
 getest. Bij een positief springt de Z80 naar adres #66.        
 In de normale BIOS staat hier de afhandeling voor het          
 MSX systeem.                                                   
 Ook bestaat de BUSREQUEST, maar die wordt op MSX niet          
 ondersteund.                                                   
                                                                
 Zo nu weet je toch wel alles wat er aan interrupts op de       
 MSX te vertellen valt. Ruud had vorige keer de overige         
 Hooks al beschreven, dus daar verwijs ik graag naar.           
                                                                
 Tot slot nog een voorbeeldje:                                  
                                                                
        DB      #FE             ;Header voor GEN80              
        DW      ST,EN,ST        ;(bij WBASS2 niet nodig)        
                                                                
        ORG     #C000                                           
ST:                                                             
        LD      HL,#FD9F        ;oude Hook bewaren              
        LD      DE,HOOKST                                       
        LD      BC,5                                            
        LDIR                    ; zie uitleg onder              
        LD      A,#C3                                           
        LD      HL,INTERR       ;nieuwe Hook neerzetten         
        DI                                                      
        LD      (#FD9F),A                                       
        LD      (#FDA0),HL                                      
        EI                                                      
 LOOP:                                                          
        XOR     A                                               
        CALL    #D8             ;wachten op spatie              
        JR      Z,LOOP          ;NZ is spatie                   
                                                                
 RESINT:                                                        
        LD      HL,HOOKST                                       
        LD      DE,#FD9F                                        
        LD      BC,5                                            
        DI                                                      
        LDIR                    ;herstel hook                   
        EI                                                      
        RET                                                     
 INTERR:                                                        
        LD      HL,(TXTPNT)                                     
        LD      A,(HL)                                          
        CP      "@"                                             
        CALL    Z,ENDTXT        ;kijk einde tekst               
        INC     HL                                              
        LD      (TXTPNT),HL                                     
        JP      #A2             ;Print ASCII waarde(BIOS)       
                                ;(de RET aan het einde          
                                ; van deze routine keert        
                                ; van de interrupt terug)       
 ENDTXT:                                                        
        LD      HL,TEXT                                         
        LD      (TXTPNT),HL                                     
        LD      A,(HL)                                          
        RET                                                     
 HOOKST:                                                        
        DS      5               ;opslag hook                    
 TXTPNT:                                                        
        DW      TEXT            ;Pointer                        
 TEXT:                                                          
        DB      "Hallo Boppers, dit is een voorbeeld van"       
        DB      "een tekst die eeuwig doorgaat totdat je"       
        DB      "op de spatiebalk ramt. Dit staat op in-"       
        DB      "terrupt. Voor meer info zie de Future "        
        DB      "Disk.......... Vaarwel Moeder !@"              
 EN:    END                                                     
                                                                
 Uiteraard zijn wij niet verantwoordelijk voor de               
 gevolgen van bovenstaande tekst.                               
                                                                
 Het enigste nieuwe commando dat we hier gebruikt hebben        
 is het LDIR commando.                                          
 M.b.v. dit commando kan men gemakkelijk delen van het          
 geheugen kopieren.                                             
 De officiele uitleg is :                                       
                                                                
 LDIR: LoaD Increment Repeat                                    
       Laadt de inhoud van de geheugen locatie waarnaar         
       registerpaar HL wijst in de geheugenlocatie              
       waarnaar registerpaar DE wijst. Daarna worden de         
       inhouden van HL en DE met 1 verhoogt en die van          
       teller BC met 1 verlaagd.                                
       Als de inhoud van BC hierna niet nul is herhaalt         
       de Z80 de instructie.                                    
                                                                
 Soortgelijke commando's zijn:                                  
                                                                
 LDI:  LoaD Increment                                           
       Deze instructie doet precies hetzelfde als LDIR          
       met het verschil dat de instructie niet herhaald         
       wordt.                                                   
                                                                
 LDDR: LoaD Decrement Repeat                                    
       Ook hetzelfde als LDIR, maar HL en DE worden             
       verlaagd. Bij deze instructie staat in HL dus            
       bovengrens van het te verplaatsen gebied.                
                                                                
 LDD:  LoaD Decrement                                           
       Valt te vergelijken met LDDR, maar wordt weer niet       
       herhaald.                                                
                                                                
 Met deze cursi hopen we je wat achtergrond                     
 informatie te geven, maar echt leren doe je alleen door        
 zelf te doen !                                                 
                                                                
 Volgende keer gaan we eens wat rekenen en nog wat dinge.       
                                                                
 Veel programmeerplezier namens:                                
                                Ruud Gelissen en                
                                Jan Willem van Helden           
                                                                
 Voor vragen zie de helplines.                                  
 (voor aanvragen bel ook gerust eens de programmeerlijn!)   
