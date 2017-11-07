                           M E M O R Y   M A P P E R


          De  Z80 kan maar 64 kB geheugen tegelijk adresseren. Om toch
          meer dan  64 kB  RAM te kunnen gebruiken is de memory mapper
          bedacht.   De  memory  mapper  behoort  niet  tot  de  MSX2-
          standaard,   en   daarom  heeft   de  MSX2   geen  standaard
          aanstuurroutines voor de memorymapper.

          Vanaf MSX2+ behoort de mapper wel tot de standaard en daarom
          zitten er  in DOS2 (dat ongeveer gelijktijdig met MSX2+ werd
          gelanceerd) wel mapperroutines.

          Veel  software die de mapper gebruikt werkt niet onder DOS2,
          dat komt  door een  verkeerd gebruik  van de  mapper. In dit
          artikel  zal ik uitleggen hoe de mapper werkt, en hoe je hem
          zo kunt  aansturen dat  het zowel  onder DOS1 als onder DOS2
          probleemloos werkt.


                                 W E R K I N G

          Het geheugen in een mapper wordt opgedeeld in blokken van 16
          kB,  die  mapperpages  of  mappersegmenten  worden  genoemd.
          Aangezien het  woord page  verwarrend is  zal ik hier verder
          het woord segment aanhouden.

          Deze segmenten worden oplopend genummerd, te beginnen bij 0.
          Een mapper  van 256 kB heeft 16 segmenten die genummerd zijn
          van 0 tot en met 15.

          De 64 kB adresbereik wordt opgedeeld in vier pages van 16 kB
          elk. Op  elke page  kan een  memory mapper worden geschakeld
          door  gebruik te  maken van de routine ENASLT op adres &H24.
          Vervolgens  kan  een  segment  worden  geselecteerd  door de
          juiste waarde  naar de corresponderende out-poort te sturen.
          Een overzicht:

          page 0          #0000-#3FFF     mapperpoort #FC
          page 1          #4000-#7FFF     mapperpoort #FD
          page 2          #8000-#BFFF     mapperpoort #FE
          page 3          #C000-#FFFF     mapperpoort #FF

          Zo  kun je  bijvoorbeeld segment 7 selecteren op adres #8000
          met de volgende instructies:

                          LD      A,7
                          OUT     (#FE),A

          Het  is op  zich heel eenvoudig om met verschillende mappers
          te werken, gewoon met ENASLT het slot van de gewenste mapper
          selecteren op de gewenste page.

          Je  mag mapperpoort  #FF niet veranderen omdat zich daar het
          systeemgebied bevindt.  Dit zal altijd segment 0 zijn van de
          mapper  waarmee de computer opstart. Dit zal bij een MSX2 of
          MSX2+ de  grootste mapper  zijn en bij de turbo R de interne
          mapper.  Dit omdat  de interne mapper van de turbo R sneller
          is dan  een externe  mapper. De communicatie van de R800 met
          de cartridgesloten gaat namelijk in een langzamere snelheid.

          Bij het opstarten worden de volgende segmenten geselecteerd:

          page 0          mapperpoort #FC         segment 3
          page 1          mapperpoort #FD         segment 2
          page 2          mapperpoort #FE         segment 1
          page 3          mapperpoort #FF         segment 0

          Wat  mij betreft  mag je  hiervan uitgaan omdat alle bekende
          MSX computers het zo doen. Officieel ligt echter alleen vast
          dat het systeemgebied zich in segment 0 begint.


                             W R I T E   O N L Y !

          De mapperpoorten  zijn officieel  write only,  wat uiteraard
          betekent  dat je ze alleen mag beschrijven en dat lezen geen
          zin heeft.  Toch gaat het lezen meestal goed en het wordt in
          programma's  die voor DOS1 zijn geschreven dan ook heel veel
          gedaan.

          Hierdoor ontstaan twee problemen:

          1) de software werkt niet meer onder DOS2
          2) de  software werkt  niet goed op een MSX2+ of turbo R met
             een externe mapper die groter is dan 512 kB

          Ik zal deze problemen hieronder bespreken.


                                    D O S 2

          Ze weten bij ASCII natuurlijk heel goed dat de mapperpoorten
          niet  mogen  worden  gelezen en  dus wordt  dezelfde methode
          gehanteerd  als voor het 'lezen' van VDP-registers in BASIC,
          telkens  als  er  een  waarde  naar  een  mapperpoort  wordt
          gestuurd wordt  deze waarde  op een  vast adres  bewaard. De
          routine  die 'leest'  welk segment er voor een bepaalde page
          is geselecteerd leest dus gewoon dat RAM adres uit!

          Deze adressen liggen vast en zijn:

          page 0          mapperpoort #FC         #F2C7
          page 1          mapperpoort #FD         #F2C8
          page 2          mapperpoort #FE         #F2C9
          page 3          mapperpoort #FF         #F2CA

          Stel  je  wilt  iets laden  in segment  7. Je  doet dus  OUT
          (#FE),7  en je  roept een  BDOS routine aan. Nu komt DOS2 in
          actie. Die  zet voor de zekerheid de mapper nog even "goed",
          hij  doet daarvoor LD A,(#F2C9) en OUT (#FE),A. De data komt
          dus in een ander segment terecht dan de bedoeling was!

          Dit   is  de   reden  waarom   programma's  die   de  mapper
          rechtstreeks  aansturen  niet  werken  onder  MSX-DOS  2. De
          oplossing voor dit probleem heet MAP. Dit programmaatje werd
          gemaakt door  Henrik Gilvad uit Denemarken. Het is eigenlijk
          heel  simpel, hij  zoekt gewoon  in de DOS2 routines naar de
          instructie LD  A,(#F2C9) en  hij maakt  daar IN A,(#FE) van.
          Hetzelfde   gebeurt  met  #F2CA  en  #F2C8,  met  MAP2,  een
          uitgebreidere versie, wordt ook #F2C7 'gepatcht'.

          Dit is een paardemiddel dat alleen gebruikt mag worden om te
          proberen bestaande  software toch onder DOS2 te laten lopen,
          de mapperpoorten worden nu immers toch weer gelezen!

          Dat dat  niet alleen in de theorie niet mag maar ook niet in
          de praktijk blijkt uit het volgende probleem.


                          E X T E R N E   M A P P E R

          Bij een  externe mapper  gaat het lezen van de mapperpoorten
          op de Japanse MSX2+ en MSX turbo R computers niet goed! De 3
          hoogste  bits  zijn  namelijk altijd  "1"! Dit  is de  reden
          waarom  Digital KC  in de  advertentie voor zijn megamappers
          zegt dat ze niet goed werken op MSX2+ en MSX turbo R.

          Omdat het  alleen de  bovenste 3  bits zijn gaat het bij een
          mapper  tot 512  kB wel goed, maar dit praktijkgeval bewijst
          dat je de mapperpoorten dus echt niet mag lezen!


                            D E   O P L O S S I N G

          Op Sunrise  Magazine #12 schreef ik nog dat MAP de oplossing
          was,  maar na een brief van Henrik Gilvad weet ik wel beter.
          De enige  goede oplossing  is om net als DOS2 de waardes die
          je  naar de  mapperpoorten schrijft  te bewaren  zodat je ze
          niet  hoeft  te lezen.  Als je  dat onder  DOS2 op  dezelfde
          adressen doet  als waar  DOS2 dat  doet, dan  is het  meteen
          compatible  met DOS2  en zal  je programma daaronder werken.
          Onder DOS1 kun je die adressen beter niet gebruiken omdat ze
          zich bevinden  in het  systeemgebied van de BDOS. Onder DOS1
          gebruik  je dus andere adressen dan onder DOS2 om de waardes
          op te slaan.

          Onderstaande source  bevat routines ReadFE, WriteFE, ReadFC,
          etc.  Gebruik  deze  routines  voortaan  in  plaats  van OUT
          (#FE),A respectievelijk IN A,(#FE). Het is slechts een klein
          beetje  langzamer en  de voordelen zijn duidelijk: het werkt
          onder alle omstandigheden op alle computers!


          ; MAPPER.GEN
          ; Standaard mapperroutines
          ; Werken onder DOS1 & DOS2
          ; Compatible met DOS2
          ; Voor eerste gebruik InitMapRouts aanroepen
          ; Niet rechtstreeks de mapper aansturen, alles
          ; via deze routines doen!!!

          ; Door Stefan Boer
          ; Sunrise Special #6
          ; (c) Stichting Sunrise 1994

          ; Init mapper routines
          ; Aanroepen voor gebruik van Read/Write routines!

          InitMapRouts:     LD    A,(#F313)
                            AND   A
                            RET   Z                 ; DOS1

                            LD    HL,#F2C7          ; DOS2
                            LD    (ReadFC+1),HL
                            LD    (WriteFC+1),HL

                            INC   HL
                            LD    (ReadFD+1),HL
                            LD    (WriteFD+1),HL

                            INC   HL
                            LD    (ReadFE+1),HL
                            LD    (WriteFE+1),HL

                            INC   HL
                            LD    (ReadFF+1),HL
                            RET

          ; ruimte voor opslag onder DOS1

          StoreFF:          DB    0
          StoreFE:          DB    1
          StoreFD:          DB    2
          StoreFC:          DB    3


          ; Read
          ; In : -
          ; Uit: A = segmentnummer

          ReadFC:           LD    A,(StoreFC)
                            RET

          ReadFD:           LD    A,(StoreFD)
                            RET

          ReadFE:           LD    A,(StoreFE)
                            RET

          ReadFF:           LD    A,(StoreFF)
                            RET

          ; Write
          ; In : A = segmentnummer
          ; Uit: -
          ; Er is net als bij DOS2 geen WriteFF want die mag je
          ; niet veranderen!

          WriteFC:          LD    (StoreFC),A
                            OUT   (#FC),A
                            RET

          WriteFD:          LD    (StoreFD),A
                            OUT   (#FD),A
                            RET

          WriteFE:          LD    (StoreFE),A
                            OUT   (#FE),A
                            RET


          Vergeet  niet  eerst  even InitMapRouts  aan te  roepen, die
          zorgt  ervoor dat  onder DOS2  de gebruikte  adressen worden
          gewijzigd in  de adressen die DOS2 zelf ook gebruikt. Let er
          verder  op  dat  de  mapper  nergens  in  je programma  meer
          rechtstreeks  wordt aangestuurd,  ook niet bijvoorbeeld door
          de MoonBlaster  replayer. Ik zal nog aan Remco vragen of hij
          een  nieuwe  versie  van  de  replayer released  waarin deze
          mapperroutines   zijn  ingebouwd,   maar  je  kunt  dit  ook
          eenvoudig zelf aanpassen.


                               T E N S L O T T E

          Wees  nu niet eigenwijs en gebruik deze routines gewoon, ook
          als je  zelf geen  DOS2 hebt. Steeds meer MSX'ers hebben een
          harddisk   en  vaak  zal  het  doorslaggevend  zijn  bij  de
          beslissing om  je produkt  al dan  niet te  kopen of  het op
          harddisk  kan worden  ge�nstalleerd. E�n  van de voorwaarden
          daarvoor  is  dat  het  onder DOS2  werkt. Bovendien  zal je
          programma op  een MSX2+  of turbo R met externe mapper van 1
          MB of meer niet werken als je dat geheugen ook daadwerkelijk
          gaat gebruiken.

          Als  je in het systeemgebied van DOS2 gaat rondneuzen zul je
          overigens zien  dat DOS2 exact dezelfde routines gebruikt om
          de  mapper aan  te sturen! Je kunt deze routines echter niet
          handig gebruiken  omdat ze  niet op een vast adres staan. Je
          kunt MAPPER.GEN op de disk vinden.

                                                          Stefan Boer
