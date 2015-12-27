H E T   . R E L   F I L E   F O R M A A T


Een GEN80  REL bevat  informatie gecodeerd in een bit-stream
dat als volgt ingedeeld is.

Als de  eerste bit een 0 is dan zullen de volgende 8 bits in
het adres van de location counter geladen worden.

Als  de eerste  bit een  1 is  dan volgen  er 2  bits met de
volgende betekenis.

00      Het is  een special  link item  die onder beschreven
wordt.
01      Program relative item, de volgende 16 bits zullen in
het adres van de location   counter  geladen  worden
nadat ze zijn opgetelt bij het basis adres  van  het
programma/code segment.
10      Data relative  item, de  volgende 16  bits zullen in
het  adres van  de loaction  counter geladen  worden
nadat ze  zijn opgeteld  bij het basis adres van het
data segment.

Een special link item bestaat uit het volgende:

1       Een 4 bits lange control field, onder beschreven.
2       Een adres field dat een 2 bits adress type field is,
gevolgd door een 16 bits adres. Het adres type field
bestaat uit het volgende.

00   Het adres is absoluut.
01   Het adres is relatief aan het programma.
10   Het adres is relatief aam het data segment.

3       Een  optioneel naam  veld dat bestaat uit een 3 bits
lengte teller gevolgd door de naam van de label in 8
bits ASCII.


    H E T   C O N T R O L   F I E L D

De volgende link items worden gevolgd door een naam veld:

0000    Deze label is PUBLIC verklaard in deze module.
0010    De naam van dit programma.

De volgend link item worden gevolgd door een adres field.

0111    Geeft de  waarde in  het adres field aan de label in
het naam veld.

De  volgende link items worden alleen gevolgd door een adres
field:

1010    Define data  size. Het  adres veld  bevat het totaal
aantal byte in het data segment van deze module
1011    Set  location counter,  veranderd het  adres van  de
location counter
1110    End of module. Geeft het einde van deze module aan
De volgende heeft geen velden
1111    End of file. Geeft het einde van de file aan
