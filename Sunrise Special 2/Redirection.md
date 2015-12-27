Onbekende MSX-DOS 2 functies:

         R E D I R E C T I O N   E N   P I P I N G 
                                                    

MSX-DOS 2  kent interessante  mogelijkheden voor programma's 
die  de BDOS-calls  gebruiken voor  input en output. BDOS is 
een  afkorting   van  Basic   Disk  Operating  System.  Alle 
CP/M-programma's  (en  dat  zijn er  nogal wat!),  de meeste 
commando's  en MSX-DOS  programma's werken  met de BDOS. Dat 
betekent dat ze niet al te snel zijn (op 3.58 MHz dan), maar 
wel met redirection en pipelining kunnen werken.

Allereerst  moet  je  dan  wel iets  weten van  het volgende 
onderwerp:


                       D E V I C E S 

Devices, of  voor de  puristen onder ons apparaatnaman, zijn 
invoer- of uitvoermethodes. Ik zal ze kort omschrijven:
CON  Uitvoer   naar  het   beeldscherm  en  invoer  van  het 
     toetsenbord.
NUL  Geen invoer  en geen uitvoer. Handig om het scherm leeg 
     te houden.
PRN  Invoer en uitvoer naar de printer.
LST  Idem.
AUX  Dit is met ADDAUX.COM, een programma van DOS2-tools van 
     ASCII  te gebruiken  als invoer van de RS-232C poort en 
     uitvoer  naar  de  RS-232C  poort. Alleen  de standaard 
     interface  werkt  hiermee.  Dus  geen  aangepast  modem 
     zonder de RS-232C ROMs.


                   R E D I R E C T I O N 

MSX-DOS  kan  ervoor  zorgen  dat  de  standaard  input-  en 
outputroutines  van  de  BDOS  niet  met  het scherm  en het 
toetsenbord werken,  maar met (tekst)files of andere devices 
dan  CON. Door  een van de symbolen <, > of >> toe te voegen 
achter een  filenaam zal  de invoer  en uitvoer niet via CON 
verlopen, maar op een andere wijze:
<   Dit  zorgt  ervoor  dat de  input van  een commando/file 
    veranderd  wordt. De  erop volgende  filenaam/device zal 
    dan de input van het toetsenbord vervangen.
>   De uitvoer  van een commando zal gestuurd worden naar de 
    filenaam  die erop volgt. De file wordt opnieuw geopend, 
    als die  al bestaat,  en er  zal dus  helemaal aan begin 
    begonnen worden met de uitgevoerde tekst.
>>  De   uitvoer  zal   naar  de  filenaam  gestuurd  worden 
    gestuurd. Het bestand zal dan gewoon blijven bestaan, de 
    uitgevoerde tekst zal ACHTER het bestand komen.

De  redirection-symbolen zullen worden opgevangen door DOS2. 
Ze zullen  niet worden  gebruikt door  het commando.  Het is 
evenwel  ook uit te zetten d.m.v. het enironment item REDIR. 
Standaard staat  dit op  ON. Maar  met SET  REDIR=OFF kun je 
toch  de redirection  (en pipelining)  tekens gebruiken voor 
een commando.  Dan kun je logischerwijze geen redirection of 
pipelining meer gebruiken.


Voorbeelden:

COPY A:*.* B:>NUL
Eigenlijk is dit natuurlijk alleen handig bij 2 drives. Want 
de  melding om  van disk  te wisselen  zal ook  niet op  het 
scherm worden afgedrukt.

Ook kun je heel makkelijk een directory afdrukken:
DIR/W>PRN


                    B A T C H F I L E S 

Als  de in- of uitvoer van een batchfile geredirectioned is, 
zal die  redirection betrekking hebben op alle commando's in 
die  batchfile. Alleen  de commando's in de batchfile die al 
een   redirection    hebben   zullen    niet   volgens    de 
batch-direction werken. Even een voorbeeldje:
Het batchbestand TEST.BAT:

ECHO Deze tekst wordt naar de printer gestuurd.>PRN
ECHO Het is nog onbekend waar deze tekst komt.

Bij het  intikken van TEST>NUL zal "Deze tekst wordt naar de 
printer  gestuurd." op papier komen te staan, als de printer 
er is,  online staat  en er  papier in  zit. En  "Het is nog 
onbekend waar deze tekst komt." zal gewoon "verdwijnen".


                    P I P E L I N I N G 

Het is  niet alleen  mogelijk om  de in-  en output  van een 
commando  naar een  device of file te sturen, maar ook om de 
uitvoer  van  het ene  commando naar  een ander  commando te 
"pipen".

Met  deze optie zijn diverse leuke dingen te bedenken. Zoals 
een programma  wat het  aantal letters,  woorden, regels  en 
pagina's telt.

Piping wordt  aangeduid met  het symbool |. Het commando dat 
links van | staat zal eerst worden uitgevoerd, en de uitvoer 
daarvan  zal  worden  doorgevoerd  naar  de  invoer  van het 
commando achter |. De outvoer van het eerste commando zal in 
een  file worden  gezet, die  dan door  de invoer van het 2e 
commando zal worden gelezen.

De naam  van die  file is %TEMPxxx.$$$ waarbij xxx een getal 
is  dat ervoor  zorgt dat  er niet 2 tempfiles dezelfde naam 
krijgen (zolang  er natuurlijk niet meer dan 1000 temp-files 
zijn  natuurlijk). De  plaats van  de files  wordt aangeduid 
door  het  environment item  TEMP. Het  is aan  te raden  om 
hiervoor  de  RAMdisk  te  gebruiken,  omdat  de file  wordt 
aangemaakt, gelezen  en weer  gewist. En  dat alles  merk je 
bijna  niet  met  een  RAMdisk.  Met  een  gewone  diskdrive 
natuurlijk  wel. Bij  harddisk-gebruik kan er natuurlijk ook 
een TEMP-directory voor worden aangemaakt.


                    B A T C H F I L E S 

Het  is   niet  mogelijk   om  pipelining  rechtstreeks  met 
batchfiles  te gebruiken.  Maar het  is wel  mogelijk om een 
batchfile te starten met COMMAND2.COM, de batchfile komt dan 
achter COMMAND2  en zal dan gewoon werken als een .COM-file. 
Noot:  Elke keer dat COMMAND2.COM opnieuw wordt gestart kost 
dat 1,5  kB van  het TPA-geheugen,  het geheugen dat MSX-DOS 
programma's gebruiken en zo'n 55 kB groot is.

Voorbeelden:

ECHO Y|DEL A:*.*>NUL
Nu zal  Y worden  ingevoerd bij de vraag of alle file gewist 
moeten  worden van  drive A:.  Die vraag zal daarbij ook nog 
eens niet op het scherm verschijnen door de redirection naar 
het NUL-device.
Dit  is wel nog te UNDELeten, en het zal geen subdirectory's 
beschadigen. Maar veel gemener is:

ECHO Y|FORMAT d:>NUL
En  vul  voor  d:  maar een  drive in.  Harddisks kunnen  zo 
(gelukkig (helaas?))  niet geformatteerd worden, daarvoor is 
immers FDISK noodzakelijk.

Zie voor  meer voorbeelden  de tekst over MSX-DOS 2.31. (Ook 
interessant voor mensen zonder een FS-A1GT!)

                                              Kasper Souren