 Bla bla bla bla
            The PCM sample processor, the truth...

                             ����
                             ����
                             ����
                             ����




 Those good  old  days...   I can  remember them  like it  was
 yesterday... Remember those stories in all the magazines when
 the Turbo-R (ST) was released in Japan?  A 28.8Mhz CPU, loads
 of built-in software and a PCM-Samplechip.  WOW!! Now it would 
 be  possible to use samples with your music, or voice-recogni
 sion. Yeah, righty...  But still, the PCM isn't at all bad...

 
Music

 Were the  music  part  is concerned,  there aren't  that many
 options, unfortunately.  Adressing the PCM chip takes too much 
 time for any other things to be done.  Music + samples there-
 fore isn't really  an option.  It is possible, but the sample
 freuency has to be  very low  (No more  than 12Khz) and there
 will be a 60Hz rustle on the back of the sound. For something
 like  a drum, that's not too big a problem. But when it comes
 to speach or something like that, you'll notice.
 
 The FD#25  game Ryu No Chie is a nice example of what I mean.
 The sample frequency there was 8Khz, the music was played via
 a slightly  modified  Moonblaster  replayer  in  stereo mode.
 Naturally, that wasn't the  best a  Turbo-R can do.  For one,
 the  R800 wasn't on, and  the R800  mode is  at least 4 times
 faster than the Z80 mode. Furthermore, the standard interrupts 
 where activated, which isn't too clever either.

FREQUENCY

 The previously mentioned magazines told us the maximum sample
 frequency on a Turbo-R was about 16KHz. Not at all true... In
 ideal circumstances a sample (play!) frequency of 400KHz(!!!)
 should  not be a problem. But, like said, under ideal circum-
 stances...Think OTIR in cases like these.  However, under not
 so ideal circumstances (read,  a full-function PCM player) 16
 Khz is not  the top  by far.  A sample frequency of 44KHz (CD
 quality) should not give  any problems  either.  However, the
 big  problem is recording those  samples.  As the samples are
 recorded with only one  bit, is  takes a lot more CPU time to
 record a sample, than it does to play one... Too bad...
 
 I haven't calculated all this, but I think recording should be 
 possible at a  sample-rate of  24Khz, playing  has very  fiew
 limits...
 
 
HOW TO PLAY A SAMPLE...
 
 Let's make a little  program that  plays a  sample from $4000
 till  $BFFF. Let's  assume we do not need any interrupts, and
 it is not possible  to abort  the process.  Here's what you'd
 get:
 
 BEGINN:  LD   HL,$4000
          LD   BC,$8000
          LD   A,%00000010
          OUT  ($A5),A
 
 MAINLP:  LD   A,(HL)
          OUT  ($A4),A
          INC  HL
          DEC  BC
          LD   A,B
          OR   C
          RET  Z
          JP   MAINLP
 
 A small player like this one would put you well over the 44Khz 
 C quality.  And when I say well over, I mean well over.  If I
 take a short look a this, it would put the sample rate at pro- 
 bably trice the frequence  of that  of a  CD player... But, I
 could be wrong...  (I'm probably  not though)  Only remaining
 problem is it won't be possible to record samples on a Turbo-R 
 at that frequence, you'll need quite a PC for that. Other pro- 
 blem is that at a frequence this high, 32kB's of sample isn't
 really a lot to listen to. But still, that 16KHz really wasn't 
 all that accurate...
 
 Anyway,  I suggest that those interested juggle a little with
 this small source, and  we'll continue next time, with a real
 player with adjustable sample-frequency!  How nice...
 
 By the way, that first OUT is to set the PCM chip for:
 
       Play sample, Don't send MIC in to speaker, Don't disable 
       Sound chips (Music/PSG), activate filter...
 
 Well, good luck...
       And till next time...
 
Tobias Keizer
