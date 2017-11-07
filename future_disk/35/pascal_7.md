Jeroenie gaat verder met de Pascal cursus...
           PASCAL (7)


 ����  We zijn bijna aan het einde gekomen van een reeks PASCAL
 ����  cursi. Er zijn nog maar twee onderwerpen die ik wil
 ����  behandelen: POINTERS en RECURSIE. Pointers is een soort
 ����  data-type in PASCAL en recursie is een soort
       programmeermethode. Omdat beide onderwerpen interessant
 en (mijns inziens) belangrijk voor een programmeur zijn
 besteed ik per onderwerp twee teksten hieraan (zodoende kan
 ik dus nog drie teksten en FD nummers vooruit). Ik begin dus
 met pointers (les 1).

 Wat is het voordeel van een FILE ten opzichte van een ARRAY:
 - oneindig (een file kan in principe oneindig lang zijn)
 - flexibele grootte (een file kan altijd langer of korter
       gemaakt worden)
 - weinig geheugen nodig (voor een file is bijna geen
       geheugenruimte nodig, wel schijfruimte natuurlijk, maar
       daar hebben we tegenwoordig genoeg van)

 Wat is het voordeel van een ARRAY ten opzichte van een FILE:
 - snel(1) (ieder element kan onafhankelijk van de andere
       opgevraagd worden, dus niet eerst de hele file de revue
       laten passeren voordat het laatste element gebruikt kan
       worden)
- snel(2) (alles staat in het geheugen en geheugen is v��l
       sneller dan disk)
- gemakkelijk (makkelijk te programmeren, denk maar eens aan
       een sorteer-algoritme voor files)

 Door genoemde voordelen te combineren zou men een super
 gemakkelijk en snel data-type moeten kunnen krijgen. Men heeft
 lang (eigenlijk niet, maar voor het verhaal staat dat wel
 leuk) gedacht en is met pointers gekomen. Het had snel en
 gemakkelijk moeten zijn. Het is snel, maar zeker NIET
 gemakkelijk. Wat zijn pointers dan wel vraag je je af?
 - snel (alles staat in het geheugen)
 - oneindig (zolang er geheugen is kunnen er pointers
       aangemaakt worden)
- flexibele grootte (tijdens programma executie kunnen nieuwe
       pointers aangemaakt worden)

 Je kunt je nu natuurlijk nog niets bij pointers voorstellen,
 maar dat ga ik even veranderen. Wat is een pointer namelijk in
 de computer (hoe werkt het)? Een pointer is (zoals de naam al
 zegt) een wijzer (NvdR: grote meid!). Genoemde wijzer kan
 wijzen naar niets of naar een data-element. Wijst het naar
 niets, dan wijst hij naar NIL (gereserveerd woord), dat
 (waarschijnlijk) het getal 0 is en dus geheugenruimte 0 is.
 Wijst het naar een data-element dan is de pointer het adres
 waar dat data-element gevonden kan worden. Dus als ik een
 pointer A heb, dan kan de inhoud van A NIL zijn of
 (bijvoorbeeld) #a084. NIL wil zeggen dat de pointer nergens
 naar wijst, en #a084 dat de pointer naar adres #a084 wijst.
 Staat op adres #a084 het getal 10, dan wijst de pointer dus
 het getal 10 aan. Grafisch wordt een en ander vaak als volgt
 weergegeven:

  A
 +-+    +----+
 | |--->| 10 |
 +-+    +----+

 Bovenstaand geval is als een pointer een byte (1 byte in het
 geheugen) aanwijst. Er zijn dus wel 3 bytes nodig om een en
 ander goed te laten werken (2 voor de pointer (16 bits adres)
 en 1 voor het data-element (byte)). Een pointer kan elk data-
 type aanwijzen dat de gebruiker maakt, hierdoor is het
 mogelijk om een zogenoemde gelinkte lijst te maken (een
 pointer die naar een pointer wijst die naar een pointer wijst
 die naar een pointer wijst die naar een pointer wijst die naar
 een pointer wijst die naar een pointer wijst die naar een
 pointer wijst die naar een pointer wijst die naar een pointer
 wijst ...............). Op zich heb je daar dus niets aan,
 maar als je alle pointers nu eens naar een RECORD laat wijzen
 dat een data-element EN een pointer bevat, dan kom je al een
 heel eind. Grafisch ziet dat er als volgt uit:

  Hoofd
 +-+    +---+-+    +---+-+    +---+-+    +---+-+    +---+-----+
 | |--->| 5 | |--->| 9 | |--->| 4 | |--->| 0 | |--->| 1 | NIL |
 +-+    +---+-+    +---+-+    +---+-+    +---+-+    +---+-----+

 Allemaal heel leuk, maar hoe gebruik je dit (oftewel geef een
 programma, dan zien we tenminste iets). Kijk maar eens naar de
 file "PASCAL7.PAS" op deze diskette, daar staat een voorbeeld
 in.

 Bij TYPE staat de declaratie van een pointer:
   typenaam1 = ^typenaam2 (* ^ staat voor 'pointer naar' *)

 Hier is dus wel iets geks aan de hand, want Next is een
 pointer naar Element, maar Element is nog niet bekent......
 MAG DAT???? Ja, dat mag, als uitzondering. Zouden we Element
 en Point in de declaratie omdraaien, dan zouden we namelijk een
 soortgelijk probleem hebben omdat in Element, Next van het
 type Point is. Bij pointers mag er dus een type vooruit genoemd
 worden. Overigens wel in de volgorde zoals in het voorbeeld
 staat (dus eerst het pointer-type en daarna pas het type waar
 de pointer naar wijst).

 De variabele declaratie zal geen problemen opleveren, dit in
 tegenstelling tot het hoofdprogramma. Het eerste nieuwe dat we
 tegenkomen is 'Main:=NIL'. Aangezien Main nog een
 ongedefinieerde waarde heeft, maken we deze eerst gedefinieerd
 (vanaf nu wijst Main dus nergens naar). Het volgende is
 'NEW(Temp)'. Hier wordt een geheugenplaats aangevraagd voor
 het data-type van Temp. (voor de opdracht NEW wijst de pointer
 Temp naar een ongedefinieerde plaats in het geheugen. Bij de
 NEW opdracht wordt er in het geheugen gezocht naar een plaats
 waar het data-type van de pointer inpast (in dit geval een
 integer en een pointer)). Deze geheugenruimte wordt dan
 gereserveerd voor zulk een data-type en de pointer wijst vanaf
 nu naar genoemde plaats in het geheugen).

 De toekenningsopdrachten zijn min of meer vanzelfsprekend als
 je weet dat pointer^ het data-element is (dus Main^ is een
 RECORD met twee velden (Value en Next), de inhoud van Value
 is dus te bereiken met Hoofd^.Value). Wel moet ik vermelden
 dat als je een pointer (bijvoorbeeld Temp) zelf veranderd, dat
 de computer dan niet de geheugenruimte vrijmaakt waar de
 pointer naar wijst. Door alsmaar 'NEW(Temp)' als opdracht te
 geven zou het geheugen dus vollopen (zorg ervoor dat je het
 adres van het data-element ergens bewaart, anders ben je het
 voorgoed KWIJT in het walhalla van je computer geheugen). In
 het voorbeeld programma raak ik niets kwijt (ook visueel
 zichtbaar) omdat ik een gelinkte lijst maak (ik laat de
 elementen naar elkaar wijzen).

 Wil je de geheugenplaats waar een pointer naar wijst vrij
 maken dan gebruik je DISPOSE (zoals ook op het einde van het
 programma te zien is).

 Zo, dat was het dan alweer. Volgende keer meer, veel meer over
 pointers, maar voor nu 2 opdrachten om jezelf te testen:

 (1) Zie voorbeeld programma, maar nu de getallen in volgorde
     van invoer in de lijst plaatsen.
 (2) Zie voorbeeld programma, maar nu de getallen gesorteerd in
     de lijst plaatsen.

 Veel plezier,

Red XIII
