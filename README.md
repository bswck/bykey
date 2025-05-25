# bykey
Class-based and function-based dispatch dictionaries

## Key features
- Dispatch registries are validated to be complete
- Dispatch keys are controlled
- Dispatch keys can be inferred from implementers
- Dispatching by types can be implemented to avoid bloated `functools.singledispatch` where C3 resolution is unnecessary

## Class-based dispatching
```py
import bykey

@bykey.dispatching(keys=[1, 2])
class Base:
    pass

# or with Base = bykey.dispatcher(keys=[1, 2])
# you could use dispatcher as an attribute in a class as well

@Base.register(1)
class Foo:
    pass

@Base.register(2)
class Bar:
    pass

Base.dispatch(1)  # Foo
Base.dispatch(2)  # Bar
```

## Function-based dispatching
```py
import bykey

@bykey.dispatching(keys=[1, 2])
def base() -> None:
    pass

# or with base = bykey.dispatcher(keys=[1, 2])

@base.register(1)
def bar() -> None:
    pass

@base.register(2)
def biz() -> None:
    pass

base.dispatch(1)  # bar
base.dispatch(2)  # biz
```




