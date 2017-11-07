Het copy commando in Basic..hmm interessant...hmm ja, daar heb ik nu altijd al eens wat over willen weten...aja, ja ja, ahum, mmmm, ja ja ja interessant...aha hmm ye ye yep ja ja
        COPY IN BASIC

                             ����
                             ����
                             ����
                             ����


 Het COPY-commando heeft meerdere betekenissen. Je kunt er
 bestanden mee copi�ren en stukjes van een beeld mee copi�ren.
 Dat laatste kan op twee manieren, namelijk van VRAM naar
 VRAM en van VRAM naar een array of andersom. De eerste
 functie kent iedereen natuurlijk wel, maar de tweede is toch
 wel onbekend. Zoals gezegd kun je dus ook een array
 gebruiken om naar te copi�ren of weer in het VRAM te zetten.

 Voordat je een stuk scherm naar een array copi�ert, moet je
 die eerst berekenen en defini�ren. De grootte van een array
 kan met het volgende programmaatje berekend worden:

 10 PRINT "Bereken grootte voor array-dimensie voor
 copy-commando"
 20 PRINT:INPUT "Linker X-coordinaat";X1
 30 INPUT "Boven y-coordinaat";Y1
 40 INPUT "Rechter X-coordinaat";X2
 50 INPUT "Onder y-coordinaat";Y2
 60 PRINT:INPUT "Schermnummer";S
 70 PRINT:INPUT "Variabele type (% = Integer, ! = Single
 precision en # = Double precision";A$
 80 D=4:IF S=6 THEN D=2
 90 D=INT((D*(ABS(X2-X1)+1)*(ABS(Y2-Y1)+1)+7)\8)+4
 100 T=2:IF A$="!" THEN T=4 ELSE IF A$="#" THEN T=8
 110 D=INT(D/T)
 120 PRINT:PRINT "DIM M(";MID$(STR$(D),2);")"

 Zo, nu komt de COPY-commando zelf aan bod. Eerst de
 COPY-commando om een deel van het beeld in het geheugen te
 zetten.

 COPY (X1,Y1)-(X2,Y2)[,P] TO V

 X1 is de linker-coordinaat van het te copi�ren gedeelte. Y1
 is de boven-coordinaat van het te copi�ren gedeelte. X2 is
 de rechter-coordinaat van het te copi�ren gedeelte. Y2 is de
 onder-coordinaat van het te copi�ren gedeelte. P is de
 pagina-nummer. V is de naam van de variable waar de tekening
 in gezet moet worden. Alleen P hoeft niet ingevuld worden,
 zoals te zien is aan de vierkante haken.

 Okee, de tekening moet natuurlijk ook weer op het beeld
 kunnen komen. Dit doen we met:

 COPY V[,S] TO (X,Y)[,P[,O]]

 V is weer de naam van de variable waar de tekening in zit. S
 is de soort bewerking wat er met de tekening moet gebeuren.
 Hierbij is 0 normaal, 1 = horizontaal spiegelen, 2 =
 verticaal spiegelen, 3 = 1 + 2. X en Y zijn de coordinaten
 waar de tekening heen moet. P is weer de paginanummer en O
 is de logische operatie. Aangezien deze tekst niet over
 logische operaties gaat, ga ik daar dus ook niet verder op
 in. S, P en O mogen weggelaten worden. Hier komt een
 voorbeeldprogrammaatje. Start het op en je ziet het effect.

 10 SCREEN5:DIM M(66):LINE (0,0)-(0,15),15
 20 LINE (0,7)-(15,7),15
 30 COPY (0,0)-(15,15) TO M
 40 COPY M,0 TO (100,50)
 50 COPY M,1 TO (100+15,80)
 60 COPY M,2 TO (100,110+15)
 70 COPY M,3 TO (100+15,150)
 80 A$=INPUT$(1)

 Zoals je ziet kun je dus makkelijk effecten gebruiken in
 BASIC, maar je moet natuurlijk wel zorgen dat het wel in het
 geheugen past, want naar een array copi�ren kost al snel
 veel geheugen. Tot de volgende keer...


Vincent

