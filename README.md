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
