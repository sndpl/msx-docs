       Basic Cursus (7)


                             ����
                             ����
                             ����
                             ����

  OUT&HFE en ander ge-emmer
                                                               
 In Basic heeft de gebruiker eigenlijk maar weinig geheugen
 tot zijn beschikking. &H0000-&H7FFF is immers Rom,
 op &H8000-&HBFFF staat je basic programma, en alles boven
 &HE000 is eigenlijk ook al linke soep... Blijft dus over
 &HC000-&HE000, en das nie bar veel! In die 8Kb kun je
 natuurlijk wel een zooitje strings en variabelen kwijt maar
 als je ook maar iets bv op sector in wil laden of een
 aantal sampletjes wil afspelen dan zit je al in de knoop.
 Een redelijk onbekende -onbeminde- methode is om basic
 gewoon op C000 te flikkeren en daardoor komen alle banks
 van 8000-BFFF weer vrij. For the ignorant, a little scheme:   
                                                               
 I Geheugen I Vrije Banks I                                    
 --------------------------                                    
 I     64Kb I         1-3 I                                    
 I    128Kb I         1-7 I                                    
 I    256Kb I        1-15 I                                    
 I    512Kb I        1-31 I and so on.....                     
 --------------------------                                    
                                                               
 Als je dit geintje dus uitvoert heb je in ��n keer een hele
 zwik geheugen tot je beschikking, wat overigens heerlijk is.
 Dit truukje werkt echter alleen met de interne of op de op
 dat moment ingeschakelde mapper. Twee mappers in basic
 gebruiken kan wel maar dat ga ik niet helemaal uitleggen.
 Eerst zul je dus Basic op $C000 moeten zetten, daarna moet
 je natuurlijk nog weten hoe je welke mapper bank in moet
 schakelen. Mapper bank0 is niet beschikbaar, dit is normaal
 gesproken &HC000-&HFFFF waar Basic z'n administratie neerzet
 en de gebruiker nog even rond kan poken/peeken. Mapper bank
 1 wordt door Basic gebruikt om de programma-data in op te
 slaan, hier staat dus normaal je programmaatje in. Nu gaan
 we dat programmaatje naar &HC000 verplaatsen. Theoretish
 mag dat niet omdat je dan in de knoop kan komen met je
 systeem variabelen die Basic in die bank opslaat. In de
 praktijk valt dit over het algemeen wel mee. 't Is mij
 in iedergeval nog nooit gelukt. Dit betekent namelijk een
 listing die Tokenized al meer dan 8Kb zou zijn. Grote jongen
 die dat bij elkaar typt! Nu is bank1 dus ook vrijgekomen
 aangezien je programmaatje daar nu niet meer staat. Je kunt
 dus -behalve bank0- alle banks gebruiken (Zie schema). Dan
 nu 't eigenlijke programmaatje wat je dan krijgt:

 10 ' MEMTEST.BAS
 20 IFPEEK(&HF677)<>&HC0THENPOKE&HF677,&HC0:POKE&HC000,0:
 RUN"MEMTEST.BAS"
 30 FORP=1TO255:OUT&HFE,P:POKE&H8000,55:NEXTP                  
 40 FORP=1TO255:OUT&HFE,P:IFPEEK(&H8000)=55THENPG=PG+1:NEXTP   
 50 PG=PG+1:PRINT PG*16;"KB GEHEUGEN"

 Nu is dit maar een stom programmatje maar wat je nu feitelijk
 met die pages doet boeit eigenlijk niet. Pleur d'r maar eens
 graphics of muziek neer. Samples wil trouwens ook prima. Als
 je 't echt netjes wil doen pleur je alles wel weer keurig
 terug als je klaar bent. Dus krijg je:

 60 POKE &HF677,&H80:POKE&H8000,0

 Als je daarna namelijk andere programma's gaat laden die er
 niet op rekenen dat basic op $C000 staat wil dat nog wel
 eens de soep in lopen. Het is overigens ook wijsheid om
 met CTRL op te starten aangezien basic dan wat minder
 geheugen nodig heeft voor z'n drive administratie.
 
 Well that's all, bye....

 Tobias 'I love my 512Kb mapper' Keizer
