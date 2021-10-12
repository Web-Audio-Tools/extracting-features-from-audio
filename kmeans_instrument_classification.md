In \[ \]:

    import numpy, scipy, matplotlib.pyplot as plt, sklearn, librosa, mir_eval, IPython.display, urllib

[← Back to Index](index.html)

# Unsupervised Instrument Classification Using K-Means<a href="#Unsupervised-Instrument-Classification-Using-K-Means" class="anchor-link">¶</a>

This lab is loosely based on [Lab 3](https://ccrma.stanford.edu/workshops/mir2010/Lab3_2010.pdf) (2010).

## Read Audio<a href="#Read-Audio" class="anchor-link">¶</a>

Retrieve an audio file, load it into an array, and listen to it.

In \[ \]:

    urllib.urlretrieve?

In \[ \]:

    librosa.load?

In \[ \]:

    IPython.display.Audio?

## Detect Onsets<a href="#Detect-Onsets" class="anchor-link">¶</a>

Detect onsets in the audio signal:

In \[ \]:

    librosa.onset.onset_detect?

Convert the onsets from units of frames to seconds (and samples):

In \[ \]:

    librosa.frames_to_time?

In \[ \]:

    librosa.frames_to_samples?

Listen to detected onsets:

In \[ \]:

    mir_eval.sonify.clicks?

In \[ \]:

    IPython.display.Audio?

## Extract Features<a href="#Extract-Features" class="anchor-link">¶</a>

Extract a set of features from the audio at each onset. Use any of the features we have learned so far: zero crossing rate, spectral moments, MFCCs, chroma, etc. For more, see the [librosa API reference](http://bmcfee.github.io/librosa/index.html).

First, define which features to extract:

In \[ \]:

    def extract_features(x, fs):
        feature_1 = librosa.zero_crossings(x).sum() # placeholder
        feature_2 = 0 # placeholder
        return [feature_1, feature_2]

For each onset, extract a feature vector from the signal:

In \[ \]:

    # Assumptions:
    # x: input audio signal
    # fs: sampling frequency
    # onset_samples: onsets in units of samples
    frame_sz = fs*0.100
    features = numpy.array([extract_features(x[i:i+frame_sz], fs) for i in onset_samples])

## Scale Features<a href="#Scale-Features" class="anchor-link">¶</a>

Use `sklearn.preprocessing.MinMaxScaler` to scale your features to be within `[-1, 1]`.

In \[ \]:

    sklearn.preprocessing.MinMaxScaler?

In \[ \]:

    sklearn.preprocessing.MinMaxScaler.fit_transform?

## Plot Features<a href="#Plot-Features" class="anchor-link">¶</a>

Use `scatter` to plot features on a 2-D plane. (Choose two features at a time.)

In \[ \]:

    plt.scatter?

## Cluster Using K-Means<a href="#Cluster-Using-K-Means" class="anchor-link">¶</a>

Use `KMeans` to cluster your features and compute labels.

In \[ \]:

    sklearn.cluster.KMeans?

In \[ \]:

    sklearn.cluster.KMeans.fit_predict?

## Plot Features by Class Label<a href="#Plot-Features-by-Class-Label" class="anchor-link">¶</a>

Use `scatter`, but this time choose a different marker color (or type) for each class.

In \[ \]:

    plt.scatter?

## Listen to Click Track<a href="#Listen-to-Click-Track" class="anchor-link">¶</a>

Create a beep for each onset within a class:

In \[ \]:

    beeps = mir_eval.sonify.clicks(onset_times[labels==0], fs, length=len(x))

In \[ \]:

    IPython.display.Audio?

## Listen to Clustered Frames<a href="#Listen-to-Clustered-Frames" class="anchor-link">¶</a>

Use the `concatenated_segments` function from the [feature sonification exercise](feature_sonification.html) to concatenate frames from the same cluster into one signal. Then listen to the signal.

In \[ \]:

    def concatenate_segments(segments, fs=44100, pad_time=0.300):
        padded_segments = [numpy.concatenate([segment, numpy.zeros(int(pad_time*fs))]) for segment in segments]
        return numpy.concatenate(padded_segments)
    concatenated_signal = concatenate_segments(segments, fs)

Compare across separate classes. What do you hear?

## For Further Exploration<a href="#For-Further-Exploration" class="anchor-link">¶</a>

Use a different number of clusters in `KMeans`.

Use a different initialization method in `KMeans`.

Use different features. Compare tonal features against timbral features.

In \[ \]:

    librosa.feature?

Use different audio files.

In \[ \]:

    #filename = '1_bar_funk_groove.mp3'
    #filename = '58bpm.wav'
    #filename = '125_bounce.wav'
    #filename = 'prelude_cmaj_10s.wav'

[← Back to Index](index.html)
