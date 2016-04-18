# Hamsters

A micro Scala utility library. Made for functional programming begginers :)

![hamster picture](http://loicdescotte.github.io/images/hamster.jpg)

## Basic validation

Validation is right biased, i.e. Right is used for the "success side" and left for the "failure" side.

```scala
 val e1: Either[String, Int] = Right(1)
 val e2: Either[String, Int] = Left("error 1")
 val e3: Either[String, Int] = Left("error 2")
 
 val validation = Validation(e1,e2, e3)
 val failures = validation.failures //List[String] : List("error 1", "error 2")
 val successes = validation.successes //List[Int] : List(1)
```
 
##  Basic monad transformers

Example : combine Future and Option types then make it work in a for comprehension.

```scala
def foa: Future[Option[String]] = Future(Some("a"))
def fob(a: String): Future[Option[String]] = Future(Some(a+"b"))

val composedAB: Future[Option[String]] = (for {
  a <- FutureOption(foa)
  ab <- FutureOption(fob(a))
} yield ab).future

```
Currently hamsters only supports FutureEither and FutureOption monad transformers but more will come!

## Right biased Either

Either is not biased is standard Scala library. With this helper, `map` and `flatMap` can be used by default as on the right side of Either, for example in for comprehension. 

```scala
import io.github.hamsters.Implicits._

val e1: Either[String, Int] = Right(1)
val e2: Either[String, Int] = Left("nan")
val e3: Either[String, Int] = Left("nan2")

// Stop at first error
for {
  v1 <- e1
  v2 <- e2
  v3 <- e3
} yield(s"$v1-$v2-$v3")  //Left("nan")
```
 
## Coming soon 
 * basic HList with conversions from/to tuples
 * basic type Union
