De assembly cursus gaat verder...
       ASSEMBLY DEEL 3


 Welkom, welkom. Welkom bij het derde deel van deze cursus
 assembly. Dit maal komen er 2 routine's aan bod, namelijk
 een 8bits maal 16bits routine en een snelle copy routine om
 het beeld snel op te bouwen.


      VERMENIGVULDIGEN

 JW schreef op FD 31 dat hij een uur nodig had om een 8bits
 maal 16bits routine te perfectioneren. Da's niet echt nodig,
 je moet gewoon een 16bits routine pakken en je sloopt er wat
 commando's uit en het werkt.


         DE ROUTINE

 ; 8bits maal 16bits
 ; In: A, DE: te vermenigvuldigen waarden
 ; Uit: HL: resultaat

 MULUW:  LD     HL,0

 MULUW1: RRA
         JR     NC,MULUW2
         ADD    HL,DE
 MULUW2: OR     A
         RET    Z
         SLA    E
         RL     D
         JP     MULUW1


           UITLEG

 Ok�, de uitleg stond al op FD 30, dus komt 'ie hier niet
 meer, op de veranderingen na natuurlijk. Allereerst de
 registers. De accu en DE zijn het makkelijkst om te
 gebruiken als input. De accu omdat je daarin snel waardes
 kunt laden en DE omdat in HL het resultaat komt en het
 handig is om BC te kunnen gebruiken als lusteller zonder de
 stack te gebruiken. De 16bits optelinstructie ADD werkt
 alleen met HL, dus moet het resultaat wel in HL komen.

 Toch even een korte uitleg. RRA schuift A 1 bit naar rechts.
 Als carry gezet is, moet DE bij HL opgeteld worden. Dan
 wordt er gecontroleerd of A al 0 is. Als dat zo is, is de
 routine klaar. Is dat niet zo, dan wordt DE met 2
 vermenigvuldigt en begint de routine overnieuw.

 Als je de routine niet snapt, lees dan FD 30 even en als het
 goed is snap je het dan wel (anders ben je een domme aap).


     SNELLE COPY ROUTINE

 Stel, je wilt snel een screen 5 scherm opbouwen. Da's niet
 moeilijk. Stel, je routine is te traag. Dat kan gebeuren.
 Dan is hier de oplossing voor je probleem, namelijk een
 snelle copy-routine.

 Laten we voor het gemak ervan uitgaan dat je patterns van
 8*8 gebruikt en dat je voor elke pattern 1 byte gebruikt.
 Omdat er maar 32 tekens op ��n rij kunnen, heb je maar 5
 bits nodig om aan te geven welk teken van een rij je nodig
 hebt. Er blijven dan 3 bits over die aangeven welke rij je
 gebruikt. Je kunt de X-positie dan uitrekenen met:

         AND     31
         ADD     A,A            ; Met 8 vermenigvuldigen
         ADD     A,A            ; (32*8=255)
         ADD     A,A

 En de Y-positie kun je uitrekenen met:

         AND     224
         SRL     A              ; Door 4 delen (32/4=8)
         SRL     A

 Het omrekenen van een byte naar coordinaten gaat dus als
 volgt:

 MOVPAT: LD      A,(HL)         ; Lees byte

         AND     31             ; 32 patterns per rij.
         ADD     A,A            ; Bereken X
         ADD     A,A
         ADD     A,A            ; A = A * 8

         LD      (SX),A         ; Sla X op

         LD      A,(HL)         ; Lees byte weer

         AND     224            ; 8 rijen
         RRA                    ; Bereken Y
         RRA                    ; Door 4 delen

         LD      (SY),A         ; Sla Y op

 Vervolgens moet de pattern geprint worden.

         PUSH    HL
         CALL    WAITVDP        ; Wacht op VDP
         CALL    COPY           ; Kopi�er pattern
         POP     HL

         LD      A,(DX)         ; Pas bestemming X aan
         ADD     A,8
         LD      (DX),A

         JR      NZ,MOVPAT1

         LD      A,(DY)         ; Pas bestemming Y aan
         ADD     A,8
         LD      (DY),A

 MOVPAT1:
         INC     HL             ; Pas pointer aan

 Zo, da's heel wat. Deze routine past ook automatisch de
 pointer naar de screendata aan en ook de bestemmings-
 co�rdinaten worden aangepast. Bouw er een lusje omheen en je
 scherm wordt snel opgebouwd. Gebruik eventueel geen CALL
 WAITVDP en CALL COPY, maar prop die routine's gewoon in deze
 routine. Voor de zekerheid komen hier nog even de routines
 WAITVDP en COPY.


     WAT EXTRA ROUTINES

 WAITVDP:                       ; Wacht totdat VDP klaar is
         LD     A,2
         OUT    (#99),A
         LD     A,#8F
         OUT    (#99),A
         IN     A,(#99)
         EX     AF,AF
         XOR    A
         OUT    (#99),A
         LD     A,128+15
         OUT    (#99),A
         EX     AF,AF
         RRA
         JR     C,WAITVDP
         RET

 COPY:   LD    A,32                     ; Register 32
         OUT   (#99),A
         LD    A,17+128                 ; Auto-increment
         OUT   (#99),A
         LD    C,#9B
         LD    B,15
         LD    HL,SX
         OTIR                           ; Pomp bytes naar
         RET                            ; register

 SX:     DEFB  0,0
 SY:     DEFB  0
 P1:     DEFB  0
 DX:     DEFB  0,0
 DY:     DEFB  0
 P2:     DEFB  0
 NX:     DEFB  8,0
 NY:     DEFB  8,0
 REST:   DEFB  0,0
 COMMAN: DEFB  #D0


          TOT SLOT

 Eventueel kun je in plaats van ��n OTIR 15 OUTI's gebruiken,
 want dat is weer ietsje sneller. LD B,15 kan dan komen te
 vervallen. Verder geldt: Hoe groter de patterns, des te
 sneller de routine.

 Als je nog idee�n hebt voor een onderwerp, stuur het dan op
 naar de FD en misschien komt er dan wel een tekstje over dat
 onderwerp.

 De sources staan ook op disk onder de naam MULUW.ASM en
 SCRCOPY.ASM.

Arjan Bakker
