# 3.1

```python
@curry(2)
def tag(name, value):
    return f"<{name}>{value}</{name}>"

italic = tag("i")

bold = tag("b")

```

# 3.2

f-строка тяжело идет, как и сам синтаксис)

```python
@curry(3)
def tag(name, attr, value):
    attributes = "".join(
        f' {key}="{val}"'
        for key, val in attr.items()
    )
    return f"<{name}{attributes}>{value}</{name}>"
```