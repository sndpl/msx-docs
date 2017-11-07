 Ons aller lieve Tobias behandelt nog een keertje de MSX 2+ schermen .....stof voor de programmerende MSX 2+ freaks dus....

  NOGMAALS: DE 2+ SCHERMEN.

                             ����
                             ����
                             ����
                             ����



 Ik weet dat ik dit onderwerp al een keer eerder hab behan-
 deld, maar bij gebrek aan andere onderwerpen toch nog maar
 een keer de 2+ schermen.

 Ken je ze nog, de oude dagen waarin men net de 2+ over had
 laten vliegen uit Japan; zonder handleiding uiteraard want
 dan weegt het immers te veel. Daar komt nog eens bij dat de
 handleiding geheel meestal in "onleesbare" Japanse "kriebels"
 was zodat deze geen waarde had. Alles zelf uitzoeken was dus
 het gevolg, en dan bleek al gauw dat de 2+ schermen
 onhandelbaar waren.
 In de oude dagen ja, want bij betere analyse blijkt namelijk
 dat het allemaal nog wel meevalt, vooral screen 11 blijkt
 helemaal niet zo onhandelbaar. Dat het allemaal wat meer
 moeite kost, dat is wel zo...

IN BASIC

 Deze idee�n waren natuurlijk toch wel ergens op gebaseerd,
 in basic is er namelijk weinig mogelijk op de 2+ schermen.
 COPY's en dergelijken gaan over het algemeen wel, maar
 tekenen wordt toch een stuk moeilijker. Alleen met welgemik-
 te VPOKE's is er het een en ander mogelijk.
 In machinetaal is, mede door de hogere snelheid, al een stuk
 meer mogelijk, en bij de juiste behandeling van de 2+
 schermen blijkt het allemaal toch nog mee te vallen.

SCREEN 10

 Dit is toch wel het meest gebruikte 2+ scherm in BASIC, des
 te groter zal de verassing zijn als ik vertel dat dit scherm
 helemaal niet bestaat. SCREEN 10 is een niet bestaande modus
 die speciaal voor BASIC gemaakt is. De grap zit em namelijk
 in het feit dat dit gewoon SCREEN 11 is maar door de BASIC
 interpreter op een speciale manier wordt behandeld. Daardoor
 zullen commando's als LINE en CIRCLE in SCREEN 10 wel goed
 gaan. Verderop in deze tekst zal ik hierop terugkomen.


SCREEN 11

 Zoals gezegd de eigenlijke modus van SCREEN 10. Dit scherm
 bezit over een totaal aantal kleuren van 65536. Hoe ASCII ooit
 aan die 12499 is gekomen weet ik niet, maar blijkbaar kunnen
 ze beter programmeren dan tellen. Er worden per pixel 16 bits
 gebruikt en bij mij levert dit nog altijd 65536 mogelijkhe-
 den op als je zwart mee mag rekenen. In SCREEN 11 is, behalve
 met welgemikte COPY's en VPOKE's, weinig spannends te doen.
 Ook de VPOKE's zijn eigenlijk al alleen interressant in
 ASSEMBLY omdat BASIC hier eigenlijk te sloom voor is.


SCREEN 12

 Hier worden er 17 bits per pixel gebruikt wat 131072 kleuren
 oplevert. Ook hier komt ASCII weer met een of ander vaag ge-
 tal, dit keer 19268, maar ook daar klopt volgens mij geen
 flikker van. Ook voor SCREEN 12 geldt weer hetzelfde, voor
 de BASIC programmeurs niet echt spannend...


DE OPBOUW

 Zoals we al jaren van MSX gewend zijn moet alles zo moeilijk
 en onhandig mogelijk zijn. Dus toen de 2+ standaard gemaakt
 werd mocht het VRAM natuurlijk vooral niet groter worden,
 nee!! Want dat zou handig zijn. Ook een snellere VDP was na-
 tuurlijk uit den boze, kom nou! Maar men wilde natuurlijk
 wel meer dan 256 kleurtjes. En dan moet je dus gaan rommelen
 zoals ze bij ASCII gedaan hebben.
 Om toch meer kleurtjes in dezelfde hoeveelheid VRAM te
 proppen werd er dus besloten om van elke 4 pixels een aantal
 bits (12) gemeenschappelijk te gebruiken. Op deze manier kon
 er voor elke 4 pixels een basiskleur worden ingesteld.
 Vervolgens kon voor elke pixel afzonderlijk een helderheid
 worden ingesteld. Hoe dit werkt zie je in het volgende
 schemaatje.


          BIT7 BIT6 BIT5 BIT4 BIT3 BIT2 BIT1 BIT0
  PIXEL0   HE   HE   HE   HE   HE   GR   GR   GR
  PIXEL1   HE   HE   HE   HE   HE   BL   GR   GR   SCREEN 12
  PIXEL2   HE   HE   HE   HE   HE   RO   RO   RO
  PIXEL3   HE   HE   HE   HE   HE   BL   RO   RO

          BIT7 BIT6 BIT5 BIT4 BIT3 BIT2 BIT1 BIT0
  PIXEL0   HE   HE   HE   HE   PA   GR   GR   GR
  PIXEL1   HE   HE   HE   HE   PA   BL   GR   GR   SCREEN 11
  PIXEL2   HE   HE   HE   HE   PA   RO   RO   RO
  PIXEL3   HE   HE   HE   HE   PA   BL   RO   RO

  HE:Helderheid    RO:Rood    GR:Groen    BL:Blauw    PA:Palet


 Zoals verwacht heeft SCREEN 12 een andere opbouw dan scherm
 modus 11. Alle bits voor Rood Groen en Blauw worden voor de
 betreffende pixels gebruikt als basiskleur. Vervolgens kan
 er met de 5 HE-bits de helderheid per pixel worden ingesteld
 zonder dat dit van invloed is op de andere 3 pixels.
 Bij SCREEN 11 echter, zijn er maar 4 Helderheids bits per
 pixel beschikbaar. En hierin schuilt dus de grap waar BASIC
 gebruik van maakt in screen 10. Het derde bitje, waar PA
 staat, is namelijk een switch die van PAlet verwisseld.
 Er wordt hier namelijk gekozen tussen het standaard palet
 zoals we dat in SCREEN 5 kennen, en het palet dat SCREEN 11
 zelf gebruikt. Is het bit laag, dan wordt de kleur-methode
 van SCREEN 11 gehanteerd, en de SCREEN 5-palet mode. Waarom
 ASCII speciaal hiervoor in BASIC een tiende scherm heeft
 gemaakt weet ik niet, volgens mij hadden ze dit net zo goed
 in SCREEN 11 kunnen doen, maar ja, van jongens die niet eens
 kunnen tellen moet je natuurlijk niet te veel verwachten.


 Verder valt er eigenlijk weinig te zeggen over deze screens.
 Je weet nu waar wat staat, pas op met COPY's, hou de coordi-
 naten wel in een viervoud. En dat is eigenlijk wel zo'n
 beetje wat er te vertellen is.

 Ter illustratie van het geheel heb ik nog even een klein MC
 programmaatje geschreven voor de V9958. Als het goed is staat
 SCREEN12.BIN wel op de disk. Dit programmaatje zet de eerste
 65536 kleuren van SCREEN 12 op het scherm waarna je met cur-
 sor up & down nog door het geheel heen kan scrollen, anders
 mis je de laatste 44 lijnen. Het programmaatje zet 32 tinten
 rood, gemengd met 32 tinten groen en 2 tinten blauw op het
 scherm in de 32 aanwezige helderheden. Dit geeft:
 32x32x2x32=65536 van de 32x32x4x32=131072 aanwezige kleuren
 als we er tenminste van uit gaan dat ik geen fouten heb ge-
 maakt, wat opzich vrij onlogisch zou zijn. Een snelheids
 wonder is het niet, in R800 mode kost het zo'n seconde of 3
 om de kleurtjes op het scherm te plaatsen en in Z80 mode een
 seconde of zes. Daarna mag je er lekker op los scrollen met
 60 pixels per seconde... Oh joy!!

Geluk d'r mee...

Keizer

