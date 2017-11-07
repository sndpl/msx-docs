 MACHINETAALCURSUS 8                                            
                                                                
 Het schijnt dat de lezers deze rubriek niet interessant        
 genoeg vinden......                                            
 Machinetaal is wel leuk. maar niet als je alleen je            
 optelsommetjes ermee kunt uitvoeren. Dat kun je ook in         
 BASIC. Ik hoop dat jullie er echter begrip voor hebben         
 dat je zonder de basisbeginselen niet echt iets leuks          
 kunt doen.                                                     
 We hopen dat je ondanks dit toch doorzet want dit is het       
 waard !!! Zo, en nu maar eens een beetje interessanter         
 proberen te doen.....                                          
                                                                
 Sprites in machinetaal.                                        
                                                                
 In navolging van de BASIC-cursus volgt hier de besturing       
 van sprites in machinetaal                                     
 (p.s. Tom, dit gaat echt veel sneller in machinetaal).         
                                                                
 Het definieren van een Sprite patroon (de vorm) is             
 vrijwel gelijk aan de methode die in basic wordt               
 gebruikt. Persoonlijk gebruiken we liever de T&E-soft          
 sprite-editor SPEN, omdat je m.b.v het "MAKE2" commando,       
 eenvoudig meerkleurige sprites in data kunt omzetten die       
 je gewoon in je programma kunt gebruiken.                      
                                                                
 HET SPRITEPATROON                                              
                                                                
 Hier volgt een voorbeeld van een 8*8 sprite                    
                                                                
 SPRPAT:db     %01100110                                        
        db     %01100110                                        
        db     %01100110                                        
        db     %01111110                                        
        db     %01100110                                        
        db     %01100110                                        
        db     %01100110                                        
        db     %01100110                                        
                                                                
 Een spritepatroon definier je regel voor regel.                
 Elke regel stel een lijn van je sprite voor.                   
 Een ��n (staat hier 1 Kubie?) stel een puntje voor, een        
 nul is een doorzichtbare plaats in je sprite.                  
                                                                
 Met de onderstaande routine kun je de spritemode op 8*8        
 zetten:                                                        
                                                                
 WRTVDP:EQU     #47             ; SCHRIJF VDPREGISTER           
                                                                
        LD      A,(#F3E0)       ; VDP REGISTER 1                
        AND     253             ; RESET BIT 1                   
        LD      B,A                                             
        LD      C,1                                             
        CALL    #48                                             
                                                                
 Alles wat sprites betreft staat in het VRAM (videoram).        
 Je kunt de Spritepatronen in het VRAM zetten m.b.v.            
 onderstaande routine:                                          
                                                                
 LDIRVM:EQU     #5C             ; LDIR NAAR VRAM                
                                                                
        LD      HL,SPRPAT                                       
        LD      DE,#7800                                        
        LD      BC,8*8                                          
        CALL    LDIRVM                                          
        RET                                                     
                                                                
 SPRPAT:db     %01100110                                        
        db     %01100110                                        
        db     %01100110                                        
        db     %01111110                                        
        db     %01100110                                        
        db     %01100110                                        
        db     %01100110                                        
        db     %01100110                                        
                                                                
 64 bytes (in registerpaar BC) worden door Ldir VRAM in         
 het Videogeheugen gezet op adres #7800.                        
 Hier begint standaart de Spritepatroontabel.                   
 Deze tabel loopt tot #7FFF, dit houdt in dat je maximaal       
 256 8*8 sprites kunt definieren of 64 16*16 sprites.           
 Zodra deze routine uitgevoerd is staat in het VRAM je          
 spritepatroon.                                                 
                                                                
 DE SPRITECOLOR                                                 
                                                                
 Je kunt een sprite per horizontale lijn een kleur geven.       
 Alle "eenjes" in je patroon krijgen nu deze kleur.             
 M.b.v. de volgende routine kun je dit bewerkstelligen:         
                                                                
 WRTVDP:EQU     #47             ; SCHRIJF VDPREGISTER           
 LDIRVM:EQU     #5C             ; LDIR NAAR VRAM                
        LD      HL,SPRCOL                                       
        LD      DE,#7400                                        
        LD      BC,16                                           
        CALL    LDIRVM                                          
                                                                
                                                                
 Naar het VRAM op adres #7400 worden nu de spritekleuren        
 geschreven. Je moet er rekening mee houden dat de              
 kleuren van de 2de sprite pas op adres #7410 moeten            
 komen te staan (dat is 16 verder !! en heeft te maken          
 met 16*16 sprites waarbij je 16 bytes voor de kleuren          
 nodig hebt.).                                                  
 De kleurnummers stemmen overeen met de paletkleuren, je        
 krijgt dus nu een half rode, half blauwe sprite.               
                                                                
 Zo, kleuren en patronen staan in het Vram, nu onze             
 sprite nog besturen.                                           
                                                                
 LDIRVM:EQU     #5C             ; LDIR NAAR VRAM                
                                                                
        LD      HL,SPRATT       ; ATTRIBUTEN                    
        LD      DE,#7600                                        
        LD      BC,4                                            
        CALL    LDIRVM                                          
        RET                                                     
                                                                
 SPRATT:DB      100,128,0,0                                     
                                                                
 Na het uitvoeren van dit programmatje zien we een sprite       
 verschijnen op het beeldscherm.                                
 De y-coordinaat is 100                                         
 De x-coordinaat is 128                                         
 In de 3de byte staat het spritenummer, in dit geval 0,         
 dat houdt in dat het eerste spritepatroon op het               
 beeldscherm verschijnt. Om de 2de sprite te zien te            
 krijgen moet je hier een 1 neerzetten.                         
 Let goed op, je krijgt nu alleen het 2de spritepatroon         
 te zien en niet de 2de spritecolors.                           
 De spritecolors zijn namelijk verbonden aan de                 
 attribuuttabel. Bij de eerste 4 bytes in de                    
 spriteattribuuttabel horen dus de eerste 8 (eigenlijk          
 16) bytes in de spritecolortabel.Bij de tweede 4 bytes         
 in de spriteattribuuttabel horen dus de tweede 16 bytes        
 in de spritecolortabel enz. (#7400+16)                         
                                                                
 Nu volgt dan een uitgebreider voorbeeld van de besturing       
 van 2 sprites.                                                 
                                                                
 WRTVDP:EQU     #47             ; SCHRIJF VDPREGISTER           
 LDIRVM:EQU     #5C             ; LDIR NAAR VRAM                
        DB      #FE                                             
        DW      ST,EN,ST                                        
                                                                
        ORG     #A000                                           
 ST:    LD      A,(#F3E0)       ; VDP REGISTER 1                
        AND     253             ; RESET BIT 1                   
        LD      B,A                                             
        LD      C,1                                             
        CALL    #48             ; SPRITES 8*8                   
        LD      HL,SPRPAT       ; PATRONEN IN VRAM              
        LD      DE,#7800                                        
        LD      BC,8*8*2                                        
        CALL    LDIRVM                                          
        LD      HL,SPRCOL       ; COLORS IN VRAM                
        LD      DE,#7400                                        
        LD      BC,16*2                                         
        CALL    LDIRVM                                          
 LOOP:                                                          
        XOR     A               ; SPATIE ?                      
        CALL    #D8                                             
        RET     NZ              ; JA, STOP                      
        LD      HL,(TABPNT)     ; HAAL YCO. UIT TABEL           
        LD      A,(HL)                                          
        LD      B,A             ; BEWAAR TABELWAARDE            
        INC     A                                               
        CALL    Z,REPEAT        ; 255 ? JA, HERHAAL             
        INC     HL              ; VOLGENDE YCO.                 
        LD      (TABPNT),HL                                     
        LD      (SPRATT),A      ; ZET YCO. SPRITE 1 GOED        
        LD      A,(SPRATT+1)                                    
        ADD     A,4                                             
        LD      (SPRATT+1),A    ; VERHOOG XCO. SPRITE 1         
        LD      (SPRATT+4),A    ; ZET YCO. SPRITE 2             
        LD      A,B             ; ZET XCO. SPRITE 2             
        LD      (SPRATT+5),A                                    
        EI                                                      
        HALT                    ; WACHT OP INTERUPT             
        LD      HL,SPRATT       ; ATTRIBUTEN                    
        LD      DE,#7600                                        
        LD      BC,4*2                                          
        CALL    LDIRVM                                          
        JP      LOOP                                            
                                                                
 REPEAT:LD      HL,TABEL        ; START TABEL                   
        LD      A,(HL)                                          
        LD      B,A                                             
        RET                                                     
                                                                
 TABPNT:DW      TABEL                                           
 TABEL: DB      10,11,13,15,18,21,26,31,37,43                   
        DB      43,37,31,26,21,18,15,13,11,10,255               
 SPRATT:DB      0,0,0,0                                         
        DB      16,50,1,0 ; SPRITE NUMMER 1                     
                                                                
 SPRCOL:DB      15,15,15,15,15,15,15,15                         
        DB      00,00,00,00,00,00,00,00 ; DUMMIES               
        DB      15,15,15,15,15,15,15,15                         
        DB      00,00,00,00,00,00,00,00 ; WORDT NIET HEEN       
                                        ; GEKEKEN               
 SPRPAT:db      %11001100                                       
        db      %00100100                                       
        db      %01111110                                       
        db      %01101011                                       
        db      %11111111                                       
        db      %11011101                                       
        db      %01100011                                       
        db      %00111110 ; EINDE EERSTE SPRITE                 
                                                                
        db      %01100110 ; 2DE SPRITE                          
        db      %01100110                                       
        db      %01100110                                       
        db      %01111110                                       
        db      %01100110                                       
        db      %01100110                                       
        db      %01100110                                       
        db      %01100110                                       
                                                                
 EN:    END                                                     
                                                                
 Voer dit maar eens uit en als het goed is zul je 2             
 sprites zien bewegen. Je kunt natuurlijk b.v. een              
 sinustabel in basic in het geheugen "poken" enz.               
                                                                
 Tot zover deze keer, voor special requests wendt je tot        
 Ray Cokes of Ruud Gelissen.                                    
 Ajuu J.W. en Ruud. Aleih gank maar es get angers leze,         
 manne.(Vert.: Wilt U alstublieft een andere tekst gaan lezen)  
