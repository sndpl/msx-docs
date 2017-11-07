 Tobias leert ons wat meer over Disk I/O in BASIC...
BASIC, de drive laten ratelen...

                             ����
                             ����
                             ����
                             ����


 Dat  de MSX  BASIC een  van de  beste -zo niet de beste- BASIC 
 varianten is mag bekend zijn. Geen BASIC soort is zo goed uit- 
 gerust als  MSX BASIC.  Werkelijk alles is mogelijk. Ik geloof 
 dat  de line-interrupt zo'n beetje het enige is dat BASIC niet 
 ondersteunt. Maar  ook daar  is indirect  een oplossing  voor.
 Want MSX BASIC laat zich ideaal met machinetaal combineren. En 
 een drivertje is zo geschreven.
 
 Van muziek tot zelfs scroll routines, allemaal op de interrupt 
 erbij. Dat het er allemaal niet sneller op wordt mag duidelijk 
 zijn,  maar het kan. Maar ook zonder die ML is er in MSX BASIC 
 een hele hoop mogelijk.
 
 
           Disk I/O


 Zo is  ook het DISK I/O gedeelte van MSX BASIC niet misselijk, 
 van binaire files tot random-access file. Het kan allemaal. Om 
 precies te zijn heeft de DISK BASIC maar twee nadelen. Het eer 
 ste grote nadeel is dat er in BASIC geen data files zijn in te 
 laden.  Elke file moet een HEADER hebben; BLOAD files dus. Dit 
 is een  probleem waar ik in een latere aflevering nog wel eens 
 op terug kom.  Een ander probleem is dat het in BASIC niet fat-
 soenlijk mogelijk is om een directory op het scherm te zetten. 
 Dat kan eigenlijk alleen maar met het FILES commando. Het eer- 
 ste  probleem is dan dus al dat de directory informatie op een 
 scherm moet  kunnen passen. En onder DOS2 hoeft dat zeer zeker 
 niet het geval te zijn. Het tweede probleem is dat we vast zit-
 ten aan de "layout" die BASIC ons voorschrijft. En manipulatie 
 van de directory is er al helemaal niet bij.
 
 Een veel gebruikte oplossing is om het scherm "uit" te zetten, 
 vervolgen  het FILES commando te gebruiken,  met een paar wel- 
 gemikte VPEEKs  de data  in variabelen te zetten om vervolgens 
 het oude scherm weer op te bouwen en de nieuwe data te printen 
 op het scherm. Aan een dergelijke kunstgreep kleven een aantal 
 nadelen.  Ten eerste is het natuurlijk een weinig elegante, en 
 zeker niet  makkelijke, oplossing.  En ten tweede is zulks erg 
 lastig in een grafisch scherm. Er moet namelijk eerst naar het 
 tekst-scherm  worden gegaan, waardoor bij  terug keer naar het
 grafische scherm heel page 0 gewist is. Allemaal niet erg han- 
 dig dus. Terwijl er een prima oplossing is...maar ja, onbekend
 maakt onbemind...
 
 
         DSKI$ & DSKO$

 Met deze twee BASIC functies is het mogelijk om sectoren resp. 
 in te  lezen en weg te schrijven. Bij de tweede functie volsta 
 ik  met de  syntaxis aangezien  het schrijven van sectoren bij 
 het lezen van directories weinig waarde heeft. De syntaxis:
 
 A$ = DSKI$ (d,s)     ; Voor het lezen van sector s van drive d
 DSKO$ d,s         ; Voor het schrijven van sector s op drive d
 
 Merk hier het verschil op tussen de syntaxis van beide instruc-
 ties. De vorm van het DSKI$ commando doet vermoeden dat de
 sector in de variabele A$ wordt gelezen of iets dergelijks.
 Niets is echter minder waar. De sector wordt namelijk ingelezen
 in een vaste buffer. En daar laten vrijwel alle BASIC boeken
 het afweten.  Geen van  de boeken vermeld namelijk waar die
 buffer dan wel in het geheugen te vinden is.
 
 De buffer staat overigens niet op een vaste plaats. De pointer 
 naar deze  buffer uiteraard wel.  En wel op adres $F351.  Door 
 dus  het adres  van $F351  / $F352 af te plukken wordt dus het 
 adres van  de sector buffer gevonden.  Minder bekend is echter 
 dat dit adres ook weg te poken is. En daar komt de truuk. Want 
 we kunnen in plaats van het adres weg te poken veel beter zelf 
 ons adres bepalen. Dan zijn er immers veel minder adres-varia-
 belen  in ons programma nodig wat de snelheid nog wel eens ge- 
 voelig ten goede kan komen.
 
 POKE &HF351,&H00 : POKE &HF352,&HC0
 
 Dit zal dus de meest logische oplossing zijn. Het adres $C000 is
 namelijk een prima test adresje voor allerlei zaken.  Voordeel 
 van  de POKE methode is ook dat er ook andere machinetaal pro- 
 gramma's gebruikt  kunnen worden zonder dat alles vast komt te 
 liggen.   Gebruiken we  bijvoorbeeld de  MoonBlaster 1.4 BASIC 
 driver dan  wordt er op adres $DA00 het een en ander aan gege- 
 vens  neergezet.   Doordat we precies weten waar onze data te-
 recht komt hoeft er dus nooit iets mis te gaan.  Want of Moon- 
 Blaster erg vrolijk wordt van een FAT in de STEPBF buffer weet 
 ik niet...
 
 
      Directory structuur

 Deze keer denk ik dat het wijsheid is om eerst alleen de DOS 1 
 directory structuur  te bespreken. De DOS 2 structuur is name- 
 lijk flink wat moeilijker. Het eerste voordeel is dat de direc-
 tory  altijd op een vaste plaats te vinden is. Namelijk sector 
 07-14 op  een dubbelzijdige disk, de enkelzijdige disk laat ik 
 maar even voor wat het is, maar met de hierna volgende formule
 is ook dat prima uit te rekenen.
 
    First DIR Sector = Number Of FATS * Sectors Per FAT + 1

 Maar, hoe komen we dan aan die informatie? Simpel, uit de BOOT 
 sector. De BOOT sector (sector 0) is een sector waaruit geBOOT 
 kan  worden, en  waar informatie over de disk staat. Zo ook de 
 informatie die we voor onze formule nodig hebben. Om een duide-
 lijker beeld te scheppen is het misschien verstandiger om maar 
 eerst eens een liniear beeld van een disk te scheppen.


Lees verder in deel 2...

 Tobias leert ons wat meer over Disk I/O in BASIC...
BASIC, de drive laten ratelen...
            Deel 2
                             ����
                             ����
                             ����
                             ����


 Om een duidelijker beeld te scheppen is het misschien verstan- 
 diger  om maar  eerst eens  een liniear  beeld van een disk te 
 scheppen.
 
                        BOOT Sector (00)
                          FAT Sectoren
                       Directory Sectoren
                       Data-Area Sectoren

 Om dus de eerste directory sector te berekenen moeten we weten 
 hoeveel FAT  sectoren er zijn, en daar moet dan 1 bijop geteld 
 worden  voor de  BOOT sector.  Nu blijkt dat een disk meer dan 
 ��n FAT  kan hebben.   Sterker nog,  voor zover ik weet hebben
 alle  disks voor  de zekerheid  minimaal 2  kopien van de FAT. 
 Maar dat  aantal hoeft  niet vast te zijn. Zo is het geen pro- 
 bleem om 5 of 6 FAT kopi�n te hebben, hoewel dat niet gebruike-
 lijk  is.  Ook is de lengte van een FAT niet vast. Op een dub-
 belzijdige disk zal dat altijd 3 zijn, maar op een enkelzijdig 
 diskje meestal 2, terwijl een harddisk er weer veel meer heeft 
 dan 3. Dit alles staat dus in de bootsector. Ons eerste comman-
 do zal dus
 
                        A$ = DSKI$ (0,0)

 zijn, waarbij drive 0 voor de default diskdrive staat.  Dan is 
 het belangrijk  om te  weten waar  in de bootsector wat staat. 
 Het overzicht hieronder is niet helemaal correct, maar wel af- 
 doende.   Zo zal het aantal bytes per sector op een MSX altijd 
 512 zijn. Cluster informatie komt nu ook nog niet aan bod, dat 
 komt allemaal in een later deel wel.
 
      +16, 1 byte;  Aantal FATs op disk
      +17, 2 bytes; Aantal entries per directory
      +21, 1 byte;  Media Descriptor ($F8 = SS, $F9 = DS)
      +22, 2 bytes; Aantal sectoren per FAT
 
 Als we  dus de eerste directory sector willen weten is het on- 
 der staande stukje BASIC voldoende.
 
 10 POKE&HF351,&H00: POKE&H352,&HC0: A$=DSKI$(0,0)
 20 NF=PEEK(&HC000+16): SF=PEEK(&HC000+22): FD=NF*SF
 
 Wederom eigenlijk niet correct. Zo kan een FAT in principe na- 
 melijk  groter zijn  dan 255  sectoren.  Erg groot is die kans 
 echter niet;  dat zou een behoorlijke harddisk moeten zijn. Nu 
 we dus de eerste directory sector weten kunnen we eindelijk be-
 ginnen met uitlezen. Als we de structuur van een directory ken-
 nen tenminste...
 
         +00, 11 bytes; Filenaam met extensie (geen .)
         +11, 01 byte;  Attribuut byte
         +22, 02 bytes; Tijd "file-creation"
         +24, 02 bytes; Datum "file-creation"
         +26, 02 bytes; Eerste cluster
         +28, 04 bytes; Lengte file (in bytes)
 
 Op  dit moment  is alleen  het eerste  nog maar belangrijk, de 
 filenaam. Nu  staat er  in een  directory sector echt niet ��n
 filenaam,  sterker nog er staan er zelfs 16. Wederom maar weer 
 een klein stukje BASIC, dat spreekt immers boekdelen...
 
 10 POKE&HF351,&H00: POKE&H352,&HC0: A$=DSKI$(0,0)
 20 NF=PEEK(&HC000+16): SF=PEEK(&HC000+22): FD=NF*SF
 30 ME=PEEK(&HC000+17): DS=FD: LS=FD+(ME/32): DIMF$(ME)
 40 ' Read Loop
 50 IFDS>LSTHENEND: ELSEA$=DSKI$(0,DS)
 60 FORLP=0TO15: AO=LP*32:F$(DE)=""
 70 FORL=0TO10: F$(DE)=F$(DE)+CHR$(PEEK(&HC000+AO+L): NEXTL
 80 DE=DE+1: NEXTLP: DS=DS+1: GOTO50
 
 Dit stukje BASIC werkt in principe prima, maar heeft twee vrij 
 grote nadelen.  Zo worden  gewiste bestanden en lege directory 
 entries  ook in de arrays geplaatst. En dat kost flink was ge- 
 heugen, en zal bijna nooit de bedoeling zijn.
 
 
                           $00 / $E5

 Twee getallen die we in de filenaam kunnen tegenkomen, die een 
 speciale betekenis hebben. Zo betekent de eerste dat het einde 
 van de  directory is bereikt, en de tweede dat het bestand ge- 
 wist  is. Ook  hier zullen we dus op moeten checken. Ons BASIC 
 programmaatje zal dus iets anders moeten worden.
  
 10 POKE&HF351,&H00: POKE&H352,&HC0: A$=DSKI$(0,0)
 20 NF=PEEK(&HC000+16): SF=PEEK(&HC000+22): FD=NF*SF
 30 ME=PEEK(&HC000+17): DS=FD: LS=FD+(ME/32): DIMF$(ME)
 40 ' Read Loop
 50 IFDS>LSTHENEND: ELSEA$=DSKI$(0,DS)
 60 FORLP=0TO15: AO=LP*32: F$(DE)=""
 70 ' Do Check
 80 IFPEEK(&HC000+AO)=&HE5THENGOTO110
 90 IFPEEK(&HC000+AO)=&H00THENEND
 100 FORL=0TO10: F$(DE)=F$(DE)+CHR$(PEEK(&HC000+AO+L): NEXTL
 110 DE=DE+1:NEXTLP:DS=DS+1:GOTO50

 Dit is  al iets beter. Uiteraard is dit dus een deelprogramma. 
 Een  GOSUB naar dit stukje BASIC zou dus beter zijn. Maar, het 
 laat wel  duidelijk zien hoe in BASIC een directory netjes, en 
 vlekkeloos kan worden uitgelezen. ON ERROR routines zijn name- 
 lijk geen probleem bij DSKI$ en DSKO$.
 
 De volgende  keer gaan we eerst nog maar eens wat verder in op 
 de materie, om de keer daarna DOS 2 directories uit te lezen.
 Met sub-directories nog wel!! Het is toch wat, die MSX BASIC!


Tobias Keizer
