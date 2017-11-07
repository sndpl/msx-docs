
       MUIZEN IN BASIC

                             ����
                             ����
                             ����
                             ����

 Wederom een tekstje voor de wat minder gevorderden. Dus al
 die oude rotten die eigenlijk alles al weten van Basic
 moeten maar gewoon ML gaan leren of zo... De muis... men
 plugt 'm in poort 1 of 2, men rammelt een beetje met ons
 electronische knaag diertje en zie daar, de muiscursor
 huppelt vrolijk 't scherm door. Om deze schokkende gebeur-
 tenis te achterhalen gebeurt er nog al 't een en ander in
 ons MSXje. De technische uitleg wou ik dan maar achterwege
 laten maar het hoe en wat betreffende 't uitlezen in basic
 wou ik wel even behandelen...
  
PAD
 Dat is 't commando waar 't allemaal mee dient te gebeuren...
 Pad wordt voornamelijk gebruikt om de status van de muis en
 de trackball uit te lezen. PHILIPS heeft echter destijds
 ooit ook nog eens van die shit zoals tekentableuas en
 lichtpennen uitgebracht die ook met pad werken.

 In de praktijk zal het uitlezen ook nog wel mee vallen, maar
 het checken of er wel een muis is aangesloten valt over 't
 algemeen tegen. Dat valt namelijk niet te checken op een MSX.
 Er is echter wel een truukje dat kan worden gebruikt, maar dat
 is niet echt netjes(NvdR: net of dat iemand boeit?!).

 De syntaxis

  A=PAD(X)
  De waarde van PAD komt hierdoor in A terecht waarbij in X
  de specifieke gegevens voor PAD dienen te worden opgegeven.
  12 of 16   Zal altijd -1 geven maar MOET worden opgevraagd
             voordat X en Y worden bepaald.
  13 of 17   De verandering op de X-as.
  14 of 18   De verandering op de Y-as.
  
 Het linker getal is steeds voor poort 1, het rechter voor de
 tweede poort. De beste manier om dan de muis coordinaten bij
 te houden is gewoon steeds PAD(13) by het X-coordinaat op te
 tellen en PAD(14) bij het Y-coordinaat op te tellen.
 PAD(12) heeft dus geen functie maar moet wel voor 't
 opvragen van PAD(13) en PAD(14) worden bepaald.
 Een voorbeeld:

  10 SCREEN5:X=127:Y=105
  20 DUMMY=PAD(12):X=X+PAD(13):Y=Y+PAD(14)
  30 IFX<0THENX=0:ELSEIFX>255THENX=255
  40 IFY<0THENY=0:ELSEIFY>211THENY=211
  50 PSET(X,Y),15:GOTO20

 Door het bewegen van de muis kunnen we nu keurig een lijntje
 trekken over 't scherm. Maar als we nou ook af en toe geen
 lijnje willen trekken? Goed, gaan we weer:

  10 SCREEN5:X=127:Y=105
  20 DUMMY=PAD(12):X=X+PAD(13):Y=Y+PAD(14)
  30 IFX<0THENX=0:ELSEIFX>255THENX=255
  40 IFY<0THENY=0:ELSEIFY>211THENY=211
  50 IFSTRIG(1)THENPSET(X,Y),15:GOTO20:ELSEGOTO20

 Nu kunnen we de muis vrijelijk door 't scherm laten bewegen
 zonder dat we een lijntje trekken. Pas als we trig1 inhouden
 zal er een lijntje worden getrokken. Maar nu weten we dus
 niet waar de muiscursor is als we niet aan 't tekenen zijn.
 Hiervoor voegen we weer twee regels toe:

  15 SPRITE$(1)=CHR$(&H80)
  45 PUTSPRITE 0,(X,Y),14,1

 Nu zal er op de plek van de muis-coordinaten ook echt een
 muiscursor verschijnen; nou ja, een grijs stipje.
 Maar Tobias zou Tobias niet zijn als ie niet nog een
 netelig probleem kon bedenken. Juist ja, misschien zit de
 muis wel in poort 2.

  05 X=0:FORL=0TO25:DUMMY=PAD(12):X=X+PAD(13):NEXTL:IFX>-5
     ANDX<5THENMO=12:TR=1
  10 X=0:FORL=0TO25:DUMMY=PAD(16):X=X+PAD(17):NEXTL:IFX>-5
     ANDX<5THENMO=16:TR=3
  15 SCREEN5:X=127:Y=105:SPRITE$(1)=CHR$(&H80)
  20 D=PAD(MO):X=X+PAD(MO+1):Y=Y+PAD(MO+2)
  30 IFX<0THENX=0:ELSEIFX>255THENX=255
  40 IFY<0THENY=0:ELSEIFY>211THENY=211
  50 PUTSPRITE0,(X,Y),14,1
  60 IFSTIRG(TR)THENPSET(X,Y),15:GOTO20:ELSEGOTO20

 Nu hebben we eindelijk een routine die met de muis in elke
 poort een muis cursor op 't scherm zet en bij een knop op
 de linker vuurknop een punt zet.

 Wauw! fantastisch, heb ik weer en half uur zitten typen en
 weer geen hond die 't leest. A well, that FutureDisk life!
 (NvdR: what life?)

Keizer...
