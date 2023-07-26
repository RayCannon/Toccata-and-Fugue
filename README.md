# Toccata-and-Fugue
J. S. Bach's Toccata and Fugue in D minor Played by a programme written in Dyalog APL with animation. 

The APL code creates a "WAV" file from scratch, and populates it with Bach famous Toccata and Fugue in D minor.

The WAV file is in stereo with 44,400 "samples" per second, each sample having 32 bits. The final "samples" are calculated (not recoded) from multiple sine waves, relating to the frequency all the notes being "played" at that  moment in time.

In addition to the fundamental frequency, multiple harmonics of the frequency are also added to mimic the sound of different organ pipes. Samples making up individual "notes" are further modified in amplitude over the duration of the note, by a simplified organ sound "envelope" ("attack", "decay", "sustain" and "release").

To this is added reverberation, in an attempt to add the ambiance of a typical large hall with a 60 decibel reduction over a 2 second period. 

As a result, each of the 20 million odd samples may have 100's of component contributing to its value. Before being written to the WAV file, the values are normalised to be in the 32 bit range required by the file format.

The data required to "power" the music has been provided in 143 APL function, one function for each musical bar (or measure). Each of these APL functions will typically divide the bar into up to 128 units. the length (in time) of each unit will depend on the "Tempo" at that moment within the piece. Where the tempo changes within a bar, I sometimes split the bar between two functions.

Within a function-bar, I define the tempo in 1/128, defined from the number of "beats per minute".  I re-use the names used for Tempo's in classical music, but with specific BPM, rather than the very vague values in common use (EG Andante can be 74 to 108 bpm, I have set it to 75) . The Tempo remains the constant for the function. The function is then split into "voices".

Each voice will consist of a "pipe" name and a set of none-overlapping notes or cords. The Pipe defines the harmonics and the envelope used in the sound creation. Each note/cord will follow directly after the last note/cord has finished. Some of the notes can be silence (called a "rest" in musical terms). For each note/cord, 3 values are defined: Length, Loudness and frequency.

The length can be W:whole=128 units, H:half=64, Q:quarter=32, E:eighth=16, S:sixteenth=8, T:thirty-second=4, F:sixty-fourth=2 or Z:1/128=1 unit. 

The loudness is expressed using music terms such as p, mp, mf, f, ff, and fff, and  originally differ by 10 decibels between term each term. (So from 20db for "ppp" to 90db for "fff"), But due to my poor loudspeakers, I have reduces the dynamic range to about 60 db.

The frequency of the notes, are defined in cycles per second (Hz) and given the names in the form of <letter><sharp or flat indicator> and <octave number>. So middle C (261.63 Hz) is "C4" using an equal temperament scale with A4 at 440Hz. (Other tunings are provided. Not that I can really tell the difference)

 I used multiple voices to prevent the discontinuities (that result in unwanted "snaps and pops") when a waves interfere with each other.

The animation has been added mainly to help me visually see where we are within the music, so when I hear an error, I can go to the function that caused the error.

As each voice is generated, a matrix of bits is built up. 
