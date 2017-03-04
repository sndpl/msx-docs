 Jan-Willem bazelt weer over dingen die een simpele ziel als ik nooit zal begrijpen...

 Data Compressie 5 (?)

 Jaa, sorry hoor, maar ik ben de tel kwijt geraakt. Niet dat
 er zoveel cursi waren, maar ik heb al een keer of 4 niks
 meer gedaan, of beloofd dat ik de listing zou gaan
 bespreken. Belofte maakt schuld, dus ....

 RUNLEN.GEN

 AL MIJN COMMENTAAR KOMT NU IN GROTE LETTERS. ZO ZIE JE HET
 VERSCHIL TUSSEN COMMENTAAR EN LISTING MAKKELIJKER.

 WE STARTEN MET WAT COMMANDO'S VOOR DE ASSEMBLER (IN DIT
 GEVAL GEN80).

* g 0,s 15

 HIERNA VOLGEN ENIGE COMMENTAAR REGELTJES....

; Runlength v3.0 1995
; (c) Jan-Willem van Helden
; Thanks to:
; - Ivo Wubbels & Falco Dam
; - David Huffman
; - Lempel-Ziv Welch

; de JCP file ziet er zo uit:

; - codebyte1 (1)
; - codebyte2 (1)
; - x-offset    (1)
; - y-offset    (1)
; - paletbit    (1) (als 0 geen palette)
; als palette=0: volgende 32 bytes palette
; daarna komt data

 IK BEGIN MET EEN KLEINE SETUP, DIE NODIG IS VOOR MSX-DOS.
 HET IS ALTIJD HANDIG OM EEN MACRO TE EDITTEN VOOR DE BIOS
 CALLS.

bios:   macro   @bios1
        ld      iy,(#fcc0)
        ld      ix,@bios1
        call    #1c
        endm

 EEN MACRO OM TEKSTEN AF TE DRUKKEN

printaf:        macro   @printaf1
        ld      de,@printaf1
        ld      c,#9
        call    #f37d
        ld      de,nexttext
        ld      c,#9
        call    #f37d
        endm

 OM DE RST20 TE SIMULEREN

rst20:  macro
        ld      a,h
        sub     d
        jr      nz,$+4
        ld      a,l
        sub     e
        endm

 NOG EEN MACRO DIE WE LATER NODIG HEBBEN

searchtabel:    macro
        ld      ix,tabel
        ld      iy,tabel+2
        ld      b,-1
;tabelsrc:
        ld      l,(ix+0) ;in HL de laagste
        ld      h,(ix+1)
        ld      e,(iy+0) ;in DE de hogere
        ld      d,(iy+1)

        rst20

        jp      c,$+7 ;DE hoger ?? -> nieuwe laagste

        push    iy
        pop     ix
;---
        inc     iy
        inc     iy
        djnz    $-26

        push    ix
        pop     hl
        ld      de,tabel
        and     a
        sbc     hl,de
        and     a
        rr      h
        rr      l
        ld      a,l
        endm

 EEN LAATSTE AANTAL EQU'S

bdos:   equ     #f37d
datfil: equ     #8000
regelv: equ     128
tabel:  equ     #d000

        ld      a,(#f342)       ; zet RAM op #4000
        ld      h,#40
        call    #24

        ld      hl,#5c          ; zet de 2e filenaam in
        ld      de,fcb          ; het File Control Block
        ld      bc,12
        ldir

        ld      hl,#82          ; bekijk de rest van de
        ld      de,verwerking   ; commandline
        ld      bc,#100-#82
        ldir

        ld      hl,tabel        ; vul de tabel met nullen
        ld      de,tabel+1
        ld      bc,(256*2)-1
        ld      (hl),0
        ldir

        ld      e,"/"           ; zoek naar de file options
        ld      hl,verwerking
        call    cmpr
        call    z,fndslash

        printaf starttext


        ld      de,free         ; DMA op einde van de file
        ld      c,#1a           ; zetten
        call    bdos

        ld      de,fcb          ; file openen
        ld      c,#0f
        call    bdos
        or      a
        jp      nz,endsequence  ; error

        ld      hl,0
        ld      (blok),hl
        ld      (blok+2),hl
        ld      (#f3ea),hl
        inc     hl
        ld      (grote),hl

        ld      a,(depacking)   ; kijken of we soms moeten
        and     a               ; uitpakken i.p.v. inpakken
        jp      nz,depacker

        printaf starttext1      ; nee hoor, we gaan inpakken

        ld      hl,(filsiz)
        call    fulprt

        printaf decstr

        printaf savetext

        ld      c,#1            ; kijken of we 'm moeten
        call    bdos            ; inpakken of der mee
        cp      "s"             ; moeten stoppen
        jp      z,loadpicture
        cp      "S"
        jp      nz,closefile

; nu wordt het plaatje daadwerkelijk ingeladen

loadpicture:
        ld      hl,(filsiz)
        ld      (fil_siz),hl
        ld      a,(screen)      ; gekozen scherm aanzetten
        bios    #5f
        ld      a,(screen12)
        inc     a
        jp      nz,n_setscreen12
        ld      bc,#819         ; screen 12 aan
        bios    #47
n_setscreen12:
        ld      a,(#ffe7)       ; sprites uit
        or      2
        ld      b,a
        ld      c,8
        bios    #47
        ld      de,#8000        ; dma op #8000
        ld      c,#1a
        call    bdos
        ld      hl,4            ; eerste 4 bytes inladen
        ld      de,fcb
        ld      c,#27
        call    bdos

;kijken of het een .DAT/.GE5 file is

        ld      a,(#8000)
        cp      #fe
        jp      nz,loadpicture_d        ;dat file
        ld      hl,3
        ld      de,fcb
        ld      c,#27
        call    bdos
        jp      loadpicture_b           ;bin file

 EEN DAT-FILE BEGINT ALTIJD MET DE X-LENGTE (2-BYTES) EN DE
 Y-LENGTE. DEZE ZIJN GEGEVEN IN DOTS. OM ZE OM TE REKENEN
 NAAR REEELE BYTES, ZIE HET VOLGENDE
(IN DE VOLGENDE TEXT!).
 Jan-Willem bazelt weer over dingen die een simpele ziel als ik nooit zal begrijpen...

 Data Compressie 5 (?)

(VERVOLG)

loadpicture_d:
        ld      hl,(#8000)
; in geval van screen 5:        delen door 2
;               6:                 4
;               7:                 2
;               8:                 1

        ld      a,(screen)
        cp      8
        jp      nc,loaddatloop2_1
        and     a
        rr      h
        rr      l
        cp      6
        jr      nz,loaddatloop2_1
        and     a
        rr      h
        rr      l
loaddatloop2_1:
        ld      a,l
        dec     a
        ld      (xofset),a      ; mijn x-offset vullen
        ld      a,(#8002)       ; y-offset bepalen
        ld      c,a
        dec     a
        ld      (yofset),a
        ld      hl,(vrmpnt)     ; gelijk aan 0
        call    loaddatfl       ; blokje inladen
loaddatloop:
        push    bc
        ld      bc,(xofset)
        inc     c
loaddatloop2:
        ld      a,(de)          ;werkelijke naar het
        call    putintable      ;scherm toeschrijven
        bios    #177
        inc     hl
        inc     de
        push    hl
        ld      hl,#c000
        rst20
        call    z,loaddatfl
        pop     hl
        dec     c
        jp      nz,loaddatloop2
        ld      hl,(vrmpnt)     ; naar het begin van de
        push    de              ; volgende regel
        ld      de,256
        ld      a,(screen)      ; opletten met die screens !
        cp      7
        jp      nc,loaddatloop3
        ld      de,128
loaddatloop3:
        add     hl,de
        ld      (vrmpnt),hl
        pop     de
        pop     bc
        dec     c
        jp      nz,loaddatloop  ; blijven doen totdat datfile
                                ; op het scherm staat
        ld      a,(nopaletdat)
        inc     a
        jp      z,loadpicture1  ; palet switch disabeled
        ex      de,hl
        ld      b,32            ; anders nog even het palet
        ld      de,palet        ; inladen (kan alleen bij
paletdatl:                      ; dat files gemaakt door
        ld      a,(hl)          ; DD-Graph.
        ld      (de),a
        push    de
        inc     hl
        ld      de,#c000
        rst20
        pop     de
        inc     de
        call    z,loaddatfl
        djnz    paletdatl

        ld      hl,palet        ; paletje aanzetten.
        call    setpal

        jp      loadpicture1
loaddatfl:                      ; het daadwerkelijke laden
        push    bc              ; van de dat-file
        push    hl
        ld      de,#8000
        ld      c,#1a
        call    bdos
        ld      hl,#4000
        ld      de,fcb
        ld      c,#27
        call    bdos
        ld      de,#8000
        pop     hl
        pop     bc
        ret
; binaire files inladen

 BINAIRE FILES ZIJN EEN STUK MAKKELIJKER. JE HOEFT ALLEEN
 MAAR TE LADEN TOT HET EINDADRES EN DAT NAAR HET VIDEORAM TE
 DUMPEN.

loadpicture_b:
;voor screen 5 geldt: 127
;            6  127
;            7  255
;            8  255 voor de X-offset
        ld      a,(screen)
        cp      7
        ld      a,255
        jp      nc,loadbinary
        ld      a,127
loadbinary:
        ld      (xofset),a
        ld      hl,(screenlength)
        ld      (screen_length),hl
        ld      de,-#4000
loadpicturel:
        ex      de,hl
        ld      de,#4000
        add     hl,de
        ex      de,hl
        push    de
        ld      de,#8000
        ld      c,#1a
        call    bdos
        ld      hl,#4000
        ld      de,fcb
        ld      c,#27
        call    bdos
        ld      hl,#8000
        ld      bc,#4000
        pop     de
        push    de
        ex      de,hl
loadpicture2:
        ld      a,(screen)
        cp      7
        push    de
        ld      d,h
        ld      a,211
        jr      nc,loaddicture
        and     a
        rl      d ; keer 2 !!!
loaddicture:
        cp      d
        pop     de
        ld      a,(de)
        call    c,putintable
        bios    #177
        inc     hl
        inc     de
        dec     bc
        ld      a,b
        or      c
        jp      nz,loadpicture2

        ld      hl,(fil_siz)
        ld      de,#4000
        and     a
        sbc     hl,de
        ld      (fil_siz),hl
        pop     de
        jp      nc,loadpicturel
        ld      a,211
        ld      (yofset),a
        ld      hl,(paletdata)  ; palet nog binnenhalen
        ld      a,h
        or      l
        jp      z,loadpicture1
        ld      de,palet
        ld      bc,32
        bios    #59
        ld      hl,palet
        call    setpal
loadpicture1:
        call    close_file_real

 BIJ HET INLADEN NET WERD PER BYTE DE ROUTINE PUTINTABLE
 AANGEROEPEN. DEZE ROUTINE MAAKT EEN TABEL AAN MET DE
 WAARDEN VAN HET AANTAL KEER VOORKOMEN VAN DE BYTE. ENKELE
 CURSI TERUG HEB IK UITGELEGD, DAT JE OM EEN GOEDE COMPRESSIE
 TE KRIJGEN DE MINST VOORKOMENDE BYTE ALS CODE-BYTE MOET
 GEBRUIKEN. IN DE VOLGENDE ROUTINE DOEN WE DAT. ALS
 UITBREIDING ZOEKEN WE OOK NOG EEN 2E CODE-BYTE. NU KUNNEN WE
 ZOWEL IN 1 ALS IN 2 BYTE(S) CRUNCHEN


; nu wordt de minst voorkomende waarde gezocht

        searchtabel
        ld      (codebyte1),a
        ld      (ix+0),#ff ; die positie effe vullen
        ld      (ix+1),#ff ; (meest voorkomend maken)

; nu wordt de een na minst voorkomende waarde gezocht

        searchtabel
        ld      (codebyte2),a

; de codebyte is gevonden
; nu wordt alles klaargemaakt om het crunchen te beginnen

        ld      hl,jwext        ;die coole .JCP
        ld      de,ext
        ld      bc,3
        ldir

        xor     a
        ld      (fcb),a         ; defaultdrive

        ld      de,fcb          ; file aanmaken
        ld      c,#16
        call    bdos

        ld      hl,0
        ld      (blok),hl
        ld      (blok+2),hl
        inc     hl
        ld      (grote),hl

; create header

        ld      a,(codebyte1)
        ld      (dma),a
        ld      a,(codebyte2)
        ld      (dma+1),a
        ld      a,(xofset)
        ld      (dma+2),a
        ld      a,(yofset)
        ld      (dma+3),a

        ld      de,dma          ; header wegschrijven
        ld      hl,4
        call    crnstoreit

        ld      a,(nopaletdat)  ; palet storen ?
        ld      (dma1),a
        ld      de,dma1
        ld      hl,1
        push    af
        call    crnstoreit
        pop     af
        inc     a
        jp      z,nostorepal    ; zo ja, dan zijn de
                                ; volgende 32 bytes paletdata
        ld      de,palet
        ld      hl,32
        call    crnstoreit

 NU GEBRUIK IK WEER DIE SEMI PROGRAMMEER TAAL DIE OOK IN DE
 VORIGE CURSI GEBRUIKT WERD.

nostorepal:
        ld      hl,0            ; A
        ld      (vrmpnt),hl
crnrestart:
        ld      bc,1            ; CNTR=0
        bios    #174            ; get A
        ld      d,a
        xor     #ff
        bios    #177
crnloop:
        call    crnendline      ; A+1 wordt geset in crnendline
        ld      a,(endfile)
        and     a
        jp      nz,crnstore
        bios    #174            ; get A+1
        cp      d               ; A=A+1 ?
        jp      nz,crnstore     ; NO
        xor     #ff
        bios    #177            ; laat vooruitgang zien
        inc     bc
        jp      crnloop         ; YEP
crnstore:
        push    hl
        push    de
        ld      h,b
        ld      l,c
        ld      d,0
        ld      e,3+1           ; is CNTR > 3 ??? (nieuw idee)
        rst20
        pop     de
        pop     hl
        jp      nc,crnstore1    ; ja
        ld      a,(codebyte1)   ; check of het 1 van de 2
        cp      d               ; codebytes is
        jp      z,crnstore1     ; ja, dan store ze als crunch
        ld      a,(codebyte2)   ; data
        cp      d
        jp      z,crnstore1
        push    hl              ; zijn ze dat niet, schrijf
        push    bc              ; dan het aantal keer de byte
        ld      b,c             ; naar de disk
        ld      hl,dma1
crnstore2:
        ld      (hl),d
        inc     hl
        djnz    crnstore2
        pop     hl
        ld      de,dma1
        call    crnstoreit
        pop     hl
        ld      a,(endfile)     ; einde ?
        dec     a
        jp      z,closefile
        jp      crnrestart      ; begin aan de volgende byte
crnstore1:
        ld      a,d
        ld      (dma+1),a       ; STORE A
        ld      a,b             ; A kleiner dan 256 ?
        and     a               ; store dan maar 3 bytes
        jp      z,crnstore3bytes
        ld      a,(codebyte1)   ; store codebyte
        ld      (dma),a
        ld      (dma+2),bc      ; STORE CNTR
        push    hl
        ld      hl,4
crnstore3:
        ld      de,dma
        call    crnstoreit      ; STORE de hele heise
        pop     hl
        ld      a,(endfile)     ; einde ?
        dec     a
        jp      z,closefile
        jp      crnrestart      ; begin aan volgende byte
crnstore3bytes:
        ld      a,(codebyte2)   ; schrijf maar 3 bytes weg
        ld      (dma),a
        ld      a,c
        ld      (dma+2),a
        push    hl
        ld      hl,3
        jp      crnstore3
crnstoreit:
        push    hl              ; het wegschrijven
        ld      c,#1a
        call    bdos
        pop     hl
        ld      de,fcb
        ld      c,#26
        jp      bdos
crnendline:
        push    bc              ; routine om te bepalen of
        push    de              ; we het einde van de regel
        ld      d,l             ; bereikt hebben
        ld      a,(screen)
        cp      7
        jp      nc,nietdelen
        ld      a,d
        sub     128
        jp      c,nietdelen
        ld      d,a
nietdelen:
        ld      a,(xofset)
        cp      d
        jp      nz,crnendline3
        ld      a,(screen)
        ld      hl,(vrmpnt)
        ld      de,256
        cp      7
        jr      nc,crnendline2
        ld      de,128
crnendline2:
        add     hl,de
        ld      (vrmpnt),hl
        cp      7
        ld      d,h
        ld      a,(yofset)
        jr      nc,crnendline4
        and     a
        rl      d               ; keer 2 !!!
crnendline4:
        cp      d
        jr      nc,crnendline3_1
        pop     de
        pop     bc
        ld      a,1
        ld      (endfile),a
        ret
crnendline3:
        inc     hl
crnendline3_1:
        pop     de
        pop     bc
        ret
closefile:
        xor     a               ; einde van het programma
        bios    #5f             ; scherm 0 aan
        ld      hl,stndpalet    ; standaard palet aan
        call    setpal
close_file:
        call    close_file_real ; file sluiten
        ld      c,0
        jp      bdos            ; exit met system-reset
close_file_real:
        ld      de,fcb
        ld      c,#10
        jp      bdos

 IN DIT PROGRAMMA ZIT OOK GELIJK DE UITPAKKER.

 (LEES DIE IN TEKST(3))!

 Jan-Willem bazelt weer over dingen die een simpele ziel als ik nooit zal begrijpen...

 Data Compressie 5 (?)

(VERVOLG VERVOLG)

depacker:
        printaf depacktext

        ld      c,#1            ; kijken of we moeten uit-
        call    bdos            ; pakken of stoppen
        cp      "d"
        jp      z,loaddepackdat
        cp      "D"
        jp      nz,closefile
loaddepackdat:
        ld      de,free         ; dma op vrije plaats
        ld      c,#1a
        call    bdos

        ld      hl,(filsiz)     ; file laden
        ld      de,fcb
        ld      c,#27
        call    bdos

        ld      a,(screen)      ; schermpje aan
        bios    #5f

        ld      a,(free)        ; data vullen
        ld      (codebyte1),a
        ld      a,(free+1)
        ld      (codebyte2),a
        ld      a,(free+2)
        ld      (xofset),a
        ld      a,(free+3)
        ld      (yofset),a
        ld      a,(free+4)
        and     a
        ld      de,free+5
        jp      nz,nodepackpalet; palet checken
        ld      hl,free+5
        ld      de,palet
        ld      bc,32
        ldir
        ld      hl,palet
        call    setpal
        ld      de,free+5+32
nodepackpalet:                  ; de werkelijke depacker
        ld      hl,0            ; startadres in VRAM
depackrout:
        ld      a,(de)          ; GET data
        ld      b,a
        ld      a,(codebyte2)   ; data = CODEBYTE2 ?
        cp      b
        jp      z,depack1loop
        ld      a,(codebyte1)   ; data = CODEBYTE1 ?
        cp      b
        jp      z,depackloop
        ld      a,(de)          ; NO
        bios    #177            ; WRITE data TO VRAM
        inc     de
        call    crnendline      ; endline ?
        ld      a,(endfile)     ; einde ?
        and     a
        jp      nz,enddepack
        jp      depackrout      ; volgende data
depackloop:
        inc     de              ; GET A
        ld      a,(de)
        ex      af,af'
        inc     de
        ld      a,(de)          ; GET CNTR
        ld      c,a
        inc     de
        ld      a,(de)
        ld      b,a
depack_loop:
        ex      af,af'
        bios    #177            ; DUMP A CNTR times
        ex      af,af'
        call    crnendline      ; endline ?
        ld      a,(endfile)     ; einde ?
        and     a
        jp      nz,enddepack
        dec     bc
        ld      a,b
        or      c
        jp      nz,depack_loop
        ex      af,af'
        inc     de
        jp      depackrout      ; next data
depack1loop:
        inc     de
        ld      a,(de)          ; GET A
        ex      af,af'
        inc     de
        ld      a,(de)          ; GET CNTR (1 byte)
        ld      c,a
        ld      b,0
        jp      depack_loop
enddepack:
        ld      c,#1            ; input ?
        call    bdos
        jp      closefile       ; einde programma

 HET EIGENLIJKE PROGRAMMA IS NU GEDAAN. ALLEEN NOG ENKELE
 SUB-ROUTINES VOLGEN

putintable:                     ; tabel vullen
        push    de
        push    bc
        ld      b,a
        inc     b
        ld      ix,tabel-2
        ld      de,2
putintablel:
        add     ix,de
        djnz    putintablel

        ld      c,(ix+0)
        ld      b,(ix+1)
        inc     bc
        ld      (ix+0),c
        ld      (ix+1),b
        pop     bc
        pop     de
        ret
fndslash:                       ; zoeken naar de /
        inc     hl
        ld      a,"5"           ; scherm bepalen
        cp      (hl)
        jp      z,scr5
        ld      a,"6"
        cp      (hl)
        jp      z,scr6
        ld      a,"7"
        cp      (hl)
        jp      z,scr7
        ld      a,"8"
        cp      (hl)
        jp      z,scr8
        ld      a,"1"
        cp      (hl)
        jp      z,scr12
nextpart:
        inc     hl              ; zoeken naar de P
        ld      a,","
        cp      (hl)
        ret     nz
        inc     hl
        ld      a,"P"
        cp      (hl)
        call    z,nopaleton     ; palet aan cq uit
        ld      a,"p"
        cp      (hl)
        call    z,nopaleton
        inc     hl
        ld      a,","
        cp      (hl)
        ret     nz
        inc     hl
        ld      a,"D"           ; depacker ?
        cp      (hl)
        call    z,depackon
        ld      a,"d"
        cp      (hl)
        call    z,depackon
        ret

; waarden goed vullen

nopaleton:
        ld      a,-1
        ld      (nopaletdat),a
        ld      de,0
        ld      (paletdata),de
        ret
depackon:
        ld      a,-1
        ld      (depacking),a
        ret
scr5:
        ex      de,hl
        ld      a,5
        ld      (screen),a
        ld      hl,#7680
        ld      (paletdata),hl
        ld      hl,128*212
        ld      (screenlength),hl
        ld      a,128-1
        ld      (xofset),a
        ex      de,hl
        jp      nextpart
scr6:
        ex      de,hl
        ld      a,6
        ld      (screen),a
        ld      hl,#7680
        ld      (paletdata),hl
        ld      hl,128*212
        ld      (screenlength),hl
        ld      a,128-1
        ld      (xofset),a
        ex      de,hl
        jp      nextpart
scr7:
        ex      de,hl
        ld      a,7
        ld      (screen),a
        ld      hl,#fa80
        ld      (paletdata),hl
        ld      hl,256*212
        ld      (screenlength),hl
        ld      a,-1
        ld      (xofset),a
        ex      de,hl
        jp      nextpart
scr8:
        ex      de,hl
        ld      a,8
        ld      (screen),a
        ld      hl,0
        ld      (paletdata),hl
        ld      hl,256*212
        ld      (screenlength),hl
        ld      a,-1
        ld      (nopaletdat),a
        ld      (xofset),a
        ex      de,hl
        jp      nextpart
scr12:
        inc     hl
        ld      a,"2"
        cp      (hl)
        ret     nz
        ld      a,-1
        ld      (screen12),a
        jp      scr8

; ontzettend moeilijke en tegelijk zeer interessante routine !

beep:
        bios    #c0
        ret

cmpr:
        ld      a,e
        cp      (hl)
        ret     z
        inc     hl
        ld      a,(hl)
        and     a
        jp      nz,cmpr
        inc     a
        ret
endsequence:                    ; routine als er geen input
        printaf endtxt          ; is
        ret

; de texten

endtxt:
        db      "Opts are: "
        db      "/n: MSX2/2+ screen number; /P: No palette"
        db      10,13
        db      "/D: depack file",10,10,13
        db      "Default: A>RUNLEN filename /n,P,D$"
depacktext:
        db      "Press [D] to depack or any other key to quit"
        db      "$"
starttext:
        db      "Runlength V3.0",10,13,"$"
starttext1:
        db      10,"File length: $"
savetext:
        db      10,"Press [S] to save or any other key to quit"
        db      "$"
runlentxt:
        db      "RUNLEN"
nexttext:
        db      10,13,"$"

; variabelen

verwerking:     ds      #100-#82
screenlength:   dw      0
screen_length:  dw      0
depacking:      db      0
screen:         db      8
fil_siz:        dw      0
paletdata:      dw      0
palet:          ds      32
nopaletdat:     db      0
xofset:         db      0
yofset:         db      0
vrmpnt:         dw      0
stndpalet:      dw      0,0,#611,#733,#117,#327,#151,#627
                dw      #171,#373,#661
                dw      #663,#411,#265,#555,#777
screen12:       db      0
endfile:        db      0
codebyte1:      db      0
codebyte2:      db      0
dma:

; Stukjes om weg te schrijven

        db      0 ; Codering
        db      0 ; Gelezen waarde
        dw      0 ; Countr waarde
dma1:   ds      4
jwext:  db      "JCP"
setpal:
        push    hl
        ld      bc,16
        bios    #47
        pop     hl
        ld      bc,#209a
        otir
        ret
ascbin: push    de
        push    hl
        ld      hl,0
        ld      (ascnum),hl
        ld      (ascnum+2),hl
        pop     hl
asclus:
        ld      a,(hl)
        sub     "0"
        jp      m,asceind
        cp      10
        jp      p,asceind
        push    hl
        call    maal10
        pop     hl
        inc     hl
        jp      asclus
asceind:
        pop     de
        push    hl
        ld      hl,ascnum
        ld      bc,4
        ldir
        pop     hl
        ret
maal10:
        ld      iy,(ascnum)
        ld      hl,(ascnum+2)
        add     iy,iy
        adc     hl,hl
        ld      e,l
        ld      d,h
        push    iy
        pop     bc
        add     iy,iy
        adc     hl,hl
        add     iy,iy
        adc     hl,hl
        add     iy,bc
        adc     hl,de
        ld      c,a
        ld      b,0
        add     iy,bc
        ld      c,0
        adc     hl,bc
        ld      (ascnum),iy
        ld      (ascnum+2),hl
        ret

ascnum: ds      4

fulprt:
        ld      de,decstr
        ld      b,7
        ld      a,"$"
dc1:    ld      (de),a
        inc     de
        djnz    dc1
dc2:    xor     a
        ld      e,a
        ld      b,16
dc3:    rl      l
        rl      h
        rl      e
        ld      a,e
        sub     10
        ccf
        jr      nc,dc4
        ld      e,a
dc4:    djnz    dc3
        rl      l
        rl      h
        ld      a,e
        add     a,"0"
        push    hl
        ld      hl,decstr+3
        ld      de,decstr+4
        ld      bc,4
        lddr
        ld      (decstr),a
        pop     hl
        ld      a,h
        or      l
        jr      nz,dc2
        ret
decstr: ds      7

fcb:    db      0
name:
inputfile:      db      "12345678"
ext:    db      "123"
        dw      0
grote:  dw      0
filsiz: ds      17
blok:   dw      0,0
free:
einde:
        end

 Zo, nu is dat hoofdstuk helemaal afgesloten. Volgende keer
 gaan we eens aan de slag met Huffman. Niet zo moeilijk,
 goede resultaten, maar erg traag voor de MSX.

 De bovenstaande listing staat op disk onder de naam
 "RUNLEN.GEN" en is public domain.

 Zo, vaerdig !

                        Jan-Willem van Helden
