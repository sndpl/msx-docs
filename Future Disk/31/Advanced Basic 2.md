Arjan vervolgt zijn cursus Advanced Basic...

Cursus Advanced BASIC (2)

 Voordat ik weer wat commando's ga uitleggen, geef ik eerst 
 de bestelinformatie van Advanced BASIC 1.01. Smael was zo 
 dom dat hij vergat dat bij de recensie op FD #30 te zetten.
 Maar goed, hier komt de informatie:

 Maak f 17,50 over op bankrekeningnummer 32.56.20.806 t.n.v. 
 Arjan Bakker te Hasselt o.v.v. AdvBASIC en je eigen naam en 
 adres. De bank is de Rabobank te Hasselt (Ov).


MORE ABOUT WINDOWS

 Nog niet alle windows-commando's waren behandeld de vorige 
 keer, dus komt hier de rest.

 Met _CSRLIN(adres) kun je de regel waar de cursor in een 
 window op staat opvragen. De regel komt dan in het opgegeven 
 adres terecht.

 Met _POS(adres) komt de x-positie van de cursor in een 
 window op het opgegeven adres terecht.

 _NWINDOW(adres) geeft het aantal gebruikte windows terug.

 _WINDATA(adres) geeft informatie over het window terug. Op 
 adres+0 staat de x-positie van het window, adres+1 is de 
 y-positie van het window, adres+2 de breedte en adres+3 de 
 hoogte.

 Een voorbeeld:

 10 SCREEN 0
 20 _WINDOW(0,0,80,5)
 30 _LOCATE(40,3)
 40 _CSRLIN(&HD000)
 50 _POS(&HD001)
 60 _NWINDOW(&HD002)
 70 _WINDATA(&HD003)

 Vanaf adres &HD000 komen de diverse data in het geheugen.


TRUUKJE

 Je kunt opgevraagde waardes ook direct in een variabele 
 stoppen. Dit gaat als volgt:

 _NWINDOW(VARPTR(N))

 Het aantal aanwezige windows staat nu in variabele N. Let er 
 wel op dat de variabele N bestaat �n integer is, want anders 
 werkt het niet. Oh ja, deze truuk werkt niet bij _WINDATA.


MEER KLEUREN OP SCHERM 0

 Screen 0 kan 4 kleuren tegelijk laten zien. Advanced BASIC 
 heeft hiervoor een viertal routines die dat wel erg 
 makkelijk voor jullie maken, namelijk:

 _COLOR(vg,ag,p1,p2). Vg is de voorgrondkleur, ag is de 
 achtergrondkleur, p1 is de periode dat de voorgrondkleur 
 zichtbaar is (in 1/50-seconden) en p2 is de periode dat de 
 achtergrondkleur zichtbaar is.

 Met _PUT(x,y,a) kun je een aantal karakters met andere 
 kleuren op het scherm zetten. X en y geven de x- en 
 y-positie  weer en a is het aantal karakters.

 _UNPUT(x,y,a) doet het omgekeerde van _PUT, namelijk het 
 weghalen van de karakters.

 _CLS wist de alternatieve schermkleuren.

 Een voorbeeld:

 10 SCREEN 0
 20 _COLOR(15,0,15,15)
 30 _PUT(0,0,80*24)
 40 _UNPUT(0,0,80*24)
 50 GOTO 30

 Het hele scherm wordt eerst gevuld met de alternatieve 
 schermkleuren en daarna worden deze weer gewist. Dan wordt 
 het scherm weer gevuld, gewist enz. enz.


TOT SLOT

 Tot slot komen hier nog wat simpele commando's.

 _CLBUF         : Wist de toetsenbordbuffer.
 _SNDOFF        : Zet het PSG-geluid uit.
 _CAPSON        : Zet CAPS-lock aan.
 _CAPSOFF       : Zet CAPS-lock uit.
 _INFO          : Geeft wat informatie over Advanced BASIC.
 _VERSION       : Geeft het versie-nummer van Advanced BASIC.


DE VOLGENDE KEER

 De volgende keer komen de nieuwe disk-commando's aan bod.
 Maar tot FD 32, dus maar!


Arjan Bakker
Arjan vervolgt zijn cursus Advanced Basic...

Cursus Advanced BASIC (2)

 Voordat ik weer wat commando's ga uitleggen, geef ik eerst 
 de bestelinformatie van Advanced BASIC 1.01. Smael was zo 
 dom dat hij vergat dat bij de recensie op FD #30 te zetten.
 Maar goed, hier komt de informatie:

 Maak f 17,50 over op bankrekeningnummer 32.56.20.806 t.n.v. 
 Arjan Bakker te Hasselt o.v.v. AdvBASIC en je eigen naam en 
 adres. De bank is de Rabobank te Hasselt (Ov).


MORE ABOUT WINDOWS

 Nog niet alle windows-commando's waren behandeld de vorige 
 keer, dus komt hier de rest.

 Met _CSRLIN(adres) kun je de regel waar de cursor in een 
 window op staat opvragen. De regel komt dan in het opgegeven 
 adres terecht.

 Met _POS(adres) komt de x-positie van de cursor in een 
 window op het opgegeven adres terecht.

 _NWINDOW(adres) geeft het aantal gebruikte windows terug.

 _WINDATA(adres) geeft informatie over het window terug. Op 
 adres+0 staat de x-positie van het window, adres+1 is de 
 y-positie van het window, adres+2 de breedte en adres+3 de 
 hoogte.

 Een voorbeeld:

 10 SCREEN 0
 20 _WINDOW(0,0,80,5)
 30 _LOCATE(40,3)
 40 _CSRLIN(&HD000)
 50 _POS(&HD001)
 60 _NWINDOW(&HD002)
 70 _WINDATA(&HD003)

 Vanaf adres &HD000 komen de diverse data in het geheugen.


TRUUKJE

 Je kunt opgevraagde waardes ook direct in een variabele 
 stoppen. Dit gaat als volgt:

 _NWINDOW(VARPTR(N))

 Het aantal aanwezige windows staat nu in variabele N. Let er 
 wel op dat de variabele N bestaat �n integer is, want anders 
 werkt het niet. Oh ja, deze truuk werkt niet bij _WINDATA.


MEER KLEUREN OP SCHERM 0

 Screen 0 kan 4 kleuren tegelijk laten zien. Advanced BASIC 
 heeft hiervoor een viertal routines die dat wel erg 
 makkelijk voor jullie maken, namelijk:

 _COLOR(vg,ag,p1,p2). Vg is de voorgrondkleur, ag is de 
 achtergrondkleur, p1 is de periode dat de voorgrondkleur 
 zichtbaar is (in 1/50-seconden) en p2 is de periode dat de 
 achtergrondkleur zichtbaar is.

 Met _PUT(x,y,a) kun je een aantal karakters met andere 
 kleuren op het scherm zetten. X en y geven de x- en 
 y-positie  weer en a is het aantal karakters.

 _UNPUT(x,y,a) doet het omgekeerde van _PUT, namelijk het 
 weghalen van de karakters.

 _CLS wist de alternatieve schermkleuren.

 Een voorbeeld:

 10 SCREEN 0
 20 _COLOR(15,0,15,15)
 30 _PUT(0,0,80*24)
 40 _UNPUT(0,0,80*24)
 50 GOTO 30

 Het hele scherm wordt eerst gevuld met de alternatieve 
 schermkleuren en daarna worden deze weer gewist. Dan wordt 
 het scherm weer gevuld, gewist enz. enz.


TOT SLOT

 Tot slot komen hier nog wat simpele commando's.

 _CLBUF         : Wist de toetsenbordbuffer.
 _SNDOFF        : Zet het PSG-geluid uit.
 _CAPSON        : Zet CAPS-lock aan.
 _CAPSOFF       : Zet CAPS-lock uit.
 _INFO          : Geeft wat informatie over Advanced BASIC.
 _VERSION       : Geeft het versie-nummer van Advanced BASIC.


DE VOLGENDE KEER

 De volgende keer komen de nieuwe disk-commando's aan bod.
 Maar tot FD 32, dus maar!


Arjan Bakker

