# M S X - D O S   2 . 3 1 
                                           

Bij de  Panasonic FS-A1GT wordt MSX-DOS 2.31 geleverd in het 
ROM.  Veel mensen  denken dat  DOS 2.31 alleen zorgt voor de 
ROM- en  SRAMdisk, en dus voor MSX-View. Dat bleek toch niet 
waar te zijn:


## S R A M D I S K   Z O N D E R   M S X - V I E W 

Veel  mensen  vroegen  zich  (en  anderen...) af  hoe ze  de 
SRAMdisk  van de  GT konden gebruiken zonder MSX-View van de 
ROMdisk op te hoeven starten.
Door mijn nieuwsgierige aard heb ik even TYPE C:AUTOEXEC.BAT 
ingetikt bij een GT-user. Wat ik daar zag was natuurlijk een 
opluchting voor de GT'er:
IF EXIST %SRAMD%\AUTOEXEC.BAT %SRAMD%\AUTOEXEC.BAT

In menselijke taal wordt dat:
Als  de  file AUTOEXEC.BAT  op de  SRAMdisk staat,  moet die 
batchfile uitgevoerd worden.

De mensen die bovenstaande vraag hadden, kunnen zich nu voor 
het  hoofd  slaan,  of  natuurlijk gewoon  een file  genaamd 
AUTOEXEC.BAT op de SRAMdisk plaaten.
Op deze disk staat een file die ook in BBS'en heeft gestaan. 
De  file  schakelt  de  ROM-mode  en  copieert  de  Europese 
karakterset naar  het RAM. En dan wordt weer teruggeschakeld 
naar DRAM-mode.

De werking is als volgt: Bij een reset worden de systeemROMs 
naar  RAM  gecopieerd,  door naar  de ROM-mode  te schakelen 
worden  die RAM-pagina's weer vrijgegeven voor "hardnekkige" 
software.  De   BIOS  wordt  vervolgens  aangepast  door  de 
Europese karakterset te copi�ren over de Japanse karakterset 
heen.  Dan wordt  de DRAM-mode teruggeschakelt, en worden de 
ROMs niet  meer over  de RAM  heengecopieerd. Dit  programma 
leent zich uitstekend om op de SRAMdisk gezet te worden. Dat 
staat ook in DOS231.PMA.

Met  "hardnekkige"  software  bedoel  ik  software die  niet 
volgens   de  standaard   MSX-DOS  2  mapperroutines  werkt. 
Eigenlijk   moeten   die  memory   mapper  plaatsen   worden 
vrijgegeven met FreSEG. Op deze disk staat een programmaatje 
uit MSX-Magazine  1 van  1990 genaamd ROMTURBO.COM. Door dit 
te   runnen   wordt   de   ROM-mode   ingeschakeld   en   de 
mappersegmenten  die eerst door de DRAM werden gebruikt weer 
vrijgegeven. Voor meer informatie over DRAM-mode raad ik aan 
om Sunrise  Special #1  nog eens te lezen, en dan vooral het 
artikel over de R800.
N.B.  Het broertje van ROMTURBO, RAMTURBO, staat ook op deze 
disk.


## E N V I R O N M E N T   I T E M S 

DOS   2.31   kent   de   mijns  inziens   zeer  interessante 
mogelijkheid om Environment Items (vanaf nu gewoon items) te 
gebruiken in batchfiles.

Je   kunt  dan,   zij  het  zeer  beperkt,  programmeren  in 
batchfiles. Een zeer simpel virus zou mogelijk zijn.
Het benoemen  van items  zal iedereen wel duidelijk zijn nu. 
Dat gaat gewoon met SET. Voorbeelden volgen zometeen.

Maar   je  kunt   ze  nu  ook  gebruiken  voor  alle  andere 
doeleinden:  Gewoon   laten  voorgaan  en  volgen  door  een 
%-teken.  Je krijgt  dan bijv. %SRAMD% voor drivenaam die de 
SRAMdisk heeft.  Dat kun je dus ook veranderen met SET. (SET 
SRAMD=drivenaam:). (Nee, geen dwerg-smiley, maar een dubbele 
punt.)


## I F 

Met  IF ==  kun je  tekst vergelijken.  Tekst vergelijken is 
niet echt zo interessant. Maar de kracht van deze functie is 
dat items  met %-tekens  eromheen als tekst gezien worden in 
batchfiles.
Je kunt ook kijken of een file wel bestaat:


## E X I S T 

Met  IF EXIST  kun je  erachter komen  of een  bepaalde file 
bestaat.  Bovenaan,  in  de  regel uit  AUTOEXEC.BAT van  de 
ROMdisk, staat  het ook  al. De  werking is zeer simpel. Als 
een  file bestaat  wordt gewoon  het commando wat erop volgt 
uitgevoerd. De file volgt gewoon achter IF EXIST.


## N O T 

Met NOT  geef je  een negatie  aan. Je kunt dus IF NOT EXIST 
filenaam.ext enz. gebruiken.


## E E N   V O O R B E E L D 

Ik  gebruik zelf een paar leuke batchfiles om mijn leven als 
HD-gebruiker  te   vergemakkelijken.  Als   je  maar  weinig 
opslagruimte  hebt, heeft  het eigenlijk weinig nut. Meestal 
doe je  dan toch  weinig in  DOS. Maar  voor een HD-user die 
GUI's  niet handig  vindt, wordt het allemaal wat aangenamer 
met  batchfiles.  Bij  een  Turbo  R  zul je  het nauwelijks 
merken, maar  bij een  3.58 MHz  Z80 zal  de snelheid  flink 
afnemen   door  het  gebruik  van  deze  batchfile.  Voor  7 
MHz-gebruikers zal het nog wel te doen zijn.


## T E D . B A T 

        CLS

Schoon scherm. Ligt aan je persoonlijke wensen.

        IF NOT %1NIKS==NIKS SET TEDFILE=%1

Als %1  plus NIKS NIET gelijk is aan niks, wordt TEDFILE aan 
%1 gelijk gemaakt. Zorgt ervoor dat de naam hetzelfde blijft 
als  je geen parameter opgeeft. Zeer handig bij het debuggen 
van een machinetaalprogramma!

        IF NOT EXIST %TEDFILE% SET TEDFILE=

Als %TEDFILE% NIET bestaat wordt %TEDFILE% gewist.

        IF EXIST %TEDFILE%.MAC SET TEDEXT=.MAC>NUL
        IF EXIST %TEDFILE%.GEN SET TEDEXT=.GEN>NUL
        IF EXIST %TEDFILE%.BAT SET TEDEXT=.BAT>NUL
        IF EXIST %TEDFILE%.DOC SET TEDEXT=.DOC>NUL
        IF EXIST %TEDFILE%.TXT SET TEDEXT=.TXT>NUL

Er  wordt  gekeken  of  de  file  bestaat met  een van  de 5 
extensies. Dit  is natuurlijk  naar wens  in te stellen. Het 
gaat  trouwens van minst belangrijke naar belangrijkste. Als 
er  bijv.  een  .GEN-  en  een  .DOC-file  bestaan  wordt de 
.DOC-file ingeladen. Die staat immers iets lager.
>NUL wordt  opgegeven om  de evt.  foutmeldingen niet op het 
scherm  te laten  komen. Zie  voor meer uitleg de tekst over 
redirection en pipelining op deze disk.

        SET TEDEXE=A:\TED\TED.COM
        IF %TEDEXT%==.MAC SET %TEDEXE%=A:\TED\MLTED.COM
        IF %TEDEXT%==.GEN SET %TEDEXE%=A:\TED\MLTED.COM
        %TEDEXE% %TEDFILE%%TEDEXT%

Het opstarten  van TED.  Voor %TED%  is de directory plus de 
naam  die TED  heeft. Door deze constructie te gebruiken kun 
je  verschillende  versies  van  TED gebruiken,  die je  met 
hetzelfde commando  opstart. MLTED is bij mij een versie met 
de instellingen (zoals TAB's) ingesteld voor machinetaal.
Als  TED.BAT  en  TED.COM  in  dezelfde directory  staan zal 
TED.COM  worden  ingeladen  als  het  commando  "TED"  wordt 
ingetikt.  Daarom MOET TED.COM in een andere directory staan 
als deze  batchfile gebruikt  worden. Of je moet iedere keer 
TED.BAT  filenaam  intikken.  En  het  was nu  juist om  het 
makkelijker  te  maken.  Je  kunt  natuurlijk  ook,  om  het 
helemaal  makkelijk te  maken de batchfile T.BAT noemen. Dan 
hoef je maar een letter in te tikken als je net uit TED zelf 
komt.


Op deze disk staan:

ROMTURBO.COM  Programma om  de 64  kB RAM  die de  DRAM-mode 
              inpikt  toch  te  kunnen  gebruiken.  Dit gaat 
              volgens de standaard: De segmenten die door de 
              DRAM-mode   worden  gebruikt   worden  met  de 
              standaard     MSX-DOS     2    mapper-routines 
              vrijgegeven.

RAMTURBO.COM  Programma   dat   de   DRAM-mode   volgens  de 
              standaard  installeert.  Als  er  niet  genoeg 
              geheugen meer is, wordt een melding gegeven.

KARSET.COM    Programma dat  de Europese  karakterset in  de 
              DRAM   zet.  Er  zit  geen  puntje,  maar  een 
              streepje door  de nul.  In de  oorspronkelijke 
              versie was het een puntje.

TED.BAT       De batchfile van het voorbeeld.


Kasper Souren