Contains all concepts and definitions I learn.

## Noise

The variations in data what is known as 'noise', each time we measure something, we might get slightly different readings, which could be caused by various factors.

Hence why, noise is added to training data because it makes the model more robust and prevents overfitting.

```python
# Original data too 'perfect'
height = [170, 175, 180]

# Becomes more realistic
height_with_noise = [170.1, 175.9, 180.2]
```

## Random Seeds

When we set a random seed `np.radnom.seed(1)`, we are telling the computer, 'start your random number generation from this specific point'.

Useful because:
- When learning/debugging, you would want the same results
- You can make sure differences in results are due to your changes and not just a random chance

## Random state
Parameter similar to [[#Random Seeds]], when you split your data into training and testing sets, you're randomly choosing which data points go into which set, `random_state` ensures you get the same split every time you run the code.

```python
from sklearn.model_selection import train_test_split
import numpy as np

x = np.array([[1], [2], [3], [4], [5], [6], [7], [8], [9], [10]])
y = np.array([0, 0, 0, 0, 0, 1, 1, 1, 1, 1])

# without random_state - different split each time
print("Without random_state:")
X_train1, X_test1, y_train1, y_test1 = train_test_split(X, y, test_size=0.2)
print("First split:", X_test1.flatten()) # might be [7, 3]

print("Without random_state:")
X_train2, X_test2, y_train2, y_test2 = train_test_split(X, y, test_size=0.2)
print("Second split:", X_test2.flatten()) # might be [1, 9]

# with random_state
print("\nWith random state=42:")
X_train3, X_test3, y_train3, y_test3 = train_test_split(X, y, test_size=0.2, random_state=42)
print("Third split:", X_test3.flatten()) # for example maybe its [8, 9]

X_train4, X_test4, y_train4, y_test4 = train_test_split(X, y, test_size=0.2, random_state=42)
print("Fourth split:", X_test4.flatten()) # going to be the same [8, 9]
```

The actual number 42 doesn't matter at all, just a common convention among programmers.
