Arjan vertelt jullie iets over dingen waar ik dus effe geen hol over kan zeggen...
         DE MATHPACK


 De MathPack is een verzameling routines voor het gebruik van
 gebroken getallen. Deze routines vormden eerst geen
 standaard, maar omdat de routines in alle MSX1-roms op
 dezelfde plek zaten, hebben de programmeurs van de MSX2-roms
 (en 2+/Turbo-R) ze op die plek gehouden en is het dus een
 standaard geworden. De informatie over de MathPack heb ik
 verdeeld over 2 delen. De eerste tekst gaat over de opbouw
 van de getallen en de tweede gaat over de functies van de
 MathPack.


            OPBOUW

 De MathPack kent drie soorten getallen, namelijk integers,
 enkele precisie en dubbele precisie. Deze getallen zijn
 hetzelfde als in BASIC.

 De integers zijn als volgt opgebouwd. De eerste 2 bytes zijn
 ongebruikt en de volgende 2 bytes geven het getal aan in de
 gebruikelijke volgorde, dus lowbyte-highbyte. Een voorbeeld:
 4942 wordt bij de MathPack: 0 0 78 19

 Getallen van enkele en dubbele precisie worden heel anders
 opgebouwd. Bij beide getaltypen bevat het eerste byte het
 teken en de exponent. De rest van de bytes (3 bij enkele en
 7 bij dubbele precisie) geeft het getal aan, ook wel de
 mantisse genoemd. Schematisch ziet dit er als volgt uit:

 +/- 0.MANTISSE +/- EXPONENT

 Als de exponent positief is, moet de mantisse naar rechts
 geschoven worden en als de exponent negatief is, moet de
 mantisse naar links geschoven worden. Een voorbeeld:
 -30 wordt opgeslagen als -0.3 E+2, de exponent is dus +2 en
 de mantisse is 3.

 De mantisse wordt opgeslagen in het zogaamde Binairy Coded
 Decimal-formaat, BCD. Dit houdt in dat ��n byte 2 decimale
 getallen op kan slaan, maar dat wordt dan wel hexadecimaal
 gedaan. Het getal 65 wordt dus opgeslagen als #65. De reden
 hiervoor is dat de computer zo sneller kan rekenen.

 Dan het teken. Het teken staat in bit 7 van het eerste byte.
 Is deze 1, dan is het getal negatief, is het 0, dan is het
 positief.

 De exponent beslaat bit 6-0 van het eerste byte. Bit 6 bevat
 het teken van het exponent. Is deze een 1, dan is de
 exponent positief, is het een 0, dan is de exponent
 negatief. Als de exponent negatief is, moeten de bits
 omgedraait worden en dan moet er 1 bij opgeteld worden.
 Verwarrend? Ja, inderdaad, dat is het. Het kan iets
 simpeler. Als de exponent negatief is, trek het dan af van
 64 en je krijgt ook het goede resultaat.

 Een paar voorbeelden:

 Stel, het eerste byte is #C1, binair dus &B11000001. Bit 7
 is 1, dus is het getal is negatief. Bit 6 is 1, dus de
 exponent is positief. Bits 5-0 geven de exponent aan, en dat
 is in dit geval dus 1.

 Stel, het eerste byte is #0A, binair dus &B00001010. Bit 7
 is 0, dus het getal is positief. Bit 6 is 0, dus de exponent
 is negatief. Bits 5-0 moeten dan omgeklapt worden, dus bits
 5-0 worden &B110101. Dan moet er 1 bij opgeteld worden, dus
 dat wordt &B110110 en de exponent is dus 54.

 Als het eerste byte 0 is, geeft dat aan dat het getal 0 is.

 Nu kun je dus een getal van enkele/dubbele precisie opslaan.
 Hier komen nog een paar voorbeelden die laten zien hoe
 getallen van dubbele precisie moet opslaan (om het te
 verduidelijken).

 5 wordt opgeslagen als #41 5 0 0 0 0 0 0
 -500 wordt opgeslagen als #C3 5 0 0 0 0 0 0
 123.123 wordt opgeslagen als #43 #12 #31 #23 0 0 0 0
 50E+10 wordt opgeslagen als #6C #50 0 0 0 0 0 0
 50E-10 wordt opgeslagen als #38 #50 0 0 0 0 0 0


           FUNCTIES

 Nu je weet hoe de getallen opgeslagen moeten worden, is het
 tijd voor de functies van de MathPack. De functies maken
 gebruik van drie adressen, namelijk:

 Naam    Adres  Lengte  Functie
 DAC     #F7F6    16    Decimale ACcuumulator (accu 1)
 ARG     #F847    16    ARGument voor bewerking (accu 2)
 VALTYP  #F663    1     Variabele type
                         2 = Integer
                         4 = Enkele precisie
                         8 = Dubbele precisie

 Zoals je ziet, is de lengte van de DAC 2 keer zo groot als
 een getal van dubbele precisie. Het getal moet in de eerste
 8 bytes gezet worden, de rest van de bytes worden gebruikt
 als werkruimte, dus zet hier niets in! Dit geldt natuurlijk
 ook voor ARG.

 De functies zijn onder te verdelen in drie groepen, namelijk
 de gewone functies, de transcedente functies en de overige
 functies.


       GEWONE FUNCTIES

 Deze functies werken alleen met getallen van dubbele
 precisie, tenzij het anders is aangegeven. Verder is het wel
 duidelijk wat ze doen. Hier komt een lijstje met de
 functies.

 Naam    Adres  Functie
 DECSUB  #268C  DAC = DAC - ARG  (aftrekken)
 DECADD  #269A  DAC = DAC + ARG  (optellen)
 DECMUL  #27E6  DAC = DAC * ARG  (vermenigvuldigen)
 DECDIC  #289F  DAC = DAC / ARG  (delen)
 DECNRM  #26FA  DAC = NORM(DAC)  (normaliseren (=voorloopnullen
                                   weghalen))
 DECROU  #273C  DAC = ROUND(DAC) (afronden)
 SNGEXP  #37C8  DAC = DAC ^ ARG  (machtsverheffen)
                                  (enkele precisie!)
 DBLEXP  #37D7  DAC = DAC ^ ARG  (machtsverheffen)


    TRANSCEDENTE FUNCTIES

 Deze functies kunnen alleen overweg met getallen van enkele
 en dubbele precisie. Zet VALTYP daarom op de juiste waarde,
 zodat de berekening goed wordt uitgevoerd. Verder spreken de
 functies wel voor zich.

 Naam    Adres  Functie
 COS     #2993  DAC = COS(DAC)   (cosinus)
 SIN     #29AC  DAC = SIN(DAC)   (sinus)
 TAN     #29FB  DAC = TAN(DAC)   (tangens)
 ATN     #2A14  DAC = ATN(DAC)   (arctangens)
 LOG     #2A72  DAC = LOG(DAC)   (natuurlijke logaritme)
 SQR     #2AFF  DAC = SQR(DAC)   (vierkantswortel)
 EXP     #2B4A  DAC = EXP(DAC)   (exponent)
 RND     #2BDF  DAC = RND(DAC)   (random getal)


       OVERIGE FUNCTIES

 Deze functies hebben te maken met het teken van het DAC.
 Eerst weer even een lijstje, daarna de uitleg.

 Naam    Adres  Functie
 SIGN    #2E17  A = SGN(DAC)     (Teken van DAC naar accu)
 ABSFN   #2E82  DAC = ABS(DAC)   (Maak DAC positief)
 NEG     #2E8D  DAC = -DAC       (Verander teken)
 SGN     #2E97  DAC = SGN(DAC)   (Teken van DAC naar DAC)

 SIGN bepaalt het teken van DAC en stopt het in de
 accumulator. Als de DAC 0 was, dan wordt A ook 0, als de DAC
 negatief was, wordt A #FF en als de DAC positief was, dan
 wordt A 0. Deze functie werkt alleen met getallen van
 dubbele precisie.

 ABSFN maakt de DAC positief. Deze functie werkt met alle
 variabelentypes. Alleen bij integers is er iets vreemds aan
 de hand. Als de DAC #8000 was, dus -32768, zou de absolute
 waarde +32768 zijn, maar dat past niet in een integer.
 Daarom wordt het getal omgezet naar enkele precisie.

 NEG verandert het teken van de DAC, dus 10 wordt -10 en -20
 wordt 20. Deze functie werkt met alle variabelentypes. Bij
 integers is hier weer hetzelfde aan de hand als bij de
 functie ABSFN.

 SGN tenslotte doet hetzelfde als SIGN, maar nu komt het
 teken in de DAC. Het resulaat is een integer met de waarde 0
 als de DAC 0 was, 1 als de DAC positief was en #FFFF als de
 DAC negatief was.

 In de MathPack zitten nog meer functies, maar daar heb ik
 (nog) geen documentatie over. Misschien komen deze een
 andere keer aan bod.

 Oh ja, de foutafhandeling van de MathPack verloopt via
 BASIC. Als je een verkeerde variabeletype gebruikt, kom je
 dus terug in BASIC. Dit kun je voorkomen door de hook H.ERRO
 (#FFB1) om te buigen, maar beter is het natuurlijk om
 gewoon geen fouten te maken.


Arjan Bakker

