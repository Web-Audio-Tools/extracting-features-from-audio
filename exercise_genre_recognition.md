In \[ \]:

    %matplotlib inline
    import seaborn
    import numpy, scipy, matplotlib.pyplot as plt, sklearn, pandas, librosa, urllib, IPython.display, os.path
    plt.rcParams['figure.figsize'] = (14, 5)

[← Back to Index](index.html)

# Exercise: Genre Recognition<a href="#Exercise:-Genre-Recognition" class="anchor-link">¶</a>

## Goals<a href="#Goals" class="anchor-link">¶</a>

1.  Extract features from an audio signal.
2.  Train a genre classifier.
3.  Use the classifier to classify the genre in a song.

## Step 1: Retrieve Audio<a href="#Step-1:-Retrieve-Audio" class="anchor-link">¶</a>

Download an audio file onto your local machine.

In \[ \]:

    filename_brahms = 'brahms_hungarian_dance_5.mp3'
    url = "http://audio.musicinformationretrieval.com/" + filename_brahms
    if not os.path.exists(filename_brahms):
        urllib.urlretrieve(url, filename=filename_brahms)

Load 120 seconds of an audio file:

In \[ \]:

    librosa.load?

In \[ \]:

    x_brahms, fs_brahms = librosa.load(filename_brahms, duration=120)

Plot the time-domain waveform of the audio signal:

In \[ \]:

    librosa.display.waveplot?

In \[ \]:

    # Your code here:

Play the audio file:

In \[ \]:

    IPython.display.Audio?

In \[ \]:

    # Your code here:

## Step 2: Extract Features<a href="#Step-2:-Extract-Features" class="anchor-link">¶</a>

For each segment, compute the MFCCs. Experiment with `n_mfcc` to select a different number of coefficients, e.g. 12.

In \[ \]:

    librosa.feature.mfcc?

In \[ \]:

    n_mfcc = 12
    mfcc_brahms = librosa.feature.mfcc(x_brahms, sr=fs_brahms, n_mfcc=n_mfcc).T

We transpose the result to accommodate scikit-learn which assumes that each row is one observation, and each column is one feature dimension:

In \[ \]:

    mfcc_brahms.shape

Scale the features to have zero mean and unit variance:

In \[ \]:

    scaler = sklearn.preprocessing.StandardScaler()

In \[ \]:

    mfcc_brahms_scaled = scaler.fit_transform(mfcc_brahms)

Verify that the scaling worked:

In \[ \]:

    mfcc_brahms_scaled.mean(axis=0)

In \[ \]:

    mfcc_brahms_scaled.std(axis=0)

### Step 2b: Repeat steps 1 and 2 for another audio file.<a href="#Step-2b:-Repeat-steps-1-and-2-for-another-audio-file." class="anchor-link">¶</a>

In \[ \]:

    filename_busta = 'busta_rhymes_hits_for_days.mp3'
    url = "http://audio.musicinformationretrieval.com/" + filename_busta

In \[ \]:

    urllib.urlretrieve?

In \[ \]:

    # Your code here. Download the second audio file in the same manner as the first audio file above.

Load 120 seconds of an audio file:

In \[ \]:

    librosa.load?

In \[ \]:

    # Your code here. Load the second audio file in the same manner as the first audio file.
    # x_busta, fs_busta =

Listen to the second audio file.

In \[ \]:

    IPython.display.Audio?

Plot the time-domain waveform and spectrogram of the second audio file. In what ways does the time-domain waveform look different than the first audio file? What differences in musical attributes might this reflect? What additional insights are gained from plotting the spectrogram? Explain.

In \[ \]:

    plt.plot?

In \[ \]:

    # See http://musicinformationretrieval.com/stft.html for more details on displaying spectrograms.
    librosa.feature.melspectrogram?

In \[ \]:

    librosa.logamplitude?

In \[ \]:

    librosa.display.specshow?

Extract MFCCs from the second audio file. Be sure to transpose the resulting matrix such that each row is one observation, i.e. one set of MFCCs. Also be sure that the shape and size of the resulting MFCC matrix is equivalent to that for the first audio file.

In \[ \]:

    librosa.feature.mfcc?

In \[ \]:

    # Your code here:
    # mfcc_busta =

In \[ \]:

    mfcc_busta.shape

Scale the resulting MFCC features to have approximately zero mean and unit variance. Re-use the scaler from above.

In \[ \]:

    scaler.transform?

In \[ \]:

    # Your code here:
    # mfcc_busta_scaled =

Verify that the mean of the MFCCs for the second audio file is approximately equal to zero and the variance is approximately equal to one.

In \[ \]:

    mfcc_busta_scaled.mean?

In \[ \]:

    mfcc_busta_scaled.std?

## Step 3: Train a Classifier<a href="#Step-3:-Train-a-Classifier" class="anchor-link">¶</a>

Concatenate all of the scaled feature vectors into one feature table.

In \[ \]:

    features = numpy.vstack((mfcc_brahms_scaled, mfcc_busta_scaled))

In \[ \]:

    features.shape

Construct a vector of ground-truth labels, where 0 refers to the first audio file, and 1 refers to the second audio file.

In \[ \]:

    labels = numpy.concatenate((numpy.zeros(len(mfcc_brahms_scaled)), numpy.ones(len(mfcc_busta_scaled))))

Create a classifer model object:

In \[ \]:

    # Support Vector Machine
    model = sklearn.svm.SVC()

Train the classifier:

In \[ \]:

    model.fit?

In \[ \]:

    # Your code here

## Step 4: Run the Classifier<a href="#Step-4:-Run-the-Classifier" class="anchor-link">¶</a>

To test the classifier, we will extract an unused 10-second segment from the earlier audio fields as test excerpts:

In \[ \]:

    x_brahms_test, fs_brahms = librosa.load(filename_brahms, duration=10, offset=120)

In \[ \]:

    x_busta_test, fs_busta = librosa.load(filename_busta, duration=10, offset=120)

Listen to both of the test audio excerpts:

In \[ \]:

    IPython.display.Audio?

In \[ \]:

    IPython.display.Audio?

Compute MFCCs from both of the test audio excerpts:

In \[ \]:

    librosa.feature.mfcc?

In \[ \]:

    librosa.feature.mfcc?

Scale the MFCCs using the previous scaler:

In \[ \]:

    scaler.transform?

In \[ \]:

    scaler.transform?

Concatenate all test features together:

In \[ \]:

    numpy.vstack?

Concatenate all test labels together:

In \[ \]:

    numpy.concatenate?

Compute the predicted labels:

In \[ \]:

    model.predict?

Finally, compute the accuracy score of the classifier on the test data:

In \[ \]:

    score = model.score(test_features, test_labels)

In \[ \]:

    score

Currently, the classifier returns one prediction for every MFCC vector in the test audio signal. Can you modify the procedure above such that the classifier returns a single prediction for a 10-second excerpt?

In \[ \]:

    # Your code here.

## Step 5: Analysis in Pandas<a href="#Step-5:-Analysis-in-Pandas" class="anchor-link">¶</a>

Read the MFCC features from the first test audio excerpt into a data frame:

In \[ \]:

    df_brahms = pandas.DataFrame(mfcc_brahms_test_scaled)

In \[ \]:

    df_brahms.shape

In \[ \]:

    df_brahms.head()

In \[ \]:

    df_busta = pandas.DataFrame(mfcc_busta_test_scaled)

Compute the pairwise correlation of every pair of 12 MFCCs against one another for both test audio excerpts. For each audio excerpt, which pair of MFCCs are the most correlated? least correlated?

In \[ \]:

    df_brahms.corr()

In \[ \]:

    df_busta.corr()

Display a scatter plot of any two of the MFCC dimensions (i.e. columns of the data frame) against one another. Try for multiple pairs of MFCC dimensions.

In \[ \]:

    df_brahms.plot.scatter?

Display a scatter plot of any two of the MFCC dimensions (i.e. columns of the data frame) against one another. Try for multiple pairs of MFCC dimensions.

In \[ \]:

    df_busta.plot.scatter?

Plot a histogram of all values across a single MFCC, i.e. MFCC coefficient number. Repeat for a few different MFCC numbers:

In \[ \]:

    df_brahms[0].plot.hist()

In \[ \]:

    df_busta[11].plot.hist()

## Extra Credit<a href="#Extra-Credit" class="anchor-link">¶</a>

Create a new genre classifier by repeating the steps above, but this time use training data and test data from your own audio collection representing two or more different genres. For what genres and audio data styles does the classifier work well, and for which (pairs of) genres does the classifier fail?

Create a new genre classifier by repeating the steps above, but this time use a different machine learning classifier, e.g. random forest, Gaussian mixture model, Naive Bayes, k-nearest neighbor, etc. Adjust the parameters. How well do they perform?

Create a new genre classifier by repeating the steps above, but this time use different features. Consult the [librosa documentation on feature extraction](http://librosa.github.io/librosa/feature.html) for different choices of features. Which features work well? not well?

[← Back to Index](index.html)
