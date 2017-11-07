 Ruud bespreekt het laatste stukje uit zijn Minesweeper source...

       MCODE CURSUS (5)


 We gaan weer verder...

 We weten nu hoeveel adressen onderzocht moeten worden en welke
 adressen dat zijn (we weten n.l. hun Offset t.o.v. het adres
 van het lege blokje). In HL staat nog steeds het adres van het
 lege blokje. Het onderzoeken van de adressen van de blokjes
 eromheen gebeurt nu met de volgende routine:

 DOCLEAR:
               LD      A,(DE) ; aantal plaatsen rondom
               LD      B,A ; die ev. nog zichtbaar moeten worden
 DOCLEARLOOP:  PUSH    HL
               PUSH    BC
               INC     DE
               LD      B,0
               LD      A,(DE) ; offset (in geheugenplaatsen)
               BIT     7,A
               CALL    NZ,NEGATIEF
               LD      C,A ; t.o.v. het lege blokje
               ADD     HL,BC ; optellen bij adres lege blokje
               BIT     6,(HL)
               CALL    Z,ZICHTBAAR ; blokje was onzichtbaar
                                   ; wordt nu zichtbaar
               POP     BC
               POP     HL
               DJNZ    DOCLEARLOOP
               RET
 ZICHTBAAR:    LD      A,(FOUND)
               INC     A
               LD      (FOUND),A
               JP      MAAKZICHTBAAR
 NEGATIEF:     DEC     B ; 255
               RET

 Om bij HL een 8 bits getal op te tellen moet je dit 8 bits
 getal in BC zetten (DE kan ook). B moet 0 gemaakt worden en in
 C moet het 8 bits getal gezet worden.

 Door GEN80 wordt -1 vertaald in 256-1=255
 -16 wordt dus 256-16=240

 Bij 8 bits getallen was het zo dat optellen met 255 hetzelfde
 betekende als er 1 van aftrekken, maar bij 16 bits getallen
 wordt er gewoon 255 bij opgeteld.
 Als B echter op 255 staat werkt de truuk wel weer.
 Hiervoor zorgt de subroutine NEGATIEF.

 Zoals je kunt zien maak ik weer gebruik van de routine
 MAAKZICHTBAAR, die ik voor de overzichtelijkheid hieronder
 nog maar even herhaal:

 MAAKZICHTBAAR:
               SET     6,(HL)            ; in DATA aangeven dat
                                         ; dit blokje al
               LD      HL,AANTALZICHTBAAR; zichtbaar is
               INC     (HL)
               RET

 Merk verder nog op dat HL telkens bewaard moet worden ( op de STACK ), omdat
 uitgegaan wordt van Offset's t.o.v. het adres van het lege blokje.

 Verder wil ik nog even kwijt dat de routine CLEARDATA vast wel
 op de een of andere manier sneller kan gemaakt worden.(b.v.
 door gebruik van de overgebleven bit in de DATA, om aan te
 geven of een leeg blokje als eens onderzocht is)
 Maar persoonlijk vond ik dat niet nodig, aangezien ik het dan
 alleen maar moeilijker zou maken dan het eigenlijk is.


 Zo, tot zover de bespreking van de routines van het spel
 Minesweeper.

 De listing staat in zijn geheel nog een keer op de Future-
 Disk als ASCII-tekst in MINESW.GEN.
 Wil je de routine in WBASS2 gebruiken, moet je wel eerst de
 header weghalen. Dus deze routine moet eruit:

               DB      #FE ; om de file in BASIC
               DW      ST  ; m.b.v. BLOAD in te kunnen laden
               DW      EN  ; weglaten bij gebruik van WBASS2
               DW      ST

 Ik wens je erg veel plezier met deze routines en daag je uit
 de volgende uitbreidingen eens te programmeren:

 - Een routine, die zelf random velden opbouwt.
   Op zich is dit niet zo moeilijk als je misschien denkt.
   Als buffer voor randomgetallen gebruik je een geschikt
   deel van het geheugen b.v. de basicrom, waarin je met
   LD   A,R een adres kiest.
   Staat op een adres een b.v. waarde >196 zet je een mijn
   neer, anders niet. M.b.v. de al geschreven routine voor
   het random wissen moet je nu vrij eenvoudig een routine
   kunnen schrijven die rondom de juiste getallen plaatst.
   Aangezien deze routine wezenlijk is voor het spel zal ik
   misschien op een van de volgende FutureDisks de routine
   publiceren.
 - fatsoenlijke routine, die aangeeft dat je gewonnen bent
   i.p.v wat nutteloze Beeps, die alleen een programmeur
   interessant vind (om te testen).
 - Als je afgaat wordt het hele veld zichtbaar gemaakt, maar
   alle vlaggen blijven staan zodat je niet kunt zien of
   hieronder echt een mijn zat.
   Los dit b.v. op door een routine te schrijven die alle 5e
   bit's in het dataveld 0 maakt (de vlaggen weghaalt), voor-
   dat VULSCHERM wordt aangeroepen.
 - Hetzelfde spel omzetten naar Screen 7, een klokje toevoe-
   gen, puntentelling bijhouden en de snelste tijd laten zien.
 - Blokjes 8x8 formaat geven (en dus meer blokjes op het
   scherm zetten).

 SUCCES !!

Ruud Gelissen
