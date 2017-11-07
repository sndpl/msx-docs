              S P R I T E S   I N   D E   P R A K T I J K   ( 2  )


          In  de vorige  keer heb  ik het gehad over het initialiseren
          van  de  sprites  en  het  daad  werkelijk plaatsen  van een
          sprite.  In  deze  aflevering  wil  ik  het  sprite  verhaal
          afsluiten  met  de kleurcodes  voor sprite  mode 2  (MSX2 en
          hoger) en de sprite botsing detectie (beide modes).


                S P R I T E   K L E U R E N   I N   M O D E   2

          In mode 1 wordt het vierde byte in de atribute van een layer
          gebruikt om  de kleur  van een  sprite layer  te definieren.
          Daardoor  is er  slechts 1  kleur in een sprite mogelijk. De
          tweede sprite  mode lost  dit anders op door hier een aparte
          kleur tabel te gebruiken. Hierdoor is het mogelijk om elke Y
          lijn  van een  sprite layer  een andere kleur te geven. Door
          somige  bits  te  zetten,  zijn zelfs  3 kleuren  op 1  lijn
          mogelijk. Door  dat er  een kleur tabel wordt gebruikt wordt
          het  vierde byte  in de  atribute tabel verwaarloosd. Let op
          dat deze byte nog wel aanwezig is, waardoor een sprite layer
          atribute nog steed 4 bytes gebruikt.

          Het basis adres van de kleur tabel wordt opgegeven in r#5 en
          r#11  zoals  in deel  in is  beschreven. Als  deze registers
          worden  beschreven   wordt  automatisch  het  adres  van  de
          atribute tabel gelijk aan de kleur tabel + 512 (&h0200).

          Voor  iedere layer zijn 16 bytes gereserveerd voor de kleur.
          Ook als  er gebruik  wordt gemaakt  van 8x8  sprites! Als er
          16x16 sprites vergroot worden gebruikt, worden er 2 Y lijnen
          met  de zelfde kleur gevuld. Een dergelijke kleuren tabel is
          dus: 16 x 32 = 512 bytes lang.


                             E X T R A   C O D E S

          Samen met  de kleur  waarden kunnen  er ook  nog andere bits
          worden  gezet,  die  speciale effecten  aan de  sprite layer
          geven. Een kleur byte bevat het volgende:

             MSB  b7  b6  b5  b4  b3  b2  b1  b0  LSB
                  EC  CC  IC  0   - Kleur code -

                  EC:     Heeft de  zelfde functie als het EC bit in de
                          atribute  tabel van  sprite mode 1. Als deze
                          "1" is  wordt de  sprite lijn 32 pixels naar
                          links  geschoven. (zie  verdere uitleg, deel
                          1)
                  CC:     Het zogenaamde  prioriteit bit.  Als dit "1"
                          is krijgt   de   sprite   lijn   de   zelfde
                          prioriteit   als   de   voorligende  sprite.
                          Hierbij worden de kleuren  van beide  sprite
                          ge 'OR'ed. Dit zorgt echter  niet  voor  een
                          sprite botsing.
                  IC:     Als deze "1" is,  wordt  een sprite  botsing
                          genegeerd.

          Van elke  Y lijn  van een  sprite kan er een andere kleur en
          code worden opgegeven.

          voorbeeld van het prioriteit bit:
          (met  prioriteit wordt  bedoeld dat  een layer met een lager
          nummer voor een layer met een hoger nummer wordt geplaatst)

          Als de kleur van sprite layer 0 (hoogste prioriteit) kleur 8
          is, en  van sprite layer 1 kleur 4. Bij sprite layer 1 wordt
          het  CC  bit  gezet.  Hierdoor  krijgt  layer  1  de  zelfde
          prioriteit  als  layer  0.  De  kleuren  worden  nu ge'OR'ed
          waardoor er kleur 12 ontstaat waar de patroon bits "1" zijn.
          Hierdoor  heeft deze  sprite lijn  dus 3 kleuren namelijk de
          kleuren 4, 8 en 12.
          Het start adres van de kleuren tabel: (basic adr: &h7400)
                  layer 0:  &h7400        (CC=0), kleur 4
                  layer 1:  &h7410        (CC=1), kleur 12


                         S P R I T E   C O N F L I C T

          Hiermee wordt  het 'botsen'  van sprites  bedoeld. In mode 1
          kan  alleen worden  nagegaan of  er een  sprite conflict  is
          opgetreden, terijl  in mode  2 ook nog de X en Y coordinaten
          van de botsen kan worden opgevraagd.

          Een  conflict onstaat als twee sprites elkaar raken. Hierbij
          wordt wel  gekeken of  beide patroon  bits van de sprite "1"
          zijn  en de kleur hiervan niet transparant is. Dus als beide
          sprites  ook  werkelijk  geconstrueerd  zijn.  Het  volgende
          voorbeeld maakt dit duidelijker:

          sprite 1:       XXXX----        sprite 2:       ////////
                          XXXX----                        ////////
           (8 x 8)        XXXX----                        ////////
                          XXXX----                        ////YYYY
                          --------                        ////YYYY
                          --------                        ////YYYY
                          --------                        ////////
                          --------                        ////////

          Als  deze  sprite als  volgt over  elkaar worden  geplaatst,
          volgt er een conflict:
                          XXXX----
                        //XXXX----
                        //XXXX----
                        //XXXX----
                        //XXZZYY
                        ////YYYY
                        ////YYYY
                        ////YYYY
                        ////////
                        ////////


          Als twee  sprites botsen, dan wordt in status reg. s#0 bit 5
          gezet.  Als  er  in  mode  2 wordt  gewerkt kunnen  de de  X
          coordinaten  worden gahaald uit s#3/s#4, en de Y coordinaten
          uit s#5/s#6.  Om nu uit te zoeken om welke sprites het gaat,
          zou  je zelf een routine kunnen maken die de coordinaten van
          de sprites met elkaar gaat vergelijken.

              MSB b7  b6  b5  b4  b3  b2  b1  b0 LSB
                  ---+---+---+---+---+---+---+---
          s#3     X7  X6  X5  X4  X3  X2  X1  X0
          s#4     1   1   1   1   1   1   1   X8
          s#5     Y7  Y6  Y5  Y4  Y3  Y2  Y1  Y0
          s#6     1   1   1   1   1   1   Y9  Y8


                             R E S T   N O G . . .

          ...de informatie  over de  5e/9e sprite op een gelijke Y as.
          In  mode 1  kunnen er  4 sprites  en in  mode 2  kunnen er 8
          sprites op  een dezelfde  beeldlijn worden  vertoond. Als er
          meer  sprites worden neergezet wordt bit 6 van s#0 gezet, en
          bevatten b0-b4 het 5e/9e sprite nummer.

          Een 5e/9e  (en hoger)  sprite wordt dan niet weergegeven. De
          meeste software programmeurs (voor al in spellen) hebben dit
          opgelost  door het  alom bekende  'knipper' effect. Hierdoor
          wordt bijvoorbeel  de 5e/9e  sprite verwisseld  met sprite 1
          waardoor  deze snel  om de beurt worden weergegeven. Op deze
          manier zijn de sprite toch allemaal zichtbaar.


          Als  het  goed  is,  is op  deze special  een assembly  file
          aanwezig met de naam:
                          "SPRITES .asc"

          Deze bevat  diverse routines  om de  sprite basis adresen te
          zetten en/of te berekenen, plus nog enkele handige routines.
          In  die routines worden ook subroutines gebruikt, die u zelf
          kunt bijvoegen (o.a. reken routines).

          Veel spritefun!
                                                 R�man van der Meulen
