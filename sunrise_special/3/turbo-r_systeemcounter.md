       T U R B O   R   S Y S T E E M C O U N T E R 
                                                               
          
          Beloofd is  beloofd, ik  zou nog een tekst schrijven over de 
          systeemcounter van de turbo R.
          
          
                     W A A R O M   E E N   C O U N T E R ? 
          
          Waarom hebben de ontwerpers van de turbo R er een counter in 
          gezet?  Het  is  bij  software vaak  nodig om  zo nauwkeurig 
          mogelijk tijdsduren te kunnen meten. Bijvoorbeeld om iets te 
          timen.  Bij  de  Z80  werd hiervoor  gewoon de  verwerkings- 
          snelheid van  de Z80  zelf gebruikt.  Bij de  Z80 staat  het 
          namelijk  van elke  instructie vast  hoe lang  de Z80 erover 
          doet,  zodat   je  precies  kunt  uitrekenen  hoe  lang  het 
          uitvoeren van een bepaald stukje code duurt.
          
          Bij  de  R800  is  dat  anders, zie  het artikel  op Sunrise 
          Special  #1. De  R800 wordt  bijvoorbeeld sneller  als alles 
          zich binnen  dezelfde 256 bytes afspeelt, en bovendien is de 
          refresh  anders  geregeld.  Bij  de  Z80  wordt  er na  elke 
          instructie  een refresh  gedaan, bij  de R800 is dat 32 keer 
          per seconde.
          
          Door dit  alles is tijdmeting op basis van klokpulsen bij de 
          R800  niet nauwkeurig.  Vandaar dat er een systeemcounter in 
          is gezet.
          
          
                                 1 6   B I T S 
          
          De  systeemcounter is  eigenlijk heel  simpel, gewoon een 16 
          bits tellertje  dat op twee I/O poorten zit en hardwarematig 
          wordt verhoogd (dus onafhankelijk van of bijv. de interrupts 
          aan of uit staan, of er wordt geladen van disk, etc.).
          
          De  frequentie van  de counter,  en dus  van de low byte, is 
          precies  ÇÇn  veertiende deel  van de  snelheid van  de Z80A 
          (3.58 MHz),  ofwel 255682 Hz. De counter wordt dus elke 3.91 
          microseconde (miljoenste seconde) verhoogd.
          
          De high byte loopt uiteraard op 1/256 van deze snelheid, dat 
          is  999  Hz.  De  high  byte  heeft  dus een  frequentie van 
          ongeveer 1 kHz.
          
          
                             I / O   P O O R T E N 
          
          De counter  zit op de I/O poorten &HE6 en &HE7, waarbij &HE6 
          de  low byte  is en &HE7 de high byte. Voor gebruik in BASIC 
          zijn deze  counters gewoonweg  te snel, maar voor gebruik in 
          ML zijn ze uitermate geschikt.
          
          De  counter  kan  worden  gereset  (op  0  gezet)  door  een 
          willekeurige  waarde te schrijven naar poort &HE6. Schrijven 
          naar poort &HE7 heeft geen enkele invloed.
          
          Beide poorten kunnen worden gelezen. Het gebruiken van beide 
          poorten  tegelijk  heeft  een  klein  probleempje,  ze  gaan 
          namelijk  zo  snel  dat  het  kan  gebeuren dat  de counters 
          verspringen net  nadat je  de eerste  hebt gelezen.  Stel je 
          lees  poort &HE6 het eerst en daar staat een 255. De kans is 
          dan groot  dat de  counter vlak daarna wordt verhoogd, en je 
          leest dan bijvoorbeeld de waarde 18 bij poort &HE7. Dit moet 
          dan echter 17 zijn!
          
          
                            T O E P A S S I N G E N 
          
          Er  zijn natuurlijk vele toepassingen, maar de belangrijkste 
          is wel  bij samplen.  De samplechip heeft trouwens z'n eigen 
          counter, die echter maar 2 bits is en wordt gebruikt voor de 
          timing  bij de  _PCMPLAY/_PCMREC commando's  in BASIC.  Deze 
          counter loopt  op de  bekende 15.75 kHz, de snelheid bij een 
          _PCMxxx comando met snelheidsparameter 0.
          
          Als we een sample play of record routine in ML willen maken, 
          kunnen  we beter  van de  low byte  van de  16 bits  counter 
          gebruik maken, omdat die veel nauwkeuriger is.
          
          De counter  wordt ook  veel gebruikt bij afspeelroutines van 
          muziek  voor MSX-MUSIC  en MSX-AUDIO. Bij deze chips moet er 
          namelijk bij  het beschrijven  van de  registers steeds even 
          gewacht  worden. Routines  die voor  de Z80  zijn geschreven 
          gebruiken dan  bijvoorbeeld twee  keer EX  SP,(HL), maar dat 
          duurt bij de R800 te kort waardoor het mis gaat.
          
          
                             M O O N B L A S T E R 
          
          Als  voorbeeld hier even een klein stukje van de MoonBlaster 
          replay. De  replay kijkt  zelf of  hij onder R800 draait, en 
          indien   dat  het  geval  is  zal  hij  aangepaste  routines 
          gebruiken om  de registers  van de MSX-MUSIC en MSX-AUDIO te 
          beschrijven. Als voorbeeld hier MSX-MUSIC.
          
          PAROUT: EX    AF,AF
                  CALL  TRBWT     ; wacht indien nodig
                  LD    A,C
                  OUT   (&H7C),A  ; selecteer register
                  IN    A,(&HE6)  ; lees counter
                  LD    (RTEL),A  ; sla waarde op
                  EX    AF,AF
                  OUT   (&H7D),A  ; schrijf waarde
                  RET
          
          TRBWT:  PUSH  BC
                  LD    A,(RTEL)  ; stand counter bij vorige keer
                  LD    B,A
          TRBWL:  IN    A,(&HE6)  ; lees counter
                  SUB   B
                  CP    7         ; al minimaal 7 verhoogd?
                  JR    C,TRBWL
                  POP   BC
                  RET
          
          Door de  routine TRBWT  (TuRBo WaiT) wordt er gewacht totdat 
          de  counter sinds  de vorige  keer dat  er een register werd 
          beschreven  7  maal is  verhoogd. De  huidige waarde  van de 
          counter wordt in (RTEL) opgeslagen voor de volgende keer dat 
          deze routine wordt aangeroepen.
          
          
                                   B A S I C 
          
          In  BASIC kan de counter ook worden gebruikt, er is namelijk 
          een  speciaal  BASIC commando  voor. Dit  is echter  wel een 
          simpel commando, het kan alleen maar wachten! Wat een humor. 
          Je koopt  een snelle  computer en  ÇÇn van  de nieuwe  BASIC 
          commando's  dient om  te wachten!  Enfin, de  syntax van het 
          nieuwe commando is:
          
          _PAUSE(x)
          
          waarbij  voor  x  een  getal tussen  1 en  32767 kan  worden 
          ingevuld. De  computer zal  dan wachten  totdat de high byte 
          van  de counter  (frequentie ca. 1 kHz) het opgegeven aantal 
          stapjes is  verhoogd. Hiermee kunnen we de dus tot op 1/1000 
          seconde nauwkeurig wachten. Bijvoorbeeld:
          
          _PAUSE(1000)
          
          wacht precies een seconde (N.B. Eigenlijk moet dit 999 zijn, 
          want zoals we weten loopt de high byte van de counter op 999 
          Hz.)
          
          Meer valt  er eigenlijk niet over de counter te zeggen. Voor 
          langere  tijdmeting is  hij niet  geschikt, want hij doet er 
          maar ruim een kwart seconde over om "klokkie rond" te gaan.
          
                                                          Stefan Boer
          