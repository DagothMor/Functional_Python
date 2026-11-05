```python
@curry(2)
def add(x, y):
    return x + y


def lazyAdd(func):
    return func.map(add(10))
```