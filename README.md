# Google-Foobar-Challenge
running-with-bunnies
```bash
from itertools import permutations

def solution(times, time_limit):
    n = len(times) - 2 # Number of bunnies
    for num_bunnies in range(n, -1, -1):
        for bunnies in permutations(range(1, n+1), num_bunnies):
            path = [0] + list(bunnies) + [n+1]
            min_times = list(times)
            for k in range(n+2):
                for i in range(n+2):
                    for j in range(n+2):
                        min_times[i][j] = min(min_times[i][j], min_times[i][k] + min_times[k][j])
            total_time = sum(min_times[path[i]][path[i+1]] for i in range(num_bunnies+1))
            if total_time <= time_limit:
                return sorted(list(bunny-1 for bunny in bunnies))
    return []
```
Expanding Nebula

```bash
from collections import defaultdict

def generate(c1, c2, bitlen):
    a = c1 & ~(1 << bitlen)
    b = c2 & ~(1 << bitlen)
    c = c1 >> 1
    d = c2 >> 1
    return (a & ~b & ~c & ~d) | (~a & b & ~c & ~d) | (~a & ~b & c & ~d) | (~a & ~b & ~c & d)

def build_map(n, nums):
    mapping = defaultdict(set)
    nums = set(nums)
    for i in range(1 << (n + 1)):
        for j in range(1 << (n + 1)):
            generation = generate(i, j, n)
            if generation in nums:
                mapping[(generation, i)].add(j)
    return mapping

def solution(g):
    g = list(zip(*g))  # transpose
    nrows = len(g)
    ncols = len(g[0])

    # turn map into numbers
    nums = [sum([1 << i if col else 0 for i, col in enumerate(row)]) for row in g]
    mapping = build_map(ncols, nums)

    preimage = {i: 1 for i in range(1 << (ncols + 1))}
    for row in nums:
        next_row = defaultdict(int)
        for c1 in preimage:
            for c2 in mapping[(row, c1)]:
                next_row[c2] += preimage[c1]
        preimage = next_row
    ret = sum(preimage.values())

    return ret
```
