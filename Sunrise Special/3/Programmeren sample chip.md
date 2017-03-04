       P R O G R A M M E R E N   S A M P L E C H I P 
                                                                
          
          In deze tekst ga ik beschijven hoe je samples gemakkelijk in 
          BASIC of ML kunt gebruiken. Deze chip zit in de Music Module 
          van  Philips,  omgebouwde  Toshiba modules  en de  standaard 
          MSX-AUDIO cartridges van Panasonic.
          
          
                      O U T   C 0 H   E N   O U T   C 1 H 
          
          Met  deze twee  OUT's wordt de hele MSX-AUDIO bestuurd. Door 
          een getal  op poort  &HC0 te  zetten kies je een register en 
          door  op poort  &HC1 een  getal te zetten stuur je dat getal 
          naar het eerder gekozen register.
          
          
          Een overzicht van de registers:
          
          Register 1: Niet gebruiken
          
          Register 2: Timer 1 preset. Beginwaarde N1 van Timer 1.
          
          Register 3: Timer 2 preset. Beginwaarde N2 van Timer 2.
          
          Register 4: Timer en Flag control.
                      Bit 0: "1" starten en "2" stoppen Timer 1.
                      Bit 1: "1" starten en "2" stoppen Timer 2.
                      Bit 2: niet in gebruik.
                      Bit 3: "1" maskeert Buffer Ready Flag.
                      Bit 4: "1" maskeert End of Sample Flag.
                      Bit 5: "1" maskeert Timer 2 Flag.
                      Bit 6: "1" maskeert Timer 1 Flag.
                      Bit 7: "1" reset alle Flags.
          Bij het  inschakelen van  dit register  worden bit 3 en 4 al 
          gemaskeerd.  Bij  het  invoeren van  de waarde  &H78 in  dit 
          register  worden alle  Flags gemaskeerd  en bij het invoeren 
          van  &H80  worden alle  Flags gereset  behalve de  al eerder 
          gemaskeerde  Flags.  Voor  meer  informatie  zie  het Status 
          register.
          
          Register 7: ADPCM besturing.
                      Bit 0: "1" reset ADPCM functies.
                      Bit 1, 2, 3: niet in gebruik.
                      Bit 4: "1" herhaalt weergave sample.
                      Bit 5: "1" voor gebruik geheugen in module.
                      Bit 6: "1" opnemen en "0" weergeven.
                      Bit 7: "1" start opnemen of weergeven.
          Bij invoeren  van &HE0  kan er  worden opgenomen en met &HA0 
          weergegeven. Met "1" worden alle bits gereset.
          
          Register 8: Instellingen
                      Bit 0: Moet altijd op "0" worden gezet.
                      Bit 1: Moet altijd op "0" worden gezet.
                      Bit 2: "1" DA en "0" AD conversie.
                      Bit 3: "1" start en "0" stopt DA/AD conversie.
                      Bit 4, 5, 6: niet in gebruik.
                      Bit 7: "1" stelt CSM in.
          Indien ADPCM mode gewenst is moeten alle bits "0" zijn.
          
          Registers 9 en 10: Startadres Sample.
          Er  is  standaard  256 kB  in blokjes  van 32  kB RAM  om te 
          samplen.  Dus  zijn  er  256/32  =  8  van &H2000  bytes. De 
          adressen  lopen  dus  van  &H0000  tot  &H1FFF.  Als er  een 
          geheugenuitbreiding  aanwezig is,  bijv. 1024 kB en er wordt 
          gesampled met een goed prgramma dat ook die 1024 kB gebruikt 
          zijn er  1024/32 =  32 van &H8000. De adressen lopen dan van 
          &H0000  tot &H7FFF.  In register  9 komt  de low-byte  en in 
          register 10 komt de high-byte. Dus als het beginadres &H03FF 
          is dan zet je in register 9 &HFF en in register 10 &H03.
          
          Registers 11 en 12: eindadres.
          Zie registers  9 en  10 alleen nu voor het eindadres. Bij 32 
          kB  is dat  &H1FFF. In  register 11  komt de  low-byte en in 
          register 12 komt de high-byte.
          
          Registers 13 en 14: prescaler.
          In  deze  registers komt  een getal  N te  staan waarmee  de 
          samplefrequentie Fs  wordt bepaald door de klokfequentie van 
          3580 kHz te delen door N. De maximale samplefrequentie is 16 
          kHz en de minimale samplefrequentie is 1.8 kHz.
          
          Opname Frequentie  |     N     | Register 13  | Register 14
          -------------------+-----------+--------------+-------------
            1.8 kHz          |   &H7FF   |    &HFF      |      7
            3   kHz          |   &H4A9   |    &HA9      |      4
            6   kHz          |   &H254   |    &H54      |      2
           10   kHz          |   &H166   |    &H66      |      1
           16   kHz          |   &H0E1   |    &HE1      |      0
          ------------------------------------------------------------
          
          Register 15: ADPCM-data.
          Dit register is een buffer voor de ADPCM data van en naar de 
          computer.  Dit register  kan geschreven  en gelezen  worden. 
          Elke byte bevat 2 ADPCM samples die maar 4 bits (ÇÇn nibble) 
          bevatten. Als  de high-nibble  sample nummer  "n" bevat  dan 
          bevat de low-nibble het sample nummer "N+1"
          
          Registers 16 en 17: ADPCM weergave.
          Deze  registers bevatten  de snelheid van de ADPCM weergave, 
          oftewel de afspeelfrequentie.
          
          Weergave frequentie   |   register 16   |   register 17
          ----------------------+-----------------+-------------------
            1.8 kHz             |      &HF4       |      &H08
            3   kHz             |      &H5D       |      &H0F
            6   kHz             |      &HBA       |      &H1E
           10   kHz             |      &H36       |      &H33
           16   kHz             |      &HF0       |      &H51
           50   kHz             |      &HFF       |      &HFF
          ------------------------------------------------------------
          
          Register 18: volume weergave ADPCM.
          Met dit register kan het volume van de weergave van de ADPCM 
          worden  ingesteld. Met  &HFF staat  het volume voluit en met 
          &H00 is  het geluid  op zijn  zachtst. Dit register kan goed 
          gebruikt worden voor fade ins en outs.
          
          Registers 19 en 20: ADPCM.
          Deze   twee   registers   bevatten   de   ADPCM-codering  en 
          -decodering.
          
          Registers 24 en 25: Output control.
          Met  deze registers  kan het geluid aan- en uitgezet worden. 
          In  register  moet  altijd  "8"  staan  om outputpoort  3 te 
          kiezen. Wanneer  in register  25 het  getal "8"  wordt gezet 
          gaat  het geluid  aan en wanneer er "0" wordt gezet gaat het 
          geluid uit.
          
          Register 26: PCM-data.
          Bufferregister voor AD en DA conversie, de data wordt hierin 
          opgeslagen in 2-complement vorm.
          
          
                      H E T   S T A T U S R E G I S T E R 
          
          Dit  register  kan gelezen  worden zonder  dat er  eerst een 
          register moet worden aangeroepen.
          
                  Bit 0: dit bit is "1" tijdens ADPCM opname/weergave.
                  Bit 1 en 2: zijn niet in gebruik en staan op "1".
                  Bit 3: deze Flag wordt op "1" gezet als de 
                         datatransport klaar is.
                  Bit 4: Als het opnemen of afspelen klaar is wordt 
                         dit bit gezet.
                  Bit 5: Gezet als Timer 2 "0" is.
                  Bit 6: Gezet als Timer 1 "0" is.
                  Bit 7: Dit bit wordt gezet als bits 3, 4, 5 en 6 
                         gezet zijn, er wordt dan een interrupt 
                         veroorzaakt. De computer kan dan zien of de 
                         interrupt veroorzaakt werd door de 
                         soundprocessor.
          
          
                            H E T   I N V O E R E N 
          
          Dan nu  hoe je al deze registers kunt programmeren. Zoals ik 
          al eerder heb geschreven doe je dit met OUT &HC0,register en 
          met OUT &HC1,waarde die naar het register moet.
          
          Dus als je het volume voluit wilt zetten doe je:
                  OUT &HC0,18:OUT &HC1,&HFF
          
          Zelf  sample ik  het liefst in MoonBlaster en daarna schrijf 
          ik die  sample(s) weg  om ze vervolgens in BASIC in te laden 
          met de MB BASIC routines.
          
          Nu begrijp je misschien waarom het makkelijk is de sample(s) 
          aan  te kunnen  sturen. Veel mensen die alleen de een sample 
          aan willen  sturen maken een songfile waarbij ze dan precies 
          timen  wanneer zo'n  sample afgespeeld  moet worden.  In het 
          softwaremenu staat  een programma  om samples op te nemen en 
          af te spelen.
          
                                                        Bart Schouten
          