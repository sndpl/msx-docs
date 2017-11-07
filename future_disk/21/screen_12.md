 Dhr. Keizer verheldert onze visie over screen 12

  WHAT THE XXXX IS SCREEN12?

    ����
    ����
    ����
    ����


 Ik was een tijdje geleden bezig met de 2+ VDP. Omdat lang niet
 alle omgebouwde 2-plussen ook 2 plusROMs hebben,  mag dit dus
 allemaal niet via de BIOS. De VDP direct aansturen dus.
 Nu had ik eigenlijk geen flauw idee hoe dit allemaal moest
 omdat ik geen data heb over de V9958. Dus, de telefoon maar
 even gepakt, want dan ben je daar immer zo achter..... Niet
 dus!!! Het aansturen van de V9958 schijnt echt een probleem
 te zijn want zelfs de grote computer wizzzzkidzzz van MSX-end
 Nederland konden me nog niet eens vertellen hoe ik screen 12
 aan moest zetten.  Logisch was geweest om 12 in A (LD A,12)
 en dan een call naar CHGMOD... NO ABSOLUTELY NOT! Waarom
 weet ik niet maar dat was geloof ik een idee want er gebeurt
 gewoon niets, maar wat dan wel?(NvdR: mijnheer Keizer, U
 bent niet helemaal duidelijk meer)
 Als eerst moet je blijkbaar screen 8 aanzetten, om daarna
 "08" te zetten in register 25. Dus:

 SCRN12: LD   A,08
         CALL CHGMOD     
         DI
         LD   A,08
         OUT  ($99),A
         LD   A,128+15
         OUT  ($99),A
         EI
         JP   ??????

 Dit kleine stukje source is genoeg  om screen 12 aan te zetten
 zonder dat  er speciale  2+ BIOS  routines worden  gebruikt...
 Om te kijken of er wel een V9958 aanwezig is,  is het volgende
 stukje source nodig:

 V9958?: DI
         LD A,$01
         OUT ($99),A
         LD A,$8F
         OUT ($99),A
         PUSH AF
         POP AF
         IN A,($99)
         LD D,A
         XOR A
         OUT ($99),A
         LD A,$8F
         OUT ($99),A
         EI
 VDP-JP: LD A,D
         AND $3E
         JP NZ,V9938R
         JP    V9958R

 Dit stukje source kijkt dus ook echt alleen of er een V9958 is
 zonder naar ROMS te kijken, iets dat veel programma's nog eens
 willen doen. Een  normale MSX2 met daarin  dus een V9958 wordt
 dus tot 2+  gebombardeerd  door deze routine.  Dat was 't wel,
 verder moet je screen 12 als screen 8 behandelen en om overig-
 ens screen 11 te krijgen moet je  de "08" die je naar register
 25 out vervangen door "24", decimaal dus...

 Van wie ik deze wijsheid heb?? Lijkt me vrij duidelijk...
 Van de MSX programmeur in Nederland, Alex Wulms.

 Well, good luck... I guess?!?!

Tobias "Thank you Alex" Keizer
