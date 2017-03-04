ROM images, laat IK daar nu geen reet van snappen.

       ROM-IMAGES LADEN


 Op de CD's van MCCM staan heel wat rom-images. Ook op andere
 CD's (die illegaal zijn) en via internet (idem dito) zijn
 heel wat roms te verkrijgen. Op een MSX-emulator zijn deze
 eenvoudig te gebruiken, maar op een MSX is het een stuk
 lastiger.

 Er is een programmaatje dat rom-images inlaadt en start,
 maar bij mij werken de meeste spelletjes  niet of niet
 goed. Ik heb geen DOS2, dus ik denk dat het daar aan ligt.
 In deze tekst zal ik beschrijven hoe je een programaatje
 kunt maken en toch rom-images (van 16kB) kunt gebruiken, en
 natuurlijk staat een simpele lader ook op de FD.

 De rom-images beginnen met de letters AB en vervolgens 2
 bytes die het adres weergeven waar na initialisatie heen
 wordt gesprongen. De letters AB mag je wissen, zodat het
 spel niet automatisch opstart na het resetten.

 De 2 bytes na AB kun je gebruiken om te kijken op welk adres
 je het programma moet zetten. Als het adres groter is dan
 #3FFF, dan kun je de rom-image op het interval #4000-#7FFF
 laden en anders vanaf adres #0000.

 Verder moet er op pagina 1 en soms op 0 ram geschakeld zijn.
 Onder DOS is dit altijd het geval, maar onder BASIC niet. Je
 kunt met het volgende programmaatje op pagina 1 tot en met 3
 hetzelfde ram-slot selecteren. Als op pagina 0 ook RAM moet
 staan, dan moet je vier keer RRA gebruiken in plaats van
 twee keer.

        in      a,(#a8)         ; Lees huidige stand
        and     240             ; Hoogste 4 bits zijn onder
        ld      b,a             ; BASIC altijd al RAM
        rra                     ; Schuif de bits naar rechts
        rra
        or      b               ; Oude stand erbij
        out     (#a8),a         ; Schrijf slotstand weg

 Zo, nu weet je alles om een 16kB rom-image te gebruiken. Nu
 gaan we daadwerkelijk het programma maken (niet helemaal
 natuurlijk, maar alleen de code waar je problemen mee kunt
 krijgen. De volledige source staat op deze FD in de file
 ROM16.LZH, waar ook een simpele BASIC-lader in staat)
 waarmee je 16kB rom-images mee kunt inladen. Het programma
 aanpassen voor 32kB is trouwens erg makkelijk.

        org     #c000

 Onder BASIC erg logisch, want op adres #4000-#7FFF mag niets
 geladen worden.

        ld      de,fcb
        ld      c,open_file
        call    bdos            ; open de rom-image

        call    initFCB         ; initialiseer FCB om te
                                ; laden

        ld      de,#8000
        ld      c,setdta        ; Zet adres waar de data
        call    bdos            ; terecht komt

        ld      hl,16384
        ld      de,fcb
        ld      c,read_block
        call    bdos            ; Lees file in

        di                      ; Interrupts uit

        call    selram          ; Schakel RAM in

        ld      hl,#8000        ; Wis A van AB zodat rom niet
        ld      a,0             ; na reset opstart
        ld      (hl),a
        ld      bc,#4000        ; Grootte
        ld      de,#4000        ; Bestemming
        ld      a,(#8003)       ; Check startadres van rom
        cp      #40
        jp      nc,jump1        ; >#4000 -> Bestemming is
                                ; goed

        ld      de,#0000

 jump1: ldir                    ; Verplaats

        ld      hl,(#8002)      ; Laadt start adres
        jp      (hl)            ; en spring ernaar toe


 Het gebruik van ROM-images die groter dan 32kB zijn is veel
 lastiger, omdat die schakelroutine's gebruiken die bij
 bepaalde roms de data op andere adressen schakelen dan bij
 andere roms. Daarom heeft fMSX bij het selecteren van roms
 een optie om de cartridge-type te kiezen, zodat de roms wel
 werken. Het programma LOADROM (voor MSX) kan wel mega-roms
 aan, maar dat programma werkt alleen onder DOS2 goed en laat
 ik die nou net niet hebben. Een ander probleem is het
 geheugen, want bij mega-roms heb je gewoon meer dan 128kB
 geheugen nodig. Ook een probleem is dat de roms zelf ook
 naar ram schrijven en dus je kopie kunnen overschrijven
 waardoor alles in de soep loopt.


 Veel plezier en succes!


Arjan Bakker
