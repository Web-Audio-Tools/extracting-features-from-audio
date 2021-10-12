In \[1\]:

    %matplotlib inline
    import seaborn
    import numpy, scipy, scipy.spatial, matplotlib.pyplot as plt
    plt.rcParams['figure.figsize'] = (14, 3)

[← Back to Index](index.html)

# Dynamic Time Warping<a href="#Dynamic-Time-Warping" class="anchor-link">Â¶</a>

In MIR, we often want to compare two sequences of different lengths. For example, we may want to compute a similarity measure between two versions of the same song. These two signals, $x$ and $y$, may have similar sequences of chord progressions and instrumentations, but there may be timing deviations between the two. Even if we were to express the two audio signals using the same feature space (e.g. chroma or MFCCs), we cannot simply sum their pairwise distances because the signals have different lengths.

As another example, you might want to align two different performances of the same musical work, e.g. so you can hop from one performance to another at any moment in the work. This problem is known as **music synchronization** (FMP, p. 115).

**Dynamic time warping (DTW)** ([Wikipedia](https://en.wikipedia.org/wiki/Dynamic_time_warping); FMP, p. 131) is an algorithm used to align two sequences of similar content but possibly different lengths.

Given two sequences, $x\[n\], n \\in \\{0, ..., N\_x - 1\\}$, and $y\[n\], n \\in \\{0, ..., N\_y - 1\\}$, DTW produces a set of index coordinate pairs $\\{ (i, j) ... \\}$ such that $x\[i\]$ and $y\[j\]$ are similar.

We will use the same dynamic programming approach described in the notebooks [Dynamic Programming](dp.html) and [Longest Common Subsequence](lcs.html).

## Example<a href="#Example" class="anchor-link">Â¶</a>

Create two arrays, $x$ and $y$, of lengths $N\_x$ and $N\_y$, respectively.

In \[2\]:

    x = [0, 4, 4, 0, -4, -4, 0]
    y = [1, 3, 4, 3, 1, -1, -2, -1, 0]
    nx = len(x)
    ny = len(y)

In \[3\]:

    plt.plot(x)
    plt.plot(y, c='r')
    plt.legend(('x', 'y'))

Out\[3\]:

    <matplotlib.legend.Legend at 0x108828210>

![](data:image/png;base64,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%0A)

In this simple example, there is only one value or "feature" at each time index. However, in practice, you can use sequences of _vectors_, e.g. spectrograms, chromagrams, or MFCC-grams.

## Distance Metric<a href="#Distance-Metric" class="anchor-link">Â¶</a>

DTW requires the use of a distance metric between corresponding observations of `x` and `y`. One common choice is the **Euclidean distance** ([Wikipedia](https://en.wikipedia.org/wiki/Euclidean_distance); FMP, p. 454):

In \[4\]:

    scipy.spatial.distance.euclidean(0, [3, 4])

Out\[4\]:

    5.0

In \[5\]:

    scipy.spatial.distance.euclidean([0, 0], [5, 12])

Out\[5\]:

    13.0

Another choice is the **Manhattan or cityblock distance**:

In \[6\]:

    scipy.spatial.distance.cityblock(0, [3, 4])

Out\[6\]:

    7

In \[7\]:

    scipy.spatial.distance.cityblock([0, 0], [5, 12])

Out\[7\]:

    17

Another choice might be the **cosine distance** ([Wikipedia](https://en.wikipedia.org/wiki/Cosine_similarity); FMP, p. 376) which can be interpreted as the (normalized) angle between two vectors:

In \[8\]:

    scipy.spatial.distance.cosine([1, 0], [100, 0])

Out\[8\]:

    0.0

In \[9\]:

    scipy.spatial.distance.cosine([1, 0, 0], [0, 0, -1])

Out\[9\]:

    1.0

In \[10\]:

    scipy.spatial.distance.cosine([1, 0], [-1, 0])

Out\[10\]:

    2.0

For more distance metrics, see [`scipy.spatial.distance`](https://docs.scipy.org/doc/scipy/reference/spatial.distance.html).

## Step 1: Cost Table Construction<a href="#Step-1:-Cost-Table-Construction" class="anchor-link">Â¶</a>

As described in the notebooks [Dynamic Programming](dp.html) and [Longest Common Subsequence](lcs.html), we will use dynamic programming to solve this problem. First, we create a table which stores the solutions to all subproblems. Then, we will use this table to solve each larger subproblem until the problem is solved for the full original inputs.

The basic idea of DTW is to find a path of index coordinate pairs the sum of distances along the path $P$ is minimized:

$$ \\min \\sum\_{(i, j) \\in P} d(x\[i\], y\[j\]) $$

The path constraint is that, at $(i, j)$, the valid steps are $(i+1, j)$, $(i, j+1)$, and $(i+1, j+1)$. In other words, the alignment always moves forward in time for at least one of the signals. It never goes forward in time for one signal and backward in time for the other signal.

Here is the optimal substructure. Suppose that the best alignment contains index pair `(i, j)`, i.e., `x[i]` and `y[j]` are part of the optimal DTW path. Then, we prepend to the optimal path

$$ \\mathrm{argmin} \\ \\{ d(x\[i-1\], y\[j\]), d(x\[i\], y\[j-1\]), d(x\[i-1\], j-1\]) \\} $$

We create a table where cell `(i, j)` stores the optimum cost of `dtw(x[:i], y[:j])`, i.e. the optimum cost from `(0, 0)` to `(i, j)`. First, we solve for the boundary cases, i.e. when either one of the two sequences is empty. Then we populate the table from the top left to the bottom right.

In \[11\]:

    def dtw_table(x, y):
        nx = len(x)
        ny = len(y)
        table = numpy.zeros((nx+1, ny+1))

        # Compute left column separately, i.e. j=0.
        table[1:, 0] = numpy.inf

        # Compute top row separately, i.e. i=0.
        table[0, 1:] = numpy.inf

        # Fill in the rest.
        for i in range(1, nx+1):
            for j in range(1, ny+1):
                d = scipy.spatial.distance.euclidean(x[i-1], y[j-1])
                table[i, j] = d + min(table[i-1, j], table[i, j-1], table[i-1, j-1])
        return table

In \[12\]:

    table = dtw_table(x, y)

Let's visualize this table:

In \[13\]:

    print '         ', ''.join('%4d' % n for n in y)
    print '     +' + '----' * (ny+1)
    for i, row in enumerate(table):
        if i == 0:
            z0 = ''
        else:
            z0 = x[i-1]
        print ('%4s |' % z0) + ''.join('%4.0f' % z for z in row)

                 1   3   4   3   1  -1  -2  -1   0
         +----------------------------------------
         |   0 inf inf inf inf inf inf inf inf inf
       0 | inf   1   4   8  11  12  13  15  16  16
       4 | inf   4   2   2   3   6  11  17  20  20
       4 | inf   7   3   2   3   6  11  17  22  24
       0 | inf   8   6   6   5   4   5   7   8   8
      -4 | inf  13  13  14  12   9   7   7  10  12
      -4 | inf  18  20  21  19  14  10   9  10  14
       0 | inf  19  21  24  22  15  11  11  10  10

The time complexity of this operation is $O(N\_x N\_y)$. The space complexity is $O(N\_x N\_y)$.

## Step 2: Backtracking<a href="#Step-2:-Backtracking" class="anchor-link">Â¶</a>

To assemble the best path, we use **backtracking** (FMP, p. 139). We will start at the end, $(N\_x - 1, N\_y - 1)$, and backtrack to the beginning, $(0, 0)$.

Finally, just read off the sequences of time index pairs starting at the end.

In \[14\]:

    def dtw(x, y, table):
        i = len(x)
        j = len(y)
        path = [(i, j)]
        while i > 0 or j > 0:
            minval = numpy.inf
            if table[i-1, j] < minval:
                minval = table[i-1, j]
                step = (i-1, j)
            if table[i][j-1] < minval:
                minval = table[i, j-1]
                step = (i, j-1)
            if table[i-1][j-1] < minval:
                minval = table[i-1, j-1]
                step = (i-1, j-1)
            path.insert(0, step)
            i, j = step
        return path

In \[15\]:

    path = dtw(x, y, table)
    path

Out\[15\]:

    [(0, 0),
     (1, 1),
     (2, 2),
     (2, 3),
     (3, 3),
     (3, 4),
     (4, 5),
     (4, 6),
     (5, 7),
     (6, 7),
     (7, 8),
     (7, 9)]

The time complexity of this operation is $O(N\_x + N\_y)$.

As a sanity check, compute the total distance of this alignment:

In \[16\]:

    sum(abs(x[i-1] - y[j-1]) for (i, j) in path if i >= 0 and j >= 0)

Out\[16\]:

    10

Indeed, that is the same as the cumulative distance of the optimal path computed earlier:

In \[17\]:

    table[-1, -1]

Out\[17\]:

    10.0

[← Back to Index](index.html)
