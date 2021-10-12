[← Back to Index](index.html)

# Jupyter Basics<a href="#Jupyter-Basics" class="anchor-link">Â¶</a>

You are looking at a **Jupyter Notebook**, an interactive Python shell inside of a web browser. With it, you can run individual Python commands and immediately view their output. It's basically like the Matlab Desktop or Mathematica Notebook but for Python.

To start an interactive Jupyter notebook on your local machine, read the [instructions at the GitHub `README` for this repository](https://github.com/stevetjoa/stanford-mir#how-to-use-this-repo).

If you are reading this notebook on <http://musicinformationretrieval.com>, you are viewing a read-only version of the notebook, not an interactive version. Therefore, the instructions below do not apply.

## Tour<a href="#Tour" class="anchor-link">Â¶</a>

If you're new, we recommend that you take the _User Interface Tour_ in the Help Menu above.

## Cells<a href="#Cells" class="anchor-link">Â¶</a>

A Jupyter Notebook is comprised of **cells**. Cells are just small units of code or text. For example, the text that you are reading is inside a _Markdown_ cell. (More on that later.)

_Code_ cells allow you to edit, execute, and analyze small portions of Python code at a time. Here is a code cell:

In \[1\]:

    1+2

Out\[1\]:

    3

## Modes<a href="#Modes" class="anchor-link">Â¶</a>

The Jupyter Notebook has two different keyboard input modes.

In **Edit Mode**, you type code/text into a cell. Edit Mode is indicated by a _green_ cell border.

To enter Edit Mode from Command Mode, press `Enter`. You can also double-click on a cell.

To execute the code inside of a cell and move to the next cell, press **`Shift-Enter`**. (`Ctrl-Enter` will run the current cell without moving to the next cell. This is useful for rapidly tweaking the current cell.)

In **Command Mode**, you can perform notebook level actions such as navigating among cells, selecting cells, moving cells, saving notebooks, displaying help. Command Mode is indicated by a _grey_ cell border.

To enter Command Mode from Edit Mode, press **`Esc`**. Other commands can also enter Command Mode, e.g. `Shift-Enter`.

To display the Help Menu from Command Mode, press **`h`**. _Use it often_; `h` is your best friend.

## Saving<a href="#Saving" class="anchor-link">Â¶</a>

Your code goes directly into a Jupyter notebook. To save your changes, click on the "Save" icon in the menu bar, or type **`s`** in command mode.

If this notebook is in a Git repo, use `git checkout -- <file>` to revert a saved edit.

## Writing Text in Markdown<a href="#Writing-Text-in-Markdown" class="anchor-link">Â¶</a>

Markdown is simply a fancy way of formatting plain text. It is a markup language that is a superset of HTML. The Markdown specification is found here: <http://daringfireball.net/projects/markdown/basics/>

A cell may contain Python code or Markdown code. To convert any Python cell to a Markdown cell, press **`m`**. To convert from a Markdown cell to a Python cell, press **`y`**.

For headings, we recommend that you use Jupyter's keyboard shortcuts. To change the text in a cell to a level-3 header, simply press `3`. For similar commands, press **`h`** to view the Help menu.

## Writing Text in $\\LaTeX$<a href="#Writing-Text-in-$\LaTeX$" class="anchor-link">Â¶</a>

In a Markdown cell, you can also use $\\LaTeX$ syntax. Example input:

    $$ \max_{||w||=1} \sum_{i=1}^{N} \big| \langle w, x_i - m \rangle \big|^2 $$

Output:

$$ \\max\_{||w||=1} \\sum\_{i=1}^{N} \\big| \\langle w, x_i - m \\rangle \\big|^2 $$

## Imports<a href="#Imports" class="anchor-link">Â¶</a>

You may encounter the following imports while using this website:

In \[2\]:

    import numpy
    import scipy
    import pandas
    import sklearn
    import seaborn
    import matplotlib
    import matplotlib.pyplot as plt
    import librosa
    import librosa.display
    import IPython.display as ipd

You can also combine imports on one line:

In \[3\]:

    import numpy, scipy, pandas

## Tab Autocompletion<a href="#Tab-Autocompletion" class="anchor-link">Â¶</a>

Tab autocompletion works in Command Window and the Editor. After you type a few letters, press the `Tab` key and a popup will appear and show you all of the possible completions, including variable names and functions. This prevents you from mistyping the names of variables -- a big time saver!

For example, type `scipy.` and then press `Tab`. You should see a list of members in the Python package `scipy`.

Or type `scipy.sin`, then press `Tab` to view members that begin with `sin`.

In \[ \]:

    # Press Tab at the end of the following line
    scipy.sin

## Inline Documentation<a href="#Inline-Documentation" class="anchor-link">Â¶</a>

To get help on a certain Python object, type `?` after the object name, and run the cell:

In \[5\]:

    # Run this cell.
    int?

In addition, if you press `Shift-Tab` in a code cell, a help dialog will also appear. For example, in the cell above, place your cursor after `int`, and press `Shift-Tab`. Press `Shift-Tab` twice to expand the help dialog.

## More Documentation: NumPy, SciPy, Matplotlib<a href="#More-Documentation:-NumPy,-SciPy,-Matplotlib" class="anchor-link">Â¶</a>

In the top menu bar, click on Help, and you'll find a prepared set of documentation links for IPython, NumPy, SciPy, Matplotlib, and Pandas.

## Experimenting<a href="#Experimenting" class="anchor-link">Â¶</a>

Code cells are meant to be interactive. We may present you with several options for experimentation, e.g. choices of variables, audio files, and algorithms. For example, if you see a cell like this, then try all of the possible options by uncommenting the desired line(s) of code. (To run the cell, select "Cell" and "Run" from the top menu, or press `Shift-Enter`.)

In \[6\]:

    x = scipy.arange(50)
    # Try these too:
    # x = scipy.randn(50)
    # x = scipy.linspace(0, 1, 50, endpoint=False)
    x

Out\[6\]:

    array([ 0,  1,  2,  3,  4,  5,  6,  7,  8,  9, 10, 11, 12, 13, 14, 15, 16,
           17, 18, 19, 20, 21, 22, 23, 24, 25, 26, 27, 28, 29, 30, 31, 32, 33,
           34, 35, 36, 37, 38, 39, 40, 41, 42, 43, 44, 45, 46, 47, 48, 49])

[← Back to Index](index.html)
