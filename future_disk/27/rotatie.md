Jan-Willems brein maakt overuren...

 ROTATIE (1)

                             ����
                             ����
                             ����
                             ����



 Een tijd terug heeft Martijn ten Brink ooit een puik stukje
 programmeerwerk afgeleverd genaamd de rotator. Hiermee was
 het mogelijk om een plaatje over een bepaalde hoek te rote-
 ren. Martijn programmeerde het in KUN-basic. Omdat dit ook
 in machinecode kan (en dan iets sneller is) zal ik in de
 nu volgende text de formule die de rotatie beschrijft uitleg-
 gen en jullie voorzien van een brokje code waar de hersenen
 weer over gepijnigd kunnen worden.


 DE FORMULE

 De rotatieformule luidt als volgt:


        X2      cos a   sin a           X1 - X0 X0
           =                *            +
        Y2   -sin a cos a       Y1 - Y0     Y0


 Hier kunnen jullie natuurlijk niks uit opmerken, maar voor
 degenen die weten wat matrices zijn en op de juiste plaatsen
 wat verticale strepen plaatsen, zien zij dat [X2 Y2]t gevormd
 worden door de vermenigvuldiging van de 2*2 matrix, die met
 de sinusjes, met de 2*1 matrix, die met de X1 etc. plus de
 2*1 matrix [X0,Y0]t. We kunnen dit natuurlijk ook
 uitschrijven, dan krijgen we:

        X2 = ( cos a) * (X1 - X0) + (sin a) * (Y1 - Y0) + X0
        Y2 = (-sin a) * (X1 - X0) + (cos a) * (Y1 - Y0) + Y0

 Voor de meesten van u is dit al iets beter leesbaar.

 Uiteraard is dit een geniale formule, maar euh, wat roteert
 er nu en hoe ????

 DE PARAMETERS

OK, het antwoord hierop luidt:

X2,Y2:  zijn de eindcoordinaten van X1,Y1
X1,Y1:  zijn de coordinaten van de pixel die je wilt
                roteren
X0,Y0:  zijn de coordinaten van het centrum waaromheen je
                wilt roteren
a:              is de hoek waarover geroteerd wordt

 Klinkt interessant, maar zonder tekeningetje of voorbeeld
 wordt het niet echt verduidelijkt. Een tekeningetje kan deze
 (geniale, hemelsche, ridicule, smaelse) textroutine niet aan,
 dus een voorbeeld.

 Stel wij willen op screen 5 (256*212) het beeld een kwartslag
 naar rechts roteren. Dat is dus over een hoek van 90 graden.
 We draaien om de as van het beeld, dus die is het middelpunt
 van het scherm (128,106). Nu rekenen we voor punt (0,0) uit
 waar ie terecht komt.

 De parameters waren dus:

 a:              de hoek = 90 graden
 X0,Y0           het centrum van de rotatie: (128,106)
 X1,Y1           het punt dat geroteerd moet worden

 Er geldt nu:

 X2 = -1 * (0 - 128) + 0 * (0 - 106) + 128       = 256
 Y2 =  0 * (0 - 128) + 1 * (0 - 106) + 106       = 0

 En inderdaad, dit zijn de coordinaten waarop punt (0,0) te-
 recht moet komen.

 HET PROGRAMMA

Kijken we nog eens naar de formule:

        X2 = ( cos a) * (X1 - X0) + (sin a) * (Y1 - Y0) + X0
        Y2 = (-sin a) * (X1 - X0) + (cos a) * (Y1 - Y0) + Y0

Alles wat we nodig hebben, zijn:
- een subroutine om cos a en sin a uit te rekenen
- een (snelle) vermenigvuldigings routine
- nog wat vram lees en schrijf rommel

 Om cos a en sin a te berekenen kun je ingewikkeld doen en effe
 de machtreeks van beiden door je Z80 laten uitrekenen, maar
 aangezien dit veel (en als ik zeg veel, dan bedoel ik veel)
 tijd kost, is het simpeler om van te voren een tabel aan te
 maken met daarin genoeg waarden om een mooie rotatie te
 krijgen (zo om en nabij de 360 dus :) ).
 Het leuke van sinus en cosinus is, dan sin a = cos (a - 90).
 als ik dus een cosinustabel heb met 360 entries, kan ik door
 er de  nog 90 waarden aan toe te voegen gelijk de sinuswaarden
 uithalen. De tijd die dus nu nodig is om een sinuswaarde op te
 halen is minimaal. De volgende routines zouden al werken:

 cosinus:
 ; pre:  in HL de hoek.
 ; post: A = cos (HL)
         ld      de,tabel
         add     hl,de
         ld      a,(hl)
         ret
 sinus:
 ; pre:  in HL de hoek
 ; post: A = sin (HL)
         ld      de,tabel+90 ; uitgaand van een 360-entry tabel
         add     hl,de
         ld      a,(hl)
         ret

 De werking hiervan spreekt voor zich.
 Een nadeel hieraan is de nauwkeurigheid van de sinus. Zoals
 u allen weet geeft de sinus-functie waarden tussen de -1 en de
 1 af. In BASIC of een hogere programeertaal is dit geen
 probleem, maar in code kunnen we niet met de komma werken en
 ziet de processor een min toch echt voor iets anders aan.
 Een oplossing hiervoor is om of 2 bytes te gebruiken, of het
 bereik van onze z80 sinus tussen de -64 en de +64 te kiezen.
 Dus als je rekenapparaat 1 geeft, geeft onze routine een 64
 uit. Waarom 64, waarom niet 128? Het probleem hierbij is dat
 we graag met plusjes en minnen werken bij sinusfunctie en dat
 binnen een byte +128 niet te realiseren valt, we zouden dan de
 nul 2 keer definie�ren.

LEES VERDER IN TEKST (2)
 Het brein van Jan-Willem maakt weer overuren...

 ROTATIE (2)

                             ����
                             ����
                             ����
                             ����



 We gaan verder waar Jan-Willem gebleven was...


 Ook hebben we nog wat rappe vermenigvuldigings routines nodig.
 Het beste formaat is een routine die onze 8-bits sinuswaarde met
 de 16-bits waarde van X1-X0 vermenigvuldigd en het resultaat
 afleverd in een 16-bits getal.
 De minimale waarde die we kunnen krijgen is -64 * 256 = -16384
 en het maximum is 64 * 256 = 16384.
 Om rap te kunnen vermenigvuldigen slaan we het boekje
 Machinetaal Z80 effe open en tikken daaruit over:

 multiply:
 ; pre:  HL bevat 2 complement X1 - X0, a bevat sinwaarde
 ; post: HL = HL * A
 ; comment:      de vermenigvuldiging is signed
         push    hl
         exx
         pop     de
         ld      c,a
         and     a
         ld      b,0
         jp      p,positief
         dec     b
 positief:
         ld      hl,0
         exx
         ld      b,16
 multilus:
         exx
         sla     l
         rl      h
         sla     e
         rl      d
         jp      nc,geenoptelling
         add     hl,bc
 geenoptelling:
         exx
         djnz    multilus
         exx
         push    hl
         exx
         pop     hl
         ret

 Zoals jullie zien is deze vermenigvuldiging mooi signed.
 Hij kan misschien nog wel wat sneller, maar ik ben er
 tijdelijk wat lui voor om de snelst werkende te maken.
 Daarvoor verwijs ik jullie naar Alex Wulms of zo.

 Maar goed, nu we beide routines hebben, kunnen we het
 mainframe van de rotator gaan cre�ren.

 HET MAINFRAME

 We kijken weer es naar de formule:

        X2 = ( cos a) * (X1 - X0) + (sin a) * (Y1 - Y0) + X0
        Y2 = (-sin a) * (X1 - X0) + (cos a) * (Y1 - Y0) + Y0

 onze code wordt dus:

 start:
         call    leespixel
         ld      (inhoud),a

         ld      a,(hoek)
         call    cosinus

         ld      hl,X1
         ld      de,X0
         and     a
         sbc     hl,de

         call    multiply

         push    hl

         ld      a,(hoek)
         call    sinus

         ld      hl,Y1
         ld      de,Y0
         and     a
         sbc     hl,de

         call    multiply

         pop     de

         add     hl,de

         ld      de,X0
         add     hl,de

         call    convert
         ld      (xcoor),a


         ld      a,(hoek)
         call    sinus

         neg

         ld      hl,X1
         ld      de,X0
         and     a
         sbc     hl,de

         call    multiply

         push    hl

         ld      a,(hoek)
         call    cosinus

         ld      hl,Y1
         ld      de,Y0
         and     a
         sbc     hl,de

         call    multiply

         pop     de

         add     hl,de

         ld      de,Y0
         add     hl,de

         call    convert
         ld      (ycoor),a

         ld      hl,(xcoor)
         ld      a,(inhoud)
         call    putpixel
         ret

 leespixel:
 ; pre:  niks
 ; post: a= inhoud gelezen pixel
         ret
 putpixel:
 ; pre:  l=xcoor,h=ycoor,a=inhoud pixel
 ; post: pixeltje op het scherm
         ret
 convert:
 ; pre:  HL bevat X2 of Y2
 ; post: A bevat goede co�rdinaatwaarde
         ld      b,6
 convertloop:
         sra     h
         rr      l
         djnz    convertloop
         ld      a,l
         ret

 Na het lezen hiervan vallen U enkele dingen meteen op:
- de code is geniaal!
- de heer Van Helden is weer eens te lui om wat routines te
  coden;
- wat doet convert nu weer?

Tsja, ik zal enkel een antwoord geven op de laatste vraag.
Het zit namelijk zo. Omdat wij de sinus een bereik hebben ge-
geven van (-64,64), zitten onze waarden in het bereik (-256*64,
256*64). Om dit schoonheidsfoutje te verhelpen moeten we HL
nog 6 keer naar rechts schuiven. Om op clipping te controleren
hoef je enkel te kijken of na de routine convert H ongelijk aan
0 is. Is dit namelijk het geval, dan probeert je Z80 op een
negatieve x of y coordinaat iets weg te dumpen.

Op de disk staan de routines die ik gegeven heb in de file
"rotator.gen". Misschien heeft iemand zin om er iets mee te
doen. Oh ja, de sinustabel maak je gewoon in basic en schrijf
je dan weg naar disk.

Ik wens eenieder die met deze routines aan de slag gaat veel
plezier.

Jan-Willem van Helden
