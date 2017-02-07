# Scala learning

## Basics

**Value**
Give the result of an expression a name

```Scala
val two = 1 + 1
```

**Variables**
If changing the binding is needed; use a var instead.

```Scala
var name = "samir"
```

## Functions
 Specify the type signature for function parameters

```Scala
def addOne(m: Int): Int = m + 1
val three = addOne(2)
```

Leave off parens on functions with no arguments

```Scala
def addOne(m: Int): Int = m + 1
three
```

**Anonymous Functions**
```Scala
(x: Int) => x + 1
three
```

Pass anonymous functions around or save them into vals

```Scala
val addOne = (x: Int) => x + 1
three
```

Function made up of many expressions, use {}

```Scala
def timesTwo(i: Int): Int = {
  println("hello world")
  i * 2
}
```

Also for anonymous functions

```Scala
{ i: Int =>
  println("hello world")
  i * 2
}
```

**Partial application**

*{ _ + 2 }* means an unnamed parameter

```Scala
def adder(m: Int, n: Int) = m + n
val add2 = adder(2, _:Int)
```

```Bash
scala> add2(3)
res50: Int = 5
```

Its possible to also partially apply any argument in the argument list, not just the last one.

**Curried functions**

Let people apply some arguments to a function now and others later.

```Scala
def multiply(m: Int)(n: Int): Int = m * n
multiply(2)(3)
```

```Scala
val timesTwo = multiply(2) _
```

```Bash
scala> timesTwo(3)
res1: Int = 6
```

Carry entire function

```Scala
val curriedAdd = (adder _).curried
val addTwo = curriedAdd(2)
```

```Bash
scala> addTwo(4)
res22: Int = 6
```

**Variable length arguments**
There is a special syntax for methods that can take parameters of a repeated type.

```Scala
def capitalizeAll(args: String*) = {
  args.map { arg =>
    arg.capitalize
  }
}
```

```Bash
scala> capitalizeAll("rarity", "applejack")
res2: Seq[String] = ArrayBuffer(Rarity, Applejack)
```
