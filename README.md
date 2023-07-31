# Toccata-and-Fugue
J. S. Bach's Toccata and Fugue in D minor Played by a programme written in Dyalog APL with animation. 

The APL code creates a "WAV" file from scratch, and populates it with Bach famous Toccata and Fugue in D minor.

The WAV file is in stereo with 44,400 "samples" per second, each sample having 32 bits. The final "samples" are calculated (not recoded) from multiple sine waves, relating to the frequency all the notes being "played" at that  moment in time.

In addition to the fundamental frequency, multiple harmonics of the frequency are also added to mimic the sound of different organ pipes. Samples making up individual "notes" are further modified in amplitude over the duration of the note, by a simplified organ sound "envelope" ("attack", "decay", "sustain" and "release").

To this is added reverberation, in an attempt to add the ambiance of a typical large hall with a 60 decibel reduction over a 2 second period. 

As a result, each of the 20 million odd samples may have 100's of components contributing to its value. Before being written to the WAV file, the values are normalised to be in the 32 bit range required by the file format.

The data required to "power" the music has been provided in 143 APL function, one function for each musical bar (or measure). Each of these APL functions will typically divide the bar into up to 128 units. The length (in time) of each unit will depend on the "Tempo" at that moment within the piece. Where the tempo changes within a bar, I split the bar between two functions.

Within a bar/function, I re-define the tempo (normally specified as the number of "beats per minute") as the number of samples per 1/128 of the bar.  I re-use the names used for Tempo's in classical music, but with specific BPM, rather than the very vague values in common use (E.G. Andante can vary between 70 to 108 bpm, I have set it to 75) . The Tempo remains the constant for the function. The function is then split into "voices".

Each voice will consist of a "pipe" name and a set of none-overlapping notes or cords. The Pipe defines the harmonics and the envelope used in the sound creation. Each note/cord will follow directly after the last note/cord has finished. Some of the notes can be silence (called a "rest" in musical terms). For each note/cord, 3 values are defined: Length, Loudness and Frequency.

The "Length" can be W:whole=128 units, H:half=64, Q:quarter=32, E:eighth=16, S:sixteenth=8, T:thirty-second=4, F:sixty-fourth=2 or Z:1/128=1 unit.  Thus the length defines a specific number of samples, depending on the tempo.

The loudness is expressed using music terms such as p, mp, mf, f, ff, etc. Originally they differ by 10 decibels between terms, so ranging from 20db for "ppp" to 90db for "fff". However, for practical reasons, I have reduces this dynamic range to about 60 db.

The frequency of the notes, are defined in cycles per second (Hz) and given the names in the form of {letter}{sharp or flat indicator} and {octave number}. So middle C (261.63 Hz) is "C4" using an equal temperament scale with A4 at 440Hz. (Other tunings are provided. Not that I can really tell the difference) To be valid names in APL, I have used "∆" for a sharp (rather than "#") and "_" for a flat.

I used multiple voices to prevent the discontinuities (that would otherwise result in unwanted "snaps and pops") when waves interfere with each other, or overlap. Also for when notes making up a single cord have different lengths.

The animation has been added mainly to help me "see" where we are within the music, so when I hear an error, I know which function caused the error. Origionally, the musical notes were all shown in a monocrome red, but now the colour of each note will depend on the pipe or pipes playing that note. This has the added benifit of highlighting different musical phrases, enhancing the whole experiance.

As each voice is generated, a matrix of bits is built up. In one dimension one bit for every 3700 samples or 1/12 of a second, and the other for each note in the scale. Additional information relating to where exactly the bar/function changes, and the name of the function. The bit matrix and additional information is stored in an APL component file. When I added the pipe colour to the animation, this 2 dimensional matrix was simply replaced by a 3 dimensional table.

The APL function "PlayIt" is used to play the sound and animation in sync. The WAV file is played back via Microsoft Window's built-in function "PlaySound". The animation is displayed via Dyalog's system function graphics. A portion of the matrix of bits is expanded into the "CBits" of a BitMap. As the music is played, the portion of the matrix selected is moved on by one pixel. So long as the computer running this code is fast enough to refresh the BitMap at a faster rate than the music being played, the animation is kept in sync with the music.

In addition to the APL workspace, I have included a copy of the sound file (converted from WAV into a MP3 file, as the WAV file is too big for Github). An MP4 file at 70MB was way too big, but is available on YouTube at https://youtu.be/M_NypHXHfls

Some functions of note within the workspace are listed below:

"lx" is the latent expression. It just displays some function name, that I am to lazy to type.

"WriteMusic" This is the main function the creates the WAV file with all the music in it, and the component file with the animation data.

"Music" This simply lists all the bar/functions. For connivance, these are all in the namespace "#.BMV565".

"PlayIt"  Reads the files and runs both sound and vision.

"Stop" Stops the sound being played, and cleans up after the graphics. Because "PlaySound" is an external compiled function from a dynamic load library (DLL), simply stopping the APL code running, does NOT stop PlaySound running, while running "Stop" does.

"Initialise" Sets up many global variables in the namespace "#.G". 

"BuildPart" Creates one of the voices making up a bar. Most of he "hard" work is done within this function and its sub-functions.

"BuildLR" This function combines all the voices within a bar and append them to either the left, right or both "channel" variables. Note both channels must have exactly the same number of samples.

"Stereo" Combines the left and right channels into the single vector required by the WAV file and also normalises the absolute volume.

"Reverberation" This function repeatedly adds a copy of the sound so far, but at a reduced amplitude, and with a delay, to reproduce the effects of a large enclosed space. It is very much a "work in progress", and contains "commented out" old code. 
