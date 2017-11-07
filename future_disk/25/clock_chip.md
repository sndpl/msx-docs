      CURSUS CLOCK-KIJKEN, de clock-chip nader bekeken.
      =================================================

                             ����
                             ����
                             ����
                             ����


 Dat de  clock-chip in de MSX  gebruikt wordt om de tijd in bij
 te houden,  zullen de meeste mensen wel kunnen gokken, maar er
 schuilt heel wat meer achter die "32 bytes S-RAM".

DE TIJD

 De tijd,  want dat is  toch waarvoor  men de  klokchip meestal
 zal  gebruiken.  In BASIC  is  dat  niet  zo'n  probleem,  een
 simpele GET TIME helpt de BASIC-er wel aan de juiste tijd, ter-
 wijl onder een DOS  omgeving een  BDOS call naar $002C ook het
 probleem  wel  uit de  wereld  helpt.  In  een  BASIC omgeving
 wellicht  makkelijk,  maar in machinetaal wordt het  echter al
 een stuk moeilijker, tenzij men natuurlijk naar  dezelfde BDOS
 call grijpt,  maar  hoe zit het eigenlijk met alle andere infor-
 matie in de clock-chip??

DE OPBOUW

 De  opbouw van  de clock-chip  doet ietwat  vreemd aan,  in de
 eerste plaats staat de  data al niet in echt S-RAM,  en is het
 geheel niet opgedeeld in bytes, hetgeen de aanhalingstekentjes
 in de inleiding verklaart. Eigenlijk is de clock-chip verdeeld
 in 4 blokken van 16 vierbits registers. Nybble werk dus, YUK!!

 Gelukkig voorziet het BIOS in een routine om de clock-chip uit
 te lezen, hetgeen de minder ervaren programmeurs weer een paar
 uur scheelt. Dus, 64 nybbles om alle data in op te slaan, maar
 waar staat  dan wat?  Dat was het  eerste probleem  waar ik op
 stuitte bij mijn speurtocht door de clock-chip. Over de eerste
 twee blokken,  die voor de tijd &  datum gebruikt worden,  was
 nog wel  informatie  te vinden.  Over de  laatste twee blokken
 echter, was niets te vinden. Nu is het ongetwijfeld zo dat wel
 in een of ander technical-databook deze informatie staat, maar
 via 'normale' wegen, bleek het niet makkelijk om deze informa-
 tie te achterhalen.  Mij is dit dan ook  alleen gelukt via het
 befaamde "trial & error" proces, waarbij de nadruk toch wel op
 error lag...

NYBBLE WERK...

 Vierenzestig nybbles dus, die via de de BIOS routine $01F5 uit
 de SUB-ROM te lezen zijn. De nybbles waarin de data staat bom-
 barderen we maar even tot registers, en dan zijn er dus nog de
 vier blokken die te selecteren zijn. De eerste twee blokken wa-
 ren dus voor de tijd en datum, de overige twee voor de instel-
 lingen van BASIC. Als invoer heeft de BIOS routine in register
 C "twee variabelen" nodig.  In bit 4 & 5 van  register C staat
 welk blok  we wensen  te lezen, 0-3 dus.  In bit 0-3 staat dus
 het registernummer 0-15. Als output komt de routine met in reg
 A het gelezen  nybble.  De laaste 4  bits dienen  genegeerd te
 worden. Maak deze dus maar voor het  gemak 0. En dan nu dus de
 tabel,  die ik zoals gezegd heb moeten  uitvogelen door domweg
 wat te proberen.  Van 3 reg's ben ik niet te weten gekomen wat
 hun functie is, en de laatste 3 registers zijn voor intern ge-
 bruik  waar men dus niets aan heeft.  Het enige dat ik er van
 weet is dat het respectievelijk de MODE, TEST en RESET regis-
 ters zouden moeten zijn, hun functie is mij onduidelijk.


 REG     �V block 0    �V BLOCK 1    �V BLOCK 2    �V BLOCK 3    �V
 �W�W�W�W�W�W�W�W�U�W�W�W�W�W�W�W�W�W�W�W�W�U�W�W�W�W�W�W�W�W�W�W�W�W�U�W�W�W�W�W�W�W�W�W�W�W�W�U�W�W�W�W�W�W�W�W�W�W�W�W�S
 $00     �V SEC 2      �V ONGEBRUIKT �V -ONBEKEND- �V STRING I.D �V
 $01     �V SEC 1      �V ONGEBRUIKT �V ADJUST   X �V CHR0    LN �V
 $02     �V MIN 2      �V MIN 2 (AL) �V ADJUST   Y �V CHR0    HN �V
 $03     �V MIN 1      �V MIN 1 (AL) �V -ONBEKEND- �V CHR1    LN �V
 $04     �V UUR 2      �V UUR 2 (AL) �V WIDTH   LN �V CHR1    HN �V
 $05     �V UUR 1      �V UUR 1 (AL) �V WIDTH   HN �V CHR2    LN �V
 $06     �V DAG V WEEK �V DAG V WEEK �V VOOR KLEUR �V CHR2    HN �V
 $07     �V DAG 2      �V DAG 2 (AL) �V ACHT KLEUR �V CHR3    LN �V
 $08     �V DAG 1      �V DAG 1 (AL) �V RAND KLEUR �V CHR3    HN �V
 $09     �V MAAND 2    �V ONGEBRUIKT �V SCREEN SET �V CHR4    LN �V
 $0A     �V MAAND 1    �V 12/24 UURS �V "BEEP"INFO �V CHR4    HN �V
 $0B     �V JAAR 2     �V SCHRIKKELJ �V LOGO KLEUR �V CHR5    LN �V
 $0C     �V JAAR 1     �V ONGEBRUIKT �V -ONBEKEND- �V CHR5    HN �V
 $0D     �V MODE------ �V MODE------ �V MODE------ �V MODE------ �V
 $0E     �V TEST------ �V TEST------ �V TEST------ �V TEST------ �V
 $0F     �V RESET----- �V RESET----- �V RESET----- �V RESET----- �V
 �W�W�W�W�W�W�W�W�U�W�W�W�W�W�W�W�W�W�W�W�W�U�W�W�W�W�W�W�W�W�W�W�W�W�U�W�W�W�W�W�W�W�W�W�W�W�W�U�W�W�W�W�W�W�W�W�W�W�W�W�S


 Ervaren ML-programmeurs  kunnen hiermee uit  de voeten,  voor
 het geval dat er echter nog  het een en ander  onduidelijk is
 zal ik alles even apart behandelen.


NOG EENS DE TIJD

 Zoals in de tabel  te zien is komen  sommige variabelen  voor
 met een 1 en een 2 erachter. Dit zijn dus Binairy Coded Deci-
 mals. Voor diegenen die niet weten wat dit inhoud; Gewoon het
 eerste getal, dat het tweede cijfer voorstelt,  met 10 verme-
 nigvuldigen en de andere nybble  erbij optellen.  Overal waar
 LN en HN staat bedoel ik HighNybble en LowNybble, wat dat in-
 houd behoeft naar  ik aanneem  geen uitleg.  Van de registers
 waar -ONBEKEND- staat heb  ik simpelweg de  functie niet  van
 kunnen achterhalen.Mocht iemand wel weten waar deze registers
 (paren) voor dienen, of mocht  iemand andere onvolkomendheden
 in de tabel  ontdekken, dan wordt  het zeer op  prijs gesteld
 als die persoon ons hiervan op de hoogte stelt. -Xela?-
 (NvdR: en jij optimist meent dat daar iemand op reageert?)

ANDERE OPSLAG METHODEN

 Een uitzondering is bijvoorbeeld al het jaartal, dit wordt ge
 vormd door (JAAR2*10)+8  en de andere nybble.  De offset voor
 het jaar is dus eigenlijk 1980.

 Dag v week is simpelweg de dag van de week.

 12/24Uurs geeft aan of de 12uurs klok  of de 24uurs klok door
 de gebruiker is ingesteld.

 Schrikkelj is een tellertje voor intern gebruik waar wordt
 bijgehouden of we wel of niet met een schrikkeljaar te maken
 hebben. Deze is overigens 2 bits lang.

 De ADJUST X & Y zijn de waarden  die we in BASIC met  het SET
 ADJUST commando kunnen instellen. Hierbij gaat het om 2-compl
 nybbles.

 De registers WIDTH tm RAND KLEUR spreken voor zich.

 Dan komt er een register dat volledig uit losse bits is opge-
 bouwd.  Bit0: Functie toesten                 (0:uit  1:aan )
         Bit1: Key Klick                       (0:uit  1:aan )
         Bit2: Type printer                    (0:MSX  1:IBM )
         Bit3: Standaard Baudrate              (0:1200 1:2400)
 Bij gebrek aan een betere naam noem ik dit maar "SCREEN SET".

 Dan het register met de BEEP informatie:   Bit 0-1: Type BEEP
                                            Bit 2-3: Volume /4

 Vervolgens het register dat de  kleur van het MSX-logo bij de
 reset routine representeerd.

 Het register dat ik STRING-ID heb genoemd heeft betrekking op
 de daarop volgende  registers.  STRING-ID bepaald  de functie
 van de daarop volgende STRING. De STRING kan namelijk PROMPT,
 TITLE & PASSWORD zijn. Bij resp. 1,0 & 2 voor STRING-ID.

 De rest van de registers zijn dus gereserveerd voor de STRING
 die door STRING-ID een functie had gekregen.

 En dan hebben we zo'n beetje alle registers gehad. Waarmee de
 clock-chip dus ook zo'n beetje afgehandeld zou mo
