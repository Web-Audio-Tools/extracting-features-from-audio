[← Back to Index](index.html)

# SoX<a href="#SoX" class="anchor-link">¶</a>

[SoX](http://sox.sourceforge.net/) is a command-line utility for audio processing, e.g. convert file format, convert sample rate, remix, trim, pad, normalize, etc.

To install, for Mac:

    brew install sox

To play or record audio from the command line:

    rec test.wav

    play test.wav

Resample to 22050 Hz:

    sox in.wav -r 22050 out.wav

Normalize gain, e.g. to avoid clipping:

    sox in.wav --norm out.wav

Down-mix all input channels to mono:

    sox in.wav out.wav remix -

Convert to 16-bit signed integer:

    sox in.wav -b 16 -e signed-integer out.wav

Trim the audio file starting at 1 min 15 sec and ending at 1 min 45 sec:

    sox in.wav out.wav trim 1:15 =1:45
    sox in.wav out.wav trim 1:15 0:30
    sox in.wav out.wav trim 75 30

Pad 1 second of silence to the beginning and 2 seconds of silence to the end:

    sox in.wav out.wav pad 1 2

# ffmpeg<a href="#ffmpeg" class="anchor-link">¶</a>

[ffmpeg](https://www.ffmpeg.org/) is a framework to convert among different audio and video file formats. Use `ffmpeg` if you want to convert WAV to MP3.

To install on MacOS:

    brew install ffmpeg

Convert a WAV file to MP3:

    ffmpeg -i in.wav out.mp3

Down-mix all input channels to mono with a bitrate of 128k:

    ffmpeg -i in.wav -ac 1 -ab 128k out.mp3

To do the above command for all wav files in a directory:

    for i in wav/*; do ffmpeg -y -i "$i" -ac 1 -ab 128k mp3/`basename "$i" .wav`.mp3; done

[← Back to Index](index.html)
