Lijntjes zijn heel cool, zo kun je van 4 lijntjes een rechthoek of een vierkant of een parallellogram (ofzo) maken, lijntjes zijn goed...

           LIJNTJES


 E�n van de lastigste VDP-commando's is wel het line-
 commando. Het VDP line-commando werkt namelijk heel anders
 dan het BASIC line-commando. In BASIC kun je lekker
 makkelijk het start- en eindpunt opgeven, maar de VDP wil
 naast het beginpunt ook de lengte van de lijn weten, en de
 richting. Okay, als je weet hoe het moet, is ook het line-
 commando een makkelijk commando, maar anders kan het je
 zeker grijze haren bezorgen.

 Het makkelijkste om op te geven is het beginpunt van de
 lijn; deze kunnen direct in VDP-register 36/37 (X) en 38/39
 (Y) gezet worden.

 Moeilijker is de lengte van de lijn. Om deze te kunnen
 bepalen, moet je eerst de betekenis van register 45 weten.
 Deze is als volgt:

 bit:   7   6    5    4    3    2     1       0
        0  MXC  MXD  MXS  DIRY DIRX EQ/NEQ MAJ/MIN

 Ik zal hier alleen even de betekenis van de benodigde bits
 uitleggen.

 DIRY:  1: lijn gaat omhoog, 0: lijn gaat omlaag
 DIRX:  1: lijn gaat naar links, 0: naar rechts

 MAJ/MIN: 1: Y is belangrijkste richting
          0: X is belangrijkste richting

 Nu kunnen we de lengte en de richting van de lijn bepalen.
 Om de X-richting te bepalen trek je het eindco"rdinaat van
 het begin-co"rdinaat. Als het resultaat positief is, dan
 moet DIRX geset worden. Als het resultaat negatief is, dan
 moet het weer positief gemaakt worden (NEG) en moet DIRX
 gereset worden. Tot slot zet je het resultaat in register
 40/41 (NX).Hetzelfde verhaal geldt voor de Y-richting,
 alleen moet je nu DIRY en register 42/43 (NY) invullen.

 En dan het vreemdste van het line-commando. Als NY groter
 dan NX is, dan moeten deze twee waarden omgewisseld worden
 en moet MAJ/MIN geset worden. Waarom dit zo gedaan moet
 worden weet ik ook niet, maar ja, het moet maar.

 Oh ja, voordat ik het vergeet, de lijnkleur moet in register
 44 komen en register 46 moet met het juiste commando gevuld
 worden. En dat commando is (zonder logische operaties)
 &b01110000. De logische operatie moet op de laagste 4 bits
 ingevuld worden, maar dat weet je waarschijnlijk wel (en
 anders lees je nu de verkeerde tekst).

 Tot slot komt hier nog even een source van een line-routine.

 ;In: H: SX
 ;    L: SY
 ;    D: DX
 ;    E: DY
 ;    C: kleur

 line:  ld      a,c
        ld      (color),a       ;kleur

        ld      a,h
        ld      (dx),A          ;begin X

        ld      c,4             ;dir X = links
        sub     d               ;bepaal lengte X-richting
        jr      nc,line1        ;positief = naar links
        ld      c,0             ;dir X = rechts
        neg                     ;maak positief
 line1: ld      b,a             ;bewaar in B
        LD      (nx),A          ;en vul in

        ld      a,l
        ld      (dy),a          ;begin Y

        set     3,c             ;dir Y = omhoog
        sub     e               ;bepaal lengte Y-richting
        jr      nc,line2        ;positief = omhoog
        res     3,c             ;dir Y = omlaag
        neg                     ;maak positief
 line2: ld      (ny),a          ;en vul in
        cp      b               ;welke richting is groter?
        jr      c,line3         ;X>Y, dan is alles goed
        set     0,c             ;Y is belangrijkste richting
        ld      (nx),a          ;vul Y in NX-register in
        ld      a,b
        ld      (ny),a          ;vul X in NY-register in

 line3: ld      a,c
        ld      (data),a        ;vul dirx/diry/ maj/min in
        ld      a,%01110000
        ld      (command),a     ;vul line-commando in
        call    copy            ;voer commando uit
        ret

 De rest van de source is niet interessant, want die dient
 voor het sturen van de juiste data naar de VDP. Voor de
 volledigheid staat de source ook op deze FD onder de
 logische naam LINE.GEN.

Arjan
