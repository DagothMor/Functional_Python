# 2.3.1

```python
@curry(2)
def concat(prefix, text):
    return prefix + text

hi = concat("Hello, ")

print(hi("Petya"))
```


# 2.3.2

Сначала обьявляем функцию
```python
@curry(4)
def salam(workd, punctuation, name, ending):
    return f"{word}{punctuation} {name}{ending}"
```

После обьявляем функцию которая заранее хранит в себе уже готовый паттерн

```python
@curry(3)
def first_settings(word, punctuation, ending):
    return lambda name: greet(word, punctuation, name, ending)

final = first_serrings("Hello")(",")("!")

```

Далее можем спокойно пользоваться и вызывать

```python
print(final("Sasha"))
print(final("Jool"))
```