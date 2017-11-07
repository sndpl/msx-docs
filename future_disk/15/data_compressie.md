                       Data Compressie

 ����   De machinetaal-cursus is naar mijn idee be�indigd
 ����   en daarom sluit ik dat dossier en begin met een
 ����   cursus. Je denkt misschien:"Hee, die titel heb ik al
 ����   eens in de MCCM zien verschijnen??? Schreven Falco
        en Ivo daar niet pagina's over vol ???"

 Inderdaad Ivo en Falco schreven er pagina's over vol, maar
 lieten je vaak zitten met aanwijzingen hoe het moest en
 bespraken maar twee methoden van compressie. In deze cursus
 wil ik er een stuk dieper op ingaan dan Falco en Ivo en
 lever ik kant en klare PD crunch-routines af, om in je eigen
 programma's te gebruiken. Maar laten we eerst het nut van
 datacompressie eens bespreken.

 Het nut van datacompressie is, zoals de naam al zegt, het
 samenpakken van de orginele data tot een kleiner geheel. Ook
 de mogelijkheid om vele files in 1 file samen te pakken,
 maakt datacompressie een fijn hulpmiddel om �n ruimte te
 besparen, �n overzicht te houden.

 Datacompressie deed zijn intrede rond de tijd van de eerste
 harddisks. Plotseling hadden gebruikers 5 MB aan
 opslagruimte i.p.v. de 360 KB op een diskette of nog minder
 op een cassette. De gebruikers wilden deze opslagruimte zo
 efficient mogelijk gebruiken om er zoveel mogelijk
 programma's op te zetten. Ook hadden ze voor de uitwisseling
 data op de "gewone" diskdrives liefst zo weinig mogelijk
 diskettes nodig. Als ze een programma van 5 MB hadden en een
 medegebruiker wilde dat programma ook graag hebben (illegale
 kopietjes draaien!) wilde men natuurlijk niet met (1 MB/360
 KB) 3 diskettes rond gaan lopen (dan is de pakkans immers
 groter) plus dat de man dan zo'n 30 � 40 files zo competent
 mogelijk over de diskettes zou moeten verdelen om niet met
 nog meer disks over straat te moeten. Daarom werden er
 verscheidene compressie methoden uitgedacht door wiskundigen
 en informatici om zo compact mogelijk data op te slaan.

 De deskundigen kwamen tot de conclusie dat er in een
 data-reeks altijd patronen of repetities van data-elementen
 zaten. Deze ontdekking resulteerde in twee methoden van
 datacompressie:

 - Het tellen van gelijke opeenvolgende data-elementen en
 deze opslaan als data-element + aantal keren opelkaar
 volgend voorkomen.

 - Het zoeken naar patronen in de data en deze m.b.v. codes
 opslaan.

 De eerste compressie methode heeft het grote nadeel in
 bijvoorbeeld tekstfiles. Als er weinig dezelfde elementen
 achterelkaar voorkomen, zou het kunnen dat de compressie
 zelfs negatief zou gaan werken. Immers, als men aan de
 standaard opslag vasthoudt, zou je elk data element opslaan
 als: "DATA-ELEMENT + aantal keer opeenvolgend voorkomen"
 je dubbel zoveel of meer ruimte nodig had dan het orgineel.
 De tweede compressie methode heeft het nadeel dat de
 (basis)patronen ongecodeerd moeten worden opgeslagen.

 Het ligt aan de orginele data welke compressie methode het
 beste werkt. Een klein voorbeeldje:

 Stel je hebt een leeg SCREEN 8 plaatje. Dit plaatje neemt
 (256*212) 54272 bytes aan opslag-ruimte in. Laten we er nu
 de eerste compressie methode op los, dan zal het plaatje nog
 maar 3 bytes innemen, namelijk "DATA-ELEMENT (0) + aantal
 keer opeenvolgend voorkomen (54272)". Dit is een compressie
 van 0.00552 % !!!!!
 Zou je nu de andere compressie methode erop los laten, dan
 zou je (afhankelijk van de patroon zoek methode) altijd een
 grotere opslag krijgen dan de 3 bytes van de andere methode.

 Maar.....neem deze tekstfile eens als voorbeeld. Als we hier
 de eerste methode op zouden loslaten zouden we een enorme
 negatieve compressie krijgen. De opeenvolging van dezelfde
 data-elementen (de letters dus) is extreem zeldzaam. Het
 woord "Boemel", normaal 6 bytes, zou worden opgeslagen
 als: "B"(1)+"o"(1)+"e"(1)"+"m"(1)+"e"(1)+"l"(1) = 12 bytes.
 Dit is een compressie van 200%, zeer negatief dus !
 Met de andere methode zou de computer kunnen vinden dat de
 "e" als patroon 2x voorkomt en dus een kleinere bitlengte
 moet krijgen dan de andere elementen (Huffman-principe). De
 compressie zou nog steeds niet groot zijn, maar altijd nog
 niet negatief. Alleen de opslag van de patroontabel zou de
 uiteindelijke compressiefile weer groter kunnen maken dan
 het orgineel.

 Overigens heeft datacompressie natuurlijk geen zin op een
 reeks als "ABCDEFGHIJKLMNOP". Hierin is zelfs door mensen
 geen patroon of opeenvolging in te vinden dus heeft het meer
 zin om deze data "gewoon" op te slaan. De beste compressie
 methode zou natuurlijk moeten kijken of de compressie wel
 zin heeft, want om negatieve compressie staat niemand te
 springen.

 Zo, dit is ongeveer de inleiding en de benodigde achtergrond
 kennis om deze cursus te begrijpen en de eruitvoorkomende
 programma's zelf te kunnen maken en nieuwe compressiemetho-
 den te programmeren.

                                       Jan-Willem van Helden
