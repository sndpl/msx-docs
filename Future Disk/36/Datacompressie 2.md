Data compressie is een heel handig iets. Het zorgt ervoor dat je meer dan drie teksten per FD kan lezen, handig toch?

     DATA-COMPRESSIE (2)


 De vorige aflevering van deze tekst eindigde met de
 opmerking van Koenie dat ik de behandelde theorie iets meer
 uit zou moeten werken en guess what: dat doe ik hier dan ook
 (de klant is koning). Natuurlijk moet je zelf nog wat
 knutselen aan deze routine's, want anders is er ook niets
 aan.

 De (de-)cruncher is ontstaan omdat ik bezig was met een
 muziekprogramma welke nu al tijden gecanceld is (NvdR: Cool,
 heeft het muziekprogramma toch nog iets opgebracht!).
 Ongecomprimeerde muziekdata neemt veel ruimte in beslag en
 daarom maakte ik een simpele (de-)cruncher, welke 8 bytes
 (wow!) kon crunchen. Natuurlijk zorgden de routine's
 eromheen ervoor dat alles gecrunched werd, maar de basis was
 8 bytes, omdat er 8 bits in ��n byte zitten.

 Voor diegenen die de vorige deel niet hebben gelezen komt
 hier eventjes een korte uitleg. In een file komen bepaalde
 bytes vaak voor, zoals bijvoorbeeld in teksten de byte 32 (=
 spatie) vaak voorkomt. Door nu voor elke byte met ��n bit
 aan te geven of een byte gelijk is aan de byte die het meest
 voorkomt, kun je (als het meezit) de files behoorlijk
 kleiner maken. Een voorbeeldje:

 De file bestaat uit de volgende 16 bytes:
 0 25 4 0 3 0 0 195 0 5 201 0 0 0 65 32

 De 0 komt het meest voor (8 keer). Door ��n bit te gebruiken
 om aan te geven of een byte 0 is (bit geset) of ongelijk
 (bit gereset), kun je de nullen achterwege laten en alleen
 de andere bytes bewaren. Gecrunched komt de data er dan zo
 uit te zien (de eerste byte van de regel is binair, de rest
 is decimaal):
 %10010110 25  4   3   195
 %10011100 5   201 65  32

 De data is nu nog maar 10 bytes lang, een besparing van zo'n
 40%. Natuurlijk moet je de computer ook laten weten wat er
 bedoeld wordt als een bit ��n is, dus kost het weer een byte
 extra.


          CRUNCH IT

 Hier volgt de crunch-routine, met achter de code wat
 commentaar zodat het mogelijk moet zijn om deze routine te
 begrijpen.

 ; Crunch-routine
 ; In: HL: adres van ongecomprimeerde data
 ; Uit: HL: adres van gecomprimeerde data
 ;      E: aantal bytes
 ; Verandert: AF, BC, DE, HL, IX
 crunch: ld      ix,crubuf+1    ; adres waar ongecrunchde
                                ; bytes komen
         ld      d,0            ; geeft aan welke bytes
                                ; gecrunched zijn
         ld      e,1            ; Lengte gecrunchde data
         ld      b,8            ; 8 bytes te comprimeren

 crunc1: ld      a,(hl)         ; lees byte
         inc     hl
         or      a              ; Gelijk aan 0 ?
         jp      z,crunc2       ; Ja -> crunc2
         ld      (ix),a         ; Nee -> sla waarde op
         inc     ix             ; verhoog pointer
         sla     d              ; Vul 0 in in statusbyte
         inc     e              ; Verhoog aantal met 1
         djnz    crunc1         ; herhalen
         ld      hl,crubuf
         ld      (hl),d         ; bewaar status
         ret                    ; finished!

 crunc2: sla     d              ; bits opschuiven
         inc     d              ; Vul 1 in bij status
         djnz    crunc1         ; herhalen
         ld      hl,crubuf
         ld      (hl),d         ; bewaar status
         ret                    ; finished!

 crubuf: defs    9              ; Plek waar data komt

 Deze routine gaat er van uit dat de meest voorkomende byte 0
 is. Als je dit wilt aanpassen, moet je na de instructie
 'LD B,8' de instructie 'LD C,x" zetten, waarbij de x staat
 voor het getal dat het meest voorkomt. Verder moet je 'OR A'
 veranderen in 'CP C' en klaar is je routine.


         DECRUNCH IT

 Okay, de crunch-routine was niet echt moeilijk h�! Dan volgt
 hier nu de decrunch-routine, welke net zo simpel is!

 ; decrunch-routine
 ; In: hl: data-string
 ; Uit: hl: uitgepakte string
 ; Verandert: AF, BC, DE, HL, IX
 dcr:    ld      a,(hl)         ; statusbyte
         ld      ix,dcrbuf      ; Plek waar uncrunchde data
                                ; terecht komt
         ld      bc,8 * 256     ; 8 bytes (B), vulbyte=0 (C)
         inc     hl

 dcr1:   add     a,a            ; Hoogste bit naar carry
         jp      c,dcr2         ; Carry gezet -> dcr2
         ld      d,(hl)         ; Haal byte uit gecrunchde
         ld      (ix),d         ; data en bewaar het
         inc     hl             ; verhoog pointers
         inc     ix
         djnz    dcr1           ; herhaal de zooi
         ld      hl,dcrbuf
         ret                    ; finished!

 dcr2:   ld      (ix),c         ; vul een nulletje in
         inc     ix             ; Verhoog pointer
         djnz    dcr1           ; herhalen maar
         ld      hl,dcrbuf
         ret                    ; finished!

 dcrbuf: defs    9              ; plek waar data komt


           TOT SLOT

 Deze routine's zijn redelijk snel, zo'n 20kB per seconde is
 toch wel te halen. Toch zijn er verbeteringen denkbaar, door
 niet groepjes van 8 bytes te gebruiken, maar gewoon van ��n
 byte. Je moet dan eerst ��n bit wegschrijven om aan te geven
 of de data gecrunched is (1) of niet (0) en als de data niet
 gecrunched is, bewaar je de te crunchen byte en kun je dit
 herhalen. Dit is wel iets lastiger, omdat je dan met
 bitstreams moet gaan werken. Het heeft dan wel als voordeel
 dat de gecrunchte files iets kleiner worden, maar veel maakt
 dat ook niet uit.

 De source van de uitgelegde routines staan ook op disk en
 wel onder de naam CRUNCH.GEN. Veel plezier!

Arjan Bakker
