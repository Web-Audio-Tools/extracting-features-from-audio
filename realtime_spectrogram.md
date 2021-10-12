[← Back to Index](index.html)

# Real-time Spectrogram<a href="#Real-time-Spectrogram" class="anchor-link">¶</a>

Out\[1\]:

Here is how you can create a real-time spectrogram in your terminal using [PyAudio](https://people.csail.mit.edu/hubert/pyaudio/).

To see the example in action, run the script in this repo, `realtime_spectrogram.py`:

    python3 realtime_spectrogram.py

The basic idea is simple. For every new audio buffer,

1.  Take an FFT, `x_fft`, of the audio buffer.
2.  Compute a `melspectrum` from the `x_fft`.
3.  Print a string, `s`, where `s[i]` is `'*'` wherever `melspectrum[i]` is above a threshold.

From here, you can manipulate this basic example to do more sophisticated real-time processing, e.g. involving machine learning models.

[← Back to Index](index.html)
