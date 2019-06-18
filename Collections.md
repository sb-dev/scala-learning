# Collections

## Arrays, Lists & Sets

Arrays and Lists preserve order, can contain duplicates, and are mutable.
Sets do not preserve order and have no duplicates.

**Array and List**

```Scala
val numberArray = Array(1, 2, 3, 4, 5, 1, 2, 3, 4, 5)
numberArray(3) = 10

val numberList = List(1, 2, 3, 4, 5, 1, 2, 3, 4, 5)
numberList(3) = 10
```

**Set**

```Bash
scala> val numbers = Set(1, 2, 3, 4, 5, 1, 2, 3, 4, 5)
numbers: scala.collection.immutable.Set[Int] = Set(5, 1, 2, 3, 4)
```

## Tuple

A tuple groups together simple local collections of items without using a class.

```Scala
val hostPort = ("localhost", 80)
```

```Bash
scala> hostPort._1
res0: String = localhost

scala> hostPort._2
res1: Int = 80
```

**Pattern matching**

```Scala
hostport match {
    case ("localhost", port) => ...
    case (host, port) => ...
}
```

Simple syntax for two value tuple.

```Scala
1 -> 2
```

## Maps

It can hold basic datatypes.

```Scala
Map(1 -> 2)
Map("foo" -> "bar")

Map(1 -> "one", 2 -> "two")
Map((1, "one"),(2, "two"))
```

## Option

Option is a container that may or may not hold something.

```Scala
trait Option[T] {
    def isDefined: Boolean
    def get: T
    def getOrElse(t: T): T
}
```

Option itself is generic and has two subclasses: Some[T] or None.
Map.get uses Option for its return type.

```Scala
val numbers = Map("one" -> 1, "two" -> 2)
```

```Bash
scala> numbers.get("two")
res0: Option[Int] = Some(2)

scala> numbers.get("three")
res1: Option[Int] = None
```

**getOrElse**

```Scala
val result = res1.getOrElse(0) * 2
```

**Pattern matching**

```Scala
val result = res1 match {
    case Some(n) -> n * 2
    case None => 0
}
```

## Functional Combinators

*List(1,2,3) map squared* applies the function *squared* to the elements of the list, returning a new list, *List(1, 4, 9)*.

### map

Evaluates a function over each element in the list, returning a list with the same number of elements.

```Scala
val numbers = List(1, 2, 3, 4)
```

```Bash
scala> numbers.map((i: Int) => i * 2)
res0: List[Int] = List(2, 4, 6, 8)
```

**Passing a function**

```Scala
def timesTwo(i: Int): Int = i * 2
```

```Bash
scala> numbers.map(timesTwo)
res0: List[Int] = List(2, 4, 6, 8)
```

### foreach

*foreach* is like map but returns nothing (intended for side-effects only)

```Bash
scala> numbers.foreach((i: Int) => i * 2)
```

### filter

Removes any elements where the function we pass in evaluates to false. Functions that return a Boolean are often called predicate functions.

```Scala
def isEven(i: Int): Boolean = i % 2 == 0
```

```Bash
scala> numbers.filter((i: Int) => i % 2 == 0)
res0: List[Int] = List(2, 4)

scala> number.filter(isEven)
res1: List[Int] = List(2, 4)
```

### zip

Aggregates the contents of two lists into a single list of pairs

```Bash
scala> List(1, 2, 3).zip(List("a", "b", "c"))
res0: List[(Int, String)] = List((1,a), (2,b), (3,c))
```

### partition

Splits a list based on where it falls with respect to a predicate function

```Bash
scala> numbers.partition(_ % 2 == 0)
res0: (List[Int], List[Int]) = (List(2, 4), List(1, 3))
```

### find

Returns the first element of a collection that matches a predicate function.

```Bash
scala> numbers.find((i: Int) => i > 2)
res0: Option[Int] = Some(3)
```

### drop & dropWhile

*drop* drops the first i element; while *dropWhile* removes the first element that matches a predicate function.

```Bash
scala> numbers.drop(1)
res0: List[Int] = List(1,3,4)

scala> numbers.dropWhile(_ % 2 != 0)
res1: List[Int] = List(2,3,4)
```

### foldLeft & foldRight

```Bash
scala> numbers.foldLeft(0)((m: Int, n: Int) => m + n)
m: 0 n: 1
m: 1 n: 2
m: 3 n: 3
m: 6 n: 4
m: 10 n: 5
res0: Int = 15

scala> numbers.foldRight(0)((m: Int, n: Int) => m + n)
m: 5 n: 0
m: 4 n: 5
m: 3 n: 9
m: 2 n: 12
m: 1 n: 14
res0: Int = 15
```

### flatten

Collapses one level of nested structure.

```Bash
scala> List(List(1, 2), List(3, 4)).flatten
res0: List[Int] = List(1, 2, 3, 4)
```

### flatMap

Combines mapping and flatening; takes a function that works on the nested lists and then concatenates the results back together.

```Bash
scala> List(List(1, 2), List(3, 4)).flatMap(x => x.map(_ * 2))
res0: List[Int] = List(2, 4, 6, 8)
```

*__Note__: Shorthand for __nestedNumbers.map((x: List[Int]) => x.map(_ * 2)).flatten__*


### Generalised functional combinators

Every functional combinator shown above can be written on top of fold

```Scala
def ourMap(numbers: List[Int], fn: Int => Int): List[Int] = {
    numbers.foldRight(List[Int]()) { (x: Int, xs: List[Int]) =>
        fn(x) :: xs
    }
}
```

```Bash
scala> ourMap(numbers, timesTwo(_))
res0: List[Int] = List(2, 4, 6, 8)
```

### Map

All of the functional combinators shown work on Maps. Maps can be thought of as a list of pairs so the functions we write work on a pair of the keys and values in the Map.

```Scala
val extensions = Map("steve" => 100, "bob"=> 101, "joe" => 201)
```

```Bash
scala> extensions.filter((namePhone: (String, Int) => namePhone._2 < 200))
res0: scala.collection.imutable.Map[String, Int] = Map((steve, 100), (bob, 101))
```