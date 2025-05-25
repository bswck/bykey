# bykey

> [!Warning]
> Work in progress.

Class-based and function-based dispatch dictionaries backed by Rust.
Generalizes the idea of `functools.singledispatch` for arbitrary constructs and fits better for the simplest cases (when no abstract classes are involved).

## Key features
- Dispatch registries are validated to be complete
- Dispatch keys are controlled
- Dispatching by types without the bloated `functools.singledispatch` for situations when C3 resolution is unnecessary

## Examples
### Class-based dispatching
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

### Function-based dispatching

#### From argument
```py
import bykey

@bykey.argument_dispatching(keys=[1, 2])
def base(arg: int) -> None:
    pass

@base.register(1)
def bar(arg: int) -> None:
    pass

@base.register(2)
def biz(arg: int) -> None:
    pass

base(1)  # -> bar(1)
base(2)  # -> biz(2)
```


#### From argument type invariant
```py
import bykey
from typing import Never

@bykey.argument_type_dispatching(keys=[str, int])
def base(arg: Never) -> None:
    pass

@base.register
def bar(arg: str) -> None:
    pass

@base.register
def biz(arg: int) -> None:
    pass

base("foo")  # -> bar("foo)
base(2)  # -> biz(2)
```

#### From manual `.dispatch()`
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

### Mixed dispatching

TODO


