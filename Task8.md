```python
from functools import reduce
def secMax(numbers):
    numbers = iter(numbers)
    first = next(numbers)
    second = next(numbers)

    initial = (first, second) 
        if first >= second 
        else (second, first)

    def update(pair, number):
        bigger, runner_up = pair
        if number >= bigger:
            return number, bigger

        return bigger, max(runner_up, number)

    return reduce(update, numbers, initial)[1]
```