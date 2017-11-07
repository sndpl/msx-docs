          
                         B U G J E   I N   M E M M A N 
                                                        
          
          Deze  tekst heb  ik gedownload  uit BBS  Roefsoft. Omdat  ik 
          aanneem  dat  dit  ook  interessant  is  om  te  weten  voor 
          niet-MST'ers, plaats  ik hem.  Noot: de  tekst is  als brief 
          gericht aan het MST.
          
                                                        Kasper Souren
          
          
          Geachte programmeurs,
          
          Bij  het  maken  van  TSR's  ben ik  op een  vreemd probleem 
          gestuit.  Misschien  ligt  het  aan  mij, maar  daar ben  ik 
          ondanks  dat ik  mijn .GEN  file flink  heb doorzocht,  niet 
          zeker van.
          
          Ik  krijg ik  te maken  met vastlopers  als ik  mijn TSR  of 
          ScrFade dubbel  probeer te laden. Misschien lopen ook andere 
          TSR's  vast,  ik  heb alleen  bij deze  twee dit  opgemerkt. 
          Uiteraard   kijken   deze   TSR's   of   ze  zelf   al  zijn 
          ge�nstalleerd.
          
          
                        G E D R A G   V A N   M I D I X 
          
          MIDIX  (mijn TSR)  is een  TSR die  50 keer  per seconde het 
          Philips-keyboard afscant,  omzet naar  MIDI data  (er kunnen 
          verschillende  "zones"  worden  gedefineerd),  en  deze naar 
          MIDI-out  stuurt  (nog  niet gebufferd).  Alle MIDI-IN  data 
          wordt  in een  ISR (Interrupt service routine) gebufferd, en 
          deze buffer  50x per  seconde door  de TSR  geleegd. De data 
          wordt  multitimbraal over  de 9 Module-kanalen gespeeld. E�n 
          MIDI-kanaal wordt gebruikt voor drumsamples.
          
          Overgens werkt  de TSR  prima, het  reloceren en afbuigen in 
          DOS  werkt nu  perfekt, alleen wilde ik (in de toekomst dus) 
          als er dubbel ge�nitialiseerd werd, checken of de oude MIDIX 
          onder DOS  was ge�nitialiseerd, en zo niet, de DOS-interrupt 
          alsnog  goed afbuigen. De adressen in de heap kunnen dan via 
          een TSRCall  naar de  oude MIDIX worden opgevraagd. Maar dat 
          kan nu dus niet.
          
          
                   W A N N E E R   L O P E N   Z E   V A S T 
          
          ScrFade en  MIDIX lopen  allebij vast  als ze voor de tweede 
          keer  wordt geinstalleerd,  en MIDIX.TSR  reeds was geladen. 
          Overgens  heb  ik  ook  eens een  redirection error  met wat 
          vreemde tekens  gezien nadat ik in BASIC MIDIX WEL twee keer 
          heb  kunnen laden,  het kan dus zijn dat ik of MemMan ergens 
          in het  geheugen heeft zitten knoeien, terwijl hij daar geen 
          recht  had. Overgens  gebruikte ik  MemMan 2.31 en 2.42 (uit 
          Waterland), DOS  2.22 van  MK, en een 1024 kB Memmory mapper 
          in  slot 1,  op een VG 8235/00, niet dat wat uitmaakte, want 
          ook onder  MSX-DOS 1 zonder MemMapper, ging het dubbel laden 
          fout.
              
          
                               S Y M P T O N E N 
          
          ScrFade: Laat  in een  gruwlijk tempo  de kleuren  wisselen. 
          Beeld niet om aan te zien, TV lijkt kapot.
          Midix: Laat een vreemd gekraak uit de module komen, hier heb 
          geen verklaring voor.
          
          Dit  duidt er volgens mij op dat de interrupt continue wordt 
          aangeroepen, TsrMan  staat continue  te racen. Eigenlijk zou 
          dan ScrFade in no-time de tijdslimiet gehaald hebben, en dan 
          het scherm zwart moeten HOUDEN, toch?
          
          Mijn intialisatie routine ziet er zo uit:
          
          Init:
              ld      b,GetMemManEnt     ;Ask for MemMan entry
              ld      de,256*'M' + Info  ;Call the MemMan info
                                         ; function
              call    ExtBio             ;Through the ExtBio hook
              ld      (MemManEntry+1),hl ;Save the MemMan entry
                                         ; address
                                         ;
              ld      hl,TTsrName        ;Pointer to TSR name-string
              MemMan  GetTsrID           ;See if this TSR already
                                         ; exists
              jp      nc,InitDouble      ; Yes, => Double installed
                                         ;  error
          
          .comment /
          Initialisatie routine  die Heap-ruimte aanvraagt, een stukje 
          in  de de  initialisatieroutine aanpast, naar de Heap LDIRt, 
          FD9Ah  afbuigt,  en,  indien  na  het  bekijken van  00038h, 
          bepaalt of  de TSR  vanuit een DOS-omgeving werd geladen. Zo 
          ja,  wordt de  JP instructie waarnaar 0038h Jumpt, afgebogen 
          naar een ISR, die ook in de heap staat. /
              
          ; einde initalisatie
              ld      de,OK etc
              ld      a,2
              ret
          ;
          InitDouble:
              ld      de,TDouble
              ld      a,3
              ret
          ;
          TDouble:
              db    "You're not under MSX-DOS, I can't refresh "
              db    "DOS-MIDI interrupt routine."
              dw    CrLf
              db    "Under basic, MIDI-interrupts are almost always "
              db    "fast enough."
              dw    CrLf
          TTsrName: TsrName
              db    " was already installed. Old MIDIX still exists."
              dw    CrLf
              db    0
          
          Dit  zou  bij  een dubbele  installatie probleemloos  moeten 
          werken.  Vreemd  is  dat  ik  na het  eerste keer  laden van 
          MemMan,  probleemloos iedere willekeurige TSR kan laden, tot 
          een Segment full error volgt.
          
          
                M O E I L I J K   D O E N   I K   D E   H E A P 
          
          Op  een MIDI-int  moet de computer elke 1200 cycli (3.58 MHz 
          Z80) kunnen  reageren. Inschakelen  van een  TSR-segment zal 
          dan  te  lang  duren,  dus  heb  ik  in de  Heap een  stukje 
          code+buffer  gereloceerd. Als  er te  weinig Heap-ruimte is, 
          wordt de buffer automatisch kleiner gemaakt.
          
          Onder  DOS haal  ik deze snelheid nog steeds niet, omdat DOS 
          altijd  de   BIOS  eerst  aanschakelt,  voordat  hij  00038h 
          aanroept.  Hiervoor heb ik de DOS-interrupt JP instructie in 
          page  3  omgebogen naar  mijn routine  in de  heap, waar  nu 
          uiteraard  het  een  en  ander  gePUSHt  moet worden.  Nadat 
          vastgesteld is  dat het  een MIDI  interrupt is,  wordt deze 
          afgehandeld, en zonder pardon weer EI en RET gegeven, dit om 
          de  VDP-interrupt in  het BIOS  te vermijden.  Mocht er  een 
          MIDI- en  VDP-interrupt tegelijk  plaatsvinden, dan  komt de 
          VDP  vlak na  EI RET  met een  interrupt voor de VDP, waarna 
          MIDIX hem gewoon via het BIOS laat lopen.
          
          
                                    I D E E 
          
          Misschien  kunnen  jullie  in  MemMan  ook  dit soort  hooks 
          implementeren,  voor  snelle  RS232-  en modem-buffers.  Een 
          nadeel  dat  ik  nu  heb,  is  dat  ik  in een  Kill-routine 
          programma's die  na mij  ge�nstalleerd zijn  en ook  op mijn 
          manier  te  werk  zijn  gegaan,  mijn  Heap-ruimte  niet mag 
          teruggeven,  omdat zij  er zonder pardon mijn heapruimte als 
          trampoline   gebruiken.   En   ik   kan  er   zeer  moeilijk 
          achterkomen, waar  zij zich afzetten om bovenop mijn heap te 
          springen...
          
          Misschien  kan   ik  een  FASTINT.TSR  maken,  wat  dan  een 
          aanvulling  op MemMan is. FastInt zou ook in Mst TsrUtils of 
          worden ingebakken.  Deze zou  dan de Fast-Interrupt routines 
          kunnen  beheren, aankoppelen en afkoppelen, als ze even niet 
          nodig zijn.  Misschien kan  hij ook helpen hij het reloceren 
          van  routines in  de heap en aanvragen van bufferruimte. (Oh 
          ja, is  een TSR  nou een Hij of een Zij? [Nvdr. Als het niet 
          bekend is, is het een Hij!]
          
          Ook  zouden parameters bij TL, een teruggeef-tekstje bij TK, 
          ROMs  als  segment-code,  een  Memory-in-bytes-manager  voor 
          nauwkeuriger geheugenbeheer  (Pop-up database,  scherminhoud 
          opslag)  en een  debugging-tool voor  TSR's handig zijn. Een 
          functie voor  het opvragen  van het  segment van een TSR zou 
          hierbij ook wonderen doen.
          
          Helaas werkt  MSX-Debugger v2.0 (prachtig...) helaas niet zo 
          makkelijk  met MemMan.  Heb ik  eindelijk via een TsrCall en 
          GetCurSeg het  Segment-no van  mijn TSR,  voer een USE1 uit, 
          luistert  MSXDEBUG niet  meer... Ook  een breakpoint  in een 
          ongebruikte hook  zetten, die  ik aanroep  vanuit mijn  TSR, 
          werkt niet...
          
          Veel  geluk/succes/(nog prettige  feestdagen? of zijn die al 
          geweest?) met  jullie prachtige  MemMan project,  van jullie 
          lastige beller,
          
                                                     Mark-Jan Bastian
