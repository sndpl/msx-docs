                      S U B D I R S   O N D E R   D O S 1


          Tot   nu   toe  was   het  onmogelijk   om  onder   DOS1  in
          subdirectory's te  kijken, laat  staan om  ze te  gebruiken.
          Maar  nu zijn er Japanse programmaatjes die dit wel mogelijk
          maken.  Het  systeem is  geheel compatible  met MSX-DOS2  en
          MS-DOS, dus het is echt ideaal.


                                 W E R K I N G

          Eerst moet  je de  disk die je ermee wilt gebruiken een naam
          geven.  Dit  gebeurt  met LABEL.COM.  Je typt  gewoon achter
          LABEL  de naam in, en de disk wordt zo genoemd. Ik weet niet
          precies waarom  dat nodig  is, maar volgens mij wordt er dan
          een  tabelletje  aangemaakt,  waarin  de  verderop besproken
          subdirectory utils de benodigde info dumpen.

          Als   je  LABEL.COM   niet  eerst   gebruikt,  weigeren   de
          DIR-utility's  om  te  werken.  Je  krijgt  dan een  Japanse
          foutmelding -  die ik eerst niet helemaal snapte ("No volume
          table").


                               C H D I R . C O M

          CHDIR  staat uiteraard  voor CHange DIRectory, en je kunt er
          dus  mee  van directory  veranderen. Als  je geen  directory
          achter CHDIR  opgeeft, wordt de huidige directory afgebeeld.
          Dat  is wel  handig, want  die krijg je niet te zien bij het
          commando DIR.  Gelukkig neemt  DIR de  subdirectory's wel al
          mee  in het  overzicht van  de files,  met erachter  vermeld
          "<DIR>".


                               M K D I R . C O M

          Dit staat  natuurlijk voor MaKe DIRectory, en hiermee kun je
          dus  de directory's  aanmaken. Ook  hier moet je weer gewoon
          achter MKDIR de aan te maken directory opgeven.


                               R M D I R . C O M

          En tenslotte nog ReMove DIRectory, juist ja, het verwijderen
          van - lege - subdirectory's.


                        H O E   K A N   D A T   N O U ?

          Als je even goed nadenkt, is het natuurlijk onmogelijk om al
          deze DIR-utils  te gebruiken.  Als je  immers eenmaal in een
          andere  directory zit,  kun je  de utility's  zelf niet meer
          gebruiken, omdat  die uiteraard  in de  root-dir - de stam -
          staan. En je zou dan dus moeten resetten om terug te keren.

          Hiervoor  heeft de  Japanse programmeur echter een oplossing
          bedacht: in  SETDIR.SYS kun  je de bestanden plaatsen die in
          elke directory komen. Uiteraard dienen tenminste de volgende
          files erin te staan:

                  COMMAND.COM
                  MSXDOS.SYS
                  CHDIR.COM
                  MKDIR.COM
                  RMDIR.COM
                  SETDIR.SYS

          Maar  het is  natuurlijk altijd  handig om andere utility's,
          zoals TED erin te zetten.

          Er zijn  echter nog meer handige programmaatjes die bij deze
          verzameling horen:


                             R A M D I S K . C O M

          Dit  is natuurlijk  een RAMdisk  programma voor DOS1. Het is
          best wel  handig, omdat  het niet  terug naar BASIC gaat, en
          toch  wel  dezelfde  kwaliteiten  heeft  als de  Nederlandse
          RAMdisk  programma's -  behalve als je natuurlijk toch al de
          hele MemMan gebruikt, want dan is RAMdisk 4.00 het beste.

          Bij  RAMDISK.COM  kun je  niet de  hoeveelheid te  gebruiken
          geheugen  opgeven,  maar  wel  welk  geheugen  gebruikt moet
          worden:

          /V : alle VRAM
          /M : alle (main) RAM
               Hierachter kan eventueel het slotnummer opgegeven
               worden, als je maar ��n memory mapper wilt gebruiken
               als RAMdisk. Bijvoorbeeld RAMDISK /M 2-2 voor de memory
               mapper in slot 2.2 (het derde gleufje van de
               slotexpander, als die in slot 2 is gestoken).

          /H : iets met ROM
               Misschien dat het hiermee mogelijk is de ROMdisk van de
               FS-A1GT alsnog te gebruiken onder DOS1, of zoiets. Iets
               anders lijkt me niet zo logisch - maar wie het weet mag
               het me laten weten!

          Verder kun  je nog  met R (dus zonder streepje!) opgeven dat
          de RAMdisk drive A: moet zijn. Dan krijg je dus:

          RAMdisk  FDD1  ( FDD2 )
             A:     B:    ( C: )

          Ook kun je opgeven vanaf welk geheugenadres het RAMdisk zich
          mag plaatsen. Dit doe je als volgt:

                  RAMDISK S xxxx

          Waarbij xxxx het hexadecimale adres is. Bijv. E000.


          Voorbeeld:
                  RAMDISK R /M/V

          Dit  maakt een  RAMdisk aan  die alle aanwezige RAM (VRAM en
          memory mapper) gebruikt, en er dan drive A: van maakt.


                             S E T B O O T . C O M

          Met dit  programma kunnen  bij het opstarten bepaalde dingen
          veranderd  worden. De  bootsector wordt  door dit  programma
          namelijk naar wens aangepast. Maar kijk wel uit hiermee, het
          programma wil  ook meteen  je hele disk formatteren - en dat
          kan best wel pijnlijk zijn, zo leert mijn ervaring hiermee.

          Achter  SETBOOT kun  je de  volgende letters  gebruiken. Het
          NIET gebruiken van deze letters impliceert dus meteen dat de
          omgekeerde handeling gebeurt.

          D : geen [CTRL] indrukken zorgt voor 2
              drives
          K : het KANA lampje aan (alleen bij
              Japanse computers)
          C : het CAPS lampje aan
          H : weer iets met ROM (zie ook bij RAMdisk
              de optie /H)


          Voorbeeld:
                  SETBOOT B:C

          Nu wordt  de bootsector van de disk in drive B: zo aangepast
          dat  het  CAPS  lampje altijd  aangaat, en  dat je  alleen 2
          drives  kunt gebruiken  als je  [CTRL] WEL inhoudt. Het KANA
          lampje gaat nu dus ook niet aan. Dit is best wel handig voor
          mensen met  maar ��n  drive. Die virtuele drive kost normaal
          namelijk  meer dan  1 kB  BASIC ruimte,  en het is lastig om
          altijd [CTRL] ingedrukt te moeten houden bij het opstarten.

                                                        Kasper Souren
