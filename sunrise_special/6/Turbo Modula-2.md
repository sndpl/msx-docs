                          T U R B O   M O D U L A - 2


          Op de  disk staat  MODULA2.PMA, met daarin de Turbo Modula-2
          compiler  van Borland!  Nu vraag je je misschien af: "Is dat
          niet illegaal?"  Volgens Borland  zelf in  ieder geval niet!
          Borland  ondersteunt hun  CP/M software niet meer, omdat dat
          betekent dat  ze ook service en updates moeten geven. Daarom
          doen  ze net  of het niet bestaat, en als je toch vraagt hoe
          je eraan  kunt komen,  krijg je te horen dat je het maar van
          iemand moet kopi�ren!

          Enfin, nadat  we dit  (waargebeurde) verhaal  hadden gehoord
          besloten  we na  enig overleg om de Modula-2 compiler gewoon
          op de Special te zetten, om zo de drempel om met Modula-2 te
          beginnen zo laag mogelijk te houden.


                                   T U R B O

          Net  als  bij Turbo  Pascal zijn  er een  aantal verschillen
          tussen  Turbo  Modula-2 en  standaard Modula-2.  Het belang-
          rijkste verschil is dat de read/write statements net als bij
          Turbo Pascal  maar in  tegenstelling tot  standaard Modula-2
          wel tot de taal behoren!

          Bij  standaard Modula-2  zitten de  read/write statements in
          losse modules,  wat betekent dat je voor elk type een aparte
          read/write nodig hebt!

          In standaard Modula-2 kun je bijvoorbeeld zoiets tegenkomen:

                  WriteString("Lengte: ");
                  WriteReal(Lengte);
                  WriteLn;

          In Turbo Modula-2 is het net als bij Turbo Pascal:

                  WRITELN("Lengte: ", Lengte);

          Wat  toch  een  stuk  fijner  is!  De  onhandige  read/write
          statements van  Modula-2 is zelfs de belangrijkste reden dat
          Turbo Pascal veel populairder is dan Modula-2! Maar gelukkig
          hebben  wij Turbo  Modula-2 voor MSX en hebben wij daar geen
          last van.

          Modula-2  heeft geen  stringtype, je moet zelf strings maken
          met ARRAY  OF CHAR. Dit is bij Turbo Modula-2 nog steeds zo,
          wel  kunnen zulke  ARRAYs OF  CHAR nu  worden vergeleken  en
          geassigned. Het  onderstaande kan  in Turbo Modula-2 dus wel
          en in standaard Modula-2 niet:

                  String := "Modula-2";
                  IF String = "Turbo Pascal" THEN
                          WRITELN("Dit kan niet!");
                  END;

          Er  zijn nog  meer verschillen met standaard Modula-2 (zoals
          meerdimensionale open-array  parameters), maar een standaard
          Modula-2  source zal  normaal gesproken gewoon kunnen worden
          gecompileerd. Let  op: de  standaardmodule Storage  heet bij
          Turbo Modula-2 STORAGE! In hoofdletters dus, let daar op!


                             D E   C O M P I L E R

          De  Turbo Modula-2  compiler is  een ge�ntegreerd pakket met
          ingebouwde editor  en 'library manager'. Als de compiler een
          fout tegen komt in de source springt hij automatisch naar de
          editor,  als je  de fout  hebt verbeterd gaat het compileren
          gewoon door!

          Als  je een  source compileert wordt er nog geen uitvoerbare
          .COM  file  van  gemaakt,  je  moet  hem daarvoor  eerst nog
          linken.

          Eigen ML-routines  kunnen in  binaire vorm  (.COM, moet  wel
          relocatable   zijn!)  of   als  relocatable   (.REL)  worden
          toegevoegd! Dit  is bij  grotere routines  veel handiger dan
          bij Turbo Pascal, waarbij met pure hex-codes wordt gewerkt.


                                    M E N U

          Je start Turbo Modula-2 door achter de DOS prompt "M2" in te
          typen. Je komt nu in het hoofdmenu terecht. Je kunt hier een
          keuze maken door de letter in te toetsen die als hoofdletter
          in de menu-optie staat. De opties in het kort:

          Work file       Geef de naam van de huidige werkfile op
          Edit            Ga naar de editor
          Compile         Compileer de huidige source
          Run             Voer een .MCD file uit
          eXecute         Voer een .COM file uit
          Link            Maak Z80-code
          Options         Instellingen wijzigen
          Quit            Verlaat de compiler
          liBrarian       Library manager
          Dir             Directory
          Filecopy        Kopieer file(s)
          Kill            Wis file(s)
          reName          Hernoem file(s)
          Type            Bekijk file

          Het  interne codeformaat van de compiler is .MCD. Als je een
          definition module  compileert wordt  het een  .SYM file, een
          implementation  module  wordt  een  .MCD  file. Je  kunt een
          aantal  .MCD's met  bijbehorende .SYM's  samenvoegen tot een
          library met de librarian.


                               D E   E D I T O R

          De editor  is ingesteld als aanbevolen door Herman Post voor
          de  editor van  Turbo Pascal  in MCCM  61. De  belangrijkste
          toetsencombinaties zijn:

          CTRL-Y          Wis regel
          CTRL-N          Voeg regel tussen
          CTRL-K, CTRL-D  Verlaat editor

          Voor  de overige  combinaties verwijs  ik naar MCCM 61, blz.
          12. De  editor is  niet echt  handig, dus  het is  misschien
          fijner  om  TED  te  gebruiken  voor het  schrijven van  het
          grootste  gedeelte  van  de source  en de  editor alleen  te
          gebruiken   voor  debuggen.   De  toetsencombinaties  kunnen
          eventueel naar eigen smaak worden ingesteld met INSTM2.COM.

                                                          Stefan Boer

