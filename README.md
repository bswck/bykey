# bykey

> [!Warning]
> Work in progress.

Generalizes the idea of `functools.singledispatch` for arbitrary constructs and fits better for the simplest cases (when no abstract classes are involved).
Implemented in Cython.

## Key features
Think of this library like a cleaner way of gathering strategies in dictionaries.

- Dispatch registries are validated to be complete
- Dispatch keys are controlled
- Conflicts are reported at definition time
- Dispatching by types without the bloated `functools.singledispatch` for situations when C3 resolution is unnecessary

## Examples
### General dispatching
```py
import bykey

disp = bykey.dispatcher(keys=[1, 2])

@disp.register(1)
class Foo:
    pass

@disp.register(2)
class Bar:
    pass

disp(1)  # Foo
disp.dispatch(2)  # Bar
```

### Callable dispatching

#### From argument
```py
import bykey

@bykey.argument_dispatch(keys=[1, 2])
def base(arg: int) -> None:
    pass

@base.register(1)
class Bar:
    def __init__(arg: int) -> None:
        pass

@base.register(2)
def biz(arg: int) -> None:
    pass

base(1)  # -> Bar(1)
base(2)  # -> biz(2)
```


#### From argument type invariant
```py
import bykey
from typing import Never

@bykey.type_dispatch(keys=[str, int])
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

@bykey.dispatch(keys=[1, 2])
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
