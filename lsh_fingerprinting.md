# Music Fingerprinting using Locality Sensitive Hashing<a href="#Music-Fingerprinting-using-Locality-Sensitive-Hashing" class="anchor-link">¶</a>

This notebook shows a simple system for performing retrieval of musical tracks using LSH.

Import libraries:

In \[26\]:

    import librosa
    import os
    import os.path

Set figure size:

In \[27\]:

    rcParams['figure.figsize'] = (15, 5)

Select training data:

In \[28\]:

    training_dir = '../train/'
    training_files = [os.path.join(training_dir, f) for f in os.listdir(training_dir)]

Define a hash function:

In \[29\]:

    def hash_func(vecs, projections):
        bools = dot(vecs, projections.T) > 0
        return [bool2int(bool_vec) for bool_vec in bools]

In \[30\]:

    def bool2int(x):
        y = 0
        for i,j in enumerate(x):
            if j: y += 1<<i
        return y

In \[31\]:

    bool2int([False, True, False, True])

Out\[31\]:

    10

In \[32\]:

    X = randn(10,100)
    P = randn(3,100)
    hash_func(X, P)

Out\[32\]:

    [6, 7, 4, 6, 3, 0, 0, 4, 0, 3]

### Create three LSH structures: Table, LSH, and MusicSearch:<a href="#Create-three-LSH-structures:-Table,-LSH,-and-MusicSearch:" class="anchor-link">¶</a>

In \[33\]:

    class Table:

        def __init__(self, hash_size, dim):
            self.table = dict()
            self.hash_size = hash_size
            self.projections = randn(self.hash_size, dim)

        def add(self, vecs, label):
            entry = {'label': label}
            hashes = hash_func(vecs, self.projections)
            for h in hashes:
                if self.table.has_key(h):
                    self.table[h].append(entry)
                else:
                    self.table[h] = [entry]

        def query(self, vecs):
            hashes = hash_func(vecs, self.projections)
            results = list()
            for h in hashes:
                if self.table.has_key(h):
                    results.extend(self.table[h])
            return results

In \[34\]:

    class LSH:

        def __init__(self, dim):
            self.num_tables = 4
            self.hash_size = 8
            self.tables = list()
            for i in range(self.num_tables):
                self.tables.append(Table(self.hash_size, dim))

        def add(self, vecs, label):
            for table in self.tables:
                table.add(vecs, label)

        def query(self, vecs):
            results = list()
            for table in self.tables:
                results.extend(table.query(vecs))
            return results

        def describe(self):
            for table in self.tables:
                print table.table

In \[35\]:

    class MusicSearch:

        def __init__(self, training_files):
            self.frame_size = 4096
            self.hop_size = 4000
            self.fv_size = 12
            self.lsh = LSH(self.fv_size)
            self.training_files = training_files
            self.num_features_in_file = dict()
            for f in self.training_files:
                self.num_features_in_file[f] = 0

        def train(self):
            for filepath in self.training_files:
                x, fs = librosa.load(filepath)
                features = librosa.feature.chromagram(x, fs, n_fft=self.frame_size, hop_length=self.hop_size).T
                self.lsh.add(features, filepath)
                self.num_features_in_file[filepath] += len(features)

        def query(self, filepath):
            x, fs = librosa.load(filepath)
            features = librosa.feature.chromagram(x, fs, n_fft=self.frame_size, hop_length=self.hop_size).T
            results = self.lsh.query(features)
            print 'num results', len(results)

            counts = dict()
            for r in results:
                if counts.has_key(r['label']):
                    counts[r['label']] += 1
                else:
                    counts[r['label']] = 1
            for k in counts:
                counts[k] = float(counts[k])/self.num_features_in_file[k]
            return counts

Train:

In \[36\]:

    ms = MusicSearch(training_files)
    ms.train()

Test:

In \[39\]:

    test_file = '../test/brennan03.wav'
    results = ms.query(test_file)

    num results 171464

Display the results:

In \[40\]:

    for r in sorted(results, key=results.get, reverse=True):
        print r, results[r]

    ../train/lady_madonna_crop.wav 64.2857142857
    ../train/lady_madonna.wav 62.9397590361
    ../train/brandenburg3_01.wav 57.91
    ../train/brahms_s1_1_perlman_02.wav 50.6964285714
    ../train/dont_stop_believin.wav 46.626984127
    ../train/konstantine.wav 45.0076335878
    ../train/Beethoven_vln_sonata5_Francescatti_01.wav 43.7115384615
    ../train/office_theme.wav 42.8313253012
    ../train/brahms_s1_1_perlman_03.wav 42.0535714286
    ../train/bach_s3_3_szeryng_01.wav 41.8928571429
    ../train/bach_p3_1_heifetz_01.wav 40.125
    ../train/Beethoven_vln_sonata5_Zukerman_01.wav 40.0377358491
    ../train/bach_p3_1_perlman_01.wav 39.6964285714
    ../train/brahms_s1_1_perlman_05.wav 37.6785714286
    ../train/brahms_s1_1_perlman_04.wav 37.4107142857
    ../train/brahms_rhapsody_02.wav 37.2
    ../train/Bach Vln Partita3 - Milstein 1955 - 03.wav 37.1609195402
    ../train/brahms_rhapsody_01.wav 36.578313253
    ../train/Beethoven_vln_sonata5_Zukerman_04.wav 36.0344827586
    ../train/bach_s3_3_szeryng_05.wav 35.8214285714
    ../train/bach_p3_1_heifetz_04.wav 34.3392857143
    ../train/Beethoven_vln_sonata5_Zukerman_02.wav 33.8208955224
    ../train/bach_p3_1_heifetz_02.wav 33.2321428571
    ../train/Beethoven_vln_sonata5_Francescatti_05.wav 33.0862068966
    ../train/bach_p3_1_heifetz_05.wav 32.8928571429
    ../train/Bach Vln Partita3 - Fischbach 2004 - 03.wav 32.6033519553
    ../train/Beethoven_vln_sonata5_Oistrakh_01.wav 32.4561403509
    ../train/Beethoven_vln_sonata5_Francescatti_02.wav 31.546875
    ../train/Beethoven_vln_sonata5_Oistrakh_04.wav 31.3928571429
    ../train/Beethoven_vln_sonata5_Francescatti_03.wav 31.0
    ../train/Beethoven_vln_sonata5_Zukerman_03.wav 30.4166666667
    ../train/moonlight.wav 30.36
    ../train/Beethoven_vln_sonata5_Francescatti_04.wav 29.641509434
    ../train/bach_s3_3_szeryng_02.wav 29.0892857143
    ../train/Bach Vln Partita3 - Milstein 1955 - 01.wav 28.8102564103
    ../train/Bach Vln Partita3 - Fischbach 2004 - 01.wav 28.6977777778
    ../train/bach_p3_1_perlman_02.wav 28.4285714286
    ../train/Beethoven_vln_sonata5_Oistrakh_05.wav 28.3181818182
    ../train/Beethoven_vln_sonata5_Zukerman_05.wav 26.6666666667
    ../train/bach_s3_3_szeryng_04.wav 26.5535714286
    ../train/Beethoven_vln_sonata5_Oistrakh_02.wav 26.4444444444
    ../train/bach_p3_1_perlman_05.wav 26.4107142857
    ../train/bach_p3_1_perlman_06.wav 26.0
    ../train/bach_p3_1_perlman_04.wav 25.6428571429
    ../train/bach_p3_1_heifetz_03.wav 25.3928571429
    ../train/bach_p3_1_perlman_03.wav 24.9464285714
    ../train/brahms_s1_1_perlman_01.wav 24.6785714286
    ../train/bach_s3_3_szeryng_03.wav 23.7678571429
    ../train/bach_s3_3_szeryng_06.wav 23.5178571429
    ../train/Bach Vln Sonata1 - Fischbach 2004 - 02.wav 23.4891774892
    ../train/Beethoven_vln_sonata5_Oistrakh_03.wav 23.0909090909
    ../train/Bach Vln Sonata1 - Milstein 1954 - 02.wav 22.6527777778
    ../train/brahms_s1_1_perlman_06.wav 17.8035714286

In \[38\]:
