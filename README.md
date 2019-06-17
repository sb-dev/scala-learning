# Basics

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
def three() = 1 + 2
three()
three
```

**Anonymous Functions**
```Scala
(x: Int) => x + 1
```

Pass anonymous functions around or save them into vals

```Scala
val addOne = (x: Int) => x + 1
addOne(1)
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

Partially apply a function with an underscore, which gives us another function:

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

## Classes

```Scala
class calculator {
  val brand: String = "HP"
  def add(m: Int, n: Int): Int = m + n
}
```

```Bash
scala> val calc = new Calculator
calc: Calculator = Calculator@e75a11

scala> calc.add(1, 2)
res1: Int = 3

scala> calc.brand
res2: String = "HP"
```

**Constructor**

Constructors aren’t special methods, they are the code outside of method definitions in your class.

```Scala
class Calculator(brand: String) {
  val color: String = if (brand == "TI") {
    "blue"
  } else if (brand == "HP") {
    "black"
  } else { 
    "white"
  }
}
```

## Inheritance

```Scala
class ScientificCalculator(brand: String) extends Calculator(brand) {
  def log(m: Double, base: Double) = math.log(m) / math.log(base)
}
```

**Overloading**

```Scala
class EvenMoreScientificCalculator(brand: String) extends ScientificCalculator(brand) {
  def log(m: Int): Double = log(m, math.exp(1))
}
```

**Abstract classes**

```Scala
abstract class Shape {
  def getArea(): Int
}

class Circle(r: Int) extends Shape {
  def getArea(): Int = (r * r * 3)
}
```

**Traits**

Traits are collections of fields and behaviors that we can extend or mixin to a class.

```Scala
trait Car {
  val brand: String
}

trait Shiny {
  val ShineRefraction: Int
}
```

```Scala
class BMW extends Car {
  val brand = "BMW"
}
```

Extend several traits using the *with* keyword:

```Scala
class BMW extends Car with Shiny {
  val brand = "BMW"
  val shineRefraction = 12
}
```

**Types**

Function can also be generic and work on any type

```Scala
trait Cache[K, V] {
  def get(key: K): V
  def put(key: K, value: V)
  def delete(key: K)
}
```

Methods can also have type parameters introduced

```Scala
def remove[K] (key: K)
```