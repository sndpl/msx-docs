De assembly cursus gaat vrolijk verder...

         ASSEMBLY (6)


 Zoals ik de vorige keer al zei ga ik het deze keer hebben
 over .LIB-files. Omdat dit nogal een uitgebreid onderwerp
 is, beslaat deze tekst 3 delen. Het eerste deel behandelt
 wat theorie en een klein stukje van de source van de
 LIB-maker, deel twee gaat over de rest van die source en
 deel 3 van deze cursus legt uit hoe je .LIB-files moet
 gebruiken in je eigen programma's.


      WAT ZIJN LIB-FILES?

 Een LIB-file is niets anders dan een file waarin alle files
 die een programma gebruikt zitten (vandaar LIB, van
 LIBrary). Een goed voorbeeld van deze files is bijvoorbeeld
 Smumpkin 3, Muzax 3 en de Final Fantasy Slide Show die op
 disk A van FD 35 stond.


       WAAROM LIB-FILES?

 Waarom zou je LIB-files gebruiken als het gebruiken van
 gewone files veel makkelijker is? Hier is een aantal redenen
 voor, namelijk:

 -Het neemt minder ruimte in beslag: Stel je hebt 4 files van
  (ik noem maar wat) 256 bytes. Als deze files los in de
  directory zouden staan, namen ze ieder (op een diskette)
  ��n kilobyte in beslag, dus met z'n allen is dat 4 kB. In
  een LIB-file nemen ze echter maar 1 kB in beslag, omdat de
  files aan elkaar worden geplakt.

 -Het is sneller: Telkens als een nieuwe file wordt geopend,
  moet de computer de juiste filenaam in de directory zoeken.
  Dit kost aardig wat tijd, zeker als de directory helemaal
  vol staat en de te zoeken file de laatste file uit de
  directory is. Een andere manier om snel te laden is alles
  op sector zetten. Dit neemt wel minder ruimte in beslag dan
  gewoon de bestanden op diskette houden, maar LIB-files zijn
  altijd nog effici�nter.

 -Er passen (in theorie) oneindig veel files in: In een
  directory passen (normaal gesproken) slechts 112 files.
  Het aantal files in een LIB-file is echter oneindig, voor
  zover de diskruimte het toelaat tenminste.

 -Het is veel netter dan gewoon alle files op disk kwakken:
  Een directory vol met bestanden is niet echt netjes, zeker
  als je meerdere programma's op ��n diskette zet.


            NADELEN

 Er zijn natuurlijk ook wat nadelen aan het gebruik van
 LIB-files, namelijk:

 -De directory van een LIB-file moet permanent in het
  geheugen staan (hoeft eigenlijk niet, maar het is wel v��l
  sneller). Een erg groot nadeel is dit niet, want meestal
  zal je directory niet meer dan 1 kB in beslag nemen. De
  routine die straks volgt gebruikt per file 16 bytes, dus
  met 64 files heb je 1 kB vol. Gebruik je echter de
  effici�ntste methode, dan kun je in 1 kB ruim 200 files
  zetten! Dit lijkt mij meer dan genoeg.

 -Het is wat extra programmeerwerk. Dit valt in de praktijk
  echter best wel mee, zelfs ik had er geen moeite mee. Dit
  geldt natuurlijk alleen voor de eerste keer, want je
  volgende programma's kun (moet) je gebruik maken van
  dezelfde routine's.


DE OPZET VAN HET PROGRAMMA

 Ik heb gekozen voor een programma dat onder DOS werkt, om de
 simpele reden dat daar gewoon meer geheugen beschikbaar is.
 Verder verwacht het programma de bestanden op drive A en
 komt de LIB-file op drive B. Dit is echter zeer makkelijk
 aan te passen. Harddisk-gebruikers zullen gewoon alles op
 ��n drive doen, net zoals mensen met maar ��n diskdrive
 (tenzij je een lekker grote ramdisk aan kunt maken). Ook
 zijn er geen foutmeldingen. Tot slot staan de namen van de
 bestanden die in de LIB-file moeten komen in de source zelf,
 evenals de naam van de aan te maken LIB-file. Dit heb ik
 gedaan om de source niet al te lang te laten worden, want
 een command-line interpreter kost aardig wat regeltjes. Ook
 is het niet de bedoeling dat ik alles voorkauw, en
 natuurlijk ben ik lui! Een andere geldige reden is dat de
 ruimte van de command-line aardig beperkt is. De beste
 oplossing zou gewoon zijn om alle bestanden in een tekstfile
 te zetten en de naam daarvan door te geven aan het
 programma. Maar goed, dat mogen jullie zelf allemaal wel
 doen.


           DE SOURCE

 Okay, dan komt nu het programma zelf.

                org     #0100

 bdos:          equ     #05
 setdta:        equ     26      ; zet lees/schrijf-adres
 open_file:     equ     15      ; open file
 close_file:    equ     16      ; sluit file
 create_file:   equ     22      ; maak file aan
 read_block:    equ     39      ; laad van disk
 write_block:   equ     38      ; schrijf naar disk

 entry_length:  equ     11 + 2 + 3

 Het programma werkt onder MSX-DOS, dus een org #0100 is op
 z'n plaats (alhoewel dit wel weggelaten kan worden bij
 GEN80. Zo is het echter voor jezelf wel duidelijk dat het
 een DOS-programma is). Verder worden wat naampjes voor disk-
 gebruik gedefinieerd en de lengte die de naam en data van
 een bestand in de directory moet innemen. Dit is hier in
 totaal 16 bytes, namelijk 11 voor de filenaam, 2 voor de
 lengte en 3 voor de plaats in de LIB-file. Dit heeft echter
 als beperking dat de maximale grootte van een file die in de
 LIB-file moet komen 64kB is. Deze grootte zul je echter
 alleen maar overschrijden bij samplekits voor de Moonsound,
 dus zo'n ramp is dat ook weer niet. Ik ga verder...

             MAIN

                call    count_files     ; tel aantal files
                ld      (files),bc

                call    calc_length     ; bereken lengte
                                        ; directory
                ld      (dir_length),hl

                call    make_dir        ; maak directory

                call    save_dir        ; bewaar directory

                call    store_files     ; bewaar files

                ld      de,fcb2
                ld      c,close_file
                call    bdos            ; klaar!
                ret

 Dit is de hoofdlus van het programma. De routine count_files
 telt het aantal files die in de LIB-file moeten komen,
 calc_length berekent de lengte van de directory, make_dir
 maakt de directory aan, save_dir schrijft de directory weg
 en store_files copi�ert alle files naar de LIB-file.
 Tenslotte wordt de LIB-file gesloten en zijn we klaar.

          COUNT FILES

 ; Tel aantal files
 ; In: -
 ; Uit: BC: aantal files
 ; Verandert: AF,BC,DE,HL
 count_files:   ld      hl,file_names
                ld      bc,0
                ld      de,11

 count_files1:  ld      a,(hl)
                or      a
                ret     z
                inc     bc
                add     hl,de
                jr      count_files1

 Deze routine telt het aantal files in de directory. Dit
 wordt gedaan door te kijken of de byte op adres HL een 0 is.
 Als dat zo is, zijn alle files geteld en anders wordt BC
 verhoogd en wordt de zooi herhaald.


       CALCULATE LENGTH

 ; Bereken lengte directory
 ; In: BC: aantal files
 ; Uit: HL: lengte directory
 ; Verandert: AF, BC, DE, HL
 calc_length:   ld      hl,3
                ld      de,entry_length

 calc_length1:  ld      a,b
                or      c
                ret     z
                add     hl,de
                dec     bc
                jr      calc_length1

 Deze routine berekent de lengte van de directory. Dit wordt
 gedaan door dom de waarde entry_length herhaald op te tellen
 en vervolgens er 3 bij op te tellen. De eerste 2 bytes van
 de directory worden namelijk gebruikt om het aantal files
 in de directory aan te geven en de laatste byte van de
 directory wordt gebruikt om aan te geven dat alle files
 geweest zijn. Natuurlijk kan het berekenen sneller gedaan
 worden door een vermenigvuldigingsroutine te gebruiken, maar
 dit was eventjes wat sneller om te programmeren (ja, ik ben
 lui!).

 De routine om de directory te maken is behoorlijk lang,
 daarom zal ik tussen de regels door wat opmerkingen maken en
 niet pas aan het einde.


        MAKE DIRECTORY

 ; Maak directory
 ; In: HL: lengte directory
 ; Uit: -
 ; Verandert: AF, BC, DE, HL
 make_dir:      ld      (getal1),hl
                ld      hl,0
                ld      (getal1+2),hl

                ld      hl,file_names
                ld      de,directory
                ld      bc,(files)

 Dit stukje initialiseert de routine. Op het adres 'getal1'
 staat de offset waar de files na de directory van de
 LIB-file beginnen. Verder wordt het adres waar de filenamen
 staan geladen (HL), het adres waar de directory moet komen
 geladen (DE) en het aantal files geladen (BC).

 make_dir1:     push    bc

                push    hl
                push    de

                ld      bc,11
                ld      de,fname1
                ldir            ; filenaam naar FCB

                pop     de
                pop     hl
                ld      bc,11
                ldir            ; filenaam naar directory

 Register BC hebben we pas op het einde weer nodig dus
 PUSH'en we die even weg. Vervolgens moet de filenaam twee
 keer gecopi�erd worden, namelijk naar de FCB en naar de
 uiteindelijke directory.

                push    hl

                push    de
                ld      de,fcb1
                ld      c,open_file
                call    bdos
                ld      de,fcb1
                ld      c,close_file
                call    bdos
                pop     de
                ld      hl,(length1)

 De file wordt geopend en de lengte wordt opgevraagd.

                ld      a,l
                ld      (de),a
                inc     de
                ld      a,h
                ld      (de),a
                inc     de

 De lengte van de file wordt in de directory gezet.

                ld      ix,getal1      ; bewaar positie in file
                ld      a,(ix)
                ld      (de),a
                inc     de
                ld      a,(ix+1)
                ld      (de),a
                inc     de
                ld      a,(ix+2)
                ld      (de),a
                inc     de

 De plek waar de file in het bestand staat wordt in de
 directory gezet. Dit is een 24-bits getal, daarom wordt dit
 eventjes met register IX gedaan.

                push    de

                ld      iy,getal2
                ld      (iy),l
                ld      (iy+1),h
                ld      ix,getal1
                call    add_32bit

 De lengte van de file wordt opgeteld bij de positie in de
 LIB-file. Hiervoor wordt een 32-bits optel-routine gebruikt,
 welke 'getal1' en 'getal2' gebruikt als parameters en het
 resultaat stopt in 'getal1'.

                pop     de
                pop     hl
                pop     bc
                dec     bc
                ld      a,b
                or      c
                jp      nz,make_dir1
                xor     a
                ld      (de),a
                ret

 Nu worden alle registers teruggehaald en wordt er een lusje
 gemaakt om de boel te herhalen. Als alle files geweest zijn,
 wordt er nog even een 0 weggeschreven en dan is deze routine
 klaar.

LEES VERDER IN DEEL 2
De assembly cursus gaat vrolijk verder...

        ASSEMBLY (6)-2


We gaan weer verder waar we gebleven waren...


        SAVE DIRECTORY

 ; Bewaar directory
 ; In: -
 ; Uit: -
 ; Verandert: AF, BC, DE, HL
 save_dir:      ld      hl,lib_name
                ld      de,fname2
                ld      bc,11
                ldir

                ld      de,fcb2
                ld      c,create_file
                call    bdos            ; cre�er file

                call    intfcb2

                ld      de,files
                ld      c,setdta
                call    bdos

                ld      de,fcb2
                ld      c,write_block
                ld      hl,(dir_length)
                call    bdos            ; schrijf directory weg
                ret

 Deze routine bewaart de net aangemaakte directory. Eerst
 wordt de naam van de library naar het FCB gecopi�erd.
 Vervolgens wordt de file gecre�erd en wordt het FCB
 geinitialiseerd voor het schrijven. Daarna wordt het Disk
 Transfer Address gezet op het adres waar het aantal files
 staat en tot slot wordt de directory weggeschreven.


         STORE FILES

 ; Bewaar files in .LIB-file
 ; In: -
 ; Uit: -
 ; Verandert: AF, BC, DE, HL
 store_files:   ld      bc,(files)
                ld      hl,file_names

 Het aantal files wordt in register BC geladen en het adres
 waar de filenamen staan in register HL.

 store_files1:  push    bc

                ld      bc,11
                ld      de,fname1
                ldir
                push    hl

                call    clrfcb1

                ld      c,open_file
                ld      de,fcb1
                call    bdos

                call    intfcb1

 De filenaam wordt gecopi�erd naar het FCB, het FCB wordt
 vervolgens schoongemaakt en daarna wordt de file geopend.
 Tot slot wordt het FCB ge�nitialiseerd.

                ld      hl,(length1)
                call    copy_file       ; kopieer file

                ld      de,fcb1
                ld      c,close_file
                call    bdos

                pop     hl
                pop     bc
                dec     bc
                ld      a,b
                or      c
                jp      nz,store_files1
                ret

 In register HL komt de lengte van de file en die wordt
 vervolgens gecopi�erd door de routine copy_file. De file
 wordt daarna gesloten en dan wordt het zooitje herhaald.


           COPY FILE

 ; Copieer file naar .LIB-file
 ; In: HL: lengte file
 ; Uit: -
 ; Verandert: AF, BC, DE, HL
 copy_file:     push    hl
                ld      c,setdta
                ld      de,directory
                call    bdos
                pop     hl

 Het lees/schrijfadres wordt op het adres gezet waar de
 directory stond.

 copy_file1:    ld      de,32768
                xor     a
                sbc     hl,de           ; file >32768 bytes?
                jp      c,copy_file2    ; Nee, copieer rest

                push    hl
                ld      hl,32768
                ld      de,fcb1
                ld      c,read_block
                call    bdos

                ld      hl,32768
                ld      de,fcb2
                ld      c,write_block
                call    bdos
                pop     hl
                jp      copy_file1

 Er wordt gekeken of de file groter is dan 32 kB. Als dat zo
 is, dan wordt er 32 kB ingeladen en vervolgens wegge-
 schreven. Daarna wordt de routine herhaald.

 copy_file2:    add     hl,de
                push    hl
                ld      de,fcb1
                ld      c,read_block
                call    bdos
                pop     hl
                ld      de,fcb2
                ld      c,write_block
                call    bdos
                ret

 De resterende data van de file wordt ingelezen en weggeschreven

 ; Tel twee 32-bits getallen op
 ; In: IX: adres getal 1
 ;     IY: adres getal 2
 ; Uit: getal 1 gevuld met resultaat
 ; Verandert: BC,DE,HL
 add_32bit:     ld      l,(ix+0)
                ld      h,(ix+1)

                ld      e,(iy+0)
                ld      d,(iy+1)

                ld      c,(ix+2)
                ld      b,(ix+3)
                add     hl,de
                jr      nc,add_32bit1
                inc     bc
 add_32bit1:    ld      (ix+0),l
                ld      (ix+1),h

 Deze routine telt twee 32-bits getallen bij elkaar op. In IX
 staat het adres van het eerste getal en in IY het adres van
 het tweede getal. Register HL en DE worden geladen met het
 eerste word van de 32-bits waarde en register BC met het
 tweede word van het eerste getal. Daarna wordt DE bij HL
 opgeteld en als er een overflow plaats vindt, wordt het
 tweede word met ��n verhoogd. Tot slot wordt het eerste word
 weggeschreven.

                ld      l,(iy+2)
                ld      h,(iy+3)
                add     hl,bc
                ld      (ix+2),l
                ld      (ix+3),h
                ret

 Het tweede word van het tweede getal wordt in register HL
 geladen en wordt bij het tweede word van het eerste getal
 opgeteld en vervolgens opgeslagen.

 ; Maak FCB 1 schoon
 ; In: -
 ; Uit: -
 ; Verandert: AF, BC, DE, HL
 clrfcb1:       ld      hl,fcbdat1
                ld      bc,25
                xor     a
                ld      (hl),a
                ld      de,fcbdat1 + 1
                ldir
                ret

 Deze routine maakt het eerste FCB snel schoon met een
 truukje met LDIR. Het eerste adres wordt namelijk eerst op 0
 gezet en het bestemmingsadres wordt dan ��n hoger dan het
 beginadres. Met LDIR wordt vervolgens het FCB gevuld met
 nullen.

 ; Initialiseer FCB 1
 ; In: -
 ; Uit: -
 ; Verandert: AF, HL
 intfcb1:       ld      hl,0
                ld      (fcb1 + 12),hl
                ld      (fcb1 + 33),hl
                ld      (fcb1 + 35),hl
                xor     a
                ld      (fcb1 + 32),a
                inc     hl
                ld      (fcb1 + 14),hl
                ret

 Deze routine initialiseert het eerste FCB voor het lezen.
 Het current block (fcb1 + 12) wordt op 0 gezet, evenals
 random record (fcb1+32/33/35). Tot slot wordt de record-
 lengte op 1 gezet.

 ; Initialiseer FCB 2
 ; In: -
 ; Uit: -
 ; Verandert: AF, HL
 intfcb2:       ld      hl,0
                ld      (fcb2 + 12),hl
                ld      (fcb2 + 33),hl
                ld      (fcb2 + 35),hl
                xor     a
                ld      (fcb2 + 32),a
                inc     hl
                ld      (fcb2 + 14),hl
                ret

 Deze routine doet hetzelfde als de routine intfcb1.

 getal1:        defw    0,0     ; opslagplaats voor de twee
 getal2:        defw    0,0     ; 32-bits getallen

 dir_length:    defw    0       ; lengte van directory

 ; Het eerste FCB
 fcb1:          defb    1       ; drive (0=default, 1 = A:)
 fname1:        defm    "           "
 fcbdat1:       defb    0,0,0,0
 length1:       defb    0,0,0,0,0,0,0,0,0,0,0
                defb    0,0,0,0,0,0,0,0,0,0,0

 ; Het tweede FCB
 fcb2:          defb    2       ; drive (0=default, 2 = B:)
 fname2:        defm    "           "
                defb    0,0,0,0,0,0,0,0,0,0,0,0,0
                defb    0,0,0,0,0,0,0,0,0,0,0,0,0

 ; Naam van de te maken .LIB-file
 lib_name:      defm    "???????????"

 ; Lijst met files die in de .LIB-file moeten
 file_names:    defm    "???????????"

                defb    0       ; 0 = laatste gehad

 files:         defw    0       ; bewaarplaats aantal files
 directory:     nop

 Vul bij lib_name de aan te maken LIB-file in en bij
 file_names de bestanden die je erin wilt hebben. De lengte
 van de naam is 11 letters en er mag GEEN '.' gebruikt
 worden. Na de laatste file moet een 'defb 0' staan, om aan
 te geven dat er geen files meer komen.


LEES VERDER IN DEEL 3
De assembly cursus gaat vrolijk verder...

        ASSEMBLY (6)-3


We gaan weer verder waar we gebleven waren...

 Tot slot bespreek ik de routine's om de LIB-files in je
 eigen programma's te gebruiken (m��r wil ik ook niet meer
 doen). Deze routine's zijn stukken simpeler, daarom besteed
 ik ook maar ��n tekst aan dit gedeelte.

 Er zijn drie routine's nodig, namelijk een routine die de
 file opent en de directory inleest, een routine die een file
 opzoekt in de directory en de pointers goed zet en een
 routine die een (gedeelte van een) file inlaadt.


           DE SOURCE

 bdos:          equ     #f37d
 fopen:         equ     15      ; open file
 fclose:        equ     16      ; sluit file
 rdblk:         equ     39      ; lees blok
 setdta:        equ     26      ; zet lees/schrijfadres

 ; Routine's voor het gebruik van een .LIB-file
 init_dir:      jp      load_dir        ; laad directory in
 file_find:     jp      search_file     ; Zoek file
 load_file:     jp      load_block      ; Laad file in

 Dit zijn de routine's die het inlezen van de directory, het
 zoeken van een file in de directory en het lezen van een
 file uitvoeren.


        LOAD DIRECTORY

 ; Laad directory
 ; In: HL: Adres filename
 ;     DE: Bestemmingsadres directory
 load_dir:      push    de
                push    de
                push    hl

                call    clrfcb

                pop     hl

                ld      bc,11
                ld      de,fname
                ldir

                pop     de
                ld      c,setdta
                call    bdos

 Het FCB wordt schoongemaakt, de naam van de LIB-file wordt
 in het FCB gezet en vervolgens wordt het lees/schrijf-adres
 gezet.

                ld      de,fcb
                ld      c,fopen
                call    bdos

                call    intfcb

                ld      hl,2
                ld      de,fcb
                ld      c,rdblk
                call    bdos

 De file wordt geopend, het FCB wordt ge�nitialiseerd en het
 aantal files in de directory wordt gelezen (eerste 2 bytes).

                pop     hl
                ld      c,(hl)
                inc     hl
                ld      b,(hl)

                ld      hl,1
                ld      de,11 + 3 + 2   ; lengte entry
 calc_length:   add     hl,de
                dec     bc
                ld      a,b
                or      c
                jp      nz,calc_length

 Het aantal files komt in BC terecht en de lengte van de
 directory wordt berekend. Dit kan wel beter met een echte
 vermenigvuldigingsroutine, maar dit was eventjes sneller
 programmeren.

                ld      de,fcb
                ld      c,rdblk
                call    bdos            ; Laad directory in
                ret

 De directory wordt geladen en klaar is de routine.


          SEARCH FILE

 ; Zoek file op in directory en stel record number in
 ; In: HL: adres te laden file-naam
 ;     DE: adres directory
 ; Uit: HL: lengte file
 ;      A: 0 indien gelukt, 255 indien mislukt
 search_file:   push    hl
                push    de

                call    cmp_str

                or      a
                jp      z,search_file1

 De file wordt vergeleken met de huidige file in de directory.
 Als A nul is, is de file gevonden.

                pop     de
                ld      hl,11 + 3 + 2
                add     hl,de
                ex      de,hl   ; DE = positie in directory
                pop     hl
                jp      search_file

 De pointer naar de directory wordt verhoogd en de routine
 herhaalt zichzelf.

 search_file1:  ex      de,hl   ; HL = adres in directory

                ld      e,(hl)
                inc     hl
                ld      d,(hl)  ; laad lengte
                inc     hl
                push    de

 De lengte van de file wordt uitgelezen en gePUSHed.

                ld      de,fcb + 33
                ld      bc,3
                ldir            ; Record Number naar FCB

                xor     a
                ld      (de),a

 De positie in de file wordt in het FCB gezet.

                pop     hl      ; lengte in HL

                pop     de
                pop     de
                ret

 De lengte van de file komt in HL en de routine is klaar.

      LOAD (PART OF) FILE

 ; Lees een (gedeelte van een) file in
 ; In: HL: aantal te laden bytes
 ; Opmerking: zelf bestemmingsadres goedzetten
 load_block:    ld      de,fcb
                ld      c,rdblk
                jp      bdos

 Geen uitleg nodig lijkt mij.

 ; Vergelijk strings met elkaar
 ; In: HL: adres eerste string
 ;     DE: adres tweede string
 ; Uit: A: 0 = gelijk, anders = ongelijk
 cmp_str:       ld      b,11

 cmp_str1:      ld      a,(de)
                cp      (hl)
                ret     nz
                inc     hl
                inc     de
                djnz    cmp_str1
                xor     a
                ret

 Vergelijk twee strings met elkaar. Ook hier geen uitleg
 nodig, lijkt mij.

 ; Initialiseer FCB
 ; in: niets
 ; uit: niets
 ; Verandert: alles
 clrfcb:        ld      hl,fcbdat
                ld      bc,25
                xor     a
                ld      (hl),a
                ld      d,h
                ld      e,l
                inc     de
                ldir
                ret

 Zie vorige tekst.

 ; init fcb
 ; in: niets
 ; uit: niets
 ; Verandert: af, hl
 intfcb:        ld      hl,0
                ld      (fcb+12),hl     ; bloknummer op 0

                ld      (fcb+33),hl     ; random block op 0
                ld      (fcb+35),hl

                xor     a
                ld      (fcb+32),a      ; current record op 0

                inc     hl
                ld      (fcb+14),hl     ; bloklengte op 1
                ret

 Zie vorige tekst.

 ; file control block
 fcb:           defb    0               ; drive
 fname:         defm    "???????????"   ; filename
 fcbdat:        defb    0,0             ; current block
                defb    0,0             ; block lengte
 length:        defb    0,0,0,0         ; file lengte
                defb    0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0

 Ook in deze routine's zijn natuurlijk weer verbeteringen
 mogelijk. Zo zou je een error kunnen laten geven als een
 file niet gevonden wordt, al is dat niet echt nodig als je
 een programma uitbrengt.

 De sources staan natuurlijk ook op disk. De library-maker
 heet MAKELIB.GEN en de library-lezer heet USELIB.GEN. Het is
 niet verboden om de sources in je eigen programma's te
 gebruiken, maar het is beter als je je eigen routine's
 maakt. Tot de volgende keer!

 Arjan Bakker
