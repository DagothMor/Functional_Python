
Сначала стартовая позиция


```
def begin():
    return Just([0, 0])
```

условимся, что Nothing это то состояние монады, после которого ничего происходить в принципе и не будет, или же, упал\смердь
(труп не сможет стать героем и спасти принцессу из башни убив дракона, если это не в рик и морти)

Далее добавляем функции прилета птиц слева и справа

```
@curry(2)
def to_left(num, pole):
    left, right = pole
    left += num

    if abs(left - right) > 4:
        return Nothing

    return Just([left, right])


@curry(2)
def to_right(num, pole):
    left, right = pole
    right += num

    if abs(left - right) > 4:
        return Nothing

    return Just([left, right])


```

С банановой кожурой так же 

```python
def banana(pole):
    return Nothing
```

функция показывания результата.

```
def show(kanatMan):
    if kanatMan == Nothing:
        print("gg")
    else:
        left, right = kanatMan.value
        print(f"жив: слева {left} птиц, справа {right} птиц")

```

Теперь нам достаточно вызвать

```
show(
    begin()
    .bind(to_left(2))
    .bind(to_right(5))
    .bind(to_left(-1))
)
```