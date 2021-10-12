In \[1\]:

    import numpy, scipy, matplotlib.pyplot as plt, pandas, librosa

[← Back to Index](index.html)

# NumPy and SciPy<a href="#NumPy-and-SciPy" class="anchor-link">¶</a>

The quartet of NumPy, SciPy, Matplotlib, and IPython is a popular combination in the Python world. We will use each of these libraries in this workshop.

## Tutorial<a href="#Tutorial" class="anchor-link">¶</a>

[NumPy](http://www.numpy.org) is one of the most popular libraries for numerical computing in the world. It is used in several disciplines including image processing, finance, bioinformatics, and more. This entire workshop is based upon NumPy and its derivatives.

If you are new to NumPy, follow this [NumPy Tutorial](http://wiki.scipy.org/Tentative_NumPy_Tutorial).

[SciPy](http://docs.scipy.org/doc/scipy/reference/) is a Python library for scientific computing which builds on top of NumPy. If NumPy is like the Matlab core, then SciPy is like the Matlab toolboxes. It includes support for linear algebra, sparse matrices, spatial data structions, statistics, and more.

While there is a [SciPy Tutorial](http://docs.scipy.org/doc/scipy/reference/tutorial/index.html), it isn't critical that you follow it for this workshop.

## Special Arrays<a href="#Special-Arrays" class="anchor-link">¶</a>

In \[2\]:

    print numpy.arange(5)

    [0 1 2 3 4]

In \[3\]:

    print numpy.linspace(0, 5, 10, endpoint=False)

    [ 0.   0.5  1.   1.5  2.   2.5  3.   3.5  4.   4.5]

In \[4\]:

    print numpy.zeros(5)

    [ 0.  0.  0.  0.  0.]

In \[5\]:

    print numpy.ones(5)

    [ 1.  1.  1.  1.  1.]

In \[6\]:

    print numpy.ones((5,2))

    [[ 1.  1.]
     [ 1.  1.]
     [ 1.  1.]
     [ 1.  1.]
     [ 1.  1.]]

In \[7\]:

    print scipy.randn(5) # random Gaussian, zero-mean unit-variance

    [ -1.12009510e+00   2.15875646e-03  -7.93208376e-01  -1.02710782e+00
       2.37388108e+00]

In \[8\]:

    print scipy.randn(5,2)

    [[-0.60527349 -0.82200312]
     [-0.67330474 -0.12914043]
     [-0.71574719 -0.5962005 ]
     [-1.03690426  0.59078457]
     [-2.22983691 -1.70858604]]

## Slicing Arrays<a href="#Slicing-Arrays" class="anchor-link">¶</a>

In \[9\]:

    x = numpy.arange(10)
    print x[2:4]

    [2 3]

In \[10\]:

    print x[-1]

    9

The optional third parameter indicates the increment value:

In \[11\]:

    print x[0:8:2]

    [0 2 4 6]

In \[12\]:

    print x[4:2:-1]

    [4 3]

If you omit the start index, the slice implicitly starts from zero:

In \[13\]:

    print x[:4]

    [0 1 2 3]

In \[14\]:

    print x[:999]

    [0 1 2 3 4 5 6 7 8 9]

In \[15\]:

    print x[::-1]

    [9 8 7 6 5 4 3 2 1 0]

## Array Arithmetic<a href="#Array-Arithmetic" class="anchor-link">¶</a>

In \[16\]:

    x = numpy.arange(5)
    y = numpy.ones(5)
    print x+2*y

    [ 2.  3.  4.  5.  6.]

`dot` computes the dot product, or inner product, between arrays or matrices.

In \[17\]:

    x = scipy.randn(5)
    y = numpy.ones(5)
    print numpy.dot(x, y)

    -4.36027379404

In \[18\]:

    x = scipy.randn(5,3)
    y = numpy.ones((3,2))
    print numpy.dot(x, y)

    [[ 0.9351335   0.9351335 ]
     [-4.22851009 -4.22851009]
     [-2.66983557 -2.66983557]
     [ 3.18545804  3.18545804]
     [ 1.82532797  1.82532797]]

## Boolean Operations<a href="#Boolean-Operations" class="anchor-link">¶</a>

In \[19\]:

    x = numpy.arange(10)
    print x < 5

    [ True  True  True  True  True False False False False False]

In \[20\]:

    y = numpy.ones(10)
    print x < y

    [ True False False False False False False False False False]

## Distance Metrics<a href="#Distance-Metrics" class="anchor-link">¶</a>

In \[21\]:

    from scipy.spatial import distance
    print distance.euclidean([0, 0], [3, 4])
    print distance.sqeuclidean([0, 0], [3, 4])
    print distance.cityblock([0, 0], [3, 4])
    print distance.chebyshev([0, 0], [3, 4])

    5.0
    25.0
    7
    4

The cosine distance measures the angle between two vectors:

In \[22\]:

    print distance.cosine([67, 0], [89, 0])
    print distance.cosine([67, 0], [0, 89])

    0.0
    1.0

## Sorting<a href="#Sorting" class="anchor-link">¶</a>

NumPy arrays have a method, `sort`, which sorts the array _in-place_.

In \[23\]:

    x = scipy.randn(5)
    print x
    x.sort()
    print x

    [ 0.70589021  0.14767722  0.06884379  0.37189002  0.43313129]
    [ 0.06884379  0.14767722  0.37189002  0.43313129  0.70589021]

`numpy.argsort` returns an array of indices, `ind`, such that `x[ind]` is a sorted version of `x`.

In \[24\]:

    x = scipy.randn(5)
    print x
    ind = numpy.argsort(x)
    print ind
    print x[ind]

    [ 0.9443719   0.2831604   0.85627     0.22827583 -0.03939166]
    [4 3 1 2 0]
    [-0.03939166  0.22827583  0.2831604   0.85627     0.9443719 ]

[← Back to Index](index.html)
