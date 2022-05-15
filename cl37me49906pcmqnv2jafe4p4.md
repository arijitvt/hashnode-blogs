## Building a Chain of Command with Function Composition

Functions are  key block for composition and now functional languages gaining more momentum, objects have yield their glories to functions.  

Here we will try compose two functions into another function and execute that function at some later point in the future.  The example functions themselves are really simple,  as their implementation immaterial to the concept. 
`foo` take  a string, prints it and to make it a little dynamic, it returns the length of the string. `bar` on the other hand takes an integer and prints that out and returns 0. Simple, huh?!
Now we want to write  another function, that takes these two functions and returns a composite function (but it does not actually execute function, passed as arguments). My mental program model is, `compositor(f, g) -> g(f)`(note it __NOT__  `g(f())`, so the result of `compositor` can be stored in a variable and later opens a more complex functionality.  For example,

 ```
result = compositor(f,g); 
# tons of other code
result()
```
 
However simple this looks like, this creates a very powerful way to creating a more complex functions from two or more arbitrary functions, using `compsitor(f_1,compositor(f2,......))`.


```python
#!/usr/bin/env python3 
import sys 

def compositor( f, g,name):
    return lambda: g(f(name))

def foo(name : str) ->int:
    print(f"This is foo {name}")
    return len(name)

def bar(v :int):
    print("This is bar {v}")
    return  0


def main(): 
    name = "arijit"
    result = compositor(foo,bar,name)
    result()
    return 0


if __name__ == "__main__":
    sys.exit(main())
``` 

However in the previous implementation the argument of `foo` has to be passed in the `compositor`, i.e. the value of the argument of `foo` has to be available the point of the composition. This is very limiting in terms of its capability. For most of the cases, when we set a handler, the arguments for the handlers are not available. So we will optimize that and do a late binding of the argument at the callsite. 

```python
#!/usr/bin/env python3 
import sys 

"""
Version 2 in the lazy binding
"""

def compositor( f, g):
    return lambda name: g(f(name))

def foo(name : str) ->int:
    print(f"This is foo {name}")
    return 0

def bar(v):
    print("This is bar")
    return  0


def main(): 
    name = "arijit"
    result = compositor(foo,bar)
    result(name)
    return 0


if __name__ == "__main__":
    sys.exit(main())
``` 
