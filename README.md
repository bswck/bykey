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

@bykey.dispatching(keys=[1, 2], common_suffix="Strategy")
class Base:
    pass

@bykey.register(1, Base)
class Foo:
    pass

@bykey.register(2)  # registry Base inferred from Bar's superclass Base
class Bar(Base):
    pass

@bykey.register(3)  # registry Base inferred from Bar's common suffix Strategy
class BarStrategy:
    pass

Base.dispatch(1)  # Foo
Base.dispatch(2)  # Bar
```

## Function-based dispatching
```py
import bykey

@bykey.dispatching(keys=[1, 2], common_suffix="_strategy")
def base() -> None:
    pass

@bykey.register(1, base)  # registry of bar inferred from common suffix "_strategy"
def bar_strategy() -> None:
    pass

@bykey.register(2, base)
def biz() -> None:
    pass

base.dispatch(1)  # bar_strategy
base.dispatch(2)  # biz
```




