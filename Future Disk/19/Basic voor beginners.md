         BASIC CURSUS

 algemene hints voor beginners


                             ����
                             ����
                             ����
                             ����


 Ja, even voor de al iets verder gevorderden onder u. Deze keer
 staat er eigenlijk niets in voor jullie dus, err, why don't
 you go tie your dick in a knot?!(NvdR: ik zal je maar niet
 de 993 redenen noemen)
 
 Even een aaqntal tips voor de beginnertjes. Gezien 't feit dat
 de basic interpreter uit ons lieve MSX-je nog langzamer is dan
 mijn oma van 94 moet je erg opletten met wat je doet in je
 programmaatje. Let hierbij vooral op loops omdat deze vaker
 worden uitgevoerd. Eerst dus even een paar DO's en DONT's.
 
 Vermijd REM of ' in een loop omdat deze vreemd genoeg tijd in
 nemen. Je zou haast verwachten dat basic gewoon door gaat na
 het tegenkomen van een REM of ' maar hij loopt vreemd genoeg
 eerst de hele regel door. Nadat je dus je REM's keurig voor of
 na de loop heb gezet ga ik je opeens vertellen dat je het ' 
 tekentje eigenlijk maar niet moet gebruiken omdat deze vreemd
 genoeg 3 bytes inneemt terwijl het drie letters tellende woord
 REM maar 1 byte inneemt. Nu gebruik ik zelf gewoon altijd '  
 maar mocht je dus ooit in geheugen probleemples komen kun je
 altijd nog je '-tjes vervangen door REM's. Maar het best vind
 ik nog altijd helemaal geen REM's of '-tjes.

 Spaties zijn ook een probleem, deze kosten namelijk tijd.
 Ik persoonlijk typ gewoon geen spaties in m'n basic werk maar
 wat ook prima werkt is eerst typen en later als alles af
 is(�n werkt) de spaties er uit halen. Pas wel op: soms zijn
 spaties verplicht om alles nog goed te laten werken.

 Wisselen tussen twee waarden.
 Ook dit kost nog wel eens tijd. Vaak krijg je allerlei IF THEN
 constucties in je programmaatje die niet al te snel zijn.
 Kijk altijd eerst of 't een en ander niet met een XOR te
 verhelpen is. XOR's zijn namelijk heel snel. Als je dus bv
 wisselen moet tussen 2 en 0 wat bv bij het veranderen van
 de frequentie nog wel eens 't geval is, krijg je meestal
 iets als:

 10 ' 0 = 60Hz      2 = 50Hz
 20 IF HZ = 0 THEN VDP(10)=2 : HZ=2 : GOTO 40
 30 IF HZ = 2 THEN VDP(10)=0 : HZ=0 : GOTO 40
 40 BLA BLA BLA....

 Veel sneller is

 10 ' 0 = 60Hz      2 = 50Hz
 20 HZ=HZ XOR 2 : VDP(10)=HZ

 Of nog sneller (en technisch gezien de enige goede manier)

 10 VDP(10)=VDP(10)XOR2

 Even voor die mensen die niet weten wat een XOR doet:
 een XOR doet niets anders dan het (de) aan gegeven bit(s) van
 een byte omgooien. Als (zoals bij 't voorbeeld) de byte dus
 &B00000010 is en de XOR &B00000010 is wordt bit twee dus ook
 steeds omgegooid. Mensen die niet weten hoe 't binaire stelsel
 in elkaar steekt raad ik aan eens hun BASIC manual door te
 lezen waar dit soort dingen vaak wel instaan. Hierbij raad ik
 aan het gedeelte over het octale stelsel over te slaan aan-
 gezien dit eigenlijk nooit wordt gebruikt in BASIC(NvdR: in
 andere talen ook vrijwel niet heb ik me laten vertellen door
 Jan-Willem!).

 Voor jullie beginners raad ik dus aan eens goed met die XOR's
 en andere operaties in BASIC te gaan spelen want het scheelt
 echt wel als je die goed onder de knie heb.
 Lieden die echter denken dat het nodig is om uit het hoofd de
 getallen van 0 tot 255 van DEC naar HEX en van HEX naar BIN 
 te kunnen omrekenen hebben wel een erg hoog doel en kunnen
 maar beter door gaan met de MC cursus elders op dit mag.
 Toegegeven, 't is wel handig om sommmige waarden in HEX,
 DEC en BIN uit 't hoofd te kennen.

 Well boys, that's about it for now.
 Read the rest of my shit in the next txt-file.

Tobias 'kill that space' Keizer
