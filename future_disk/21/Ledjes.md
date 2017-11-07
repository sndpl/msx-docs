Keizer maakt nooit wat af. Dat is een statement. Keizer probeert dit statement in deze tekst eens en voor altijd tot het verleden te verdoemen


    UNFINISHED BUSINESS...

                             ����
                             ����
                             ����
                             ����


 De vorige keer had ik al wat  zitten zemelen over de ledjes in
 de Turbo-R. Nu was ik echter druk bezig met een poker-spel dat
 ik op 't laatste  moment toch niet  af kreeg zodat ik het pro-
 grammaatje dat erbij hoorde op 't  laatste moment toch niet af
 kreeg. Nu dus maar even in een text-file hoe je alle leds moet
 aansturen. Om maar gelijk met het slechte nieuws te beginnen,
 twee leds vallen sowieso al af. De power led en de Renshaturbo
 led. Deze zijn beide hardware-matig en dus niet aan te sturen.
 De FDD led valt eigenlijk ook al af.  Hij is wel aan te sturen
 maar met mijn gebrekkige kennis van het programmeer vak dus
 not sorry  'bout that.  Misschien  dat Smael  dit weet maar
 ik dus niet... Blijven er vier leds over:
   CAPS, KANA, PAUSE en R800


 CAPS:
 
 DEFUSR=&H0132:A=USR(x) ' X=0, UIT. X<>0, AAN.


 KANA: (ALLEEN IN MC)

 LD IX,$0135
 CALL  $015F
 RET

 
 PAUSE:

 OUT (&HA7),&B0000001


 R800:

 OUT (&HA7),&B1000000


 Natuurlijk  kan je  MC data  even in  WB-ASS2, juist ja..  die
 assembler die elk zichzelf-respecterend-mens  heeft, DISen  om
 de opcodes even weg te  poken en  te USRen.  Dat lousy excuse
 for  een intro  demo dat deze keer op de FD staat(NvdR:
 herstel; de volgende keer op de FD staat) gebruikt de
 Turbo-R  leds al zodat dat het enorme gemis van de vorige keer
 weer een beetje  gecompenseerd  wordt. Well, I'm getting a bit
 vague here coz  it's late so  I'll take a  little coffee break
 now and type some more later. C YA!!!

 Tobias 'No remark this time' Keizer...

 Whoops,  voor ik 't vergeet...  Dat R800  gemier van de vorige
 keer was  ook niet  goed... Het stukje  MC + Basic  dat ik  de
 eerste keer geef was zoals 't hoorde. Niks geen subrom  of zo.
 Ik was gewoon even verward  omdat die  rountine dus geen A=USR
 (130) slikt als je die USR op &H0180 hebt gezet. Waarom ie dat
 niet wil weet ik niet, maar met dat kleine stukje MC werkt het
 allemaal wel...

 Succes, Keizer...
