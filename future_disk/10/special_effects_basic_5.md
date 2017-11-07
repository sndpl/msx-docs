                SPECIAL EFFECTS BASIC 5 ???                     
                ---------------------------                     
                                                                
 Helaas , maar deze keer geen SEB...... Dit zou een dubbel      
 jubileum moeten worden (FD 10 en SEB 5) maar ik heb geen       
 tijd gehad om de nodige voorbeelden te schrijven en me te      
 verdiepen in het hoe en wat om de computer te laten            
 "denken" (dit zou dus het onderwerp zijn.) en VECTOR MANIA     
 (zie elders) nam (en neemt) veel tijd in beslag.               
                                                                
 Maar niet getreurd!!(NvdR. Nee, natuurlijk niet!!) Op deze     
 disk staat de goede versie van de SEB manual. Hoe u deze       
 ingepakte file moet uitpakken ziet u in de file "SEBPATCH.LDR" 
 die u kunt laden vanuit het software menu.                     
                                                                
 De volgende keer dus een (lange) SEB over het denken van de    
 computer en om deze tekst toch niet heel afgezaagd te maken    
 is hieronder toch nog een (mini) SEBje ....                    
                                                                
                                                                
                     SEB vier drievierde                        
                     -------------------                        
                                                                
 Deze keer alleen de behandeling van de twee moeilijkste        
 diskbasic commando's :                                         
                                                                
                       DSKI$ en DSKO$                           
                                                                
 en de RAMdisk van de MSX wordt eens besproken.                 
                                                                
                                                                
                            DSKI$                               
                            -----                               
 Dit is een afkorting van DISK INPUT (STRING). Met dit          
 commando kan er informatie van een disk worden gelezen. De     
 gehele syntax is :                                             
                                                                
 A$=DISK$(A,B)  A is het nummer van de drive en B is het        
                sectornummer. Ik ga nu niet verder in op de     
                indeling van sectors, tracks etc. Kijk eens in  
                een MCCM, daar staan vaak teksten over de       
                opbouw van een disk. B moet positief zijn en    
                mag niet groter dan 1440 worden voor            
                gewone 80 tracks diskette. Voor een harddisk    
                kan dit getal varieren van 0 to 65535. Om A     
                uit te "rekenen" kan de volgende regel worden   
                ingetypt :                                      
                                                                
 PRINT ASC("A:")-64 'VUL VOOR A: DE DRIVE IN                    
  1                                                             
                                                                
 Als u dus van drive B: wilt lezen moet A 2 zijn.               
                                                                
 De informatie van de gewenste SECTOR staat NIET (!) in A$,     
 maar wordt opgeslagen in het RAM. Om deze te lezen moet u      
 het volgende programma in typen :                              
                                                                
 10 A$=DSKI$(1,0)'DRIVE A SECTOR 0                              
 20 A=PEEK(&HF351)+256*PEEK(&HF352) 'HIER STAAT HET ADRES VAN   
    DE BEGINPLAATS VAN DE GELEZEN DATA                          
 30 FOR I=0 TO 100 '100 KARAKTERS ZOEKEN, DIT KAN VERANDERD     
    WORDEN.                                                     
 40 PRINT CHR$(PEEK(A+I)); : NEXT I 'LAAT HET KARAKTER ZIEN     
    WAT OP DE DISK OOK STAAT.                                   
 50 END                                                         
                                                                
 U ziet dat dit erg omslachtig is, maar in A$ zou het niet      
 passen, omdat in een string nu eenmaal maar 255 karakters      
 kunnen en in een sector 512.                                   
                                                                
                            DSKO$                               
                            -----                               
                                                                
 Om op een disk iets rechtstreeks te schrijven is er het        
 commando DSKO$. Dit is precies hetzelfde als DSKI$. Maar de    
 syntax is nu :                                                 
                                                                
 DSKO$(A,B)  Waarbij A en B hetzelfde betekenen als bij         
             DSKI$. Dit commando schrijft de informatie van     
             dezelfde adressen waar bij DSKI$ van werd          
             gelezen naar disk.                                 
             Als u deze informatie veranderd, kunt u dus ook    
             zelf iets op de sectors schrijven.                 
                                                                
 In het onderstaande voorbeeldje wordt de naam van deze cursus  
 op sector 20 van drive B: geschreven. (LET OP , in sector 0-   
 17 staat informatie van de disk. Gebruik bij experimenteren    
 een lege disk, anders zou het wel eens verkeerd met bv. de     
 FAT kunnen aflopen !)                                          
                                                                
 10 A$="SPECIAL EFFECTS BASIC NUMMER VIER DRIE VIERDE"          
 20 A=PEEK(&HF351)+256*PEEK(&HF352)                             
 30 FOR I=1 TO LEN(A$) 'POKE A$ OP ADRES A EN VOLGENDE          
 40 POKE A+(I-1),ASC(MID$(A$,I,1))                              
 50 NEXT I                                                      
 60 DSKO$(2,20) 'SCHRIJF OP SECTOR 20 VAN DRIVE B               
 70 END                                                         
                                                                
 Als u nu het vorige programmaatje aanpast om van drive B op    
 de 20e sector te lezen (A$=DSKI$(2,20)) dan zult u zien dat    
 er de tekst van regel 10 in de 2e listing verschijnt.          
 Experimenteer gerust , maar wel met een lege disk !!!          
 (Wij zijn niet aansprakelijk voor de gevolgen, van onkundig    
 gebruik!)                                                      
                                                                
                           RAMDISK                              
                           -------                              
                                                                
 Jazeker , ook u MSX 2 heeft een RAMDISK ! Maar helaas is       
 deze niet zo uitgebreid als bv. de RAMDISK (H:) van de         
 Dos-2 Cartridge.                                               
 Er zijn een aantal commando's speciaal voor de RAMDISK, maar   
 er zijn ook veel gebreken. Op het laatste komen we later nog   
 even terug.                                                    
                                                                
 CALL MEMINI   Dit "maakt" de RAMDISK aan. Als het goed is      
               veschijnt er als u RETURN drukt :                
                                                                
 32000 bytes allocated                                          
                                                                
 U ziet dat deze RAMDISK 32000 bytes (zo'n 31 KB.) vrij         
 maakt. Hierop kunt u laden en saven door simpel i.p.v. het     
 disknummer MEM: te typen. Als u bv. de file NAAM.EXT op de     
 RAMDISK wilt saven typt u :                                    
                                                                
 SAVE"MEM:NAAM.EXT"                                             
                                                                
 Dit is precies hetzelfde met laden. Maar als u de              
 inhoudsopgave wilt zien, dan gaat dit NIET met                 
 FILES"MEM:*.*" !!! Daarvoor is een appart commando evenals     
 KILL en NAME :                                                 
                                                                
 CALL MFILES  Laat u de inhoudsopgave van de RAMDISK zien       
 CALL MKILL("<naam>")  Verwijderd een bestand van de RAMDISK    
 CALL MNAME("<naam>"as"<naam>") NAMED een bestand van de        
                                RAMDISK.                        
                                                                
 LET OP DE HAAKJES BIJ CALL MKILL EN CALL MNAME !!!!!!!!!!!!    
                                                                
 Dit zijn alle commando's die er zijn. Om tijdelijk BASIC       
 programma's te saven is dit een goede oplossing maar na een    
 RESET is alles weg !!!!!!                                      
                                                                
 Toch is het niet allemaal rozegeur en maneschijn wat deze      
 RAMDISK betrefd. Als we een ML file willen laden of saven      
 dan gaat dit niet om de simpele reden dat je geen MEM: kunt    
 typen bij het BLOAD-en BSAVE commando (er verschijnt           
 dan een foutmelding).                                          
                                                                
 Maar nog een groter nadeel is het ontbreken van MEM: bij       
 het COPY commando. Door deze twee gebreken is het dus          
 onmogelijk om bv. grafische files te laden en saven en er      
 kunnen ook geen files effe gecopi erd worden. Daardoor wordt   
 deze RAMDISK niet vaak gebruikt en daarom wordt er ook niet    
 vaak over gesproken. Maar voor de basicprogrammeur is deze     
 RAMDISK toch wel handig. SAVEN en LADEN gaat immers wel en     
 ook het MERGE commando ondersteund deze interne disk. (NvdR.   
 Saven en Laden, gaat wel verschrikkekekeklijk langzaam!)       
                                                                
 Let er wel op dat er altijd in het begin CALL MEMINI moet      
 worden gedaan !!                                               
                                                                
 P.S. I.p.v. CALL kan ook het kortere _ (laag streepje ,        
 <SHIFT>+<->) worden gebruikt.                                  
                                                                
                                                                
 Dit was het weer voor deze keer. De volgende keer zoals        
 beloofd de echte SEB 5 met veel voorbeelden. Bij deze SEB      
 zijn geen voorbeelden (ook niet echt nodig, geloof ik)(NvdR:   
 geloven doe je in de kerk, Tom!)                               
                                                                
 Tot de volgende keer ,                                         
                                                                
                                               TOM WAUBEN   
