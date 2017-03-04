Arjan heeft het over de Mathpack en ik snap er weer eens geen hout van...

       DE MATHPACK (2)


 Een vorige keer schreef ik dat ik nog niet alle functies van
 de MathPack had. Nu heb ik ze wel, dus kan ik weer een
 lijstje met functies geven. Ik begin met ...


   ...DATA-VERPLAATSINGEN

 Naam    Adres  Functie      Type              Verandert
 MAF     #2C4D  DAC  -> ARG  Dubbele precisie  A,B,D,E,H,L
 MAM     #2C50  (HL) -> ARG  Dubbele precisie  A,B,D,E,H,L
 MOV8DH  #2C53  (HL) -> (DE) Dubbele precisie  A,B,D,E,H,L
 MFA     #2C59  ARG  -> DAC  Dubbele precisie  A,B,D,E,H,L
 MFM     #2C5C  (HL) -> DAC  Dubbele precisie  A,B,D,E,H,L
 MMF     #2C67  DAC  -> (HL) Dubbele precisie  A,B,D,E,H,L
 MOV8HD  #2C6A  (DE) -> (HL) Dubbele precisie  A,B,D,E,H,L
 XTF     #2C6F  DAC <-> (SP) Dubbele precisie  A,B,D,E,H,L
 PHA     #2CC7  (SP) -> ARG  Dubbele precisie  A,B,D,E,H,L
 PHF     #2CCC  (SP) -> DAC  Dubbele precisie  A,B,D,E,H,L
 PPA     #2CDC  ARG  -> (SP) Dubbele precisie  A,B,D,E,H,L
 PPF     #2CE2  DAC  -> (SP) Dubbele precisie  A,B,D,E,H,L
 PUSHF   #2EB1  (SP) -> DAC  Enkele precisie   D,E
 MOVFM   #2EBE  (HL) -> DAC  Enkele precisie   B,C,D,E,H,L
 MOVFR   #2EC1  CBED -> DAC  Enkele precisie   D,E
 MOVRF   #2ECC  DAC  -> CBED Enkele precisie   B,C,D,E,H,L
 MOVRMI  #2ED6  (HL) -> CBED Enkele precisie   B,C,D,E,H,L
 MOVRM   #2EDF  (HL) -> BCDE Enkele precisie   B,C,D,E,H,L
 MOVMF   #2EE8  DAC  -> (HL) Enkele precisie   A,B,D,E,H,L
 MOVE    #2EEB  (DE) -> (HL) Enkele precisie   B,C,D,E,H,L
 VMOVAM  #2EEF  (HL) -> DAC  VALTYP            B,C,D,E,H,L
 MOVVFM  #2EF2  (HL) -> (DE) VALTYP            B,C,D,E,H,L
 VMOVE   #2EF3  (DE) -> (HL) VALTYP            B,C,D,E,H,L
 VMOVFA  #2F05  ARG  -> DAC  VALTYP            B,C,D,E,H,L
 VMOVFM  #2F08  (HL) -> DAC  VALTYP            B,C,D,E,H,L
 VMOVAF  #270D  DAC  -> ARG  VALTYP            B,C,D,E,H,L
 VMOVMF  #2710  (HL) -> DAC  VALTYP            B,C,D,E,H,L


           REKENEN

 De volgende routine's zijn wel handig (niet allemaal, maar
 de meeste wel):

 Naam    Adres  Functie         Verandert
 UMULT   #314A  DE = BC * DE    Alles
 ISUB    #3167  HL = HL - DE    Alles
 IADD    #3172  HL = HL + DE    Alles
 IMULT   #3193  HL = HL * DE    Alles
 IDIV    #31E6  HL = DE / HL    Alles
 IMOD    #323A  HL = DE mod HL  Alles

 Het resultaat van deze functies komt ook op DAC+2 te staan
 en VALTYP wordt op 2 gezet. Als je wilt rekenen, gebruik je
 de routine's ISUB en IADD natuurlijk niet. De deling- en
 vermenigvuldigingsroutine's zijn echter wel weer handig, als
 je je code niet al te groot wilt laten worden tenminste.


       VERGELIJKINGEN

 Met de volgende routine kun je getallen met elkaar
 vergelijken. Na het aanroepen van een van deze functies
 wordt in A een waarde teruggegeven. Als A 0 is, dan waren de
 twee variabelen gelijk, als A 1 is, dan was de linker
 variabele kleiner dan de rechter en als A 255 is, dan was de
 linker groter dan de rechter variabele. De routines
 veranderen overigens alle registers.

 Naam    Adres  Functie                 Precisie
 FCOMP   #2F21  Vergelijk CBDE met DAC  Enkel
 ICOMP   #2F4D  Vergelijk DE met HL     Integer
 XDCOMP  #2F5C  Vergelijk ARG met DAC   Dubbel

 De routine ICOMP is wat mij betreft overbodig, want die zit
 ook al gewoon in de BIOS (DCOMPR - RST #20) en meestal zal
 je de code van die routine voor de snelheid ook al direct in
 je routine zetten.


        SLECHTE CODE

 Wat overigens opvalt bij het bestuderen van de routine's van
 de Mathpack, is dat de routine's niet altijd gemaakt zijn op
 snelheid. Zo kom je bijvoorbeeld de volgende source tegen
 (met fictieve namen eventjes):

 move:  ld      a,(hl)
        ld      (de),a
        inc     hl
        inc     de
        djnz    move
        ret

 Dit kun je veel beter gewoon met LDIR doen. Register C wordt
 dan wel aangepast wat eerder niet gebeurde, dus even
 PUSH'en, POP'en register B 0 maken is wel noodzakelijk. Op
 een Turbo R kun je dit makkelijk aanpassen (bij de
 RAM-mode), op een MSX2(+) zal het heel wat meer moeite
 kosten.

 De volgende keer komen de resterende routine's van de
 MathPack aan bod.


 Arjan Bakker
