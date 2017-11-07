Leest iemand dit?

     ASSEMBLY CURSUS 9 (1)



        OEPS OEPS OEPS...

 ...en nog eens oeps. Vorige keer stond in deze cursus dat er
 een source van een cache-routine op disk zou staan. Dat was
 helaas niet geval. Als het goed is, staat het dingetje nu
 w�l op deze FD, en wel onder de naam CACHE.GEN.

 Vorige keer had ik ook beloofd om die cache-routine te
 bespreken. Aangezien belofte schuld maakt, doe ik dat nu dan
 maar ook. De routine in kwestie kan alleen sectoren cachen,
 dus geen files. Daar had ik namelijk geen zin in, maar
 misschien komt het er ooit nog eens van. Misschien ook
 niet...


           KESJING

 Caching is niets anders dan bepaalde informatie in het
 geheugen op te slaan. In het geval van MSX betekent dat
 bepaalde informatie maar ��n keer vanaf diskette ingeladen
 hoeft te worden, mits er natuurlijk genoeg geheugen aanwezig
 is (meestal meer dan 128 kB). Ik zal eerst maar eens globaal
 uitleggen hoe de cache-routine die op deze FD staat werkt.

 Ergens in het geheugen staat een tabel van 2880 (1440
 sectoren * 2 bytes). Elk byte-paar geeft aan waar een sector
 in het geheugen staat. Als beide bytes 0 zijn, dan staat de
 sector nog niet in het geheugen, anders wel. De eerste byte
 geeft aan in welke mapper-page de sector staat, de tweede
 byte geeft aan op welk adres (#4000-#7fff) de sector staat.
 Let op: dit byte is alleen de hi-byte! Deze opzet is vooral
 handig als een hele diskette gecached moet kunnen worden.
 Het is namelijk het makkelijkst en het snelst.

 De routine gebruikt dus 2 bytes voor het opslaan van de plek
 waar de gecachde sector staat, namelijk ��n voor de
 mapper-page en ��n voor het adres. Na elke gecachde sector
 wordt het adres met 512 verhoogd, maar aangezien de
 adres-pointer alleen de hi-byte bijhoudt, wordt deze met 2
 verhoogd (2*256=512). Zodra het adres boven #8000 uitkomt,
 wordt deze teruggezet op #4000 en wordt de mapper-page met
 ��n verhoogd.

 Nu ga ik verder met de source. Tussen de source door zal ik
 af en toe wat extra informatie geven.

 Eerst eventjes wat constanten...

 bdos:           equ     #f37d
 enaslt:         equ     #0024

 setdta:         equ     26
 diskrd:         equ     47

 minimum_pages:  equ     9
 first_page:     equ     8

 De eerste 4 equ's hebben geen commentaar nodig, lijkt me. De
 waarde minimum_pages geeft aan hoeveel pages van 16kB er
 nodig zijn om de cache-routine te kunnen gebruiken. Het
 geheugen moet trouwens in ��n slot zitten (daar wordt de
 routine namelijk simpel van). In dit geval zijn er 9 blokken
 van 16 kB nodig, dus een geheugen van 144kB of meer is
 vereist. De waarde first_page geeft aan welke mapper-page
 als eerste gebruikt wordt de cache-routine. Ik ga verder met
 de source.

 init:          jp      init_cache
 read:          jp      read_sectors

 Dit zijn de entries voor het gebruik van de cache-routine.
 De routine 'init' initialiseert de cache-routines en 'read'
 wordt gebruikt om sectoren te lezen (vanaf diskette of
 vanuit RAM). Ik ga weer verder.

 init_cache:    call    count_map       ;count number of pages
                cp      minimum_pages   ;enough memory
                jr      c,no_cache      ;no? -> no caching possible

 Eerst wordt de routine 'count_map' aangeroepen om te kijken
 hoeveel geheugen er aanwezig is. Het aantal blokken wordt in
 de accumulator gezet. Vervolgens wordt gekeken of er wel
 genoeg geheugen is om caching toe te passen. Als er te
 weinig geheugen aanwezig is, dan wordt er gesprongen naar de
 routine 'no_cache'. Nog meer source...

                ld      (number_pages),a   ; store number of
                                             available pages

                ld      a,first_page    ;first page to put
                                         sectors in
                ld      (next_page),a
                ld      a,#40           ;first address of
                                         cached sectors
                ld      (next_address),a

                ld      hl,cache_table
                ld      de,cache_table + 1
                ld      bc,1440 * 2 - 1
                ld      (hl),0
                ldir    ; clear cache-table
                ret

 Het aantal aanwezige mapper-pages wordt opgeslagen, de
 geheugen-pointers worden ge�nitialiseerd en de cache-table
 wordt geleegd.

 ; no caching possible
 no_cache:      ld      hl,read_disk1
                ld      (read + 1),hl
                ret

 De entry 'read' wordt omgebogen, zodat een aanroep meteen
 tot diskactie leidt. Caching was immers niet mogelijk...

 ; Count number of pages in memory mapper
 ; Out: A: number of pages
 count_map:     in      a,(#fe)
                push    af
                call    store_old
                call    check_map
                call    restore_old
                pop     af
                out     (#fe),a
                ld      a,d
                or      a
                ret     nz
                dec     a
                ret

 Deze routine kijkt hoeveel geheugen er in de primaire mapper
 zit. 'store_old' bewaard alle bytes die zich op #8000
 bevinden, zodat er geen geheugen verloren gaat door het
 onderzoeken van de mapper-grootte. 'check_map' telt het
 aantal aanwezige mapper-pages. 'restore_old' herstelt het
 geheugen weer en tot slot wordt er nog even gekeken of het
 aantal mapper-pages 0 is (=256 = 4MB). Als dat zo is, dan
 wordt het aantal pages gelijk aan 255 gemaakt (da's meer dan
 genoeg).

 ; save old mapper data
 store_old:     ld      bc,0
                ld      hl,cache_table

 store_old_lp:  ld      a,c
                out     (#fe),a
                ld      a,(#8000)
                ld      (hl),a
                xor     a
                ld      (#8000),a
                inc     hl
                inc     c
                djnz    store_old_lp
                ret

 Dit stukje code bewaard de originele geheugen-inhoud. Op
 adres wordt de waarde 0 gezet, zodat later gezien kan worden
 dat dit stukje geheugen nog niet geteld is.

 ; Count number of pages
 ; Out: D: number of mapper pages
 check_map:     ld      d,0

 check_map_lp:  ld      a,d
                out     (#fe),a

                ld      a,(#8000)
                or      a
                ret     nz

                dec     a
                ld      (#8000),a

                inc     d
                jr      check_map_lp

 Deze routine telt het aantal aanwezige mapper-pages. Als er
 op adres #8000 een 0 staat, dan is de huidige page niet
 meegeteld en kan het aantal pages met ��n verhoogd worden.

 ; Restore old mapper data
 ; In: D: number of mapper-pages
 restore_old:   ld      c,0
                ld      b,d
                ld      hl,cache_table

 restore_old_lp:ld      a,c
                out     (#fe),a

                ld      a,(hl)
                ld      (#8000),a

                inc     hl
                inc     c
                djnz    restore_old_lp
                ret

 Hier wordt de oorspronkelijke geheugeninhoud weer hersteld.

LEES VERDER IN DEEL 2!

 Leest iemand dit? Stuur dan een briefje of E-mailtje naar de redactie!

     ASSEMBLY CURSUS 9 (2)


 We gaan vrolijk door waar we gebleven waren...

 ; Read sectors
 ; In: L: drive (0=A, 1=B)
 ;     DE: first sector
 ;     H: number of sectors
 ;     BC: destinationaddress
 read_sectors:  push    hl

                ld      hl,cache_table
                add     hl,de
                add     hl,de
                ld      a,(hl)
                or      a       ;already cached?
                jp      nz,read_cache

 Eerst wordt gekeken of de sector al gecached is. Als dat zo
 is (A<>0) dan wordt naar de routine read_cache gesprongen
 waar de sector vanuit RAM gelezen wordt.

                push    bc
                ld      a,(number_pages)
                ld      b,a
                ld      a,(next_page)
                cp      b
                pop     bc
                jr      z,read_disk

 Hier wordt gekeken of er nog wel plek in het geheugen is om
 de sector te cachen. Als dat niet zo is, wordt naar
 'read_disk' gesprongen waar de sectoren direct van diskette
 worden gelezen. Let op: HL staat nog op de stack! Dit is zo
 gedaan omdat HL nu nog eventjes nodig is. Lees maar eens
 verder...

                ld      (hl),a         ; save current cache-page

                ld      a,(next_address)
                inc     hl
                ld      (hl),a          ; store hi-byte cache-
                                          address

 De mapper-page en het adres waar de sector moet komen te
 staan in het "cache-geheugen" worden opgeslagen.

                push    bc
                push    de
                ld      d,b
                ld      e,c
                ld      c,setdta
                call    bdos    ;set read address
                pop     de
                pop     bc
                pop     hl      ; drive + number of sectors
                                 back from stack

 Hier wordt het Disk Transfer Address goed gezet.

                push    bc
                push    de
                push    hl
                push    bc      ;destinationaddress
                ld      h,1
                ld      c,diskrd
                call    bdos    ;read 1 sector

 Een sector wordt ingelezen.

                in      a,(#fd)
                pop     hl
                push    af
                push    hl

                ld      a,(next_page)
                out     (#fd),a

                ld      a,(#f342)
                ld      h,#40
                call    enaslt

 Er wordt RAM geselecteerd op #4000-#7fff en de juiste page
 wordt ingesteld.

                ld      a,(next_address)
                ld      d,a
                ld      e,0
                pop     hl      ;Home Location
                ld      bc,512
                ldir    ;move it to cache-memory

 De sector wordt naar het cache-geheugen verplaatst...

                ld      a,(#fcc1)
                ld      h,#40
                call    enaslt

                pop     af
                out     (#fd),a
                pop     hl
                pop     de
                pop     bc

                call    change_pointer  ;change cache-pointer

                jp      read_sectorslp  ;repeat for next sector

 .. en de geheugeninstellingen worden weer hersteld.
 Vervolgens wordt de routine change_pointer aangeroepen waar
 het volgende adres voor de sector wordt veranderd en
 eventueel de volgende pagina. Tot slot wordt gesprongen naar
 read_sectorslp waar gekeken wordt of er nog meer sectoren
 zijn om te lezen.

 change_pointer:ld      a,(next_address)
                inc     a
                inc     a
                ld      (next_address),a        ;next 512 bytes
                cp      #80     ;does it fit in the same page?
                ret     nz      ;Yes -> finished here
                ld      a,#40   ;Begin of mapperpage (#4000)
                ld      (next_address),a
                ld      a,(next_page)
                inc     a
                ld      (next_page),a   ;Next mapperpage for
                                         caching
                ret

 Het volgende adres in het cache-geheugen wordt berekend. Als
 het adres #8000 wordt, wordt het adres weer op #4000 gezet
 en wordt de volgende pagina met ��n verhoogd.

 ; read sector from disk
 read_disk1:    push    hl      ;number of sectors+drive has
                                 to be pushed
 read_disk:     push    de

                ld      d,b
                ld      e,c
                ld      c,setdta
                call    bdos    ;set read address

                pop     de
                pop     hl      ;number of sectors+drive back
                                 from stack
                ld      c,diskrd
                call    bdos    ;read rest of the sectors from
                                 disk
                ret

 Deze routine leest de resterende sectoren van diskette. Naar
 read_disk1 wordt gesprongen als er bij het initialiseren van
 de cache-routine's gebleken is dat er �berhaupt te weinig
 geheugen voor caching is en naar read_disk wordt gesprongen
 als er geen cache-geheugen meer over is.

 ; Move sector from cache-memory to destination address
 ; In: HL: pointer to position-information
 ;     BC: destination address
 ;     DE: number of sectors
 ; Note: number of sectors to be read and drive are pushed on the
 ; stack (HL)
 read_cache:    push    bc
                push    de

                push    bc
                push    de
                push    hl

                ld      a,(#f342)
                ld      h,#40
                call    #24

                pop     hl
                pop     de
                pop     bc

                in      a,(#fd)
                push    af

                ld      a,(hl)
                out     (#fd),a

 Deze routine leest een sector vanuit het cache-geheugen.
 Eerst wordt het juiste geheugen geschakeld..

                inc     hl
                ld      h,(hl)
                ld      l,0
                ld      d,b
                ld      e,c
                ld      bc,512
                ldir

 ..vervolgens wordt de sector verplaatst..

                ld      a,(#fcc1)
                ld      h,#40
                call    #24

                pop     af
                out     (#fd),a

                pop     de
                pop     bc
                pop     hl

 ..en tot slot worden de geheugeninstellingen weer hersteld.

 read_sectorslp:dec     h
                ret     z

                inc     de

                inc     b
                inc     b
                jp      read_sectors

 Het aantal sectoren wordt verlaagd, de te lezen sector wordt
 verhoogd en het bestemmingsadres wordt met 512 verhoogd.

 number_pages:  defb    0

 next_page:     defb    8
 next_address:  defb    #40

 cache_table:   defs    1440 * 2

 Tot slot volgen het aantal ram-pages, de volgende pagina en
 het volgende adres voor het cachen van sectoren en als
 allerlaatste is er de tabel waarin staat of de sectoren
 gecached zijn en waar ze dan gecached zijn.

 Nog een leuk nieuwtje: Smael gaat binnenkort proberen om een
 cache-routine in de FD-routines te implementeren, als er nog
 genoeg ruimte voor is. Ikzelf zie het nut er niet van in,
 maar ja, als hij het zo graag wil...(NvdR: laat die jongen
 toch, hij weet niet beter). Maar goed, het zit er weer op
 voor deze keer. Tot de volgende FD!


Arjan
