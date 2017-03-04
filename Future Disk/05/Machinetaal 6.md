 Welkom bij machinetaal cursus deel 6.                          
                                                                
 Wat zullen we vandaag weer eens vertellen.                     
 Voorstel van R.G.: Hoe werkt basic                             
 J.W. : NEEEEEE !                                               
 R.G. : De stack ?                                              
 J.W. : vooruit dan maar.                                       
                                                                
 Het SP-register (STACK-POINTER).                               
                                                                
 Kort samengevat zou je het SP-register kunnen aanduiden        
 als een register dat de plaats in het geheugen aangeeft        
 waar allerlei adressen worden opgeslagen.                      
 Wat gebeurt er nu eigenlijk als je een CALL geeft ?            
 Eerst wordt het adres waar SP heen verwijst gevuld met         
 het adres waar bij een RET heen moet worden teruggekeerd,      
 dan wordt het SP-register verlaagd met 2, zodat het het        
 een nieuw adres aangeeft waar bij een nieuwe CALL het          
 adres weer kan worden opgeslagen, uiteindelijk wordt een       
 JP naar het te CALLen adres uitgevoerd.                        
                                                                
 Het SP-register kan ook worden gebruikt voor het opslaan       
 van andere registers, waarvan men later de waarde weer         
 nodig heeft.                                                   
                                                                
 Een PUSH HL zorgt er bijvoorbeeld voor dat de inhoud van       
 HL wordt opgeslagen in de geheugenplaats waarheen SP           
 wijst. M.B.V een POP HL wordt deze inhoud weer herstel         
 naar zijn oude waarde, maar een PUSH HL en een POP DE          
 mag ook best, zo kan men dus gemakkelijk HL in DE              
 "overladen".                                                   
                                                                
 Pas altijd een beetje op met het gebruik van SP !              
 Laat een stapel (STACK) nooit te veel groeien, de stapel       
 wordt van boven af naar beneden in het geheugen opgebouwd      
 en wijst bij het opstarten van je MSX meestal de boven-        
 grens van het vrije geheugen aan (ergens in de #Fxxx,          
 hangt af van b.v. het opstarten met of zonder CTRL-toest       
 ingedrukt ); zou je nu een programma maken waarbij niet        
 elke CALL wordt afgesloten met een RET zou deze stapel         
 steeds lager in het geheugen terecht komen en b.v je           
 programma kunnen overschrijden........                         
                                                                
 Let er bij het gebruik van SP verder nog op dat het            
 laatste op de stapel gekomen getal/adres er het eerste         
 er weer afgaat, programmeer dus zo :                           
                                                                
 PUSH   AF                                                      
 PUSH   HL                                                      
 PUSH   DE                                                      
 PUSH   BC                                                      
 PUSH   IX                                                      
 PUSH   IY                                                      
                                                                
 POP    IY                                                      
 POP    IX                                                      
 POP    BC                                                      
 POP    DE                                                      
 POP    HL                                                      
 PUSH   AF                                                      
                                                                
 P.S.  Dit zijn alle mogelijke vormen van PUSH en POP.          
 P.P.S Je ziet wel dat m.b.v een PUSH AF ook de                 
       vlagstatus wordt opgeslagen.                             
 P.S.S.S.De STACK wordt in BASIC o.a. gebruikt voor het         
         opslaan van terugkeeradressen bij een GOSUB            
                                                                
 TIP:                                                           
 Wil je er zeker van zijn dat de stapel bij b.v. de             
 terugkeer naar basic weer zijn oude waarde heeft (het          
 adres waar basic wordt gestart) gebruik dan:                   
                                                                
        LD     (STACK),SP      ; aan het begin                  
        LD     SP,(STACK)      ; aan het einde                  
 STACK: DW      0                                               
                                                                
                                                                
 Het R-Register.(Refres-Register)                               
                                                                
 Het R-register is een soort klokje dat razensnel tot 128       
 telt en dan weer overnieuw vanaf nul begint.                   
 Je kunt R maar op 2 manieren gebruiken :                       
                                                                
 1.     LD      A,R                                             
 2.     LD      R,A                                             
                                                                
 R kan dienen als RANDOMIZER m.b.v LD   A,R krijg je in A       
 in soort RANDOM getal, let wel op dat bit 7 altijd nul is      
 , wil je b.v. in A een RANDOM kleur gebruik je :               
                                                                
 LD     A,R                                                     
 AND    %00001111                                               
                                                                
 In A staat nu een willekeurig getal van 0 to 15.               
                                                                
 Hooks.                                                         
                                                                
 Letterlijke vertaling luidt : HAKEN en deze benadert           
 redelijk de betekenis.                                         
 Een HOOK is een opeenvolging van 5 bytes in het RAM die        
 normaal allemaal met RET (#C9) gevuld zijn.                    
 Deze "RAMHAKEN" zijn speciaal voor de programmeur              
 toegevoerd aan het werkgeheugen van BASIC en worden            
 aangeroepen op verschillende tijdstippen, afhankelijk          
 van de HOOK.                                                   
                                                                
 Als we nu die 5 bytes vervangen door andere waardes kan        
 de hook plotseling iets anders gaan doen dan alleen maar       
 te RETten, je zou er b.v. een JP naar een ander adres in       
 het geheugen kunnen zetten waar je eigen routine begint,       
 of een JP naar een BIOS routine.                               
                                                                
 Een voorbeeltje:                                               
 De ROM-routine die een teken op het scherm zet (CHPUT)         
 , geeft een CALL naar de HOOK op #FDA4 in het geheugen.        
 Stel nu dat je alle tekens die op het scherm komen ook         
 naar de printer wil sturen, de BIOS-routine die een            
 teken naar de printer stuurt is #A5 (LPTOUT).                  
 Je kunt dit dus doen door in de HOOK een JP naar #A5 te        
 zetten.                                                        
 Het JP commando bestaat uit 3 bytes.                           
 1. #C3 voor de computer aan te geven dat er een JP gaat        
    plaats vinden.                                              
 2. #xx waarbij xx staat voor het lagere deel (!) van het       
    adres waar naartoe gesprongen gaat worden                   
 3. #xx waarbij xx staat voor het hogere deel van het           
    adres waar naartoe gesprongen gaat worden.                  
 Een JP naar #00A5 moet dus zo in het geheugen gezet            
 worden: #C3 #A5 #00                                            
 M.b.v het volgende programma kan de HOOK dan worden            
 "omgebogen":                                                   
                                                                
 LD     A,#A5                                                   
 LD     (#FDA4+1),A     ; of #FDA5                              
 XOR    A               ; = LD  A,0 allen sneller               
 LD     (#FDA4+2),A                                             
 LD     A,#C3                                                   
 LD     (#FDA4),A                                               
 RET                                                            
                                                                
 Je ziet dus dat je het beste als laatste het #C3               
 commando kunt geven, omdat anders de HOOK misschien nog        
 aangeroepen kan worden voordat je het juiste adres hebt        
 neergezet en de computer dan een JP #C9C9 zou uitvoeren.       
                                                                
 Tot zover deze machinetaalcursus, volgende keer zullen         
 we je o.a. uitleggen hoe het INTERUPT-systeem van je MSX       
 in elkaar zit en hoe je dit m.b.v. een HOOK kunt               
 gebruiken voor talrijke doeleinden.                            
 Nu zullen je we alleen nog even zeggen welke HOOKs je          
 hiervoor kunt gebruiken: #FA9A of #FD9F (je ziet 5 bytes       
 verder in het geheugen.                                        
 Een andere HOOK die we nog even wilden noemen is:              
 #FDD1, die wordt aangeroepen aan het begin van de              
 routine die enkelvoudige toetsaanslagen verwerkt.              
 M.b.v. deze HOOK kun je b.v. een toetsaanslag voor een         
 ander doeleinde gebruiken.                                     
                                                                
 Dat was het dan weer (ja, ik begin nu niet weer met een        
 heel verhaal onder het motto: dat wilden we nog even           
 zeggen Jan-Willem.), niet meer deze keer.                      
                                                                
 Veel programmeerplezier : Jan-Willem van Helden                
                           Ruud Gelissen.                       
                                                                
 p.s. leest iemand ooit deze tekst ?  
 
 
