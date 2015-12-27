# M E M M A N   2 . 4 2 
                                          

In  de speciale MemMan rubriek op de Special mag de nieuwste 
versie van MemMan natuurlijk niet ontbreken. Eerst maar even 
wat er allemaal nieuw is in deze nieuwe versie...


## W A T   I S   N I E U W ? 

1 - Voor mensen die de 32 kB RAM willen kunnen aansturen die 
    in BASIC achter de ROM's zitten, is er de functie GetTPA 
    toegevoegd.
2 - Voor HD-gebruikers  wordt er een mogelijkheid geboden om 
    de  TSR's makkelijker  in te  laden: in  het environment 
    item  TL  dient de  drive en  de subdirectory  te worden 
    opgegeven. Daarna  kijkt TL  niet alleen  in de  huidige 
    directory, maar ook de met TL aangegeven directory.
3 - Met TV.COM kun je nu ook zien in welk segment de diverse 
    TSR's  zich  bevinden.  Zo kun  je precies  zoveel TSR's 
    wissen dat ��n segment vrijkomt.
4 - De  stack van  MemMan is  nu in  te stellen en/of uit te 
    lezen met  de functies  GetMMSP en  SetMMSP. Tot nog toe 
    gebruikt  alleen Tracer,  die er  al op  voorbereid was, 
    deze mogelijkheid.
5 - In BASIC  kun je  nu een  overzicht krijgen van de extra 
    commando's  die de TSR's je verschaffen door CMD HELP in 
    te tikken. Tracer was (ook) hier op voorbereid. Maar ook 
    het ingebouwde MSX TsrUtils is er mee uitgerust.
    Helaas moet de TSR dat wel zelf opvangen, het zou mooier 
    geweest zijn  wanneer je,  als programmeur,  de gewenste 
    tekst in de TSR op een bepaalde plaats zet, zodat MemMan 
    CMD HELP afwerkt.
6 - TSR's  kunnen  MemMan  nu aanroepen  door naar  #4002 te 
    springen;  daar zit een MemMan entry. Hierdoor worden ze 
    sneller en kleiner.
7 - IniChk laat  de functieafhandelingsroutine van MemMan nu 
    in  HL achter.  Daarna is  een JP (HL) voldoende om naar 
    MemMan te springen. Maar TSR's hebben het, zoals in punt 
    6 besproken, makkelijker.


## H O E   K O M   J E   E R A A N ? 

Heel simpel te beantwoorden: Op deze disk staat MEMM242.PMA. 
Deze library  bevat alle  13 bestanden  die bij  MemMan 2.42 
behoren. O.a. een nieuwe versie van BK, de oude kon niet met 
meer dan 4 MB overweg.

Als je geen MSX-DOS 2 hebt, kopieer MEMM242.PMA en PMEXT.COM 
dan  naar  een  lege  diskette  (of  een  diskette  met  nog 
voldoende ruimte) en typ het volgende in:

PMEXT MEMM242 A:

Als  je  MSX-DOS 2 hebt kan het veel sneller door de RAMdisk 
te  gebruiken.  Maak een  RAMdisk aan  en doe  vervolgens de 
Special diskette in de drive. Typ nu het volgende in:

PMEXT MEMM242 H:

Na afloop natuurlijk wel nog even de files naar een diskette 
kopi�ren.


## D A T U M   E N   T I J D 

Leuk aan  MemMan is  dat ze  de datum  en tijd  van de files 
aanpassen.  Bij de meeste files is de tijd op 2:42 gezet, en 
de datum op 19/09/92, de releasedatum van deze MemMan.

Voor uitleg  en technische  specificaties bij MemMan verwijs 
ik u naar de tekstfiles in MEMM242.PMA.

Stefan Boer
Kasper Souren
