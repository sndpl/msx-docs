 Ruud vervolgt deze speciale programmeercursus...


      MCODE SPECIAL (2)

 We vervolgen waar we op FD #19 gebleven waren...

         Het DATAveld

 We hebben dus een veld van 16*13 blokjes, waaronder al dan
 niet mijnen zitten. Wat moeten we nu precies van een blokje
 weten:

 1. Mijn of niet
 2. Al zichtbaar gemaakt of niet
 3. Op veilig gesteld of niet
 4. Aantal aangrenzende mijnen.

 De gegevens 1 t/m 3 zijn vragen, die met ja of nee beantwoord
 kunnen worden: Per gegeven is 1 Bit genoeg (JA: Bit staat op 1;
 Nee: Bit staat op 0). Het laatste gegeven is een getal tussen
 de 0 en de 8: hiervoor zijn 4 bits nodig.
 We hebben dus per veld genoeg aan 7 bits, dus zeker genoeg
 aan 1 byte(8 bits). Om al onze gegevens te kunnen opslaan
 wordt gebruik gemaakt van 16*13 bytes: het DATAveld
 (label:DATA).  Een byte in het DATAveld heeft de volgende
 betekenis:

bitnr.  7       6       5    4         3    2    1   0
       mijn  zichtbaar vlag  x     aantal aangrenzende mijnen

 De bovenste nibble (4 bits) zijn de gegevens en de onderste
 geeft het aantal aangrenzende mijnen aan.
 De eerste byte in het DATAveld heeft betrekking op het
 blokje linksboven in beeld op xco,yco:(0,0).
 De tweede byte heeft betrekking op het blokje op
 xco,yco:(16,0). De 16e byte heeft betrekking op het blokje
 op xco,yco:(0,16) etc..

 Om te kunnen laten testen of ons programma werkt vullen
 we nu eerst een dataveld zelf met mijnen, zoals in
 MINESW.GEN het geval is.


VULSCHERM (kopieren in MC)

 Deze routine vult het scherm aan de hand van het DATAveld
 met de juiste blokjes. Het HL register is de Adrespointer:
 deze geeft aan met welk adres in DATA gewerkt wordt. B is
 een teller die telt hoeveel blokjes we gehad hebben. Telkens
 als 1 blokje gedaan is worden zowel HL als B met 1 verhoogd.
 De routine wordt herhaald tot B>16*13 geworden is:
 het hele scherm is dan gevuld.


 Voor het neerzetten van een blokje is de routine COPY
 essentieel. Deze routine copieert een stuk uit het VRAM
 (in ons geval een van de blokjes van Page 1) naar een
 andere plaats in het VRAM, net als het basic commando COPY.
 Net als bij SETPAL ga ik hier niet uitleggen hoe deze routine
 precies werkt, maar vertel ik slechts wat je nodig hebt om
 hem te gebruiken. In het HL regeister moet het adres staan
 waar de gegevens staan die nodig zijn voor de COPY. In dit
 geval is dit het adres BLOKDATA. Je ziet hier hieronder
 hoe die gegevens eruit moeten zien.


BLOKDATA:                 ; gegevens voor het COPY commando
SX:           DB      0,0 ; source xco (welke xco komt het
                            blokje vandaan)
SY:           DB      0   ; source yco (      yco     )
SPAGE:        DB      1   ; source page(      pagina )
DX:           DB      0,0 ; destination xco
                            (welke xco moet het naar toe )
DY:           DB      0   ;             yco
DPAGE:        DB      0   ;             page
LX:           DB      16,0; lengte in x-richting
LY:           DB      16
OVERIGE:      DB      0,0,0
COMMANDO:     DB      #D0 ; plaats blokje m.b.v. snelle copy

; p.s. vergelijk met in BASIC:
; COPY (SX,SY)-(SX+LX,SY+LY),SPAGE TO (DX,DY),DPAGE


 Merk op dat voor de x-coordinaten 2 bytes gereserveerd zijn,
 dit omdat een xco in screen 7 tussen de 0 en 512 ligt en dus
 2 bytes in beslag neemt.
 Als screen 5 aanstaat staan er altijd nullen in de 2de byte
 voor de xco.

 Omdat alle blokjes 16x16 groot zijn en alle gekopieerd worden
 van Page 1(SOURCE YCO=0) naar Page 0, liggen veel gegevens
 al vast.
 Het kopieren van een blokje komt er nu nog op neer de
 juiste SX,DX en DY in te vullen en dan m.b.v.

                      LD        HL,BLOKDATA
                      CALL      COPY

 het eigenlijke kopieren uit te voeren.

 De VDP start na het geven van het COPY commando met het
 kopieren, de Z-80 werkt echter gewoon door met zijn eigen
 instructies!
 De VDP is dus eigenlijk een soort co-prossesor. Pas hiermee
 op: als het commando COPY wordt gegeven staat de COPY dus nog
 niet klaar; dat kan soms onhandig zijn als je er zeker van
 moet zijn dat een bepaalde COPY gebeurd is voordat je verder
 gaat met de volgende Z-80 instructies (hier is dat niet het
 geval).
 Als je wilt wachten tot de VDP klaar is kun je de volgende
 routine gebruiken.

COPYREADY:    LD      A,2
              CALL    VDPSTA            ; lees statusregister 2
              RRA
              JP      C,COPYREADY       ; de VDP is nog niet
                                          klaar: wacht hier
              RET                       ; klaar voor volgende copy.


 Het invullen van de juiste SX betekent dat we moeten weten welk
 symbool op deze plaats van het beeld staat m.a.w. we moeten in
 het DATAveld kijken.

 Bit voor Bit wordt gekeken in het DATAveld m.b.v de volgende
 routine:

VULSCHERML:   LD      A,(HL) ; in A staan de gevgevens over
                               het blokje
              BIT     5,A
              JR      NZ,VLAG
              BIT     6,A
              JR      Z,ONZICHTBAAR ; maakt niet uit wat
                                      eronder zit
              BIT     7,A
              JR      NZ,MIJN

              AND     %00001111 ; het blokjes is een getal
                                  of leeg
GETAL:        ADD     A,A
              ADD     A,A
              ADD     A,A
              ADD     A,A ; *16
              LD      (SX),A
              JP      SETDX

 De volgorde van het afgaan van de Bit's is niet willekeurig:
 het is niet de bedoeling dat er een mijn neergezet wordt
 terwijl het blokje nog niet eens zichtbaar was
 Als geen van de bovenste Bits in de DATA aanstaat, moet er
 een getal neergezet worden.
 M.b.v. de AND %00001111 worden de bovenste Bit's van het
 A-register op 0 gezet en de onderste niet verandert: in A
 staat nu het getal dat aangeeft hoeveel mijnen er om het
 blokje heen liggen.
 Als het getal 0 is moet het blokje met SX=0 gekopieerd
 worden.
 Als het getal 1 is moet het blokje met SX=16 gekopieerd
 worden.
 Als het getal 2 is moet het blokje met SX=32 gekopieerd
 worden.
 Vandaar dat het getal met 16 vermenigvuldigd wordt om de SX
 te krijgen.

 Wat er ook voor blokje afgedrukt wordt: uiteindelijk (als de
 SX bepaald is) komt de MSX aan bij de volgende routine:

SETDX: ; bereken aan de hand van B de destination xco en yco
              LD      A,B
              AND     %00001111 ; getal tussen 0 en 15
              ADD     A,A
              ADD     A,A
              ADD     A,A
              ADD     A,A ; *16
              LD      (DX),A
                       ; getal tussen 0..16..32..48..64....240
              LD      A,B
              AND     %11110000 ; idem: wordt na 16* deze loop
                                  met 16 verhoogd
              LD      (DY),A ; en vormt zo precies de
                               destination yco

 In deze routine wordt aan de hand van het nummer van het
 blokje waarmee we bezig zijn berekent waar op het beeldscherm
 dit blokje terecht moet komen. Kijk goed naar deze routine:
 het is een vaak toegepast principe! Dit is alleen mogelijk
 voor 16x16 blokjes (voor 8x8 valt wel iets soortgelijks te
 bedenken) en dit is dan ook een van de redenen dat je als
 programmeur de tekenaar altijd moet leren "in machten van 2"
 te tekenen.

 SX,DX,DY en alle overige gegevens zijn nu ingevuld, dus het
 blokje kan gekopieerd worden en de routine net zolang
 herhaald tot het hele scherm vol is (merk op dit duurt een
 fractie van een seconde voor 16*13=208 COPY's ).

 Maar dit was het helaas alweer voor deze keer. WE GAAN OP
 FD #22 vrolijk verder met deze cursus...

 Ruud Gelissen
