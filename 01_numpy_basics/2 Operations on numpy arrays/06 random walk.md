## 4.7 Example: Random Walks

A random walk starts at 0 and moves by either `+1` or `-1` at each step.

### Pure Python

```python
import random

position = 0
walk = [position]

nsteps = 1000

for _ in range(nsteps):
    step = 1 if random.randint(0, 1) else -1
    position += step
    walk.append(position)
```

### NumPy Version

Generate all random steps at once and compute their cumulative sum:

```python
nsteps = 1000

rng = np.random.default_rng(seed=12345)

draws = rng.integers(0, 2, size=nsteps)
steps = np.where(draws == 0, 1, -1)

walk = steps.cumsum()
```

### Statistics

```python
walk.min()
```

Output:

```python
-8
```

```python
walk.max()
```

Output:

```python
50
```

### First Crossing Time

Find when the walk first reaches a distance of at least 10 from the origin:

```python
(np.abs(walk) >= 10).argmax()
```

Output:

```python
155
```

### Key Points

- A random walk is the cumulative sum of random steps.
- `np.where()` converts random draws into `+1` and `-1`.
- `cumsum()` efficiently computes the walk path.
- `min()` and `max()` show the walk's range.
- `argmax()` finds the first index where a condition becomes `True`.


## Simulating Many Random Walks at Once

NumPy can simulate thousands of random walks simultaneously using a 2D array.

### Generate 5,000 Random Walks

```python
nwalks = 5000
nsteps = 1000

draws = rng.integers(0, 2, size=(nwalks, nsteps))
steps = np.where(draws > 0, 1, -1)

walks = steps.cumsum(axis=1)
```

- Each row represents one random walk.
- `axis=1` computes the cumulative sum across each row.

### Maximum and Minimum Values

```python
walks.max()
```

Output:

```python
114
```

```python
walks.min()
```

Output:

```python
-120
```

### Walks That Reach ±30

```python
hits30 = (np.abs(walks) >= 30).any(axis=1)
```

Count how many walks reach `30` or `-30`:

```python
hits30.sum()
```

Output:

```python
3395
```

### First Crossing Time

```python
crossing_times = (
    np.abs(walks[hits30]) >= 30
).argmax(axis=1)
```

Example output:

```python
array([201, 491, 283, ..., 219, 259, 541])
```

### Average Crossing Time

```python
crossing_times.mean()
```

Output:

```python
500.57
```

### Using Other Step Distributions

Instead of coin flips (`±1`), steps can come from a normal distribution:

```python
draws = 0.25 * rng.standard_normal((nwalks, nsteps))
```

This produces random steps with a normal distribution and standard deviation `0.25`.

### Key Points

- Use a 2D array to simulate many random walks at once.
- `cumsum(axis=1)` computes each walk's trajectory.
- `any(axis=1)` checks conditions across each walk.
- `argmax(axis=1)` finds first crossing times.
- Vectorization makes large simulations extremely efficient.