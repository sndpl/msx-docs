Turbo-R basic(1)                        


De tekst Turbo-R basic(2) is ooit al eens geplaatst op         
FutureDisk #2 en hebben we herplaatst aangezien de             
FutureDisk nu een stuk meer gelezen wordt en het wel bij dit   
artikel past.                                                  

In deze tekst ga ik wat dieper in op het commando              
CALL PCMPLAY, dus als je dit al geen reet interesseert         
dan kun je nu al op <ESC> duwen.                               

Voor wat basisinformatie verwijs ik even naar de andere        
Turbo-R basic tekst op deze disk.                              

We hebben in het verleden (en ook op deze FutureDisk) nog al   
wat sample-liedjes voor de Turbo-R in het demo-menu gehad.     

Waarschijnlijk als je al eens geprobeerd hebt om een liedje    
door de hoofdthema's(rode draad deuntjes door een muziekstuk   
heen) te samplen en op je Turbo-R na te maken(house-liedjes    
dienen zich hier het makkelijkst voor, aangezien deze muziek   
heel simpel opgebouwd is;veel herhaling enz.). Heel vaak heb   
je niet genoeg aan 64K(basic-tool memory) in het sample-       
programma dat bij je Turbo-R zit(dat je trouwens het beste     
kunt gebruiken voor samples) aangezien je dan meestal maar te  
weinig gesampled krijgt.                                       
Met 128K en dus 2 maal 64K lukt het meestal wel een            
fatsoenlijk liedje te maken(kijk maar naar NO LIMIT op deze    
disk!). Alleen is het dan een probleem hoe je dit gaat         
afspelen in BASIC. Je kunt natuurlijk met de Ramdisk werken    
(dus CALL Ramdisk), en daar je samples op zetten. Het          
probleem wat je echter dan krijgt, is dat het net iets te      
lang duurt voordat je ��n van de samples vanaf de Ramdisk      
weer in het gewone RAM gezet krijgt.                           

De oplossing is heel simpel! Zet gewoon je twee samples in     
het VRAM, daar passen ze allebei net in. Hoe dat kan, zal ik   
in het volgende voorbeeld programmaatje laten zien.            

10 SCREEN 5                                                    
20 SET PAGE,0: BLOAD"SAMPLE1.PCP",S                            
30 SET PAGE,2: BLOAD"SAMPLE2.PCP",S                            
40 'ZET HIERNA JE AFSPEELROUTINE(S)                            

In regel 10 roepen we screen 5 aan(4 pages) waarna we de       
eerste sample van 64K op page 0 en 1 zetten en sample 2 op     
de tweede en de derde.                                         
Het afspelen van je sample(of delen van de sample) op          
normale snelheid gaat dan als volgt:                           

CALL PCMPLAY(@BEGINADRES,EINDADRES,1,S)                

,S          staat er natuurlijk voor dat het VRAM afgespeeld   
wordt en ,S moet je dus niet vergeten te typen.                

Voor beginadres en eindadres geldt echter wel altijd dat       
beginadres>eindadres; achteruit afspelen gaat helaas niet.     

De tweede sample begint in ons geval ongeveer bij @65000.      

Tot zover het commando CALL PCMPLAY. Vanaf deze FutureDisk     
is de Turbo-R basic corner dus weer tijdelijk terug(wegens     
vele verzoeken). De volgende keer ga ik in op de               
PCM-outpoorten, waarmee overigens in "NO LIMIT" al mee         
geklooid wordt.                                                
Veel plezier met samplen!                                      

Koen "ik kan ook nog wat anders dan uit men nek lullen" Dols
                                                                

Turbo-R basic (4.0 en 4.1)                   
----------------------------                  

In deze tekst worden (bijna) alle bevelen die aan basic 2.0 toe-
gevoegd zijn uitgelegt. De MSX-2+ bevelen staan dus ook nog     
uitgelegd.                                                      
Nadere informatie: In de A1ST zit basic versie 4.0 en in GT     
basic 4.1.                                                      

Bevel 1:                                                        
Set scroll(X,Y,A,B)                                             

Het getal dat we voor X invullen geeft aan hoeveel      
pixels het beeldscherm horizontaal verplaatst/ver-      
schoven wordt.Het beeld dat links verdwijnt komt hier-  
bij natuurlijk op de rechterkant van het beeldscherm    
weer terug.(De waarde van X kan 0 t/m 511 zijn).        
Voor Y geldt hetzelfde principe alleen scrollt U het    
beeld hierbij vertikaal(De waarde van Y kan 0 t/m 255   
zijn).                                                  
Met de A kan men de eerste en laatste 8 pixels          
uitzetten waardoor een vervelend geflikker vermeden     
wordt ( Voor A gelden de waarden 0 en 1, wel of niet).  
Voor B kan men ook alleen de waarden 0 en 1 invullen    
(met zelfde betekenis) en kan men bepalen of men de     
pagina achter de afgebeelde pagina mee wil laten        
scrollen.                                               

Bevel 2:                                                        
Call Kanji X                                                    

Met dit bevel kan de Kanji mode aangezet worden,waar-   
door de karakters veranderen. Voor X kan 0 t/m 3 inge-  
vuld worden. Op elk scherm kan nu PRINT gebruikt        
worden PRINT #1 heb je dus niet meer nodig.             

Bevel 3:                                                        
Call CLS                                                        

Dit bevel heeft dezelfde functie als CLS maar           
aangezien CLS niet in de Kanji mode(zie hierboven)      
werkt moet men dan dus dit commando gebruiken.          

Bevel 4:                                                        
Call ANK                                                        

Met dit bevel wordt de Kanji Mode uitgeschakeld als     
deze 'aan' was.                                         

Bevel 5:                                                        
Put Kanji(X,Y),T,K                                              

Met dit bevel kan een bepaald Kanji-teken en/of een    
ander karakter dat de computer kent op het beeldscherm
afgebeeld worden. De X en Y zijn de co�rdinaten op het
beeldscherm van het te plaatsen karakter. De T staat   
voor het karakternummer en de K voor de kleur.         

Voorbeeldprogramma dat U alle karakters laat zien:     

10 SCREEN 7                                            
20 FOR A=8000 TO 30000                                 
30 PUT KANJI(50,100),A,13                              
40 FOR I=1 TO 90:NEXT I                                
50 NEXT A                                              
60 END                                                 

Bevel 6:                                                        
SCREEN X                                                        

Dit bevel is uitgebreider geworden men kan nu voor X    
de getallen 0 t/m 12 invullen. Alleen Screen 9 bestaat  
(nog) niet.                                             

Bevel 7:                                                        
CALL PALETTE=(K,R,G,B)                                          

Als de Kanji-mode 'aan' staat werkt het commando        
Color=(K,R,G,B) niet meer.                              
De K staat voor het kleurnummer(0 t/m 15) en met de     
waardes R(hoeveelheid rood ,0 t/m 7),G(hoeveelheid      
groen,0 t/m 7) en B(hoeveelheid blauw,0 t/m 7) kan      
deze oorspronkelijke kleur K verandert worden.          

Bevel 8:                                                        
MOTOR ON/OFF                                                    
De Turbo-R heeft geen kassette-recorder aansluiting     
dus deze commando's komen te vervallen.                 

Bevel 9:                                                        
CALL PCMREC(@BEGINADRES,EINDADRES,kwaliteit,vol.reactie,KB,S)   
Voor het opnemen van een sample.                        
Als beginadres kan men 0-65535 in vullen evenals het    
eindadres. Voor kwaliteit kan 0 t/m 3 ingevuld worden.  
Volume-reactie houdt in dat de Turbo-R pas reageert     
bij een bepaalde hardheid van de klank.Hiervoor kan     
men 0 t/m 127 invullen.Voor KB moet men 1 invullen als  
men iets geheugen wil sparen. De S kan ingevuld worden  
als U wilt dat de sample in het video-geheugen opge-    
slagen wordt.                                           

Bevel 10:                                                       
CALL PCMPLAY(@BEGINADRES,EINDADRES,kwaliteit)                   
Voor het afspelen van een eerder opgenomen sample.      
Zie voor informatie bij CALL PCMPLAY                    


Hopelijk heeft deze tekst U wat meer geleerd over basic        
3 en 4. Misschien op FutureDisk #3 een vervolg...              


                       Koen Dols        

