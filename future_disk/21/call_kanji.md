 Een aardige uitleg door de DHR KEIZER. Hoe typ ik het Japanse woord voor "schaamhaar" in zonder dat ik het zelf weet?


CALL KANJI??

                             ����
                             ����
                             ����
                             ����

 Yep,  voor  de  2-plussers -neen, dit  zijn geen  kleuters  of
 peuters-  onder  jullie wel.  Iedereen kent  dat _Kanji  gedoe
 wel,  veel  mensen  weten  echter  niet dat  dit meer  dan wat
 grotere lettertjes op kan leveren.

 Modi:
 De Kanji mode op de 2+ kent weer een aantal verschillende
 modi. Vier om precies te zijn. Het verschil zit hem vooral
 in de letter-breedte en letter hoogte. De verschillen:
 _KANJI0 ; 8*16, low res, non interlace.
 _KANJI1 ; 6*16, low res, non interlace.
 _KANJI2 ; 8*16, highres, interlace.
 _KANJI3 ; 6*16, highres, interlace.

 De 8*16 en de 6*16 staan voor de pixel breedte/hoogte van de
 ASCII karakters. Het normale font wordt namelijk uitgezet. In
 plaats hiervan wordt er een speciaal karaktersetje door de Jis
 rom ingeschakelt dat de normale ascii letters moet vervangen.
 De JIS-Karaktes -zoals de Kanji-letters heten- zijn echter
 altijd 16*16. De jis letters uit mode 0 & 1 en uit mode 2 & 3
 zijn dus verder gelijk. Het voordeel van deze schermen was dus
 eigenlijk alleen voor de Japanners merkbaar. Ze konden immers
 kanji op hun scherm krijgen die leesbaar zijn. Voor ons zijn
 er echter ook voordelen. Zo is nu eindelijk dat gepiemel met
 OPEN "GRP:" AS #1 afgelopen in een grafisch scherm. Staat de
 kanji-driver aan, dan volstaat een PRINT gewoon. Tevens kunnen
 commando's als locate en input na het aanroepen van de kanji
 driver gewoon gebruikt worden in de grafische schermen. Maar
 zelf vind ik bijvoorbeeld de karakters in KMODE2 een stuk
 mooier dan die iele ascii lettertjes. Jammer is wel dat de
 interlacing na lange type sessies gaat storen.

 Niet alles werkt:
 Neen helaas niet, maar daar hebben de heren bij ASCII wel een
 oplossing op gevonden. Zo zal het commando CLS domweg een
 "Illegal function call" opleveren. Evenals COLOR= niet meer
 toegestaan is. De oplossing hiervoor is vrij simpel. _CLS doet
 het prima in een kanji scherm, terwijl _PALETTE de oplossing
 blijkt te zijn voor het COLOR= probleem. Verder vind ik het
 toch wel storend dat het niet mogelijk is om de gewenste kanji
 op de gewenste X-Y positie te krijgen in een grafisch scherm.
 De enige manier waarop dit mogelijk is, is door het gebruiken
 van Jis-codes -die ik later in deze text nog bespreek- die vry
 onhandig zijn en in feite nooit voorkomen. In grafische modi
 ka dit erg lastig zijn daar er dus karakters zijn met verschil
 lende breedtes. De 'standaard' gebruiker zal echter aan locate
 genoeg hebben.

 Jis en Shift-Jis.
 Om het grote aantal tekens -Kanji, Katakana en Hiragana- uit
 de Japanse taal in een computer te krijgen was een nieuw en
 beter systeem nodig. De ascii set gaf immers maar ruimte voor
 256 tekens min 32 tekens voor de schermaansturig. Voor de Jap-
 anse taal bij lange na niet genoeg. Er moest dus een nieuw 2-
 byte systeem worden ontworpen waarmee dus 65536 tekens kunnen
 worden opgeslagen. Na een aantal lange druilerige zondag mid-
 dagen was men er uit in Japan. De Jis standaard vond het licht
 en alles ging veranderen. 65536 tekens zou zelfs voor de Chi-
 nese taal genoeg zijn, maar goed... Vrijwel alle mogelijk Kan-
 ji uit de Japanse taal werden in het Jis-systeem opgenomen,
 evenals alle Katakana en Hiragana, met accenten en zonder.
 Maar zelfs toen hadden de heren nog ruimte over. In de Jis2
 standaard staan dus, "alle" Kanji, Katakana, Hiragana, de
 Europese schrift-tekens, het arabische numerieke stelsel, de
 Griekse schrift-tekens, russisch, oud latijns, dubbele getal-
 len, dubbele letters, alle! mogelijke leestekens uit alle wet-
 telijk erkende talen(NvdR: ook Ijslands?), "alle" valuta-
 symbolen, "alle" symbolen van maten en eenheden, tekeningen!
 voor educatieve software, de nodige symbolen van
 telefoontjes, vliegtuigen enz, afkortingen van bv TEL, FAX,
 No. enz..... Te veel om op te noemen -en toch deed ik dat
 net-. In de meeste 2+en en in de Turbo-R zitten zo wel de
 JIS1 als de JIS2. In alle DOS2 cartridges zit de JIS1..
 In totaal zit er in de Turbo een dikke 256K aan Jis
 lettertjes wat genoeg is voor 8192 karakters, waarvan er
 zeker zo'n 7000 gebruikt worden. Om deze enorme hoeveelheid
 aan karakters te kunnen gebruiken zijn er 2 aanstuur-codes
 in gebruik. JIS en SHIFT-JIS. Het verschil tussen die 2 is
 nogal vaag maar het komt er op neer dat JIS meer iets van
 een adres weg heeft en SHIFT-JIS meer een logische code is.
 Een shift-jis code begint altijd met een getal hoger dan
 HEX 80 gevolgd door een ander getal hoger dan HEX 20. Hoe
 dit bij Jis zit weet ik niet echt.
 In de praktijk werkt men meestal met Shift-Jis en de lul die
 Jis heeft bedacht mogen ze van mij dan ook morgen ophangen....

WERKING ONDER BASIC

 Onder Basic kan men ook de gewenste kanji of kana op 't
 scherm krijgen, maar natuurlijk niet door met een toets-
 combinatie te werk te gaan. Na een _KANJI2 kan men op CTRL-
 SPACE drukken wat onder in 't scherm een command line
 oplevert. Op de plaats die normaal door de functie toetsen
 wordt ingenomen staat nu een 2 de cursor waar men kan gaan
 typen. De bedoeling van het geheel is dat in "Romaji" -dit
 is naam voor japans geschreven op een fonetische manier-
 de gewenste kanji in typen, alle let ter gre pen worden
 indien mogelijk vertaald naar Hiragana -de naam van het
 Japanse fonetische lettergreepschrift- en als het woord nu
 herkend wordt zal de bijbehorende Kanji op het scherm worden
 gezet. Omdat het Japans echter veel homofonen kent kan men
 met de spatie echter kiezen uit alle kanji met die
 uitspraak, niet ideaal maar het is een oplossing. Probeer
 maar eens "MORI" in te typen, het Japans voor boom/bos.
 Na het typen van "MO" zal "MO" vervangen worden voor het
 Hiragana teken voor de klank MO en na het typen van de
 "RI" zal ook "RI" vervangen worden door het hiragana teken
 voor de klank "RI", heeft overigens iets weg van een
 handgeschreven "Y".
 Drukt men nu op de spatie dan zullen de Hiragana tekens
 "MO RI" worden vervangen door een Kanji, welke dit zal zijn
 weet ik ook niet uit m'n hoofd maar uiteindelijk moet er
 een teken op het scherm komen dat wel wat weg heeft van
 3 "*" sterretjes, twee onder, een boven. Als na
 het drukken op de spatie -net zolang tot het teken in beeld
 is dat men zoekt- het juiste teken is gevonden kan men met
 return het teken op de plaats van de eerste cursor krijgen.
 Verder is het ook mogelijk om in het Europese-font uit de
 Jis2 rom te typen. Na een druk op F-1 zulllen alle tekens
 in JIS letters op het scherm komen waardoor mooie lettertjes
 te verkrijgen zijn. Na een druk op return komen deze
 prachtige 16*16 Europese letters op de plek van de eerste
 cursor. Nog een druk op F-1 en deze stand wordt weer uit
 gezet.

KATAKANA

 Verder is er ook het wat bekendere Katakana. Dit schrift
 wordt door de Japanners gebruikt om Engelse woorden in
 Japanse tekens te schrijven. De werkwijze is hetzelfde als
 bij de Kanji alleen drukt men na het typen van het hele
 woord op F-2 zodat alle hiragana tekens vertaald worden
 naar katakana tekens. Lukraak weg typen kan echter niet.
 Het Japans kent maar een paar klanken zodat de Engelse
 woorden meestal zwaar verminkt naar katakana vertaald
 moeten worden. Zo zal "FutureDisk" vertaald moeten worden met
 "Pyu tya Di su ku". Zie aan 't eind van deze text de tabel
 met alle katakana klanken. Om uit de commandline te komen
 volstaat een tweede druk op CTRL-SPACE, overigens soms "pakt"
 de computer die toetsdruk niet gelijk. Gewoon nog een keer
 drukken... Om uit een Kanji scherm te komen is _ANK het
 commando.

DE KATAKANA KLANKEN:

 A I U E O  BA BI BU BE BO < De "H"-klank met accent.
 KA KI KU KE KO  PA PI PU PE PO < idem.
 SA SI SU SE SO  DA DI DU DE DO < De "T"-klank met accent.
 TA TI TU TE TO  ZA ZI ZU ZE ZO < De "S"-klank met accent.
 NA NI NU NE NO  PYA   PYU   PYO< "PI" + "Y" klank.
 HA HI HU HE HO  TYA   TYU   TYO< "TI" + "Y" klank.
 MA MI MU ME MO
 YA  YU  YO
 RA RI RU RE RO
 WA     NN

 Dit klopt niet helemaal, de meeste uitspraken verschillen wel
 iets van wat hier staat maar over het algemeen is het
 makkelijker om dit aan te houden...

 Dat was het dan wel voor deze keer, ik neem aan dat de
 brievenschrijver van een aantal disks geleden tevreden is.
 De 2-plus heb ik nu een paar keer achter elkaar behandeld
 en ik denk dat ik maar stop met de 2+ en weer even op
 MSX 2 overstap omdat we geloof ik niet zo gek veel 2+ leden
 hebben. Nou dat was 't wel, dus... See ya!

Keizer...
