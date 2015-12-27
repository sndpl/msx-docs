                      R E S O U R C E 
                                       

RESOURCE is  een CP/M  Z80 disk-disassembler  die natuurlijk 
ook  funktioneert onder  MSX-DOS. RESOURCE heb ik gedownload 
in SUN-BBS.  Het programma is al zo'n 10 jaar oud zijn, denk 
ik.

Als je  RESOURCE eenmaal doorhebt (en dat probeer ik met dit 
artikel  te bereiken), kun je er hele leuke dingen mee doen. 
Als je  weer eens  denkt van:  "Hmm, dat programma doet iets 
wat ik in mijn eigen programma wil gebruiken" hoef je alleen 
maar even RESOURCE te gebruiken.


                     I N S T E L L E N 

RESOURCE  heeft een  filenaam nodig  in de  prompt. Je  tikt 
bijv. in

        RESOURCE COMMAND2.COM

en je krijgt de volgende tekst op je beeldscherm:

        SUPER CPM Z80 DIS-ASSEMBLER VER 4.00
        ENTER PROGRAM LENGTH:

Bij de lengte kun je in het begin het beste gewoon op RETURN 
drukken. Dan  wordt de  lengte genomen  die in  de directory 
staat. Verdere uitleg hierover volgt.

Dan krijg je

        ENTER PROGRAM STARTING ADDRESS:

voor  je  ogen.  Hier  moet  je  0100  opgeven. Meestal  zal 
RESOURCE  voor  .COM-files  worden gebruikt,  en die  worden 
altijd op (#0100) ingeladen en gestart.

Dan volgt:

        ENTER PROGRAM OFFSET INTO FILE: 

Hier  kun  je  het  aantal  bytes, die  als header  gebruikt 
worden, instellen. Bij .BIN-files moet je hier 7 invullen.

De volgende vraag is:

        DO YOU WANT AN ASCII DUMP?

Als er tekst in de file staat, moet je hier de Y indrukken.

Bij de volgende vragen druk ik (ook) altijd de Y in:

        DO YOU WANT THE LISTING PASS?
        DO YOU WANT THE SOURCE OUTPUT PASS?
        DO YOU WANT THE LABLE CROSS REFRENCE PASS?
        MAKE ALL INTERNAL TABLE REFRENCES LABLES?
        DO YOU WANT TABS USED IN OUTPUT?
        LIST OUTPUT TO DISK FILE?

De werking  hiervan is me niet overal geheel duidelijk, maar 
mij leverde overal Y het juiste effekt op. (Y was natuurlijk 
het eerste dat ik probeerde.)

Dan volgen er de intrigerende vragen:

        ENTER "BYTE" AREA ADDRESS PAIRS (1 SET PER LINE):
        ENTER "WORD" AREA ADDRESS PAIRS (1 SET PER LINE):

Hier druk  ik maar  gewoon RETURN in. Ik denk dat je hier in 
kunt  stellen  bij  welke  gedeelten  "DB"  en  "DW"  worden 
gebruikt.  Als dat  zo is, is het handig als je weet waar de 
tekst precies  staat. Maar daarom heb je "ASCII DUMP" op aan 
gezet.

Helaas  moet je  al die  instellingen telkens  opnieuw doen. 
Maar daarom kun je dan een batchfile maken die gebruik maakt 
van KEYFAKE (ook op deze Special).


                       S T A R T E N 

Als  je alles  volgens bovenstaande tekst heb ingevuld krijg 
je 1  of 2  files op  disk: COMMAND2.ASM en COMMAND2.PRN. De 
.ASM-file  bevat  de  pure  source.  Deze  kun  je  zo  weer 
assembleren.  In COMMAND2.PRN  staan allerlei  extra dingen. 
Het enige  dat mij  tot nog  toe hiervan geboeid heeft is de 
ASCII-dump helemaal in het begin van de file.

Waarschuwing:  de  grootte van  de files  kan al  gauw enorm 
zijn!   De    disassembleer-tijd   is    zeer   groot.   TED 
disassembleren  kostte  mij  op mijn  turbo R  20 (twintig!) 
minuten. De .ASM-file was 300 kB en de .PRN-file 500 kB. Ook 
had ik alles op de RAMdisk gedaan (1024 kB intern).

Als je  geen turbo R hebt en je wilt een groot programma (10 
kB  is al  vrij groot) disassembleren is het aan te raden om 
je computer te laten beginnen als je bijv. gaat eten. Als je 
dan terugkomt  kun je  blij zijn  als hij klaar is. (Je kunt 
natuurlijk   ook   gaan   chatten!  Dat   is  een   favoriet 
tijdverdrijf  van mij.)  Houdt rekening  met factor  30 a 35 
voor de grootte van de file. Probeer het zoveel mogelijk via 
RAMdisk te doen.


                     V O O R B E E L D 

Als  voorbeeld zal  ik nu  even een machinetaalprogrammaatje 
intikken en de geRESOURCEde versie ook afbeelden.


        BDOS:   EQU     5
        POINT:  EQU     #8000

                LD      C,9
                LD      DE,TEKST
                CALL    BDOS
                LD      HL,POINT
                LD      B,"K"
                LD      (HL),B
                RST     0

        TEKST:  DB      "Dit is een test voor RESOURCE"
                DB      13,10,"$"

Ik heb  een paar  variabelen gebruikt  om te  laten zien wat 
RESOURCE hiervan maakt.


         V O O R B E E L D   N A   R E S O U R C E 

                ORG     00100H

Dit  is het  startadres. en dit je weglaten als je met GEN80 
of M80 werkt.

        X0005   EQU     00005H

Het BDOS-aanroepadres. Als je met TED werkt kun je het beste 
onvoorwaardelijk X0005 door BDOS laten vervangen.


        X0177   EQU     00177H
        X017C   EQU     0017CH
        X017D   EQU     0017DH
        X018F   EQU     0018FH
        X0196   EQU     00196H

Deze  labels worden in de tekst gebruikt. Alle tekst dien je 
toch  te  vervangen  door  DB's, dus  eigenlijk kunnen  deze 
labels verwijderd worden.


                LD      C,009H
                LD      DE,T010F
                CALL    X0005

De BDOS-aanroep.


                LD      HL,08000H
                LD      B,04BH

Even  zien wat  er gedaan  wordt met gewone veriabelen: hier 
komt de "intelligentie" van RESOURCE te voorschijn. Er wordt 
verschil  gemaakt  tussen tekst  (zie boven)  en een  gewoon 
adres.


                LD      (HL),B
                RST     000H

Bij RST's worden uiteraard geen labels gebruikt.


        T010F:  LD      B,H
                LD      L,C
                LD      (HL),H
                JR      NZ,X017D
                LD      (HL),E
                JR      NZ,X017C
                LD      H,L
                LD      L,(HL)
                JR      NZ,X018F
                LD      H,L
                LD      (HL),E
                LD      (HL),H
                JR      NZ,X0196
                LD      L,A
                LD      L,A
                LD      (HL),D
                JR      NZ,X0177
                LD      B,L
                LD      D,E
                LD      C,A
                LD      D,L
                LD      D,D
                LD      B,E
                LD      B,L
                DEC     C
                LD      A,(BC)
                INC     H
                END

Dit is de tekst. Tekst is te herkennen aan het voorkomen van 
relatief  veel JR  NZ's, dat  is het gevolg van spaties. Ook 
bestaat het vrijwel helemaal uit onzinnige LD's.
Tenslotte wordt elke source be�indigd met END.


                       G E B R U I K 

Het  bovenste gedeelte  van de  .PRN-file kun  je het  beste 
uitprinten. Dan  kun je  de onzin-instructies heel makkelijk 
vervangen door de juiste tekst.

De  gebruikte  labels  zijn als  volgt opgebouwd:  eerst een 
letter met de vermoedelijke betekenis van het label, dan het 
adres waar het label stond bij de originele file.

De volgende letters ben ik al tegengekomen:

T: tekst.
A: sprong-adres in het programma zelf
Y: onopgelost label, meestal een CALL-adres zoals de BDOS
X en Y: onopgelost label (?)
D: vermoedelijk een adres buiten de programma-code, maar wat 
   toch aangeroepen wordt met "JP".


Als je  een label doorhebt, kun je het onvoorwaardelijk door 
een duidelijk label laten vervangen met TED.

Ik ga  zelf altijd  de hele  source na van het begin tot het 
einde.  Als ik dat gedaan heb, kan ik de source aanpassen en 
in mijn eigen programma's gebruiken.

Vragen  of opmerkingen  (of fanmail) naar de Sunrise postbus 
sturen  t.a.v.  Kasper  Souren.  Ik zou  graag weten  hoe ik 
bepaalde gedeelten automatisch door DB's kan laten omzetten. 
Dat  zou   heel  veel  werk  schelen.  Op  deze  disk  staan 
RESOURCE.COM  en  RESOURCE.BAT,  een  kleine  batchfile  die 
gebruik  maakt  van  KEYFAKE om  RESOURCE automatisch  in te 
stellen (even aanpassen voor MSX-DOS 1).

                                              Kasper Souren

