Het laatste deel over de MATH PACK...

        DE MATH-PACK (3)


 In dit laatste deel over de math-pack komen de wat comple-
 xere routine's aan bod, en dan voornamelijk de string-
 routine's.

 De routine FIN (&H3299) zet een string om naar een decimaal
 getal in DAC. Hiervoor heeft deze routine register HL nodig
 om het startadres van de string aan te geven en register A om
 het eerste teken te weten. Als resultaat staat op DAC dus het
 getal. Register C bevat de waarde 255 als de string geen punt
 bevatte en de waarde 0 als er wel een punt in stond en regis-
 ter B bevat het aantal cijfers achter de punt.

 De routine's FOUT (&H3425) en PUFOUT (&H3426) zetten het
 getal in DAC om naar een string. Het verschil tussen beide
 routine's is dat FOUT de string niet formatteerd en PUFOUT
 wel en dat PUFOUT de DAC positief maakt.

 De invoer en uitvoer is bij beide routine's hetzelfde.
 Register A geeft het formaat van de string aan, waarbij de
 afzonderlijke bits de volgende betekenis hebben:

 bit 7: 0 = ongeformatteerd, 1 = geformatteerd
 bit 6: 0 = zonder komma's , 1 = met komma's om de drie
        cijfers
 bit 5: 0 = niets          , 1 = voorafgaande spaties worden
        opgevuld met punten
 bit 4: 0 = niets          , 1 = "$" wordt voor de waarde
        gezet
 bit 3: 0 = niets          , 1 = "+" wordt voor de waarde
        gezet (indien de waarde positief was)
 bit 2: 0 = niets          , 1 = +/- wordt achter de waarde
        gezet
 bit 1: niet gebruikt
 bit 0: 0 = fixed point    , 1 = floating point

 Register B geeft het aantal cijfers voor de punt aan en
 register C geeft het aantal cijfer na de punt aan (inclusief
 de punt). Als er dus 3 tekens na de punt komen, moet je de
 waarde 4 in register B zetten.

 Na het uitvoeren van ��n van deze routine's komt in register
 HL het adres van de string.

 De routine's FOUTB (&H371A), FOUTO (&H371E) en FOUTH
 (&H3722) zetten een integer getal in DAC+2/3 om naar
 respectievelijk een binaire, octale en hexadecimale string.
 Let er trouwens op dat VALTYP op 2 staat. Deze 3 routine's
 hebben dus als resultaat een string waarvan het beginadres
 in register HL komt te staan.


       TYPE CONVERSIE

 De routine's FRCINT (&H2F8A), FRCSNG (&H2FB2) en FRCDBL
 (&H303A) zetten het getal DAC om naar respectievelijk een
 2-byte integer (DAC+2/3), een enkele precisie waarde en een
 dubbele precisie waarde. Na aanroep bevat VALTYP het type
 van DAC (dus 2, 4 of 8) en zijn alle registers veranderd.

 De routine FIXER op adres &H30BE maakt van het getal DAC een
 gehele waarde. Ook hier worden alle registers veranderd.


       MACHTSVERHEFFEN

 Er zijn nog 3 extra routine's voor het machtsverheffen.
 SGNEXP (&H37C8) is voor getallen van enkele precisie, DBLEXP
 (&H37D7) is voor getallen van dubbele precisie en INTEXP
 (&H383F) is voor 2-byte integers. De eerste 2 routine's
 gebruiken DAC als basis en ARG als macht en de laatste
 gebruikt DE als basus en HL als macht. Bij alle drie de
 routine's wordt het resultaat in DAC gezet en worden alle
 registers verandert.

          TOT SLOT

 Er is ook nog een routine om register HL decimaal op het
 scherm te zetten, namelijk PRTHL (&H3412). Deze routine is
 echter niet offici�el, maar werkt volgens mij wel altijd.

 Ik geloof dat ik nu wel alles over de math-pack heb
 behandeld. Mocht ik echter nog wat onbehandelde dingetjes
 tegenkomen, dan zal ik daar later op terugkomen maar wat mij
 betreft is dit de laatste tekst over dit onderwerp.

Arjan Bakker
