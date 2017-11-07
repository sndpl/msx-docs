Welkom bij alweer de vierde aflevering van de                  
 machinetaalcursus.                                             
                                                                
 Nadat we de vorige keren Z80 machinetaal behandelden           
 zullen we nu eens echt met MSX machinetaal beginnen.           
                                                                
 De eerste MSXjes hadden meestal 32 KROM en 16 KRAM tot         
 hun beschikking, in de latere modellen werd dit steeds meer.   
 De Z80 kan eigenlijk niet meer dan 64K tegelijk besturen.      
 Ho, zegt de Memorymapper bezitter : Wat moet ik dan met mijn   
 overige Krammetjes. Wel, luidt het antwoord, de Z80 kan niet   
 meer dan 64K TEGELIJK besturen, met behulp van slotschakeling  
 kan de Z80 veel meer geheugen aanspreken.                      
 Aangezien het slotschakelen redelijk ingewikkeld is en voor    
 de beginnende programmeur niet echt nodig zullen we deze       
 techniek pas later behandelen maar nu de indeling van de       
 eerste 64K bespreken.                                          
                                                                
 De eerste 32K die bij het opstarten van je MSX door de         
 computer bestuurd worden zijn ROM.                             
 ROM wilt zeggen Read Only Memory, geheugen dat alleen maar     
 door de computer gelezen kan worden.                           
 Dit geheugen bevat de BASIC en de BIOS en kan niet door een    
 programmeur beschreven worden en loopt van &H0000 tot &H7FFF.  
 Het onderste gedeelte van het ROM (&H0000 tot &H3FFF)          
 bestaat uit BIOS en van &H4000 tot &H7FFF is BASIC.            
                                                                
 De tweede 32K, dus het geheugen van &H8000 tot &HFFFF, is      
 RAM. In dit RAM kan je dus je programma's kwijt.               
 Houdt er wel rekening mee dat op &H8000 BASIC programma's      
 bewaard worden en vanaf ongeveer &HE000 beginnen de            
 opslagarray's van BASIC en BIOS.                               
                                                                
 Nu is dit natuurlijk allemaal leuk om te weten, maar wat heb   
 je d'er aan ????                                               
                                                                
 Wel, de BIOS is een verzameling van routines die door de       
 ontwikkelaars van MSX geschreven zijn om het leven van de      
 MSX-er te vergemakkelijken. Als je elke chip met eigen         
 routines zou moeten aan spreken, zou je maanden aan het        
 programmeren zijn voor een "simpele" scroll demo of zo..       
                                                                
 De BIOS bestaat dus uit een rij routines die allemaal hun      
 eigen werking hebben.                                          
 We zullen er een paar bespreken ....                           
                                                                
        CHKRAM:                                                 
                Adres:  0000h                                   
                Invoer: geen                                    
                                                                
        Deze routine reset de MSX en vult het geheugen met de   
        juiste beginwaarden (SCREEN 0 en zo..)                  
                                                                
        CHGMOD                                                  
                Adres:  005Fh                                   
                Invoer: A                                       
                                                                
        Deze routine zet de gewenste screenmode aan.            
        Wil je dus SCREEN 5 dan laad je A met 5 en roep je      
        CHGMOD aan.                                             
                                                                
        CHGET:                                                  
                Adres:  009Fh                                   
                Invoer: geen                                    
                                                                
        Deze routine leest een teken van het toetsen bord.      
        Als er geen teken wordt ingedrukt wacht de routine      
        tot dit wel gebeurd.                                    
                                                                
        CHPUT:                                                  
                Adres:  00A2h                                   
                Invoer: A                                       
                                                                
        Deze routine stuurt een ASCII teken naar het scherm.    
        In A moet dus de ASCII waarde van de letter, die je     
        wilt sturen staan.                                      
                                                                
        LPTOUT:                                                 
                Adres:  00A5h                                   
                Invoer: A                                       
                                                                
        Deze routine stuurt een ASCII teken naar de printer.    
        Als het lukt om een teken te sturen zal de CARRY vlag   
        laag staan. Anders is de uitvoer mislukt.               
                                                                
        BEEP:                                                   
                Adres:  00C0h                                   
                Invoer: geen                                    
                                                                
        Roep maar eens aan !!! En sta versteld van het          
        resultaat.                                              
                                                                
        GTTRIG:                                                 
                Adres:  00D8h                                   
                Invoer: A                                       
                Uitvoer:A                                       
                                                                
        Met deze routine test je of er een vuurknop of de       
        spatie balk wordt ingedrukt.                            
        De mogelijke waarden van A, bij de invoer kunnen zijn   
                                                                
                0       spatiebalk                              
                1       1e vuurknop joystick (of muis) 1        
                2       2e   " "      "  "     "  "    1        
                3       1e   " "      "  "     "  "    2        
                4       2e   " "      "  "     "  "    2        
                                                                
        Bij het verlaten van de routine staan de volgende       
        mogelijkheden:                                          
                                                                
                00h     niet ingedrukt                          
                FFh     wel  ingedrukt                          
                                                                
        FORMAT                                                  
                Adres:  0147h                                   
                Invoer: geen                                    
                                                                
        Deze routine doet hetzelfde als een _format in BASIC    
                                                                
 Met al deze wetenschap kunnen we nu natuurlijk enige           
 routines schrijven.                                            
                                                                
        ORG     &HC000                                          
                                                                
 CHGMOD:EQU     &H5F                                            
 CHGET: EQU     &H9F                                            
 CHPUT: EQU     &HA2                                            
 BEEP:  EQU     &HC0                                            
 GTTRG: EQU     &HD8                                            
 CHKRAM:EQU     &H00                                            
                                                                
        LD      A,0                                             
        CALL    CHGMOD                                          
        CALL    CHGET                                           
        LD      (PLACE),A                                       
        CALL    CHPUT                                           
        LD      HL,TEXT                                         
        LD      (TXTPNT),HL                                     
                                                                
 LOOP:                                                          
        LD      A,0                                             
        CALL    GTTRG                                           
        JP      NZ,EINDE                                        
        LD      HL,(TXTPNT)                                     
        LD      A,(HL)                                          
        CP      "$"                                             
        JP      Z,EINDE                                         
        LD      (TXTPNT),HL                                     
        CALL    CHPUT                                           
        CALL    BEEP                                            
        JP      LOOP                                            
 EINDE:                                                         
        JP      CHKRAM                                          
 TXTPNT:                                                        
        DEFW    TEXT                                            
 TEXT:                                                          
        DEFM    "Hallo Computeraartje. Je hebt net een "        
 PLACE:                                                         
        DEFB    0                                               
        DEFM    " getypt. Als deze text je niet bevalt, "       
        DEFM    "moet je op de spatie balk duwen. Als je dat"   
        DEFM    " doet zal je MSX je op reset springen. "       
        DEFM    "ZO, GENOEG TEXT ..... LATEN WE MAAR EENS "     
        DEFM    "RESETTEN !!!!!!!!$"                            
                                                                
        END                                                     
                                                                
 Zo dat is dan de eerste keer dat we een grote routine          
 publiceren en het zal niet de laatste keer zijn ....           
 Deze routine wacht op invoer via de BIOS routine CHGET.        
 Hierna zal de routine beginnen met een tekst te printen.       
                                                                
 Boven de programmalisting staat een aantal maal EQU,           
 dit  is  geen  commando maar  en komt  dus ook  niet in het    
 geheugen te staan.                                             
 De  assembler vervangt  na deze  reeks EQU's  de labels  die   
 ervoor staan door het getal erachter.                          
 Wordt nu  b.v een  CALL BEEP  gedaan voert  de computer  een   
 CALL &HC0 uit.                                                 
                                                                
 Het CP commando in deze routine trekt vande inhoud van het     
 A  register  het  Asciiteken  $  af.  Wanneer  nu  in  A het   
 dollarteken komt, levert deze aftrekking nul op.               
 Het JumP Zero commando in de volgende regel zorgt er nu voor   
 dat er naar het LABEL : EINDE wordt gesprongen.                
 Door CP wordt de inhoudt van het A register niet veranderd.    
                                                                
 Hopelijk  verdient de  rest van  de instructies geen uitleg,   
 anders bel je gerust de programmeer lijn maar eens.            
 ( zie helplines )                                              
                                                                
 Zo, dank zij de terug blader optie kan je alles nog eens       
 mooi overlezen. Nu kun je eindelijk eens iets meer met je      
 MSX dan alleen maar optellen en zo..                           
                                                                
 In de volgende cursus zullen we nog wat instructies            
 behandelen en nog wat BIOS. Misschien beginnen we daarna wel   
 met de besturing van de VDP of de DISK-DRIVE in MCODE.         
                                                                
 In elk geval,                                                  
                Veel succes namens:                             
                                        Ruud Gelissen en        
                                        Jan-Willem van Helden   
                                                                
                                                                
 Als boeken raden wij aan:                                      
                                                                
 MSX-HANDBOEK VOOR GEVORDERDEN -- A.RENSINK -- KLUWER           
                                               TECHNISCHE       
                                               BOEKEN           
                                                                
 ZAKBOEKJE Z80 -- J.B.VONK -- KLUWER TECHNISCHE BOEKEN          
                                                                
 MACHINETAAL Z80 -- J.B.VONK/E.J.J.DOPPENBERG -- KLUWER         
                                                 TECHNISCHE     
                                                 BOEKEN         
                                                                
 ASCII TECHNICAL DATA-BOOK -- ASCII Systems Division --         
                                            ASCII CORPERATION   
                                                                
                                                                
