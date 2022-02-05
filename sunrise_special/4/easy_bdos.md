                               E A S Y   B D O S 
                                                  
          
          Opm: Ik ga er in dit  artikel  van  uit  dat  de  lezer  een
               behoorlijke kennis van de BDOS bezit.
          
          Het  aansturen  van  een  diskdrive  is  een   zeer   lastig
          karweitje. Zelfs ervaren programmeurs zullen meerdere  malen
          verwensingen aan bepaalde chipfabrikanten maken tijdens  het
          schrijven  van  software  om  een  FDC   (Floppy   Diskdrive
          Controller) aan te sturen. Dat is ��n van de redenen  waarom
          de BDOS standaard in MSX zit. De BDOS maakt elke kennis  van
          FDC's overbodig voor het aansturen  van  een  diskdrive.  De
          BDOS software schept als het ware een interface van en  naar
          de gebruiker dat vele malen eenvoudiger is  dan  het  direkt
          aansturen van  de FDC.  (Nvdr: en het werkt ook nog op bijna 
          alle computers.)
          
          We zijn de ontwerpers van de BDOS dan  ook  eeuwig  dankbaar
          voor deze vriendelijke geste, maar er schort nog het ��n  en
          ander  aan  dit  'gebruikersvriendelijke'   interface.   Het
          schrijven van een goede routine om  een  file  van  disk  te
          laden is nl. niet zo eenvoudig als de BDOS  doet  vermoeden.
          Om dit te bewijzen zal ik eerst maar een voorbeeld geven van
          de meest eenvoudige methode om een file te laden.
          
          Stel we  willen  een  MoonBlaster  muziekstuk  inladen.  Een
          dergelijke file is praktisch altijd kleiner  dan  16 kB. Met
          deze kennis kan dan de volgende routine geschreven worden.
          
          LD_MBM: LD   C,15
                  LD   DE,FCB
                  CALL &HF37D          ; Open file om eruit te lezen
                  INC  A               ; If A=255, then error
                  JR   Z,NO_OK         ; Fout tijdens openen file
                  LD   HL,0
                  LD   (FCB+12),HL     ; Zet start(lees)blok op 0
                  INC  HL
                  LD   (FCB+14),HL     ; Zet recordlengte op 1
                  LD   C,26
                  LD   DE,MUSADR
                  CALL &HF37D          ; Leesadres van file is MUSADR
                  LD   C,39
                  LD   DE,FCB
                  LD   HL,&H4000       ; Lees max. 16kB
                  CALL &HF37D          ; Lees blok
                  DEC  A               ; A=1, then error
                  JR   NZ,NO_OK
          OK:          :
                       :
                  RET
          
          NO_OK:       :
                       :
                  RET
          
          FCB:    DB   0
                  DM   "MB_MUSIC","MBM"
                  DW   0,0
                  DW   0,0,0,0,0,0,0,0
                  DW   0,0
                  DB   0
          
          Na   de   bestudering   van   dit   'programma'   valt    de
          foutafhandeling  van  de  feitelijke  laadaktie  (C=39)  op.
          Indien de BDOS hier een foutmelding geeft  is  de  laadaktie
          goed? Dit komt omdat we probeerden 16 kB in te laden terwijl
          we weten dat de file per  definitie  kleiner  dan  16kB  is.
          Zelfs de BDOS  zal  niet  voorbij  het  eind  van  een  file
          proberen te lezen en geeft dientengevolge  een  foutmelding.
          
          Deze routine rammelt werkelijk aan alle kanten en  kan  veel
          beter geschreven worden. Dit kan door de recordlengte gelijk
          te maken aan de bestandslengte. Na het openen  van  de  file
          staat  de  bestandslengte  in  het  FCB.  Door  deze  in  de
          recordlengte te kopi�ren en daarna  bij  de  laadaktie  niet
          16 kB in te lezen, maar 1 record gaat het wel goed.
          
          Dit soort routines heeft verder als nadeel dat  ze  absoluut
          niet universeel inzetbaar zijn. Voor het laden van muziek en
          andere data gaat het wel, maar  om  graphics  behoorlijk  te
          laden moet meer uit de kast gehaald worden. Het  zou  er  op
          neerkomen dat er voor elke soort te laden  file  een  aparte
          routine  zou  moeten  komen  met  eigen  error  handling.  U
          begrijpt, dit levert enorme listings  op  met  routines  die
          toch heel erg veel op elkaar lijken.
          
          Wat ik hiermee probeer aan te  tonen  is  dat  de  BDOS  een
          uiterst noodzakelijk stukje software is, maar dat er bij het
          gebruik toch nog wat haken en  ogen  aan  zitten.  Naast  de
          normale  BDOS  error  handling  (BDOS  uitvoer  via  het   A
          register) is er nl. ook nog de error pointer  hook  die  bij
          ernstige diskdrive fouten aangeroepen  wordt.  Probeer  maar
          eens een file te cre�ren met  de  diskdrive  protect  schuif
          open. Uw machinetaal programma zal zonder pardon naar  BASIC
          terugkeren (een verbijsterde programmeur achterlatend).
          
          
                    K A N   H E T   E E N V O U D I G E R ? 
          
          Natuurlijk, is dan mijn antwoord. Praktisch is gebleken  dat
          het goede gebruik van de BDOS helemaal niet zo eenvoudig  is
          als de makers doen geloven. Wat we nodig hebben is  software
          die een nieuw interface tussen de diskdrive en de  gebruiker
          vormt waardoor de gebruiker absoluut geen kennis van de BDOS
          meer nodig heeft. Het enige waar de gebruiker  zich  nog  om
          hoeft te bekommeren is dat hij ervoor  moet  zorgen  dat  de
          gegevens op de juiste plaats terecht komen.
          
          Laten we eerst maar eens gaan kijken wat deze software  moet
          doen. Praktisch blijkt dat 95% van alle BDOS gebruik beperkt
          blijft tot het laden of  bewaren  van  gegevens.  Het  heeft
          praktisch dus ook  weinig  zin  om  onze  nieuwe  besturings
          software iets anders te kunnen laten doen.
          
          Foutmeldings uitvoer van de routines kan  beter  met  de  Cy
          flag gedaan worden dan met het A  register  (zoals  de  BDOS
          doet). Dit  komt  omdat  via  de  Cy  flag  onmiddelijk  een
          spronginstruktie  gedaan  kan  worden  terwijl  met  het   A
          register eerst een flag zet instruktie gedaan moet worden.
          
          
                      E A S Y   B D O S   F U N C T I E S 
          
          De nu te beschrijven routines zijn terug  te  vinden  in  de
          file  EASYBDOS.ASC  die  op  deze  disk  te  vinden  is.  De
          verzameling routines heet de fileshell omdat ze nl. als  een
          schil om de BDOS heen  zitten.
          
          De routines laten nog te wensen over. Zo kan er maar 1  file
          tegelijkertijd open staan, terwijl de BDOS er veel meer  kan
          hebben. Het toevoegen van een dergelijke optie zou  voor  de
          gemiddelde programmeur echter  weinig  problemen  op  moeten
          leveren. Ik kan me  voorstellen  dat  FOPEN  en  FCREAT  een
          nummer in het A register teruggeven die dan bij resp. FBLKRD
          en FBLKWR weer meegegeven moeten worden.  Elk  nummer  staat
          dan voor een bepaald FCB waarvan het adres dan wel  door  de
          routines berekent kan worden. Let er daarbij wel op  dat  de
          variabele FSTAT dan niet meer naar behoren funktioneert, dus
          ook dit moet dan aangepast worden.
          
          Een  andere beperking  is die dat de maximale lengte van een 
          te  lezen  file  65535  bytes is.  Deze beperking  is echter 
          redelijk  eenvoudig  op  te  lossen.  Het  gebruik van  deze 
          routines is in eerste instantie ook alleen maar bedoeld voor 
          demo's en kleine utilities. Voor het wat serieuzere werk zal 
          ik deze twee beperkingen er nog wel eens uit halen.
          
          
          F I L E S H E L L   I N S T A L L 
          Name: FINSTL
          In  : A, (see BIOS routine &H24)
          Out : Cy, on error
          
          Deze   routine   installeert   de   software.   Naast    het
          initialiseren  van  enkele  systeemvariabelen   buigt   deze
          routine de BDOS pointer error hook af zodat bij een ernstige
          fout niet meteen naar BASIC gesprongen wordt.  Elke  aanroep
          die vanaf nu rechtstreeks naar de BDOS  gemaakt  wordt,  zal
          afgevangen worden zodat de error handling ook dan actief is.
          A moet bij invoer de slot en  subslot  waarden  van  page  1
          bevatten. Het programma zou dit  ook  zelf  wel  uit  kunnen
          zoeken, maar dat gaat hier wat ver.
          
          Heb je bv. de BASIC ROM nog 'aan' staan, dan is  de  aanroep
          als volgt:
          
          LD   A,(&HFCC1)              ; Slot en subslot van MAIN-ROM
          CALL FINSTL
          RET  C                       ; Error
          
          Is de BASIC ROM vervangen  door  RAM  geheugen,  dan  is  de
          aanroep:
          
          LD   A,(&HF342)              ; Slot en subslot van MAIN-RAM
          CALL FINSTL
          RET  C
          
          Indien  FINSTL  niet  uitgevoerd  wordt  zal   elke   andere
          fileshell routine aanroep onmiddelijk een foutmelding  terug
          geven.
          
          
          F I L E S H E L L   R E S E T 
          Name: FRESET
          In  : nothing
          Out : Cy, on error
          
          Met FRESET kan  de  fileshell  software  weer  van  de  BDOS
          verwijderd worden. De BDOS wordt  niet  langer  afgetapt  en
          error handling wordt niet meer gedaan.  Indien  er  nog  een
          schrijf  file  open  staat  wordt  deze  eerst   automatisch
          gesloten.
          
          
          O P E N   F I L E 
          Name: FOPEN
          In : HL, filename address
          OUt: Cy, on error
          
          FOPEN moet aangeroepen worden wanneer een file gelezen  moet
          worden (net als bij de BDOS overigens). HL moet  bij  invoer
          wijzen naar de filenaam. Dit ziet er als volgt uit:
          
                  LD   HL,MBMNME
                  CALL FOPEN
                  RET  C
                    :
                    :
          
          MBMNME: DM   "MB_MUSIC","MBM"
          
          De filenaam MOET 11 karakters lang zijn. Indien de naam niet
          8 karakters lang is  moet  de  naam  met  spaties  aangevuld
          worden. Ditzelfde geldt ook voor de extensie.
          
          
          F I L E B L O C K   R E A D 
          Name: FBLKRD
          In  : DE, blocklength
                HL, load address
          Out : Cy, on error
                A , ="r" if file is loaded.
          
          FBLKRD leest een blok  van  een  geopende  file.  Indien  de
          blocklengte groter is dan de (resterende) filelengte past de
          routine deze automatisch aan. HL  mag  alle  waarden  hebben
          behalve het adresgebied tussen &H4000 en  &H8000  (page  1).
          Deze beperking wordt door het MSX systeem opgelegt.  Sommige
          programmeurs schijnen dit nog steeds niet te  weten.  Indien
          de file helemaal geladen is staat bij uitvoer  het  karakter
          "r" in register A (De r van 'ready'). Willen we nu een  file
          laden die groter is dan 16 kB in blokken van 16 kB dan  gaat
          dat bv. als volgt:
          
                  LD   A,1
          LOOP:   OUT  (&HFE),A        ; Zet memory mapper
                  PUSH AF
                  LD   DE,&H4000       ; Lees max. 16kB blok
                  LD   HL,&H8000       ; Vanaf adres &H8000
                  CALL FBLKRD
                  EX   AF,AF           ; Bewaar uitvoer
                  POP  AF
                  INC  A               ; Volgende memory map. pagina
                  EX   AF,AF           ; Haal uitvoer weer op
                  RET  C               ; Error
                  CP   "r"             ; Klaar met laden
                  RET  Z
                  EX   AF,AF
                  JR   LOOP
          
          
          C R E A T E   F I L E 
          Name: FCREAT
          In  : HL, filename address (See FOPEN)
          Out : Cy, on error
          
          Met FCREAT kan een nieuwe file gemaakt worden (net  als  bij
          de BDOS).
          
          
          F I L E B L O C K   W R I T E 
          Name: FBLKWR
          In  : DE, blocklength
                HL, startaddress
          Out : Cy, on error
          
          Deze routine werkt in principe net  zo  als  de  block  laad
          routine, met het verschil dat de file  automatisch  gesloten
          wordt na het optreden van een foutmelding.
          
                  LD   DE,&H1000       ; Schrijf 4kB
                  LD   HL,&H8000       ; Vanaf adres &H8000
                  CALL FBLKWR
                  RET  C               ; Fout opgetreden
                  LD   DE,&H1000
                  LD   HL,&HB000
                  CALL FBLKWR
                  RET  C
                  CALL FCLOSE          ; Sluit file indien klaar
                  XOR  A               ; Maak Cy laag
                  RET
          
          
          C L O S E   F I L E 
          Name: FCLOSE
          In  : Nothing
          Out : Nothing
          
          Deze routine doet niets meer dan het aanroepen van het  BDOS
          close commando en is nodig om een net aangemaakte file af te
          sluiten.
          
          
          C H A N G E   D I S C 
          Name: CHNDSK
          In  : A
          Out : Nothing
          
          Change disk  is een  extraatje. Indien  A bij  aanroep 0  is 
          wordt  de boodschap "Insert source disk" op het scherm gezet 
          en in alle andere gevallen "Insert destination disk". Daarna 
          wordt gewacht tot de spatiebalk ingedrukt wordt. Indien twee 
          keer achter  elkaar een source disk aangevraagd wordt zal de 
          routine meteen terug springen zonder een tekst op het scherm 
          te zetten.
          
          
          De nu volgende routines zijn niet voor de gebruiker bedoeld,
          maar ik noem ze toch maar even.
          
          INTOFF:
          Deze routine zet de VDP interrupt (50/60 Hz) generator  uit.
          Hierdoor gaat het laden (vooral op trage Sony's) sneller.
          
          INTON:
          Laat zich raden.
          
          FBDOS:
          Dit is de routine waar elke  BDOS  aanroep  na  aanroep  van
          FINSTL naar toe springt.
          
          ERRCDE:
          Dit is de routine waar naartoe gesprongen wordt bij een BDOS
          error handling via de pointer hook. Deze routine  heeft  een
          'Abort, Retry' optie  ingebouwd  en  voorkomt  dat  bij  een
          dergelijke fout naar  BASIC  gesprongen  wordt.  De  routine
          wordt bv. aangeroepen wanneer de disk uit de  drive  gehaald
          wordt tijdens bv. een laadaktie.
          
                              !!! WAARSCHUWING !!!
          
          Net als enkele belankrijke  variabelen  (vanaf  PG1SET)  mag
          deze routine niet op page 1 gemapt worden. Zet deze  routine
          bij voorkeur op page 3 (vanaf &HC000).
          
          Bij  een abort  wordt de stack hersteld en worden de slot en 
          subslot instellingen  van page 1 hersteld naar de waarde die 
          bij FINSTL opgegeven is. Dit moet omdat de BDOS ROM zich nl. 
          op PAGE 1 nesteld tijdens disk akties. Hierna wordt de stack 
          hersteld  en geeft de routine een standaard BDOS error terug 
          (A=1).  Met  deze  methode  wordt  terugspringen  naar BASIC 
          voorkomen.
          
          Bij een Retry wordt het C register  1  gemaakt  waardoor  de
          BDOS de disk aktie zal proberen te herhalen!
          
          PRTERR:
          Deze routine wordt gebruikt door alle fileshell routines  om
          foutmeldingen op het scherm te zetten. Invoer gaat via het A
          register waar het foutnummer in staat.  Deze  methode  zorgt
          ervoor  dat  de  feitelijke  fileshell  routines  korter  en
          overzichtelijker worden.
          
          
          Als  laatste  zijn  er  dan  nog  de  fileshell  variabelen.
          Vanaf PG1SET mogen de variabelen niet meer op PAGE 1  gemapt
          zijn. Zet deze dus het liefst vanaf adres &HC000.
          
          FSTAT:
          Hierin staat de fileshell status. De opbouw is als volgt:
          Bit 0, hoog, dan is FINSTL uitgevoerd (fileshell aktief)
          Bit 1, hoog, dan is de huidige file open voor lezen
          Bit 2, hoog, dan is de huidige file open voor schrijven
          (Bit 1 en 2 kunnen dus nooit gelijktijdig hoog zijn)
          
          CURDRV:
          Huidige drive.
          255, dan is CHNDSK nog nooit aangeroepen
          0  , dan is de huidige disk de 'source disc'
          1  , dan is de huidige disk de 'desination disc'
          
          FLELEN:
          Het aantal nog te LEZEN bytes van een open file.
          
          _##ERR:
          Bewaarplaats van de oude BDOS error double pointer
          
          OLDDOS:
          Bewaarplaats van oude BDOS aanroep hook
          
          PG1SET:
          Bewaarplaats van bij FINSTL opgegeven slot instellingen
          
          SP_SAV:
          Bewaarplaats voor StackPointer register
          
          FCB:
          Het FCB
          
          
          Dat  is  het dan.  Probeer de  listing eens  uit. De  minste 
          problemen  zullen  optreden  als  de complete  listing vanaf 
          &HC000 begint.  Lees ook eens het artikel over de BDOS error 
          handling  om wat  gedetailleerdere informatie te krijgen. Ik 
          zou   zeggen,   klieder  naar   hartelust  in   het  gegeven 
          programmeervoer en  haal de twee eerder genoemde beperkingen 
          er  zelf  eens  uit. Let  vooral op  hoe snel  het met  deze 
          routines  mogelijk is  om complete  laad of save routines in 
          elkaar te gooien en hoe klein deze dan zijn. Tot de volgende 
          preek.
          
                                                     Alex van der Wal
