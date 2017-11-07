                            P R I N T R O U T I N E 
                                                     
          
          Met  de hier  volgende printroutine wordt een tekst op alle 
          breedten goed  op het scherm gezet, voor zover dat mogelijk 
          is.
          
          
          BDOS:           EQU    #0005
          
          ;BDOS functie
          StrOut:         EQU    9
          
          LINLEN:         EQU    #f3b0            ;Aantal tekens op
                                                  ;huidige scherm
          
          
                          call   Print
                          db     2,2              ;Kantlijnen
          
                          db     "PRINTROUTINE",1
                          db     "============",1
                          db     1
                          db     "Dit is een test voor mijn "
                          db     "printroutine. "
                          db     "Hopelijk werkt het goed.",1
                          db     1
                          db     "De bedoeling is dat de "
                          db     "tekst door de routine zelf goed "
                          db     "op het scherm gezet wordt, "
                          db     "ongeacht schermbreedte. "
                          db     "Nu nog automatisch afbreken :-)",1
                          db     1
                          db     "Hier begint een nieuwe alinea!",0
                          rst    0
          
          De  routine wordt  aangeroepen met de tekst achter de call. 
          Zo weet  je precies  wat er op het scherm komt te staan, en 
          hoef  je niet met labels te werken. Om aan te geven dat een 
          nieuwe regel  begint moet  een 1  worden gebruikt,  en de 0 
          geeft het einde van de tekst aan.
          
          
          ;P R I N T 
          ;Doel  : Zet tekst op scherm
          ;Input : hl: pointer naar tekst
          ;Output: carry hoog: kantlijnen te groot
          ;Noot  : eerste 2 bytes van de tekst zijn respectievelijk
          ;        de rechter- en linkerkantlijn
          
          
          Print:          ex     (sp),hl
          
          Dit zorgt ervoor dat de tekst achter de call kan staan.
          
          
                          ld     a,(LINLEN)
                          dec    a
                          sub    (hl)
                          jr     c,pError
          
          Rechterkantlijn  van breedte  aftrekken. Als  het resultaat 
          kleiner dan 0 is, wordt teruggesprongen met de carry hoog.
          
                          inc    hl
                          sub    (hl)
                          jr     c,pError
                          ld     (pLength),a
                          ld     a,(hl)
                          inc    hl
          
          Vervolgens wordt hetzelfde met de linkerkantlijn gedaan. De 
          linkerkantlijn is  nog nodig  om te  bepalen vanaf  waar de 
          buffer (83 tekens) moet worden beschreven.
          
          
                          push   hl
                          ld     hl,pBuffer1
                          ld     d,0
                          ld     e,a
                          add    hl,de            ;Tel linkerkantlijn
                                                  ;op bij buffer
                          ld     (pAddress3),hl
                          pop    hl
          
          pLoop1:         ld     a,(pLength)
                          ld     b,a
                          ld     de,(pAddress3)
          pLoop2:         ld     a,(hl)
                          cp     " "              ;Spatie?
                          call   z,pSpace
                          cp     1                ;Einde alinea?
                          jr     z,pAlinea
                          or     a                ;Einde tekst?
                          jr     z,pQuit
          
          Bij  een spatie worden de adressen bewaard. Bij een 0 wordt 
          niet zomaar  geRETurnd, want hl moet weer verwisseld worden 
          met het returnadres (dit i.v.m. tekst achter call).
          
          
                          ld     (de),a
                          inc    hl
                          inc    de
                          djnz   pLoop2
          
                          cp     " "
                          jr     z,sPrint2
          
          Als het  laatste teken  een spatie  is, hoeft er niet terug 
          worden gegaan naar de vorige spatie.
          
          
                          ld     a,(pFlag)
                          or     a
                          jr     z,sPrint2
          
          Als er nog geen spatie in de regel zit (pFlag wordt door de 
          routine pSpace  gezet), kan  er niet worden gekeken naar de 
          vorige spatie in de regel. In dit geval is de schermbreedte 
          te klein, en komt de tekst niet goed op het scherm.
          
          
                          xor    a
                          ld     (pFlag),a
                          ld     hl,(pAddress1)
                          ld     de,(pAddress2)
                          inc    hl
          sPrint2:        call   sPrint1
                          jr     pLoop1
          
          pSpace:         ld     (pAddress1),hl
                          ld     (pAddress2),de
                          ld     (pFlag),a
                          ret
          
          Adressen van spatie bewaren en vlag zetten.
          
          
          pQuit:          call   sPrint1
                          xor    a                ;Carry laag
                          ex     (sp),hl
                          ret
          
          Carry  laag,  want  carry hoog  bij terugkeer  betekent een 
          error.
          
          
          pAlinea:        call   sPrint1
                          inc    hl
                          jr     pLoop1
          
          Bij een nieuwe alinea moet de 1 worden overgeslagen.
          
          
          sPrint1:        push   hl
                          ld     a,13
                          ld     (de),a
                          inc    de
                          ld     a,10
                          ld     (de),a
                          inc    de
                          ld     a,"$"
                          ld     (de),a
          
          Zet waarden  voor einde van regel en einde van string op de 
          juiste adressen.
          
                          ld     de,pBuffer1
                          ld     c,StrOut
                          call   BDOS
                          pop    hl
                          ret
          
          pError:         xor    a                ;Zoeken naar 0
                          ld     bc,-1
                          cpir
                          ex     (sp),hl
                          scf
                          ret
          
          bc  wordt op  -1 gezet  om het  hele geheugen te doorzoeken 
          naar de 0. scf is nog om de carry weer hoog te zetten. cpir 
          zet de carry namelijk weer laag.
          
          
          ;Data
          
          pBuffer1:       ds     79+3," "         ;Buffer voor regel
          pAddress1:      dw     pBuffer1
          pAddress2:      dw     0
          pAddress3:      dw     pBuffer1         ;Buffer +
                                                  ;linkerkantlijn
          pFlag:          db     0                ;Vlag voor spatie
                                                  ;in regel
          pLength:        db     80
          
          
          De  buffer  is  79+3  omdat  een  regel  in  principe  80-1 
          karakters lang kan zijn, en 3 bytes nodig zijn om return en 
          het einde van de string aan te duiden.
          
                                                       Kasper Souren
