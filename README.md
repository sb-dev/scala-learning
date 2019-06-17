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

## Types

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

## Apply methods

```Scala
object FooMaker {
  def Apply() = 0
}
```

```Bash
scala> val newFoo = FooMaker()
newFoo: Foo = Foo@5b83f762
```

```Scala
class Bar {
  def Apply() = 0
}
```

```Bash
scala> val bar = new Bar
bar: Bar = Bar@5b83f762

scala> bar()
res0: Int = 0
```

## Objects

Objects are used to hold a single instance of a class. Often used for factories.

```Scala
object Timer {
  var count = 0

  def currentCount(): Long = {
    count += 1
    count
  }
}
```

```Bash
scala> Timer.currentCount()
res0: Long = 1
```

Classes and Objects can have the name. The object is called a 'Companion Object'. Companion objects are commonly used for Factories.

```Scala
class Bar(foo: String)

object Bar {
  def apply(foo: String) = new Bar(foo)
}
```

## Functions are objects

A function is a set of traits. Specifically, a function that takes one argument is an instance of a Function1 trait.

```Scala
class AddOne extends Function1[Int, Int] {
  def apply(m: Int): Int = m + 1
}
```

*__Note__: There is Function0 through 22.*

Short-hand:

```Scala
class AddOne extends (Int => Int) {
  def apply(m: Int): Int = m + 1
}
```

```Bash
scala> val plusOne = new AddOne()
plusOne: AddOne = <function1>

scala> plusOne(1)
res0: Int = 2
```

## Packages

We can organise code inside of packages.

```Scala
package com.example
```

Object are a useful tool for organising static functions.

## Pattern matching

*Matching on values*

```Scala
times match {
  case 1 => "one"
  case 2 => "two"
  case _ => "some other number"
}
```

*Matching with Guards*

```Scala
times match {
  case i if i == 1 => "one"
  case i if i == 2 => "two"
  case _ => "some other number"
}
```

*Matching on type*

```Scala
def bigger(o: Any): Any = {
  o match {
    case i: Int if i < 0 => i - 1
    case i: Int => i + 1
    case d: Double if d < 0.0 => d - 0.1
    case d: Double => d + 0.1
    case text: String => text + "s"
  }
}
```

*Matching on class member*

```Scala
def calcType(calc: Calculator) = calc match {
  case _ if calc.brand == "HP" && calc.model == "20B" => "financial"
  case _ if calc.brand == "HP" && calc.model == "48G" => "scientific"
  case _ if calc.brand == "HP" && calc.model == "30B" => "business"
  case _ => "unknown"
}
```

## Case classes

Case classes are used to conveniently store and match on the contents of a class

```Scala
case class Calculator(brand: String, model: String)

val hp20b = Calculator("HP", "20b")
```

Case classes automatically have equality and toString methods:

```Scala
hp20b == hp20B
val hp20B = Calculator("HP", "20b")
```

```Bash
scala> val plusOne = new AddOne()
res0: Boolean = true
```

Case classes are designed to be used with pattern matching:

```Scala
def calcType(calc: Calculator) = calc match {
  case Calculator("HP", "20B") => "financial"
  case Calculator("HP", "48G") => "scientific"
  case Calculator("HP"), "30B") => "business"
  case Calculator(ourBrand, ourModel) => "Calculator: %s %s is of unknown type".format(ourBrand, ourModel)
}
```

Alternatives:

```Scala
 case Calculator(_, _) => "Calculator of unknown type"
```

Or not specify that it's a Calculator

```Scala
 case _ => "Calculator of unknown type"
```

Or re-bind the matched method with another value

```Scala
case c@Calculator(_, _) => "Calculator: %s of unknown type".format(c)
```

## Exceptions

**try-catch-finally**

```scala
try {
  emoteCalculatorService.add(1, 2)
} catch {
  case e: ServerIsDownException => log.error(e, "the remote calculator service is unavailable. should have kept your trusty HP.")
} finally {
  remoteCalculatorService.close()
}
```

**Expression oriented**

```Scala
val result: Int: try {
  remoteCalculatorService.add(1,2)
} catch {
  case e: ServerIsDownException => {
    log.error(e, "the remote calculator service is unavailable. should have kept your trusty HP.")
    0
  }
} finally {
  remoteCalculatorService.close()
}
```