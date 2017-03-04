# ASSEMBLY (8)

 De vorige keer hebben jullie deze rubriek moeten missen 
 vanwege inspiratiegebrek, maar voor deze keer heb ik toch 
 iets weten te verzinnen, namelijk het gebruiken van extra 
 opties bij een DOS-programma (dus bv PRINT FD.TXT, waarbij 
 PRINT.COM een programma is om een tekst naar de printer te 
 sturen en FD.TXT de af te drukken tekst is).

 Het kunnen gebruiken van die extra optie is heel simpel. Op 
 adres #80 staat het aantal tekens dat na het commando werd 
 ingevoerd en op adres #81 en verder staat de ingevoerde 
 tekst. Het aantal tekens is overigens exclusief de byte #0d 
 (Carriage Return). Hier volgt een (heel simpel) voorbeeld-
 programmaatje:
```
         org     #0100

 bdos:   equ     5
 strout: equ     9

         ld      a,(#0080)      ; aantal tekens
         or      a
         jr      z,notext       ; 0 tekens = geen tekst

         ld      e,a            ; aantal bytes
         ld      d,0
         ld      hl,#0081
         add     hl,de          ; pointer naar einde tekst+1
         ld      a,"$"
         ld      (hl),a         ; vervang #0d door "$"

         ld      de,#0081
         ld      c,strout
         call    bdos           ; print string
         ret

 ;geen tekst ingevoerd
 notext: ld      de,text
         ld      c,strout
         call    bdos           ; print tekstje
         ret

 text:   defb    "No text entered$"
```
 Dit programmaatje staat ook op disk, zowel de source als de 
 geassembleerde file, welke ASSEMB8.GEN en ASSEMB8.COM heten.


## CACHE-ROUTINES

 Ongeveer een jaar geleden heb ik voor de FD eens een cache-
 routine geschreven. Deze is echter nooit gebruikt, voor-
 namelijk omdat de MoonSound-replayer erg veel ruimte 
 gebruikt. De volgende keer zal ik deze cache-routine's 
 bespreken, want daar heb ik nu vanwege de toch wel krappe 
 deadline echt geen tijd voor. Voor de liefhebbers staat de 
 source wel op disk (CACHE.GEN). Oh ja, deze cache-routine 
 cached alleen sectoren, geen files. Misschien maak ik voor 
 de volgende keer ook nog een cache-routine voor files, maar 
 dat zien jullie dan wel.

Arjan
