                  B D O S   F O U T A F H A N D E L I N G 
                                                             
          
          Dit is een onderwerp waar ik nog nooit een artikel over  heb
          gezien, en onder het motto "Beter laat dan nooit" moet  daar
          nu maar eens verandering in komen. Dit  artikel  is  bedoeld
          voor diegenen die niet verwonderd kijken als  de  term  BDOS
          valt.
          
          De BDOS kent twee manieren om  fouten  af  te  handelen.  De
          meest bekende is die van het A  register  dat  na  een  BDOS
          aanroep met 0 of een andere waarde gevuld is. Is A=0, dan is
          de BDOS aktie goed gegaan en in alle andere gevallen  is  er
          iets mis gegaan. De tweede methode is wat onbekender.  Onder
          MSX-DOS zijn dit de fouten die een  'Abort,  Retry,  Ignore'
          melding opleveren. Onder BASIC zijn dit de  fouten  waardoor
          het lopende programma  onderbroken  wordt  (ook  machinetaal
          programma's) en naar BASIC teruggekeerd wordt.
          
          De eerste methode is bij alle BDOS gebruikers  bekend,  maar
          de tweede roept vaak vele vraagtekens op  (zelfs  bij  mij).
          Dat komt in de eerste plaats  omdat  ik  zo  goed  als  geen
          documentatie bezit over  deze  methode,  maar  dat  merkt  u
          straks nog wel. U moet me dus maar vergeven dat ikzelf  hier
          en daar ook nog wat vragen open laat omdat ik simpelweg niet
          over de documentatie beschik om alles tot  in  perfektie  te
          behandelen.  Als  er  dus  mensen  zijn  die  de  door   mij
          onbeantwoorde vragen wel kunnen beantwoorden, dan zou ik het
          zeer op prijs stellen als deze mensen zich  in  kontakt  met
          onze glorieuze (ahum ahum) stichting stellen.
          
          
                                 A A N R O E P 
          
          De tweede methode wordt bv. aangeroepen wanneer de disk  uit
          de drive verwijderd wordt tijdens een BDOS aktie,  maar  ook
          indien de  'Disk  write  protect'  aktief  is  wanneer  iets
          gesaved wordt. De aanroep  is  op  het  eerste  gezicht  wat
          ingewikkeld, het gaat nl. via een dubbele pointer aanroep.
          
          Bij een aanroep naar de foutafhandeling wordt  eerst  de  16
          bits adreswaarde  vanaf  adres  &HF323  gelezen  (de  eerste
          pointer). Daarna wordt het 16  bits  adres  gelezen  van  de
          eerder gelezen adreswaarde. Daarna wordt de code  uitgevoerd
          vanaf het net gelezen adres.
          
          Oftewel bv.:
          
          (&HF323) = &H72AE
          (&H72AE) = &H72B0
          &H72B0   = Start van fout afhandelingscode.
          
          Het lijkt misschien wat onlogisch, maar daarover moet u niet
          bij mij zijn om over te klagen.
          
          
                         G E G E V E N S   I N V O E R 
          
          Bij de foutafhandelings  code  aangekomen  staat  in  het  C
          register een waarde die de opgetreden  fout  omschrijft.  De
          bij  mij  (overigens  niet  met  100%   zekerheid)   bekende
          foutcodes zijn:
          
          C >= 128 (bit 7 is hoog), dan "Bad FAT"
          C AND &H0F =  1, dan "Disk write protected"
          C AND &H0F <  4, dan "Disk offline"
          C AND &H0F = 11, dan "Disk write error"
          
          Of er ook nog anderen zijn weet ik niet, maar dit zijn  toch
          wel de belangrijkste.  Persoonlijk  denk  ik  dat  de  lijst
          langer moet zijn omdat de "Disk write  error"  mij  iets  te
          algemeen is. Is deze fout gelijk aan een "Disk full" of gaat
          het om een fysieke fout op het medium?  Wat ik ook mis  zijn
          de foutmeldingen die  via  PHYDIO  (CALL  &H0144)  verkregen
          kunnen worden.
          
          
                        G E G E V E N S   U I T V O E R 
          
          Het is ook mogelijk om via het C register een  waarde  terug
          te geven aan de BDOS. Persoonlijk ken  ik  er  twee.  Indien
          voor de RET instruktie (om de foutafhandeling te verlaten) C
          op 0 wordt gezet komt dat overeen met Abort.  Indien  C  een
          andere waarde heeft (bij voorkeur C=1) komt dat overeen  met
          Retry.
          
          Hier kleven echter nog wat haken en ogen aan.  De  Abort  is
          nl. de schrik voor elke ML programmeur omdat dan automatisch
          naar BASIC teruggekeerd  wordt!  De  Retry  werkt  wel  naar
          behoren en werkt als een (Sunrise) zonnetje. Het  moet  vast
          wel mogelijk zijn om te voorkomen  dat  met  de  Abort  naar
          BASIC gesprongen wordt, maar het blijkt dat zelfs mensen met
          de nodige documentatie niet weten hoe dat moet. Daarom heeft
          iemand op een helder moment het  volgende  truukje  bedacht,
          wat misschien niet helemaal netjes is, maar nood breekt wet.
          
          
                          D E   A B O R T   A B O R T 
          
          Wat  we  proberen  is  om  de  Abort  mogelijkheid  zelf  te
          programmeren (dus niet meer via de BDOS). Dat  betekent  dat
          de BDOS aanroep vroegtijdig afgebroken moet worden.  Om  dat
          te kunnen doen moet men in het uiterste  geval  drie  dingen
          weten, te weten:
          - De stackpointer voor aanroep naar de BDOS
          - De slot en subslot settings van page 1 voor aanroep
          - De slot en subslot (turbo R) settings van page 0
          
          Deze laatste is alleen  nodig  indien  de  BIOS  op  page  0
          vervangen is door bv. RAM geheugen. Om dit te verduidelijken
          zal ik eerst maar even noemen  wat  de  BDOS  allemaal  doet
          indien deze aangeroepen wordt.
          
          Ten eerste wordt de BIOS op page 0 geschakeld. Daarna  wordt
          de BDOS op page 1 geschakeld. Dit laatste is de reden waarom
          het FCB nooit op page 1 mag staan en waarom - volgens de MSX
          standaard - niet op het adresgebied van page 1  geladen  mag
          worden alhoewel dit op de meeste MSX'en wel zal werken  (met
          uitzondering van de Sony's). De oude  settings  van  page  1
          worden echter wel bewaard om ze na de uitvoer  van  de  BDOS
          actie te kunnen herstellen. Of de settings van  page  0  ook
          hersteld worden weet ik niet, maar ik  heb  mijn  vermoedens
          dat dit wel gebeurt (vanwege MSX-DOS).
          
          Aangenomen   dat   we   in   een   eigen   geschreven   fout
          afhandelingsroutine zitten (Door de eerder genoemde  pointer
          aanroep af te buigen). Willen we een Retry doen dan  is  dat
          simpel. Maak register C maar 1 en geef een  RET  instruktie.
          Simpeler  kan  niet.  Om  bij  een  Abort  de  BDOS  aanroep
          vroegtijdig af  te  breken  moeten  we  de  eerder  genoemde
          gegevens  over  de  stackpointer   en   de   page   settings
          herstellen.
          
          Page 0 moet alleen dan hersteld worden wanneer deze voor  de
          BDOS aanroep niet als BIOS geschakeld was (slot  0,  subslot
          0).
          
          Er vanuit gaande dat page 0 als BIOS geschakeld is kan  page
          1 als volgt hersteld worden.
          
          Was de BASIC-ROM geschakeld voor aanroep:
                 LD   A,(&HFCC1)       ; Slot & subslot van MAIN-ROM
                 CALL &H24
                 LD   SP,(SP_SAV)      ; Voor BDOS aanroep bewaarde SP
                 LD   A,1              ; Simuleer BDOS error
                 JP   CONT             ; Ga verder vanaf CONT:
          
          Was de MAIN-RAM geschakeld voor aanroep:
                 LD   A,(&HF342)       ; Slot & subslot van MAIN-RAM
                 CALL &H24
                 etc.
          
          Door het A register van een andere waarde dan 0 te  voorzien
          kan een BDOS error code gesimuleerd worden.  Het  lijkt  dan
          nl. net alsof de BDOS een foutmelding via  methode  1  terug
          geeft. Het bewaren van de SP voor de  BDOS  aanroep  is  ook
          heel simpel:
          
                 LD   (SP_SAV),SP
                 CALL &HF37D            ; Roep BDOS aan
          CONT:  NOP                    ; Continue na Abort
          
          Kijk in dit verband ook eens naar  de  listing  EASYBDOS.ASC
          waar dezelfde methode in verwerkt is.
          
          
                             B R O E V A   H A R O 
          
          Oftewel "Hoera bravo", maar dat zag Obelix niet zo duidelijk
          meer toen hij dronken door Lutetia liep. U zou nu  voldoende
          informatie moeten hebben om na enige zelfstudie eens wat  te
          gaan proberen. Mag ik daarbij nog wel de tip geven om altijd
          met een klad diskette te  werken,  want  de  BDOS  heeft  de
          oneigenaardige neiging om diskjes te  vernielen  wanneer  er
          aan de BDOS geknoeid is.  Vergeet  dus  nooit  om  afgebogen
          pointers of zelfs de BDOS aanroep hook te herstellen voordat
          weer regulier van de BDOS gebruik gemaakt gaat worden!
          
          Van alle systeemonderdelen van MSX heeft de  BDOS  het  voor
          elkaar gekregen om haar  geheimen  het  langst  te  bewaren.
          Langzaam maar zeker zal ook  de  BDOS  haar  geheimen  prijs
          geven en dat is maar goed  ook.  Veel  succes  bij  uw  BDOS
          projekten.
          
                                                     Alex van der Wal
  