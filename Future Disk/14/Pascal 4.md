                                                                
                           PASCAL (4)                           
                           ==========                           
                                                                
 Programmeren is masochisme, maar wat is het  lekker(NvdR:??).  
 Nou  ja, als je aan het programmeren BENT niet, maar als het   
 werkt en die kl*t* machine doet eindelijk precies wat JIJ      
 wilt, dat is pas een hoogtepunt. Waarom vertel ik dit          
 allemaal? Als eerste omdat ik een reactie van Koen wil         
 uitlokken  (h�  etter, waar ben je)(NvdR: hiero!) en als       
 tweede een meer belangrijke reden (Koen is tenslotte niet      
 belangrijk)(NvdR: moet je iemand horen!) om je te stimuleren   
 om toch  vooral door te gaan, want uiteindelijk lukt het       
 wel[ (dat  MSX'je  kan veel meer dan je denkt).                
                                                                
 Deze vierde aflevering van mijn PASCAL cursus is gewijd aan    
 PROCEDURE's en FUNCTION's. In elke programmeertaal ontstaat op 
 een bepaald moment behoefte aan funkties en procedures.        
 Waarom? Nou, gewoon, omdat je geen zin hebt om een bepaald     
 stuk code dat je vaker nodig hebt (bijvoorbeeld een invoer     
 routine) ook vaker in te tikken. Als je het geheel ��n keer    
 hebt ingetikt, dan wil je het vaker gebruiken, niet alleen     
 omdat je maar ��n keer wil tikken, maar ook omdat dat stuk het 
 doet en je bij overtikken fouten kunt maken.                   
 Wat is nu het verschil tussen FUNCTION en PROCEDURE? Het       
 verschil zit 'm in de uitkomst. FUNCTION heeft een uitkomst,   
 PROCEDURE niet. Een heel eenvoudige FUNCTION ziet er als volgt 
 uit:                                                           
                                                                
   FUNCTION Optel(Getal1:integer;Getal2:integer):integer;       
    BEGIN   (* Optel *)                                         
     Optel := Getal1 + Getal2;                                  
    END;    (* Optel *)                                         
                                                                
 Bovenstaande funktie is dus niets. Je zou hem nooit gebruiken  
 in een programma, omdat optellen al standaard aanwezig is. Wel 
 zou je een funktie schrijven om machten te berekenen, omdat    
 dat niet standaard in PASCAL aanwezig is. Het voorbeeld geeft  
 wel duidelijk de struktuur van een funktie weer.               
 Als eerste staat het gereserveerde woord FUNCTION, waarna de   
 naam van de funktie volgt (let op; gebruik hiervoor geen       
 gereserveerde woorden!). Tussen haakjes volgen de variabelen   
 die je nodig denkt te hebben in de funktie tesamen met hun     
 type (dit stuk is niet verplicht, omdat je niet altijd         
 variabelen nodig hebt, denk hierbij aan een random funktie).   
 Als laatste volgt er nog het type van de uitkomst van de       
 funktie.                                                       
                                                                
 Na deze eerste regel volgt de body van de functie, die         
 overigens precies gelijk is aan de body van een programma      
 (ook hier kun je variabelen declareren en andere funkties of   
 procedures maken). In het code gedeelte is slechts een ding    
 verplicht en dat is het toekennen van een waarde aan de        
 funktie (in het voorbeeld "Optel := Getal1 + Getal2"). Doe     
 je dit niet, dan krijg je gegarandeerd een foutmelding.        
 Een klasieke fout is overigens het volgende:                   
                                                                
   Optel := 0;                                                  
   FOR Teller := 1 TO 10 DO                                     
    Optel := Optel + Getal[Teller];                             
                                                                
 Hiermee zou je een ARRAY kunnen optellen (denk je), maar het   
 is verboden om de naam van de funktie (Optel) rechts van het   
 toekenningsteken te plaatsen en dus werkt het niet. Om het wel 
 aan de praat te krijgen zul je een variabele "Som" moeten      
 declareren en het volgende moeten doen:                        
                                                                
   Som := 0;                                                    
   FOR Teller := 1 TO 10 DO                                     
    Som := Som + Getal[Teller];                                 
   Optel := Som;                                                
                                                                
 Zo, nu is de uitleg van een procedure een makkie (dus niet).   
 Eerst een voorbeeld:                                           
                                                                
   PROCEDURE Uitkomst(Getal:integer);                           
                                                                
    BEGIN   (* Uitkomst *)                                      
     Writeln('De uitkomst is : ',Uitkomst);                     
    END;    (* Uitkomst *)                                      
                                                                
 Alweer zo'n gemakkelijk voorbeeldje (anders begint Koen te     
 klagen dat 'ie 't nie begrijp, eikel!).(NvdR: leer jij maar    
 eens een tekstroutine programmeren!) Je ziet dat de struktuur  
 nagenoeg gelijk is.                                            
 Een procedure heeft geen uitkomst en dat blijkt uit de eerste  
 regel (heading). Verder mag de procedure geen waarde krijgen   
 (Uitkomst := 10 is dus uit den boze). Voor de rest is alles    
 gelijk (er mogen dus ook variabelen gedeclareerd en funkties   
 of procedures gemaakt worden).                                 
                                                                
 Je weet nu bijna alles van funkties en procedures, ik moet     
 alleen nog parameter variabelen uitleggen. Bij een funktie of  
 procedure geef je gewoonlijk ��n of meer variabelen mee,       
 alleen, je geeft ze niet mee, je copieert ze. Wat ik bedoel    
 is, dat de waarde die een variable bezit bij aanroep van de    
 funktie of procedure, wordt gecopieerd. Aanchouw het volgende  
 voorbeeld:                                                     
                                                                
   FUNCTION T1(Getal1:integer;Getal2:integer):integer;          
                                                                
    BEGIN   (* T1 *)                                            
     T1 := Getal1 + Getal2;                                     
     Getal1 := Getal1 * 2;                                      
    END;    (* T1 *)                                            
                                                                
 En vervolgens in het hoofdprogramma de volgende opdrachten:    
                                                                
   A := 10;                                                     
   B := 20;                                                     
   Iets := T1(A,B);                                             
                                                                
 Deze funktie heeft value parameters, wat betekent dat bij de   
 aanroep wordt gekeken wat de waarde van de formele parameters  
 (A en B) is en die gecopieerd wordt naar de actuele parameters 
 (Getal1 en Getal2). De uitkomst is (hoe kan het anders) 30. De 
 vraag is echter, wat wordt A? In dit geval veranderd A NIET (A 
 blijft 10). De copie (Getal1) verandert, maar de formele       
 parameter (A) blijft gelijk, er bestaat geen link. Daarom      
 noemt men dit value parameters ofwel waarde parameters, alleen 
 de waarde wordt doorgegeven.                                   
 De andere variabelen zijn parameter variabelen. Een funktie of 
 procedure met parameter variabelen ziet er als volgt uit:      
                                                                
   FUNCTION T2(VAR Getal1:integer;Getal2:integer):integer;      
                                                                
    BEGIN   (* T2 *)                                            
     T2 := Getal1 + Getal2;                                     
     Getal1 := Getal1 * 2;                                      
    END    (* T2 *)                                             
                                                                
 Herhalen we nu de aanroep, dan veranderd A wel in 20. A wordt  
 nu namelijk als referentie meegegeven. Alle bewerkingen op     
 Getal2 vinden nu rechtstreeks plaats op A.                     
                                                                
 Zo, dat was alles wat ik over funkties en procedures kwijt     
 wilde. Als oefening deze keer kun je  alle  voorgaande         
 oefeningen opnieuw doen, alleen met dit verschil dat je in     
 het hoofdprogramma alleen maar aanroepen doet naar funkties    
 of procedures. Oh ja, natuurlijk niet maar een procedure       
 waarin het hele hoofdprogramma gecopieerd wordt (dan heeft     
 het oefenen nog geen nut). Oefen trouwens ook lekker veel      
 met het verschil tussen parameter en value parameters.         
                                                                
 Groetjes,                                                      
                                                                
                                      Jeroen 'KUBIE' Smael      
                                                                
                                                    C YA @!!    

