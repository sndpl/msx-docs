
      MCODE SPECIAL (4)

           part 1


 We gaan verder met deze zeer special cursus...

 VEILIG (de routine die een vlag plaats/weghaald)

 Als je op SHIFT drukt moet de computer een vlag plaatsen,
 stond er echter al een vlag wil je graag dat deze weggehaald
 wordt. De snelheid van de machinetaal levert dit keer een
 probleem op: als de speler 1 keer op SHIFT gedrukt denkt te
 hebben is onze hoofdlus vaak meerdere keren (soms wel 4) door-
 lopen, hetgeen tot gevolg heeft dat je de vlag aan en uit
 ziet flikkeren i.p.v. netjes omwisselen. Je kunt dit voorkomen
 m.b.v de volgende routine, die je moet plaatsen voordat
 op de toets gereageerd wordt:

   LD      A,(VORIGSHIFT)   ; als de vorige keer shift al
   AND     A                ; ingedrukt was, nu niet meer
   RET     NZ               ; reageren (voorkomen van: aan/uit)
   INC     A                ; wordt 1 (dus NotZero)
   LD      (VORIGSHIFT),A   ; volgende keer niet reageren

 Als er nu op SHIFT gedrukt wordt, wordt dit opgeslagen in het
 label VORIGSHIFT(er staat 1 in).
 Zodra nu de volgende keer weer op SHIFT wordt gedrukt (door
 te lang inhouden) wordt dit geblokkeerd door de RET NZ.
 Als SHIFT losgelaten wordt moet VORIGESHIFT natuurlijk wel
 weer op nul gezet worden, omdat je anders maar 1 vlag kan
 plaatsen per spel.

 Als er op Shift gereageerd moet worden vindt eerst weer een
 aanroep van de routine die eerder beschreven werd:

         CALL    HAALBLOKNR        ; in A komt bloknr.

 In A staat nu het bloknummer, dit is dus de plaats in het
 dataveld waarop gedrukt werd en waarop de speler een vlag wil
 plaatsen. Net als in de routine GEDRUKT wordt eerst weer geke-
 ken of je op deze plaats wel een vlag mag plaatsen:

    LD      HL,DATA
    LD      C,A
    LD      B,0               ; nu in HL het DATA-blokje,
    ADD     HL,BC             ; waarop je stond.
    BIT     6,(HL)            ; al zichtbaar ?
    RET     NZ                ; ja: geen vlag

 Dan wordt onderzocht of er een vlag geplaatst moet worden of
 weggehaald moet worden:

 BIT     5,(HL)            ; was het al een vlag ?
 JR      Z,NUVLAG          ; we plaatsen een vlag
 RES     5,(HL)            ; weg met de vlag
 LD      A,144             ; Source Xco van Onzichtbaar blokje
 LD      (SX),A
 LD      HL,BLOKDATA       ; laat ook zien.
 JP      COPY              ; einde VEILIG

 Als de vlag weggehaald moets worden wordt er een onzichtbaar
 blokje overheen geplaatst en in het dataveld aangegeven dat
 de vlag weg is. Moest de vlag geplaatst worden volgt de
 volgende routine:

NUVLAG:
         SET     5,(HL)            ; Zet DATAblokje op vlag.
         LD      A,160             ; Source Xco van vlag
         LD      (SX),A            ; DX en DY al in HAALBLOKNR
                                     gezet
         LD      HL,BLOKDATA       ; laat ook een vlag zien.
         JP      COPY              ; einde VEILIG


TESTEIND (testen of je gewonnen bent)

 Testen of alles zichtbaar is wat zichtbaar moet zijn, komt er
 op neer door te kijken of het aantal zichtbare blokjes (dat we
 in GEDRUKT geteld hebben) + het aantal mijnen (dat vast ligt) =
 het aantal blokjes op het scherm (16*13). De volgende routine
 doet dat:

TESTEIND:     LD      A,(AANTALMIJNEN) ; als alle blokjes die
                                         geen mijn zijn
              LD      B,A
              LD      A,(AANTALZICHTBAAR) ; zichtbaar zijn
              ADD     A,B
              CP      MAXBLOKJES ; heb je gewonnen.
              RET     NZ
              LD      A,1
              LD      (EINDBT),A
              jp      #c0
              RET

 Merk op dat er, omdat deze routine in de hoofdlus (altijd)
 wordt aangeroepen, nu een onafgebroken rij BEEP's klinken als
 je gewonnen hebt.
 Als je wilt kun je in de hoofdlus een test op EINDBT toevoegen,
 die springt naar een soort van einddemo, maar ik vind dit niet
 interessant genoeg om zoiets te gaan programmeren (lees: ik
 ben hier te lui voor) (aangezien ene K.Dols er toch niet aan
 begint deze tekst te lezen verwacht ik hier geen N.V.D.R .....
 of toch??.....)(NvdR: Gaap gaap).

 Zo, het spel is nu in principe klaar en kan al gespeeld wor-
 den, maar we willen nog graag dat het drukken op een leeg
 hokje een soort kettingreactie van zichtbaar maken tot gevolg
 heeft. Hoe kun je nu zoiets bereiken ?
 Als een leeg hokje zichtbaar wordt gemaakt, wordt naar de
 routine CLEARDATA gesprongen.
 In deze routine wordt linksboven op het scherm begonnen met
 kijken of er een leeg hokje staat (in praktijk wordt in het
 DATAveld gekeken).
 Zolang als er geen leeg hokje wordt gevonden wordt er 16 bij
 de xco opgeteld tot het einde van de regel bereikt wordt.
 Dan wordt er 16 bij de yco opgeteld en weer begonnen op
 xco=0 ect. tot het einde van het scherm (en DATAveld) bereikt
 is.
 Als er wel een leeg hokje gevonden wordt, wordt er doorge-
 sprongen naar de routine DOCLEAR.
 In DOCLEAR gebeurt het eigenlijke zichtbaar-maak-werk. Als er
 een hokje zichtbaar gemaakt wordt, wordt de teller FOUND
 verhoogd. De routine wordt dan nogmaals herhaald (dus het hele
 scherm wordt opnieuw onderzocht naar e.v. (nieuwe, maar ook
 oude) lege hokjes: dit is dus de kettingreactie.
 Als er tenslotte geen nieuwe hokjes zichtbaar gemaakt worden,
 wordt de routine VULSCHERM aangeroepen, die het hele scherm
 opnieuw vult met blokjes.

 Dat regelt de volgende routine:

 Welke routine? Zie voor deze routine tekst (2)!

      MCODE SPECIAL (4)

           part 2


 En we gaan verder met die routine:

 CLEARDATA:
              CALL    MAAKZICHTBAAR ; maak het lege hokje
                                    ; zichtbaar
 CLEARREPEAT: ; naar dit label wordt teruggesprongen als er
              ; hokjes zichtbaar gemaakt zijn: deze kunnen leeg
              ; zijn => herhaal
              LD      B,MAXBLOKJES
              LD      HL,DATA
              XOR     A        ; begin linksboven
              LD      (XCO1),A ; om alle coordinaten te door-
                               ; lopen en te kijken welke
              LD      (YCO1),A ; blokjes rondom staan
 CLEARDATALOOP:
              PUSH    HL
              PUSH    BC
              LD      A,(HL)
              CP      ZICHTBAARLEEG ; =&B01000000
              CALL    Z,DOCLEAR
              POP     BC
              POP     HL
              INC     HL
              LD      A,(XCO1) ; verhoog xco tot einde
              ADD     A,16 ; beeldscherm steeds met 16 =>
                           ; volgend blokje
              CALL    Z,YHOGER ; als xco=256 dan is xco 0:
                               ; verhoog yco
              LD      (XCO1),A
              DJNZ    CLEARDATALOOP


              LD      A,(FOUND) ; is er een blokje zichtbaar
              AND     A         ; gemaakt
              JP      Z,VULSCHERM ; nee, sluit af met opbouw
                                  ; van hele scherm.
              XOR     A ; begin opnieuw : nog niks zichtbaar
                        ; gemaakt
              LD      (FOUND),A
              JP      CLEARREPEAT ; en herhalen tot alles
                                  ; zichtbaar is

 YHOGER:      LD      A,(YCO1)
              ADD     A,16
              LD      (YCO1),A
              XOR     A ; xco op 0
              RET

 Als we in DOCLEAR aankomen weten we zeker dat in XCO1 en YCO1
 de coordinaten staan van het lege blokje. Rondom deze coordinaten
 moeten we dus gaan kijken. Als het blokje geen rand-blokje is
 zijn er 8 plaatsen rondom. In HL staat het adres in het DATA
 veld waar het lege blokje zich bevindt.
 Er moet dan gekeken worden op de volgende adressen:

  HL-17: linksboven het lege blokje
  HL-16: recht er boven
  HL-15: rechtsboven
  HL-1:  links
  HL+1:  rechts
  HL+15: linksonder
  HL+16: recht er onder
  HL+17: rechtsonder

 Als het blokje zich bevindt aan ��n van de randen (of in de
 hoeken) moeten er minder geheugenadressen in het DATAveld
 onderzocht worden. We kunnen de volgende data aanmaken:


 ;---- Rondtedata's: Geven aan hoeveelbyte verder/terug de
 ;     blokjes staan, die rondom een leeg hokje zitten.
 ;     Eerste byte is aantal blokjes = aantal bytes in die
 ;     rondtedata.

  DATAMIDDEN:   DB      8,-17,-16,-15,-1,1,15,16,17
  DATALINKS:    DB      5,-16,-15,1,16,17
  DATARECHTS:   DB      5,-17,-16,-1,15,16
  DATABOVEN:    DB      5,-1,1,15,16,17
  DATAONDER:    DB      5,-17,-16,-15,-1,1
  DATALB:       DB      3,1,16,17
  DATARB:       DB      3,-1,15,16
  DATALO:       DB      3,-16,-15,1
  DATARO:       DB      3,-17,-16,-1

 Om te weten welke Rondtedata gebruikt moet worden, moeten we
 eerst weten waar het blokje zich op het scherm bevindt:
 n.l. aan ��n van de randen of in ��n van de hoeken of ergens
 in het midden.

 Dit wordt onderzocht in de routine ZOEKRONDTEDATA.

 ZOEKRONDTEDATA:
              LD      A,(XCO1)
              CP      16
              JR      NC,NIETLINKS
 LINKS1:      LD      A,(YCO1)
              CP      16
              JR      NC,NIETBOVEN1
 LINKSBOVEN:  LD      DE,DATALB
              RET
 NIETLINKS:   CP      15*16
              JP      C,NIETRECHTS
 RECHTS1:     LD      A,(YCO1)
              CP      16
              JR      NC,NIETBOVEN2
 RECHTSBOVEN: LD      DE,DATARB
              RET
 NIETBOVEN1:  CP      MAXBLOKJES-16 ; onderrand dataveld
                                    ; (laatste regel)
              JR      C,LINKSMID
 LINKSONDER:  LD      DE,DATALO
              RET
 LINKSMID:    LD      DE,DATALINKS
              RET
 NIETRECHTS:  LD      A,(YCO1)
              CP      16
              JR      NC,NIETBOVEN
 BOVEN1:      LD      DE,DATABOVEN
              RET
 NIETBOVEN:   CP      MAXBLOKJES-16 ; idem
              JR      C,MIDDEN
 ONDER1:      LD      DE,DATAONDER
              RET
 MIDDEN:      LD      DE,DATAMIDDEN
              RET
 NIETBOVEN2:  CP      MAXBLOKJES-16 ; idem
              JR      NC,RECHTSONDER
 RECHTSMID:   LD      DE,DATARECHTS
              RET
 RECHTSONDER: LD      DE,DATARO
              RET

 Deze routine is niet bijster interessant en ook niet zo leuk
 om te schrijven, maar ze werkt en heeft tot resultaat dat er
 in DE, de juiste Rondtedata komt te staan.

 Maar dan...plotseling een eind aan deze tekst:

 Op de volgende FD het allerlaatste deel van deze zeer interes-
 sante cursus!

Ruud Gelissen
