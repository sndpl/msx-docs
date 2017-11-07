      BASIC Cursus?!

����  Okay mensen. Tom ligt er uit (Yes!) en Tobias herstart
����  bij deze dus de BASIC cursus. Natuurlijk kan Tobias niet
����  verder gaan waar Tom gebleven was. In de eerste plaats
����  niet omdat Tom een eikel is en in de tweede plaats
      omdat Tobias' aanpak compleet anders is. Wij bedanken U
voor Uw begrip. Het woord is aan Tobias...

     BASIC CURSUS (6)

                            ����
                            ����
                            ����
                            ����

  SHIFT en ander gekakel...


 Toen ik een klein jongetje was had ik maar ��n droom.
 (Waag 't niet Koen!) Ik wou zo graag de Shift toets en dat
 soort onzin toetsen uitlezen(NvdR: and that's your dream?).
 Maar ja, met inkey$ ging dat zo moeilijk. Toen ging ons
 Tobiasje naar een oude wijze Guru. Roman van der Meulen.
 Roman was alleen een beetje lui en riep alleen 't woord
 "Toetsen bord buffer" uit om vervolgens weer te gaan slapen.
 Toen ging ons Tobiasje maar naar een andere wijze Guru:
 Jan van Valburg. Maar Jan was nogal druk met allerlei
 vunzige bezigheden achter z'n computer (WIDEOPEN.GIF) en kon
 ons Tobiasje ook niet helpen. Ten einde raad ging ons
 Tobiasje huilend terug naar huis, nog steeds niet wetend hoe
 hij de Shift toets moest uitlezen. Toen kwam Tobiasje een
 minstens 50 kilo te zware, wild behaarde, en veel te naive
 hippie tegen: Wammes Witkop. Wammes had een eigen blad voor
 MSX en kende dus heel veel mensen. Ook kende hij iemand die
 wel graag toetsen uitlas: Hayo Rubingh(NvdR: dat was vroeger
 ook echt mijn hobby!). Hayo was echter druk bezig andere
 muziek-programma's af te zeiken en verwees Tobiasje door
 naar Albert Huitsing die eigenlijk 't hele MIDI gedeelte
 heeft gedaan waar Hayo altijd de eer van opstreek/opstrijkt.
 Albert kon Tobiasje wel helpen en legde het volgende uit:

 OK lieden, het hele gezeik over de interrupt en al dat geemmer
 over hoe al die data in de toetsen bordbuffer komt ga ik
 niet weer helemaal uitleggen. Als je dat niet weet zoek je
 maar de dichtst bijzijnde brug op. KEYBUF ga ik niet
 behandelen omdat die niet zo gek interressant is. NEWKEY
 echter is veel interressanter. NEWKEY loopt van &HFBE5 tot
 &HFBEF en is dus 11 bytes groot. Wat precies klopt gezien
 't feit dat er 11 rijen zijn. NEWKEY kan vanuit BASIC gewoon
 met een PEEKje worden uitgelezen alleen dan is 't wel
 belangrijk om de matrix van zowel de Jappen als de
 internationalen te weten. Die rommel die toen door PHILIPS
 en SONY is gemaakt wilt nog wel eens moeilijk doen, maar
 voor zover ik weet klopt die int. matrix wel redelijk. Eerst
 de beide matrixen:

               Toetsenbord matrix Europeaan:

 Adres  Rij           B i t   n u m m e r
              7    6    5    4    3    2    1    0

 &HFBE5  0    7    6    5    4    3    2    1    0
 &HFBE6  1    ;    ]    [    \    =    �    9    8
 &HFBE7  2    B    A    ACC  /    .    ,    `    '
 &HFBE8  3    J    I    H    G    F    E    D    C
 &HFBE9  4    R    Q    P    O    N    M    L    K
 &HFBEA  5    Z    Y    X    W    V    U    T    S
 &HFBEB  6    F-3  F-2  F-1  CODE CAPS GRPH CTRL SHFT
 &HFBEC  7    RET  SEL  BS   STOP TAB  ESC  F-5  F-4
 &HFBED  8    RGHT DOWN UP   LEFT DEL  INS  HOME SPC
 &HFBEE  9    4    3    2    1    0    not  not  not   numeriek
 &HFBEF  A    .    ,    �    9    8    7    6    5     eiland

                 Toetsenbord matrix Japanner:

 Adres  Rij           B i t   n u m m e r
              7    6    5    4    3    2    1    0

 &HFBE5  0    7    6    5    4    3    2    1    0
 &HFBE6  1    ;    [    @    Yen  ^    �    9    8
 &HFBE7  2    B    A    _    /    .    ,    ]    :
 &HFBE8  3    J    I    H    G    F    E    D    C
 &HFBE9  4    R    Q    P    O    N    M    L    K
 &HFBEA  5    Z    Y    X    W    V    U    T    S
 &HFBEB  6    F-3  F-2  F-1  KANA CAPS GRPH CTRL SHFT
 &HFBEC  7    RET  SEL  BS   STOP TAB  ESC  F-5  F-4
 &HFBED  8    RGHT DOWN UP   LEFT DEL  INS  HOME SPC
 &HFBEE  9    4    3    2    1    0    not  not  not   numeriek
 &HFBEF  A    .    ,    �    9    8    7    6    5      eiland


 Nu je beide matrixen hebt kun je al aan de slag. Met een
 ordinairde PEEK valt 't gewenste adres uit te lezen. Is een
 bit 0 dan is de betreffende toets ingedrukt. Om dus te kijken
 of de shift toets is ingedrukt, zoals kleine Tobiasje dat
 wilde, maken we dus 't volgende listinkje:

 10 IF PEEK(&HFBEB)<>&B11111111 THEN PRINT"J" ELSE PRINT "N"

 Maar een ieder kan zien dat dit niet echt netjes is, wordt
 namelijk F-1 ingedrukt, verschijnt er ook J in beeld. F-1
 staat immers ook in rij 6. Maar hoe test je nu op alleen
 bit 0 van rij 6? Simpel, 't zooitje ANDen. Dan krijg je dus:

 10 IF (PEEK(&HFBEB)AND&B00000001)=0 THEN PRINT"J"

 Natuurlijk mogen de rijen en kolomen verschillen maar dit is
 wel zo'n beetje hoe je 't best 't keyboard kan uitlezen. Dit
 is overigens ook handig als er iets ingevoerd moet worden maar
 je dit niet aan de basic interpreter wil overlaten. Ook voor
 pull-down menutjes  -die vaak met een non�active toets
 geactiveerd worden- is dit handig. Well folks, that's about
 it for today... next time I'll errr... well I don't know, but
 it'll be more usefull than this shit.... bye

 Tobias 'fushigi' Keizer
