Hmmm....nu wordt het moeilijk...ik zeg niks...
       DATACOMPRESSIE

                             ����
                             ����
                             ����
                             ����


 Lang, heel lang geleden stonden er wat teksten over
 datacompressie op de FD (ik meen zo rond FD 20). Aangezien
 er veel manieren zijn om data te crunchen, is hier weer een
 tekstje over een manier om data te crunchen die in bepaalde
 gevallen best wel handig kan zijn.

 De compressiemethode die ik nu ga bespreken, heeft voor
 zover ik weet geen naam, maar dat maakt op zich niet uit. De
 methode is snel te implementeren en het (de)crunchen gaat
 aardig snel.

 De compressiemethode gaat ervan uit dat een byte in zekere
 mate herhaald wordt. Per byte wordt onderzocht of de byte
 hetzelfde is. Als dat zo is, wordt er een 0 weggeschreven
 naar een bitstream. Als het niet gelijk was, wordt er een 1
 weggeschreven en de byte zelf. Bij teksten bijvoorbeeld komt
 de spatie heel vaak voor. Hier wordt dus de spatie met 1 bit
 opgeslagen en de overige bytes met 9 bits. Om alles
 duidelijk te maken, komt hier een voorbeeld:

 Stel, we hebben de volgende reeks bytes:

   0 10 25 10 4 6 12 10 50 10 10

 De 10 komt het meeste voor in deze reeks. Telkens als er 10
 staat in de reeks, dan schrijven we een 0 weg en anders
 schrijven we een 1 weg gevolgd door de byte zelf. Hier komt
 eventjes de gecrunchde data (links in bytes, rechts in
 bits):

   0 = 1 00000000
  10 = 0
  25 = 1 00011001
  10 = 0
   4 = 1 00000100
   6 = 1 00000110
  12 = 1 00001100
  10 = 0
  50 = 1 00110010
  10 = 0
  10 = 0

 De lengte van de ungecrunchde data was 11 bytes, dus 88
 bits. De gecrunchde data is echter maar 59 bits groot.
 Natuurlijk moet je ook weten welke byte er met een 0 bedoeld
 wordt, dus dat kost weer 8 bits extra, maar de data wordt
 nog altijd kleiner.


          VARIATIES

 Er zijn nog veel variaties op deze methode te verzinnen. Bij
 tekeningen bijvoorbeeld kun je voor ��n hele tekening een
 vaste byte nemen, maar je kunt het ook per lijn doen en dat
 is waarschijnlijk wel efficienter. Deze compressie-methode
 is alleen handig bij tekstdata en tekeningen, want daarin
 zitten veel dezelfde bytes. In programma's zijn er ook wel
 bytes die vaak voorkomen, maar die komen dan niet vaak
 genoeg voor om deze goed op deze manier te kunnen inpakken.
 (Nvdr: euhmmm...Arjan...zou je dit voor de volgende keer
 eens wat meer kunnen uitdiepen?)

Vincent
