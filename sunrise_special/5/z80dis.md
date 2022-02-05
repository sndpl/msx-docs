                                  Z 8 0 D I S 
                                               
          
          Z80DIS   is   een   programma  dat   van  een   willekeurige 
          machinetaalfile een assemblerlisting maakt. Het programma is 
          geschreven  door Kenneth  Gielow voor  CP/M, en draait (dus) 
          ook op MSX.
          
          Het  is  een  aardige  klus  om  uit  een  brok ML  weer een 
          fatsoenlijke assemblerlisting,  inclusief labels  enzovoort, 
          te  fabriceren. Maar Z80DIS doet het heel aardig. Volgens de 
          maker heeft  het programma dan ook kunstmatige intelligentie 
          aan  boord. Wat  hij hier nou precies onder verstaat weet ik 
          niet, maar het werkt wel in ieder geval...
          
          Z80DIS  moet  altijd in  de default-drive  aanwezig blijven, 
          omdat het  programma met overlay-files werkt die tussentijds 
          worden ingeladen.
          
          Na de nodige meldingen begint het programma met:
          
            Input   filename:  ________.COM
            Output  filename:  none.MAC
            Listing filename:  ________.PRN
          
          De  invoer-routine is  misschien even wennen. Laat je echter 
          niet misleiden:  er kan  gewoon een  drivenaam en een andere 
          extensie  ingegeven  worden.  Als  er  alleen  een  filenaam 
          ingetypt  wordt, zal  Z80DIS hier  dus zelf filenaam.COM van 
          maken.
          
          Let op: al staat de filenaam al voorgeprint, typ niet alleen 
          de  drivenaam of  een gedeelte  van de  naam in. Als je iets 
          wilt  wijzigen,  moet je  meteen alles  intypen (behalve  de 
          extensie, als je die niet wilt wijzigen).
          
            Descriptive title:  ______________________________________
          (Dit wordt bovenaan de ASM-listing gezet.)
          
            File load address: 
            Dis start address:
            Dis stop  address:
          
          Als  er   een  stuk  code  middenin  een  file  moet  worden 
          gedissassembleerd,  moet er eerst uitgerekend worden waar de 
          file eigenlijk zou moeten beginnen, om het interessante stuk 
          op het goede geheugenadres in te laden.
          
          Bijvoorbeeld: BLOAD-files beginnen altijd met een header van 
          7 bytes.  Als er  een BLOAD-file  van &H8000 tot &H87FF moet 
          worden gedisassembleerd, vul dan in: file load address: 7FF9 
          (=  &H8000 -  7), dis  start address  8000, dis  stop addres 
          87FF.
          
          
           Do you want to run a full output (as opposed to XREF only)?
          
          Als hier  "N" ingegeven  wordt, wordt in de .PRN-uitvoerfile 
          alleen  de structuur  en de  gebruikte labels gegeven. Wordt 
          hier "Y" ingegeven, dan wordt ook de source-listing gegeven. 
          Zie ook verderop in de tekst.
          
           On which disk do you want the scratch file to reside? (A-G)
          
          De  disk voor  opslag van  tijdelijke files.  Natuurlijk kan 
          hier het beste een RAMdisk voor genomen worden.
          Let op:  Z80DIS kan  drive H: niet aan. Geef dus onder DOS 2 
          eerst bv. een ASSIGN G: H:
          
          Nvdr.  Als je op RETURN drukt neemt Z80DIS de default drive. 
          Als je  dus op  de RAMdisk  werkt, moet  je gewoon op RETURN 
          drukken.
          
            Do you want to process all Z80 codes
                                (as opposed to 8080 equivalents only?)
          
          Ik neem aan dat iedereen hier gewoon voor "Y" kiest...
          
          
               C O N T R O L   B R E A K   D E F I N I T I O N S 
          
          Na  deze  vragen  gaan we  naar het  volgende deel,  waar de 
          "control break definitions" kunnen worden gemaakt.
          
          Hiermee kan aangegeven worden hoe de ML is opgedeeld in:
          I: Instructies
          B: Bytes (DEFB)
          W: Words (DEFW)
          A: ASCII karakters (DEFM) 
          S: Losse ruimte, die niet wordt gedisassembleerd. (DEFS)
          T: "Table of addr".
          Een  "Table of  addr" is  een stuk met allemaal DEFW (LABEL) 
          instrukties. Ik  weet eerlijk gezegd niet wat hiermee gedaan 
          kan worden.
          
          Als  een van  deze letters ingetypt wordt, wordt er gevraagd 
          om een  beginadres waar dit type code begint. Op deze manier 
          kan  van heel  het stuk  code de vorm worden bepaald, waarin 
          het wordt gedisassembleerd.
          
          Verdere geldige commando's in dit gedeelte zijn:
          
          H: Help                     beknopt overzicht commando's
          C: Clear break table        begin opnieuw met een lege 
                                      indeling
          L: List break table         laat de huidige "control break 
                                      definitions" zien
          P: Print break table        lijst naar printer
                                      (Dit werkt helaas niet. Als de 
                                      printer off-line is wordt er 
                                      gewacht maar als hij on-line 
                                      gezet wordt, wordt er niets 
                                      afgedrukt.
          K: Kill break entry point   voor als er een verkeerd adres 
                                      is ingevoerd.
                                      Als er een niet bestaande entry 
                                      point wordt verwijderd, wordt 
                                      geen melding gegeven, dus let 
                                      zelf goed op.
          FL: load break table
          FS: Save break table        De indeling kan ook naar disk 
                                      worden weggeschreven. (Standaard 
                                      extensie: BRK.) Dit is heel 
                                      handig: als er later ontdekt 
                                      wordt dat er een foutje is 
                                      gemaakt, hoef je niet heel de 
                                      indeling opnieuw in te typen.
          En het belangrijkste:
          
          *: Auto-assignment
          
          Hier komt  de kunstmatige  intelligentie om  de hoek kijken. 
          Het  programma is namelijk in staat het hele stuk code af te 
          zoeken en zelf de "control break table" in te delen.
          
          Alle dingen  die Z80DIS niet thuis kan brengen ('anomalies') 
          worden  gemeld. (Er  wordt ook  de mogelijkheid geboden deze 
          meldingen uit te printen, wat helaas weer niet lukt.) Hierna 
          komt men  weer in  het gedeelte  waar de  "break table" zelf 
          ingedeeld kan worden.
          
          Het  is vaak  het beste  om "Auto-assignment" meerdere keren 
          achter alkaar  uit te  voeren, om  zo het beste resultaat te 
          krijgen.
          
          Bij grote stukken code kan het voorkomen dat Z80DIS afbreekt 
          en  (slordig)  zonder  enige melding  meteen naar  DOS wordt 
          teruggekeerd.  In  dit  geval  is  het geheugen,  dat Z80DIS 
          gebruikt  voor de  opslag van labels en dergelijke, vol. Het 
          gebruik van  DOS 1 of 2 heeft geen invloed op de grootte van 
          dit geheugen. Het enige wat gedaan kan worden is een kleiner 
          stuk code disassembleren - of zelf de "break table" indelen.
          
          De  grootte  van  het stuk  dat kan  worden gedisassembleerd 
          verschilt per brok code. Omdat er een tellertje meeloopt, is 
          het  meestal na te gaan hoe groot het stuk code genomen moet 
          worden zodat het geheugen nog net niet vol is.
          
          De resultaten  van "Auto-assignment"  zijn behoorlijk  goed, 
          maar  er  zitten  een  paar  onvolkomenheden  in  (zoals  te 
          verwachten was). Dingen die ik tot nu toe ontdekt heb:
          
          - Als na  een CALL  een LD  HL,nn of  LD IX/IY,nn instruktie 
          staat,  maakt  hij  van  deze  ene instruktie  meestal ASCII 
          tekens.
          
          - Z80DIS  geeft ook  labels aan die midden in een instruktie 
          voorkomen. (met  bv. LABEL:  EQU $-1) Dit is natuurlijk heel 
          mooi  maar  meestal  betekent  dit  dat er  een fout  in die 
          instruktie zit.
          
          Met  andere woorden: wie na het disassembleren de listing op 
          fouten  na  wil kijken,  kan het  best beginnen  met in  een 
          tekstverwerker  alle  "$"  tekens  na  te  zoeken.  Ook  het 
          nakijken  van  alle DEFM's  is aan  te raden,  zoals uit  de 
          eerste opmerking volgt.
          
          - Het  programma is  niet opgewassen  tegen blokken  vol met 
          bytes  &HC9. Het  ziet dit  niet als bytes maar als allemaal 
          losse RET  instrukties, met als gevolg dat het geheugen voor 
          labels binnen de kortste keren vol is.
          
          Als  deze dingen (eerste en derde opmerking) handmatig in de 
          break table  worden veranderd,  kan er  geen auto-assignment 
          meer gedaan worden, want Z80DIS herstelt dan zelf de 'foute' 
          situatie weer.
          
          
          Het laatste commando is:
          Q: Quit
          
          Het  control break-gedeelte  wordt verlaten  en er wordt met 
          het echte  disassembleerwerk begonnen.  Als de .BRK file nog 
          niet  is weggeschreven,  wordt gevraagd of dit alsnog gedaan 
          moet worden.
          
          Ook  tijdens  het disassembleren  kan het  gebeuren dat  het 
          geheugen vol  is en  er zonder  melding naar  DOS gesprongen 
          wordt.  Kijk uit:  het kan  dus zo  zijn dat er dan een niet 
          complete assembleerlisting op disk staat.
          
          Er is  weer aan een tellertje te zien of het einde inderdaad 
          gehaald wordt zonder geheugenproblemen.
          
          
                         D E   I N D E L I N G   V A N 
          
                   D E   . P R N   E N   . M A C   F I L E S 
          
          De gemaakte .PRN file bevat de volgende dingen:
          
          - titel en een paar andere meldingen
          
          - de "control break table"
          
          - een lijst van alle labels, met vermelding van alle 
            adressen waar die labels gebruikt worden en de funktie die 
            ze op dat adres hebben. (zie verderop)
          
          - een lijst van alle subroutines, met weer alle 
            aanroepadressen/funkties vermeld. Ook is er een een regel 
            opengehouden voor de beschrijving van de subroutine.
          
          - de source-listing
            Aan de linkerkant van de listing is een kolom voor, bij 
            elke instruktie, de geheugenadressen en de inhoud van de 
            adressen die de instruktie in beslag neemt. Bij elke 
            subroutine zijn regels gereserveerd voor commentaar 
            (funktie, inputs, outputs).
          
          De  .MAC  file  is  een source-listing  die meteen  door een 
          geschikte  assembler   heen  gehaald   kan  worden.  Dit  is 
          hetzelfde  als het laatste deel van de .PRN file maar zonder 
          kolommen met getallen aan de linkerkant.
          
          De source-listings zijn niet direkt geschikt voor GEN80/M80. 
          Dit  komt  door  de  naamgeving van  de labels.  In de  file 
          Z80DIS.PMA zit echter wel het programma ZSM.COM hiermee zijn 
          de sources wel te assembleren.
          
          De hexadecimale getallen zijn van de vorm xxxxH.
          
          Mensen die de listings in WBASS willen gebruiken, moeten:
          - in een tekstverwerker de TABS in spaties omzetten;
          - EX AF,AF' in EX AF,AF veranderen
          - 'string'  in "string" veranderen (bijv. bij DEFB)
          - Zelf iets verzinnen als er bv. LABEL: EQU $-1 voorkomt.
          
          
                  L A B E L S   E N   H U N   F U N K T I E S 
          
          Een label bestaat uit 6 tekens. Aan een paar labels wordt al 
          een standaard naam (van een CP/M routine) toegekend. Voor de 
          overige  labels hangt het eerste teken af van de funktie van 
          het betreffende geheugenadres:
          
          C  als er ergens een CALL/RST naar het label gemaakt wordt
          J    "     "         JP/JR     "                       "
          D  als het betreffende adres voor data-opslag gebruikt wordt
          I  (immediate) als het label gewoon een getal is (dat alleen 
             in een registerpaar geladen wordt. Alle 16 bits getallen 
             worden namelijk labels gemaakt.)
          .  als het betreffende label niet wordt gebruikt in de 
             listing, bijvoorbeeld als het een begin is van een 
             routine die nergens wordt aangeroepen.
             
             Als C en J allebei voor hetzelfde label voorkomen, wordt 
             C genomen. Dit is ook zo bij C en I.
          X  voor alle overige combinaties van funkties
          
          Dat  van alle 16 bits getallen labels worden gemaakt is soms 
          handig, soms  niet. Als  er ergens een CALL naar een routine 
          gemaakt  wordt  en  puur  toevallig  wordt precies  dezelfde 
          waarde ergens in HL geladen, wordt aan de CALL .... en de LD 
          HL,.... hetzelfde label toegekend.
          
          Het tweede teken is een # als er in de ASM-listing precies 1 
          keer  naar het  label verwezen wordt en een . (punt) in alle 
          andere gevallen.  Het derde  t/m zesde teken bestaat uit het 
          betreffende adres (hexadecimaal).
          
          
          Bij de vermelding van adressen, met de funktie die het label 
          op  elk adres  heeft, worden  voor de  funkties de  volgende 
          afkortingen gebruikt:
          
          C : CALL naar label
          Cr: RST naar label
          J : JP naar label
          Jr: JR naar label
          Sb: byte schrijven
          Sw: word schrijven
          Lb: byte lezen
          Lw: word lezen
          Iw: het label is gewoon een getal
          
          
                       I N   D E   P R A K T I J K . . . 
          
          ... is  het een heel mooi programma maar heeft het toch zijn 
          beperkingen qua grootte van het te disassembleren stuk code. 
          Ook kan het soms behoorlijk lang duren.
          
          Ik heb een aantal dingen uitgeprobeerd en ben op ongeveer de 
          volgende  resultaten  gekomen  (hoewel  ik het  niet precies 
          ge-timed heb):
          
          Op  een turbo R met ramdisk doet Z80DIS bijna 2 minuten over 
          een  "auto  assignment"  ronde  van  12  kB  code.  (Dit  is 
          overigens ongeveer  de grootte  waar je er nog net zeker van 
          bent  dat het  geheugen niet vol raakt.) Om de code optimaal 
          te krijgen  moet er  4 a  5 keer  auto assignment uitgevoerd 
          worden.  Daarna duurt  het disassembleren zo'n 2 1/2 minuut. 
          (Hier is 8 tot 9 kB het maximum.)
          
          In Z80  mode gaat  het ongeveer 5 a 6 keer zo langzaam. Hier 
          kom  je dus  uit op  meer dan een uur voordat de ASM-listing 
          klaar  is.   (In  het  ideale  geval  dat  Z80DIS  dus  niet 
          "afbrak"...  Anders ben  je nog een tijd aan het zoeken naar 
          de maximaal  toegestane grootte van het stuk ML.) Mensen die 
          daarna  de listing gaan nakijken op fouten, de control break 
          instellingen  veranderen  en  weer  gaan  disassembleren  en 
          nakijken, mogen zo'n 2 uur uittrekken...
          
          Die 12  kB code  levert een .PRN file van bijna 200 kB op en 
          een  .MAC file van 80 kB. Mensen zonder veel geheugen hoeven 
          dus geen grote .PRN files aan te maken.
          
          Natuurlijk zal een klein stuk ML weinig problemen opleveren. 
          Hier hoeft ook minder keer auto assignment te worden gedaan. 
          Maar wees alvast gewaarschuwd...
          
          Toch  is  en  blijft  Z80DIS  een  prachtig  hulpmiddel voor 
          iedereen  die wel  eens een  sourcelisting van  een stuk  ML 
          nodig heeft  of door  wil spitten.  En welke  MSX'er met een 
          beetje ML-kennis heeft dat nou nooit?
          
          
                                    T I P S 
          
          Mensen  die  TED  gebruiken,  kunnen  het  beste  een aparte 
          TED-file   maken   met   de  beste   instellingen  voor   de 
          assemblerlistings:
          
          - TAB-instellingen (F2/T) op AAN, AAN, UIT om de file zo 
            klein mogelijk te houden. (Door de laatste UIT te zetten 
            duurt het laden wel iets langer. Maar Z80DIS kent niet 
            overal optimaal TABS toe, dus het is het beste om hem toch 
            uit te laten staan.)
          - TABs op kolommen 9, 17, 25, ... 8n+1 (net als onder 
            BASIC/DOS)
          - Edit mode (F3/S/E) AAN, om geen spaties aan het eind van 
            regels te krijgen.
          - "Bewaar instellingen bij elke tekst" (F2/B/E) UIT, om geen 
            overbodige .TED files te krijgen. De instellingen zijn 
            toch voor elke file hetzelfde.
          
          Misschien handig  om te  weten: De  .BRK file  is een gewone 
          tekstfile  die in een tekstverwerker ingeladen/veranderd kan 
          worden.  Alleen   hier  staan  de  control  breaks  in  veel 
          beknoptere vorm in als Z80DIS ze weergeeft.
          
          Ook  handig om  te weten: Als Z80DIS tijdens Auto Assignment 
          naar DOS  teruggaat, staat  in de  tekstfile Z80DIS2.$$$  de 
          lijst met anomalies.
          
                                                         Roderik Muit
