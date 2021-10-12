In \[1\]:

    import IPython.display as ipd

[← Back to Index](index.html)

# Symbolic Representations<a href="#Symbolic-Representations" class="anchor-link">Â¶</a>

**Symbolic music representations** comprise any kind of score representation with an explicit encoding of notes or other musical events. These include machine-readable data formats such as MIDI. Any kind of digital data format may be regarded as symbolic since it is based on a finite alphabet of letters or symbols.

## Piano-Roll Representations<a href="#Piano-Roll-Representations" class="anchor-link">Â¶</a>

Around the late 19th and early 20th centuries, self-playing pianos called **player pianos** became popular. The input for these pianos is a continuous roll of paper with holes punched into it. This paper roll is called a **piano roll**. Performances by famous musicians such as Gustav Mahler, Edvard Grieg, Scott Joplin and George Gershwin have been recorded onto piano rolls.

Similar to the player piano, the pianola is not a player piano but it sits in front of a piano. Here is a pianola in action:

In \[4\]:

    ipd.display( ipd.YouTubeVideo("2A6ZXZwl3nA", start=106) )

Today, a **piano-roll representation** generically refers to any visualization of note information resembling a piano roll. See below for examples of piano-roll representations by Stephen Malinowski. Here, the horizontal axis represents time, and the vertical axis represents pitch.

In \[6\]:

    ipd.display( ipd.YouTubeVideo("LlvUepMa31o", start=15) )

In \[7\]:

    ipd.display( ipd.YouTubeVideo("Kri2jWr08S4", start=11) )

## MIDI Representations<a href="#MIDI-Representations" class="anchor-link">Â¶</a>

Another symbolic representation is based on the **MIDI** standard ([Wikipedia](https://en.wikipedia.org/wiki/MIDI)), or Musical Instrument Digital Interface. The advent of MIDI in 1981-83 caused a rapid growth in the electronic musical instrument market.

MIDI messages encode information for each note event such as the note onset, note offset, and intensity (represented as "velocity" in MIDI terminology). On computers, MIDI files contain a list of MIDI messages and other metadata.

The **MIDI note number** is an integer between 0 and 127 that encodes the note's pitch. Most importantly, C4 (middle C) has MIDI note number 60, and A4 (concert A440) has MIDI note number 69. MIDI note numbers separated by 12 are separated by one octave, e.g. 72 = C5, 84 = C6, etc.

The **key velocity** is an integer between 0 and 127 which controls the intensity of the sound.

The **MIDI channel** is an integer between 0 and 15 which prompts the synthesizer to use a specific instrument.

MIDI subdivides a quarter note into **clock pulses** or **ticks**. For example, if the number of pulses per quarter note (PPQN) is defined to be 120, then 60 ticks would represent the length of an eighth note.

MIDI can also encode tempo in terms of **beats per minute** (BPM) thus allowing for absolute timing information.

## Score Representations<a href="#Score-Representations" class="anchor-link">Â¶</a>

**Score representations** encode explicit information about musical symbols such as clefs, time signatures, key signatures, notes, rests, dynamics, etc. However, score representations, the way we define it here, does not include any description of the final visual layout and positioning of these symbols on the page.

For better or for worse, **MusicXML** has emerged as a universal format for storing music files for use among different music notation applications. Here is an excerpt of a MusicXML file:

        <measure number="2">
            <note>
                <pitch>
                    <step>B</step>
                    <alter>-1</alter>
                    <octave>4</octave>
                </pitch>
                <duration>1</duration>
                <voice>1</voice>
                <type>16th</type>
                <stem>down</stem>
                <beam number="1">begin</beam>
                <beam number="2">begin</beam>
            </note>
            <note>
                <pitch>
                    <step>D</step>
                    <octave>5</octave>
                </pitch>
                <duration>1</duration>
                <voice>1</voice>
                <type>16th</type>
                <stem>down</stem>
                <beam number="1">continue</beam>
                <beam number="2">continue</beam>
            </note>
            ...
        </measure>

[← Back to Index](index.html)
