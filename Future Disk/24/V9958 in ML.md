 Ons liever Tobias heeft zich flink uitgesloofd, dus alweer een tekst van deze plezante zeer aardige en lieve jongen uit Nijverdal, bellen meiden, hij is nog vrij!

V9958 in ML

                             ����
                             ����
                             ����
                             ����


 Ook dit heeft geloof ik al een keer eerder op de FD gestaan,
 maar omdat ik weer over SCREEN 12 in basic begon toch nog een
 keer de ML versie van SCREEN 12.

 Als eerste is het natuurlijk handig om te weten of de V9958
 wel aanwezig is. Ik persoonlijk kijk hier eigenlijk niet
 eens naar. Als je zo dom bent om een 2+ programma op een
 MSX 2 te runnen moet je natuurlijk niet vreemd opkijken als
 het dan niet werkt. Maar soms kan het voorkomen dat je echt
 ��n aparte routine voor de MSX2 en de MSX2+ hebt en dan is
 het natuurlijk wel handig om te weten of de V9958 wel
 aanwezig is. Dus eerst even een klein stukje assembly:

 CHKVDP: DI
         LD  A,$01
         OUT ($99),A
         LD  A,$8F
         OUT ($99),A
         PUSH AF
         POP AF
         IN  A,($99)
         LD  D,A
         XOR A
         OUT ($99),A
         LD  A,$8F
         OUT ($99),A
         EI
 JP_RTN: LD  A,D
         AND $3E
         JP  NZ,V_9938
         JP  V_9958

 Vervolgens is het misschien wel handig om SCREEN 12 aan te
 zetten. Als je graag wilt mag je van mij SCREEN11 ook wel
 aanzetten hoor! Okay, here we go:

 SCRN11: LD  A,$08
         CALL $5F
         DI
         LD  A,$18
         OUT ($99),A
         LD  A,$99
         OUT ($99),A
         EI

 SCRN12: LD  A,$08
         CALL $5F
         DI
         LD  A,$08
         OUT ($99),A
         LD  A,$99
         OUT ($99),A
         EI

 En dit zou toch echt moeten werken want mijn SCREEN12.BIN op
 deze FD gebruikt dit stukje code ook.

 Dit is overigens ook handig voor het BASIC volk dat de MSX2+
 commando's i.v.m. compatibiliteit met de MSX2's met een
 V9958 in stand wil houden. Gewoon intypen in WB, assembleren
 en de .BIN file BLOADen aan het begin van je programmaatje.

 Hey guys, guess what?! It's 02.19 and I'm tired. Sleep well
 and see you in the morning. I'm gonna have sweet dreams
 about Amalia now....

Dikke zoen,

Keizer...
