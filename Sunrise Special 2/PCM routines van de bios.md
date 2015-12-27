# P C M - R O U T I N E S   V A N   D E   B I O S 
                                                       

Vertaling  van een  gedeelte van het artikel over de turbo R 
uit het Japanse MSX-Magazine:


## P C M P L Y 
```
Funktie   : PCM-weergave
Adres     : Main ROM #0186
In        : A
            Bit 7 6 5 4 3 2 1 0
                | | | | | `-`---> frequentie
                | `-------------> 0
                `---------------> VRAM/MRAM
            EHL (adres van de data)
            DBC (lengte van de data)
Terug     : carry flag
            0 normale beeindiging
            1 abnormale beeindiging
            |
            `--> A (reden van de abnormale beeindiging)
                 1 fout bij het instellen van de frequentie
                 2 onderbreking veroorzaakt door de STOP
                   toets
                 EHL (adres waar de onderbreking plaatsvond)
Verandert : alle registers
```

De  PCM-geluidsdata worden  als bit  7 van register A ��n is 
uit het video-RAM gehaald en als het nul is uit het main RAM 
gehaald.  Bovendien   hebben  de   waarden  van  het  D-  en 
E-register  slechts een  betekenis in  het geval dat de data 
zich in het video-RAM bevindt.
D.m.v.  bit  1  en  bit 0  van de  accumulator stelt  men de 
sample-frequentie  in.  Maar,  pas  op:  15.75  kilohertz is 
alleen  mogelijk  wanneer de  turbo R  in de  R800-DRAM-mode 
staat:

```
00 W> 15.75   kilohertz
01 W>  7.875  kilohertz
10 W>  5.25   kilohertz
11 W>  3.9375 kilohertz
```

## P C M R E C 
```
Functie   : PCM-opname
Adres     : Main ROM #0189
In        : A
            Bit 7 6 5 4 3 2 1 0
                | | | | | | `-`-> frequentie
                | | | | | `-----> compressie
                | `-`-`-`-------> trigger
                `---------------> VRAM/MRAM

            EHL (adres van de data)
            DBC (lengte van de data)
Terug     : carry flag
            0 normale be�indiging
            1 abnormale be�indiging
            |
            `--> A (reden van de abnormale be�indiging)
                 1 fout bij het instellen van de frequentie
                 2 onderbreking veroorzaakt door de STOP
                   toets
                 EHL (adres waar de onderbreking plaatsvond)
Verandert : alle registers
```
De codering  van ingave bij bit 7, 1 en 0 van het A register 
is  hetzelfde als  bij PCMPLY.  Bit 6  t/m bit  3 van  het A 
register heten 'trigger' en duiden de sterkte van het geluid 
aan, waarbij  het zover  komt dat de opname begint. Als deze 
waarde nul is, wordt de opname onmiddellijk gestart.

Verder  worden,  als  bit  2  van  register  A  ��n  is,  de 
opname-data  gecomprimeerd. Als  het nul  is, worden ze niet 
gecomprimeerd.
Het comprimeren  houdt in  dat, als de input 0 is, het wordt 
vervangen door een code. Dit kan heel veel bytes schelen.

Bernard Lamers