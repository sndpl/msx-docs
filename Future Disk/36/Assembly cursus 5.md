Arjan programmeert er rustig op los...

        ASSEMBLY (5)


 Deze keer gaan we het hebben over screensplits, paletsplits
 en alle andere soorten splits die je maar kunt bedenken. Een
 goed voorbeeld van een screensplit zie je nu terwijl je aan
 het lezen bent. Eigenlijk zijn er zelfs 2 screensplits, want
 alleen de tekst beweegt en de border staat stil.

 Een split is eigenlijk heel simpel om te maken. Het kan
 zelfs op 2 verschillende manieren. Je kunt namelijk de VDP
 een interrupt laten genereren als de lijn voorbij is en je
 kunt zelf testen of de VDP al voorbij de lijn is. Deze
 laatste methode zal ik hier bespreken, de eerste komt een
 volgende keer aan bod.

 Zoals gezegd is een split heel simpel. Eerst doe je nog wat
 dingetjes totdat je denkt dat de VDP ongeveer is bij de lijn
 waar je de split wilt hebben. Dan zet je in register 19 NA
 welke lijn je de split wilt hebben. Daarna test je of bit 0
 van statusregister 1 een 1 is. Als dat zo is, is de VDP
 klaar met die lijn. Vervolgens selecteer je een andere page,
 schermmode, palet of whatever. Dit kost wel wat tijd, dus
 zorg ervoor dat de lijnen bij de split dezelfde kleur
 hebben want anders gaat die lijn knipperen.

 Een voorbeeldje: stel je wilt de helft van het scherm page 0
 hebben en de andere helft page 1. Dan zet je in register 19
 de waarde 105, dus komt de split bij lijn 106. Lijn 106 maak
 je dan op beide pagina's dezelfde kleur en vervolgens test
 je of de lijn al voorbij is. Daarna selecteer je page 1 en
 dan wacht je weer op een interrupt om page 0 te selecteren.

 De volgende source zal het wel duidelijker maken. De source
 staat ook op disk onder de naam SCRSPLIT.GEN. Natuurlijk is
 er ook een klein voorbeeldje bij, namelijk SCRSPLIT.BAS,
 welke deze routine gebruikt. Dit programmaatje laat ook zien
 wat er gebeurt als je de lijnen bij de split niet dezelfde
 kleur geeft.

         defb    #fe                    ; BLOAD-header
         defw    start,einde,start

         org     #d000

 start:  ei
         halt

         ld      a,31           ; 31 = page 0, 63 = page 1
         out     (#99),a        ; 127 = page 2, 255 = page 3
         ld      a,2 + 128
         out     (#99),a        ; Selecteer page 0

         xor     a
         call    #d8
         or      a
         ret     nz             ; Spatie ingedrukt = stoppen

         ld      a,105          ; Lijn voor screensplit
         out     (#99),a
         ld      a,19 + 128
         out     (#99),a        ; Stuur lijn naar register 19

         ld      a,1
         out     (#99),a
         ld      a,15 + 128
         out     (#99),a        ; Selecteer statusregister 1
         nop
         nop                    ; geef VDP wat tijd
 split:  in      a,(#99)        ; lees statusregister 1
         bit     0,A            ; test bit 0 (0 = niet
                                ; voorbij lijn)
         jp      z,split        ; herhaal test indien nodig

         ld      a,63
         out     (#99),a
         ld      a,2 + 128
         out     (#99),a        ; selecteer page 1

         xor     a
         out     (#99),a
         ld      a,15 + 128
         out     (#99),a        ; Selecteer statusregister 0

         jp      start          ; herhaal

 einde:

 Een kleine opmerking wil ik nog maken over het selecteren
 van statusregister 0. Dit register moet geselecteerd worden,
 omdat de BIOS met IN A,(#99) dit register wil lezen terwijl
 'ie dit register meestal niet eerst selecteert (om tijd te
 sparen).

 Als je maar ��n split hebt, hoef je register 19 maar ��n
 keer te veranderen, zodat je weer (iets) meer tijd hebt voor
 andere dingen. Dit heb ik hier niet gedaan voor de duidelijk-
 heid. Natuurlijk is het mogelijk om meerdere splits te maken.
 Je zet de routine voor de split vaker in je code, maar dan
 telkens met een andere lijn. Zo simpel is dat.

 Dit was het dan voor deze keer. De volgende keer zal ik
 .LIB-files behandelen (een file waar je alle files in
 propt). Dat wordt een lekker lange aflevering, maar dat mag
 ook wel eens een keertje, of niet soms?


Arjan Bakker
