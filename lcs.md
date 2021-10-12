[← Back to Index](index.html)

# Longest Common Subsequence<a href="#Longest-Common-Subsequence" class="anchor-link">Â¶</a>

To motivate dynamic time warping, let's look at a classic dynamic programming problem: find the **longest common subsequence (LCS)** of two strings ([Wikipedia](https://en.wikipedia.org/wiki/Longest_common_subsequence_problem)). A subsequence is not required to maintain consecutive positions in the original strings, but they must retain their order. Examples:

    lcs('cake', 'baker') -> 'ake'
    lcs('cake', 'cape') -> 'cae'

We can solve this using recursion. We must find the optimal substructure, i.e. decompose the problem into simpler subproblems.

Let `x` and `y` be two strings. Let `z` be the true LCS of `x` and `y`.

If the first characters of `x` and `y` were the same, then that character must also be the first character of the LCS, `z`. In other words, if `x[0] == y[0]`, then `z[0]` must equal `x[0]` (which equals `y[0]`). Therefore, append `x[0]` to `lcs(x[1:], y[1:])`.

If the first characters of `x` and `y` differ, then solve for both `lcs(x, y[1:])` and `lcs(x[1:], y)`, and keep the result which is longer.

Here is the recursive solution:

In \[1\]:

    def lcs(x, y):
        if x == "" or y == "":
            return ""
        if x[0] == y[0]:
            return x[0] + lcs(x[1:], y[1:])
        else:
            z1 = lcs(x[1:], y)
            z2 = lcs(x, y[1:])
            return z1 if len(z1) > len(z2) else z2

Test:

In \[2\]:

    pairs = [
        ('cake', 'baker'),
        ('cake', 'cape'),
        ('catcga', 'gtaccgtca'),
        ('zxzxzxmnxzmnxmznmzxnzm', 'nmnzxmxzmnzmx'),
        ('dfkjdjkfdjkjfdkfdkfjd', 'dkfjdjkfjdkjfkdjfkjdkfjdkfj'),
    ]

In \[3\]:

    for x, y in pairs:
        print lcs(x, y)

    ake
    cae
    ctca
    zxmxzmnzmx
    dfjdjkfdjkjfdkfdkfj

The time complexity of the above recursive method is $O(2^{n\_x+n\_y})$. That is slow because we might compute the solution to the same subproblem multiple times.

### Memoization<a href="#Memoization" class="anchor-link">Â¶</a>

We can do better through memoization, i.e. storing solutions to previous subproblems in a table.

Here, we create a table where cell `(i, j)` stores the length `lcs(x[:i], y[:j])`. When either `i` or `j` is equal to zero, i.e. an empty string, we already know that the LCS is the empty string. Therefore, we can initialize the table to be equal to zero in all cells. Then we populate the table from the top left to the bottom right.

In \[4\]:

    def lcs_table(x, y):
        nx = len(x)
        ny = len(y)

        # Initialize a table.
        table = [[0 for _ in range(ny+1)] for _ in range(nx+1)]

        # Fill the table.
        for i in range(1, nx+1):
            for j in range(1, ny+1):
                if x[i-1] == y[j-1]:
                    table[i][j] = 1 + table[i-1][j-1]
                else:
                    table[i][j] = max(table[i-1][j], table[i][j-1])
        return table

Let's visualize this table:

In \[5\]:

    x = 'cake'
    y = 'baker'
    table = lcs_table(x, y)
    table

Out\[5\]:

    [[0, 0, 0, 0, 0, 0],
     [0, 0, 0, 0, 0, 0],
     [0, 0, 1, 1, 1, 1],
     [0, 0, 1, 2, 2, 2],
     [0, 0, 1, 2, 3, 3]]

In \[6\]:

    xa = ' ' + x
    ya = '  ' + y
    print ' '.join(ya)
    for i, row in enumerate(table):
        print xa[i], ' '.join(str(z) for z in row)

        b a k e r
      0 0 0 0 0 0
    c 0 0 0 0 0 0
    a 0 0 1 1 1 1
    k 0 0 1 2 2 2
    e 0 0 1 2 3 3

Finally, we backtrack, i.e. read the table from the bottom right to the top left:

In \[7\]:

    def lcs(x, y, table, i=None, j=None):
        if i is None:
            i = len(x)
        if j is None:
            j = len(y)
        if table[i][j] == 0:
            return ""
        elif x[i-1] == y[j-1]:
            return lcs(x, y, table, i-1, j-1) + x[i-1]
        elif table[i][j-1] > table[i-1][j]:
            return lcs(x, y, table, i, j-1)
        else:
            return lcs(x, y, table, i-1, j)

In \[8\]:

    for x, y in pairs:
        table = lcs_table(x, y)
        print lcs(x, y, table)

    ake
    cae
    ctca
    zxmxzmnzmx
    dfjdjkfdjkjfdkfdkfj

Table construction has time complexity $O(mn)$, and backtracking is $O(m+n)$. Therefore, the overall running time is $O(mn)$.

[← Back to Index](index.html)
