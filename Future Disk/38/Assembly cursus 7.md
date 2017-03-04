ASSEMBLY CURSUS 7


Dit is alweer het zevende deel van de assembly cursus,
tenminste, als ik me niet vergis. Maar goed, het is niet
echt belangrijk om te weten welk deel het is want dat maakt
geen zak uit voor de inhoud. En nu ik het toch over de
inhoud heb, ik heb de laatste tijd moeite om een leuk onder-
werp te verzinnen. Mocht je dus eens een keer iets willen
weten, schrijf of bel ons even en wellicht krijg je op de
volgende FD dan al een antwoord. Maar goed, voor deze keer
heb ik nog wel een onderwerpje, namelijk werken met BCD-
getallen


  BCD-GETALLEN

Het idee voor dit onderwerp kreeg ik tijdens het maken van
het spelletje dat deze keer op de FD staat. Ik had namelijk
lange getallen nodig om de score aan te duiden en dan heb je
aan 16 bits niet genoeg. Je kunt dan wel gaan werken met 32
bits getallen, maar het afdrukken daarvan gaat niet echt
snel en het is ook een beetje lastig om te programmeren.

Nu zal een ervaren programmeur natuurlijk zeggen: Waarom
gebruik je niet de MathPack (MP)? Nou, dat zal ik eens
eventjes fijn uitleggen. Ten eerste is de MP mij te sloom,
omdat de MP ook rekening houdt met een komma, negatieve
getallen en een exponent, en dat heb ik allemaal niet nodig.
Ten tweede...hmm, ik weet geen tweede reden te verzinnen,
maar de reden die ik opgaf was voor mij al genoeg om de MP
maar niet te gebruiken.

Goed, BCD-getallen dus. BCD wil niets anders zeggen dan
Binary Coded Decimal. In gewoon Nederlands houdt dit in dat
je met 4 bits ��n getal aangeeft en dat betekent dat je met
1 byte 2 getallen kunt weergeven. Een klein voorbeeldje om
alles te illustreren:

9 is en blijft negen.
19 wordt &b0001 1001 = &h19
52 wordt &b0101 0010 = &h52

Okee, dit lijkt me nu wel duidelijk. Nu is een notatie
natuurlijk niet genoeg om de getallen te kunnen gebruiken,
want zonder correctie zou bijvoorbeeld 19 (=&h19) + 52
(=&h52) 107 (=&h6b) worden en dat is dus niet goed, want het
goed antwoord zou &h71 moeten zijn. Bij aftrekken gaat het
ook fout (&h52 - &h19 = &h39, het goede antwoord is echter
&h33).

Gelukkig kent de Z80 een hele handige instructie om het
resultaat te corrigeren, namelijk DAA. Dit betekent Decimal
Adjust Accumulator. Deze instructie test eerst op de H-flag
(bit 4). Als deze geset is (zoals bij ons het geval was),
dan moet het getal gecorrigeerd worden en anders natuurlijk
niet.

Als er een getal gecorrigeerd moet worden, kijkt de Z80 naar
de N-flag (bit 1). Deze flag geeft aan of er een aftrekking
of een optelling plaats vond. Let op: deze instructie werkt
alleen als er ADD of SUB gebruikt werd. Als er een getal
afgetrokken werd, moet register A met 6 verlaagd worden en
anders juist met 6 verhoogd worden. Tot slot wordt de Carry-
flag geset als het resultaat niet in een BCD-getal past (b.v.
99 (&h99) + 2 (&h02) = 101 (&h01). Hier wordt de carry dus
geset).

Met deze informatie zou je zelf een optel/aftrek-routine
kunnen maken voor BCD-getallen. Een kleine tip: reken van
achter naar voren en gebruik ADC/SBC om te rekenen. Mocht je
er niet uitkomen, lees deze tekst dan nog eens of kijk eens
in de file CALC.GEN (staat op deze FD) en dan zal het wel
duidelijk zijn.


     RLD/RRD

Voor het vermenigvuldigen/delen van BCD-getallen is het
handig om getallen ��n plek naar links of naar rechts op te
kunnen schuiven. Hiervoor heeft de Z80 ook instructies,
namelijk RLD en RRD. Het gebruik hiervan is ietwat lastiger
dan bij gewone schuifinstructies, maar echt lastig is het
ook weer niet.

De instructies RLD/RRD verschuiven getallen via register A
en het getal op adres HL, waarbij RLD natuurlijk naar links
schuift en RRD naar rechts. De werking van de instructies
blijkt uit het volgende schemaatje:

RLD:
           >--->--->--->--->
           !               !
   &b0000 0000     &b0000 0000
           !          ! !  !
           <---<---<--- <---
    reg A               (HL)

RRD
           <---<---<---<---<
           !               !
   &b0000 0000     &b0000 0000
           !          ! !  !
           --->--->---> --->
    reg A               (HL)

Om alles nog wat duidelijker te maken, komen hier nog twee
voorbeeldjes:

RLD:
A: &h04
HL: &hC000
(HL): &h59

Na afloop van de instructie RLD is het resultaat:

A: &h05
HL: &hC000
(HL): &h94

RRD:
A: &h04
HL: &hC000
(HL): &h59

Na afloop van de instructie RRD is het resultaat:

A: &h09
HL: &hC000
(HL): &h45

Volgens mij moeten jullie nu ook wel in staat zijn om een
vermenigvuldigings-routine te programmeren. Als het toch
niet lukt, kijk dan eventjes naar de routines in CALC.GEN
en dan zal alles (hoop ik) wel duidelijk zijn.


Arjan
