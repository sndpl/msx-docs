De assembly cursus gaat verder...
         ASSEMBLY (4)

                             ����
                             ����
                             ����
                             ����


 Het schiet eindelijk een beetje op met deze cursus, maar ja,
 deze cursus is ook maar een kwestie van inspiratie. Voor
 deze keer heb ik wat routines in elkaar geknutseld om de 2
 extra kleuren van de MSX 2 in screen 0 te kunnen gebruiken.
 Ik ga niet vertellen hoe je ze aan moet zetten enzo, want
 dat is een kwestie van wat VDP-registertjes vullen. Nee,
 deze tekst gaat over hoe snel de kleuren op een bepaalde
 positie te zetten of weg te halen.

 Normaal gesproken begint de tabel op adres 2048 in het VRAM.
 Deze tabel is 240 bytes groot (=24*80 bits), tenzij je het
 aantal lijnen op 26,5 zet, want dan is deze tabel 270 bytes
 groot. Vergeet dan ook niet om deze tabel te verplaatsen als
 je meer dan 24 regels gebruikt, want de schermtabel
 overschrijft dan de kleur-tabel (de schermtabel is dan 2160
 bytes groot).

 Maar goed, ��n byte van de kleurtabel geeft voor 8 plaatsen
 aan of ze aan (1) of uit (0) staan. Adres 2048 geeft dus van
 coordinaten (0,0) tot (7,0) aan of ze aan of uit staan,
 adres 2049 van (8,0) tot (15,0) enz. Een regel gebruikt dan
 10 bytes (=80 bits). Het adres van een coordinaat kun je dan
 uitrekenen door de y-coordinaat met 10 te vermenigvuldigen,
 de x-coordinaat door 8 te delen, deze 2 optellen en bij 2048
 (het startadres van de tabel) op te tellen. De volgende
 routine doet dat:

 ; Bereken adres in kleurtabel
 ; In: D: x-coordinaat
 :     E: y-coordinaat
 ; Uit: HL: adres in kleurtabel

 CALCAD: LD    HL,2048          ; Start kleurtabel

         LD    A,D              ; Deel X door 8
         SRL   A
         SRL   A
         SRL   A
         CALL  HLA              ; en tel bij HL op

         LD    A,E              ; Vermenigvuldig Y met 10
         ADD   A,A
         LD    B,A
         ADD   A,A
         ADD   A,A
         ADD   A,B
         CALL  HLA              ; en tel bij HL op
         RET                    ; Einde

 De routine HLA doet niets anders dan HL en A bijelkaar op te
 tellen en maakt alleen gebruik van de registers A en HL.
 Deze routine ziet er als volgt uit:

 HLA:    ADD   A,L
         LD    L,A
         RET   NC
         INC   H
         RET

 Nu kunnen we het adres berekenen van een karakter. Nu moeten
 we nog het bitnummer berekenen. De volgende routine doet
 dat:

 CALCBT: LD    A,D              ; Laad x-coordinaat
         AND   7                ; laagste 3 bits
         LD    B,A              ; aantal keren schuiven
         LD    A,128            ; eerste positie is 128
         RET   Z                ; Positie = 0? Ja -> klaar!
 DIVIDE: RRA                    ; Deel positie door 2
         DJNZ  DIVIDE           ; Herhalen
         RET

 Nu hebben we het bitmasker om een karakter aan te zetten.
 Als je een karakter uit wilt zetten, moet je dit masker
 inverteren (XOR 255). We kunnen voorts een routine schrijven
 die karakters aan en uitzet. Hier komen ze:

 WRTVDP: EQU   #47
 WRTVRM: EQU   #0177
 RDVRM:  EQU   #0174
 BIGFIL: EQU   #016B

 ; CLS-routine
 CLS:    LD    HL,2048
         LD    BC,240
         LD    A,0
         JP    BIGFIL

 ; Print-routine
 ; In: D: X-coordinaat
 ;     E: Y-coordinaat
 ;     B: aantal
 PUT:    PUSH  BC

         CALL  CALCAD           ; Bereken adres

         CALL  RDVRM            ; Lees oude byte
         LD    C,A              ; Opslaan in C

         CALL  CALCBT           ; Bereken bitmasker

         OR    C                ; Originele byte eroverheen
         CALL  WRTVRM           ; Wegschrijven

         INC   D                ; Pas X aan
         LD    A,D
         CP    80               ; Laatste positie?
         JP    C,PUT1           ; Nee -> Y niet aanpassen

         LD    D,0              ; Pas X en Y aan
         INC   E

 PUT1:   POP   BC
         DJNZ  PUT              ; Herhaal routine
         RET

 ; Verwijder alternatieve kleuren
 ; In: D: X-coordinaat
 ;     E: Y-coordinaat
 ;     B: aantal
 UNPUT:  PUSH  BC

         CALL  CALCAD           ; Bereken adres

         CALL  RDVRM            ; Lees byte
         LD    C,A              ; Sla het op

         CALL  CALCBT           ; Bereken bitmasker

         CPL                    ; Inverteer het
         AND   C                ; Originele masker erover
         CALL  WRTVRM           ; Schrijf byte weg

         INC   D                ; Pas X aan
         LD    A,D
         CP    80               ; Laatste positie?
         JP    C,UNPUT1         ; Nee -> Y niet aanpassen

         LD    D,0              ; Pas X en Y aan
         INC   E

 UNPUT1: POP   BC
         DJNZ  UNPUT            ; Herhaal routine
         RET


 Deze routines kunnen natuurlijk nog wat aanpassingen
 gebruiken, zoals de routines CALCBT en CALCAD direct in de
 code te gebruiken, maar voor een cursus moet dat juist niet
 omdat het dan een beetje onoverzichtelijker wordt. De
 routines zelf spreken voor zich, want er staat genoeg
 commentaar bij. Oh ja, de routines staan ook op diskette en
 wel in de file ALTCOL.ASM. Tot een volgende keer!


Vincent
