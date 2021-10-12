In \[1\]:

    %matplotlib inline
    import numpy, scipy, scipy.spatial, matplotlib.pyplot as plt
    plt.rcParams['figure.figsize'] = (14, 2)

[← Back to Index](index.html)

Dynamic Programming<a href="#Dynamic-Programming" class="anchor-link">¶</a>
===========================================================================

**Dynamic programming** ([Wikipedia](https://en.wikipedia.org/wiki/Dynamic_programming); FMP, p. 137) is a method for solving problems by breaking them into simpler subproblems, each solved only once, and storing their solutions for future reference. The act of storing solutions to subproblems is known as **memoization** ([Wikipedia](https://en.wikipedia.org/wiki/Memoization)).

Example: min coin sum<a href="#Example:-min-coin-sum" class="anchor-link">¶</a>
-------------------------------------------------------------------------------

Given a positive value and a list of possible coin values, write a function that determines the minimum number of coins needed to achieve the input value. Example:

    coins = [1, 5, 10, 25]
    min_coin_sum(1) -> 1
    min_coin_sum(6) -> 2 # 1*5 + 1*1
    min_coin_sum(49) -> 7 # 1*25 + 2*10 + 4*1
    min_coin_sum(52) -> 4 # 2*25 + 2*1

Suppose that we use the coin values `[1, 5, 10, 25]`. The recursive solution to this uses the observation:

    min_coin_sum(val) = 1 + min(
        min_coin_sum(val-1),
        min_coin_sum(val-5),
        min_coin_sum(val-10),
        min_coin_sum(val-25),
    )

Suppose `val = 49`. Notice that

    49 = 48 + 1 
    49 = 44 + 5
    49 = 39 + 10
    49 = 24 + 25

In other words, 49 is achieved by adding one coin to the minimum coin total used to achieve 48, 44, 39, or 24.

Let's create a recursive solution:

In \[2\]:

    def min_coin_sum(val, coins=None):
        if coins is None:
            coins = [1, 5, 10, 25]
        if val == 0:
            return 0
        return 1 + min(min_coin_sum(val-coin) for coin in coins if val-coin >= 0)

Test:

In \[4\]:

    coins = [1, 5, 10, 25]
    print('val num_coins')
    for val in (1, 6, 42, 49):
        print('%3d %d' % (val, min_coin_sum(val, coins)))

    val num_coins
      1 1
      6 2
     42 5
     49 7

Notice how it takes a little while to compute the answer to `49`? That's because the recursive solution is computing the solution to subproblems multiple times. For example, the solution to `49` uses the solutions to `44` and `39`. However, the solution to `44` itself needs the solution to `39` which it needlessly computes again from scratch.

### Memoization<a href="#Memoization" class="anchor-link">¶</a>

A better solution is to use *memoization* by storing the answer to previous subproblems and referring to them later:

In \[5\]:

    def min_coin_sum(val, coins=None):
        if coins is None:
            coins = [1, 5, 10, 25]

        # Initialize table.
        table = [0 for _ in range(val+1)]
        
        # Dynamic programming.
        for cur_val in range(1, val+1):
            table[cur_val] = 1 + min([table[cur_val-coin] for coin in coins if cur_val-coin >= 0])
        return table[val]

In \[6\]:

    coins = [1, 5, 10, 25]
    print('val num_coins')
    for val in (1, 6, 42, 49, 52):
        print('%3d %d' % (val, min_coin_sum(val, coins)))

    val num_coins
      1 1
      6 2
     42 5
     49 7
     52 4

Notice how this executes much faster than the fully recursive version. The time complexity is only $O(v n)$, where $v$ is the input coin value, and $n$ is the number of coin values.

[← Back to Index](index.html)
