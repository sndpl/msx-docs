# R E C U R S I E F   P R O G R A M M E R E N


Dit is een onderwerp waar de gemiddelde MSX'er niet veel van
zal weten, maar dat toch buitengewoon interessant is. Het is
een programmeertechniek die al ontwikkeld is voordat het  in
werkelijkheid gebruikt kon worden  (lees:  computers  genoeg
geheugen hadden).


## D E   T E C H N I E K

Een recursieve routine is een routine die zichzelf aanroept.
Dit klinkt mischien een beetje  vreemd,  maar  het  is  best
handig. Je zou je een routine voor kunnen stellen die op een
gegeven moment gegevens nodig heeft die met dezelfde routine
gevonden kunnen worden.

Een voorbeeld:

Stel je hebt een touw dat x meter lang is  en  je  wilt  dit
touw in stukjes van 1 en 2 meter gaan verdelen. De vraag  is
nu op hoeveel manieren dit kan bij een gegeven x.

Bij een lengte x=20 meter is dit al heel moeilijk te vinden,
maar bij een lengte van  2  meter  is  het  aantal  manieren
gelijk aan x, nl. 2.

x = 2 meter  => Manieren: 11
                  2
                  Het kan dus op twee manieren.
x = 1 meter  => Manieren: 1
                  Het kan dus op 1 manier.

Dit zijn twee gevallen waarbij het aantal manieren gelijk is
aan het aantal meter touw en dat  is  een  heel  belangerijk
gegeven.

x = 4 meter  => Manieren: 1111
                  112
                  121
                  211
                  22
                  Het kan hier op 5 manieren.

Met een beetje inzicht in getallen kan je afleiden  dat  het
aantal mogelijkheden bij x=4 gelijk is aan:

Mog (x=4) = Mog.(x-2) + Mog.(x-1)
  =    2      +    3
  =    5

Het aantal mogelijkheden bij  x=3  is  ook  gelijk  aan  het
aantal meters lengte, maar dat is puur toevallig  en  verder
niet van belang.

Wat we nu hebben gezien is dat we het  aantal  mogelijkheden
kunnen onderverdelen in deelprobleempjes en die op hun beurt
ook  weer  in  deelprobleempjes,  net  zolang  tot  we   een
deelprobleem hebben waarvan we het antwoord al weten.

Nu is het mogelijk om dit in programmacode te schrijven.  Ik
zal dat in  de  taal  C  doen  omdat  mijn  PASCAL  naar  de
achtergrond is gezakt sinds ik met C werk.

int aantal_mog(int x)
{
if (x <= 2)   /* Dit is de luie regel (zie verder) */
return x;
else return aantal_mog(x-2) + aantal_mog(x-1);
}

Dit stukje programma zal zelfs voor mensen die nog nooit een
regel C gezien hebben wel te begrijpen zijn.  De  parameters
die achter de 'return' opdracht staan zijn de parameters die
aan de  vorige  routine  teruggegeven  worden.  In  assembly
gebeurd dit met CALL en RET.

De 'luie regel' is de regel die het  werk  in  deelproblemen
verdeelt en die het vereenvoudigde probleem  doorgeeft.  Dit
programma is echter zo kort dat de luie regel meteen ook  de
enige regel van de hele routine is.

Het 'else' gedeelte  geeft  het  deelprobleem  door  aan  de
volgende routineaanroep. De 'return x' geeft een waarde  van
x terug die we van tevoren al wisten (x <= 2). Ik kan het me
heel  goed  voorstellen  als  dit  alles  maar  moeilijk  te
bevatten is, maar het klopt toch allemaal echt.


## V O O R -   E N   N A D E L E N

Het allergrootste voordeel van recursief programmeren is dat
het extreem weinig programmacode oplevert. Ik daag  u  graag
uit om dit probleem zonder recursie op  te  lossen.  Het  is
vast wel mogelijk, maar het wordt  dan  wel  een  heel  lang
programma met heel veel sprongen en andere onoverzichtelijke
ellende.

Nog een voordeel is dat zodra je recursie onder de knie hebt
(lees: zelf een paar keer doen) het heel  eenvoudig  is.  De
probleemomschrijving lijkt nl. al heel veel op de feitelijke
programmacode en dus is het omzetten een makkie.

Het grootste nadeel  van  recursie  is  dat  -  alhoewel  de
routines zelf heel weinig geheugen nodig hebben - ze tijdens
executie enorme stukken werkruimte op kunnen slokken. In een
situatie waar je probeert om voor x=500 in te vullen zal  je
wel begrijpen  dat  er  extreem  veel  deelproblemen  zullen
ontstaan  die  allemaal  stackruimte  nodig  hebben  om  hun
terugkeeradres op te zetten.  Zo  heb  je  voor  10000  CALL
instrukties in assembly een stack nodig met een  lengte  van
20000 bytes en geloof me,  dat  haal  je  met  deze  methode
makkelijk.

Dit is ook meteen de reden waarom  ik  niet  een  uitgewerkt
assembly programma gebruik. Het is op een  MSX  gewoon  niet
praktisch omdat je meestal maar 64 kB  direkt  adresseerbaar
geheugen hebt waarvan meestal de helft  voor  het  operating
system is  en  dan  praten  we  nog  niet  eens  over  eigen
programma's. Dit artikel is daarom ook meer van informatieve
dan van praktische aard voor MSX omdat ook C op MSX een ware
ramp is (programmatechnisch gesproken dan).


## C O N C L U S I E

Het zal sommige mensen wel  niet  aanstaan,  maar  voor  dit
soort dingen heb je toch snel een PC of Atari  nodig  (werkt
heel lekker met C) omdat die veel meer direkt  adresseerbaar
geheugen hebben en omdat die  hardware  veel  geschikter  is
voor een taal als C. Een recursie toepassing in assembly  is
af te raden omdat dat behoorlijk moeilijk is terwijl je in C
feitelijk het probleem letterlijk over kunt tikken.

Affijn, recursie is een heel interessante programmeermethode
met vele mogelijkheden zoals  sorteren  of  het  beheren  en
onderhouden van boomstrukturen (waar ik  ook  nog  wel  eens
over kan schrijven  omdat  dat  dingen  zijn  die  prima  in
assembly kunnen.) Een andere leuke toepassing  van  recursie
is bv. het probleem van de torens van Hanoi, maar dat is  op
dit moment iets te ingewikkeld om hier te behandelen.

Recursie is een veel gebruikte methode maar  wordt  voor  de
meeste  mensen  pas  interessant  op  het  moment   dat   ze
professioneel met computers aan de gang gaan. Het is  daarom
voor deze  mensen  gewoon  leuk  om  alvast  eens  met  deze
techniek in aanraking te komen.

Alex van der Wal
