# try-go

## What is this

Annotations about my studies in GoLang.

**References:**

- [Go in Action](https://www.manning.com/books/go-in-action)
- [A Tour of Go](https://tour.golang.org/)
- [Go by Example](https://gobyexample.com/)

# 1 - Fundamentals

## Overview
- Strongly typed
- Compiled
- Stylitically nice
- Opinionated
- easy learning curve
- cross-compile

Go is a programming language designed by Google to help solve Google's problems, and Google has big problems.

The goals of the Go project were to eliminate the slowness and clumsiness of software development at Google, and thereby to make the process more productive and scalable. The language was designed by and for people who write—and read and debug and maintain—large software systems.

Go's purpose is therefore not to do research into programming language design; it is to improve the working environment for its designers and their coworkers. Go is more about software engineering than programming language research. Or to rephrase, it is about language design in the service of software engineering.

## Naming

**There is no public and private keywords**

Go takes an unusual approach to defining the visibility of an identifier, the ability for a client of a package to use the item named by the identifier. Unlike, for instance, private and public keywords, in Go the name itself carries the information: the case of the initial letter of the identifier determines the visibility. If the initial character is an upper case letter, the identifier is exported (public); otherwise it is not:

- upper case initial letter: Name is visible to clients of package
- otherwise: name (or _Name) is not visible to clients of package

This rule applies to **variables**, **types**, **functions**, **methods**, **constants**, **fields**... everything. That's all there is to it.

## Garbage collector

The language is much easier to use because of garbage collection.

Of course, garbage collection brings significant costs: general overhead, latency, and complexity of the implementation. Nonetheless, we believe that the benefits, which are mostly felt by the programmer, outweigh the costs, which are largely borne by the language implementer.

Although Go is a garbage collected language, therefore, a knowledgeable programmer can limit the pressure placed on the collector and thereby improve performance. (Also, the Go installation comes with good tools for studying the dynamic memory performance of a running program.)

- Super convenient
- References
- Completed goroutines


## If with a short statement

Like for, the if statement can start with a short statement to execute before the condition.
Variables declared by the statement are only in scope until the end of the if.

```go
package main

import (
	"fmt"
	"math"
)

func pow(x, n, lim float64) float64 {
	if v := math.Pow(x, n); v < lim {
		return v
	}
	return lim
}

func main() {
	fmt.Println(
		pow(3, 2, 10),
		pow(2, 2, 10),
		pow(3, 3, 20),
	)
}
```

## Switch with no condition

Switch without a condition is the same as switch true.
This construct can be a clean way to write long if-then-else chains.

```go
package main

import (
	"fmt"
	"time"
)

func main() {
	t := time.Now()
	switch {
	case t.Hour() < 12:
		fmt.Println("Good morning!")
	case t.Hour() < 17:
		fmt.Println("Good afternoon.")
	default:
		fmt.Println("Good evening.")
	}
}
```

## Defer

A defer statement defers the execution of a function until the surrounding function returns.
The deferred call's arguments are evaluated immediately, but the function call is not executed until the surrounding function returns.

```go
package main

import "fmt"

func main() {
	defer fmt.Println("world")

	fmt.Println("hello")
}
```

**Output**

```
hello world
```

### Stacking defers

```go
package main

import "fmt"

func main() {
	fmt.Println("counting")

	for i := 0; i < 10; i++ {
		defer fmt.Println(i)
	}

	fmt.Println("done")
}
```

**Output**

```
counting
done
9
8
7
6
5
4
3
2
1
0
```

## Closures

```go
package main

import "fmt"

func adder() func(int) int {
	sum := 0
	return func(x int) int {
		sum += x
		return sum
	}
}

func main() {
	pos, neg := adder(), adder()
	for i := 0; i < 10; i++ {
		fmt.Println(
			pos(i),
			neg(-2*i),
		)
	}
}
```

## Marshalling

Marshalling is the process of converting a data field, or an entire set of related structures, into a serialized string that can be sent in a message. 
Go offers built-in support for JSON encoding and decoding, including to and from built-in and custom data types.

## Function values

Go supports anonymous functions, which can form closures. Anonymous functions are useful when you want to define a function inline without having to name it.

```go
package main

import (
	"fmt"
	"math"
)

func compute(fn func(float64, float64) float64) float64 {
	return fn(3, 4)
}

func main() {
	hypot := func(x, y float64) float64 {
		return math.Sqrt(x*x + y*y)
	}
	fmt.Println(hypot(5, 12))

	fmt.Println(compute(hypot))
	fmt.Println(compute(math.Pow))
}
```

## Interfaces

An interface type is defined by a set of methods.
A value of interface type can hold any value that implements those methods.

Interfaces in Go provide a way to specify the behavior of an object: if something can do this, then it can be used here. We've seen a couple of simple examples already; custom printers can be implemented by a String method while Fprintf can generate output to anything with a Write method. Interfaces with only one or two methods are common in Go code, and are usually given a name derived from the method, such as io.Writer for something that implements Write.

A type can implement multiple interfaces. For instance, a collection can be sorted by the routines in package sort if it implements sort.Interface, which contains Len(), Less(i, j int) bool, and Swap(i, j int), and it could also have a custom formatter. In this contrived example Sequence satisfies both.

 
# 3 - Packaging and tooling

The design of Go's package system combines some of the properties of libraries, name spaces, and modules into a single construct.

Every Go source file, for instance "encoding/json/json.go", starts with a package clause, like this:

```go
package json
```

### Remote packages

It's worth noting that the go get command downloads dependencies recursively, a property made possible only because the dependencies are explicit. Also, the allocation of the space of import paths is delegated to URLs, which makes the naming of packages decentralized and therefore scalable, in contrast to centralized registries used by other languages.

An important property of Go's package system is that the package path, being in general an arbitrary string, can be co-opted to refer to remote repositories by having it identify the URL of the site serving the repository.

Here is how to use the doozer package from github. The go get command uses the go build tool to fetch the repository from the site and install it. Once installed, it can be imported and used like any regular package.

```
$ go get github.com/4ad/doozer // Shell command to fetch package
```


# 4 - Arrays, Slices and Maps
An array in Go is a fixed length data type that contains a contiguous block of elements of the same type. 
This could be a built-in type such as integers and strings, or it can be a struct type.

## Slice Internals

A slice is a data structure that provides a way for us to work with and manage collections of data. 
Slices are built around the concept of dynamic arrays that can grow and shrink as we see fit.


# 6 - Concurrency

Go uses Communicating Sequential Processes([CSP](https://en.wikipedia.org/wiki/Communicating_sequential_processes)). Based on message passing via channels.

In summary, CSP is practical for Go and for Google. When writing a web server, the canonical Go program, the model is a great fit.

## Goroutine

A goroutine is a lightweight thread of execution.

In the following example we are gonna have:


```go

package main

import ( 
 "fmt" 
 "sync" 
 ) 
 
 // main is the entry point for all Go programs. 
 func main() { 
     // wg is used to wait for the program to finish. 
     // Add a count of two, one for each goroutine. 
     var wg sync.WaitGroup 
     wg.Add(2) 
     
     fmt.Println("Start Goroutines") 
     
     // Declare an anonymous function and create a goroutine. 
     go func() { 
         // Schedule the call to Done to tell main we are done. 
         defer wg.Done() 
         
         // Display the alphabet three times 
         for count := 0; count < 3; count++ { 
         for char := 'a'; char < 'a'+26; char++ { 
         fmt.Printf("%c ", char) 
         } 
         }


     }() 
     
     // Declare an anonymous function and create a goroutine. 
     go func() { 
         // Schedule the call to Done to tell main we are done. 
         defer wg.Done() 
     
         // Display the alphabet three times 
         for count := 0; count < 3; count++ { 
            for char := 'A'; char < 'A'+26; char++ { 
                fmt.Printf("%c ", char) 
            } 
            } 
         }() 
         
     // Wait for the goroutines to finish. 
     fmt.Println("Waiting To Finish") 
     wg.Wait() 
     
     fmt.Println("\nTerminating Program") 
 }
 
```

We are using the [WaitGroup](https://golang.org/pkg/sync/#WaitGroup)

A WaitGroup waits for a collection of goroutines to finish. The main goroutine calls Add to set the number of goroutines to wait for. Then each of the goroutines runs and calls Done when finished. At the same time, Wait can be used to block until all goroutines have finished.

**Output:**

```
Start Goroutines
Waiting To Finish
a b c d e f g h i j k l m n o p q r s t u v w x y z a b c d e f g h i j k l m n o p q r s t u v w x y z a b c d e f g h i j k l m n o p q r s t u v w x y z A B C D E F G H I J K L M N O P Q R S T U V W X Y Z A B C D E F G H I J K L M N O P Q R S T U V W X Y Z A B C D E F G H I J K L M N O P Q R S T U V W X Y Z 
Terminating Program
```

The Go standard library has a function called [GOMAXPROCS](https://golang.org/pkg/runtime/#GOMAXPROCS) in the [runtime](https://golang.org/pkg/runtime/) package that will allow you to specify the number of logical processors to be used by the scheduler. This is how we can change the runtime to run our goroutine in parallel.

```go
 // This sample program demonstrates how to create goroutines and 
 // how the goroutine scheduler behaves with two logical processors. 
 package main 
 
 import ( 
 "fmt" 
 "runtime" 
 "sync" 
 ) 
 
 // main is the entry point for all Go programs. 
 func main() { 
     // wg is used to wait for the program to finish. 
     // Add a count of two, one for each goroutine. 
     var wg sync.WaitGroup 
     wg.Add(2) 
     
     // Allocate two logical processors for the scheduler to use. 
     runtime.GOMAXPROCS(2) 
     
     fmt.Println("Start Goroutines") 
     
     // Declare an anonymous function and create a goroutine. 
     go func() { 
         // Schedule the call to Done to tell main we are done. 
         defer wg.Done() 
         
         // Display the alphabet three times. 
         for count := 0; count < 3; count++ { 
         for char := 'a'; char < 'a'+26; char++ { 
         fmt.Printf("%c ", char) 
         } 
         } 
     }() 
     
     // Declare an anonymous function and create a goroutine. 
     go func() { 
         // Schedule the call to Done to tell main we are done. 
         defer wg.Done() 
         
         // Display the alphabet three times. 
         for count := 0; count < 3; count++ { 
         for char := 'A'; char < 'A'+26; char++ { 
         fmt.Printf("%c ", char)

         } 
         } 
     }() 
     
     // Wait for the goroutines to finish. 
     fmt.Println("Waiting To Finish") 
     wg.Wait() 
     
     fmt.Println("\nTerminating Program") 
 }

```

**Output:**

```
Start Goroutines
Waiting To Finish
a b c d e f g h i j k A B C D E F G H I J K L M N O P Q R S T U V l W X Y Z A B C D E F m G H n I J o p K L q r M N s t O P u v Q R w x S T y z U V a W X b c Y Z d e A B f g C D h i E F j k G H l m I J n o K L p q M N r s O P t u Q R v w S T x y U V z a W X b c Y Z d e f g h i j k l m n o p q r s t u v w x y z 
Terminating Program
```

## Atomic Functions

## Mutexes


## Channels

Channels are the pipes that connect concurrent goroutines. You can send values into channels from one goroutine and receive those values into another goroutine.

Using atomic functions and mutexes work, but they don't make writing concurrent programs easier, less error prone or fun. In Go, we don't just have atomic functions and mutexes to keep shared resources safe and eliminate race conditions. We also have [channels](https://golang.org/ref/spec#Channel_types) which synchronize goroutines as they send and receive the resources they need to share between each other.

When a resources needs to be shared between goroutines, channels acts as a conduit between the goroutines and provide mechanism that guarantees a synchronous exchange. When we are gonna declare a channel, the type of data needs to be specified.

```go
// Unbuffered channel of integers. 
unbuffered := make(chan int) 
// Buffered channel of strings. 
buffered := make(chan string, 10)
```
Here we can see two types of channel. If we are using a buffered channel, we need to specify the size of the channel's buffer as the second argument.

Sending a value pointer into a channel, we can use the **<-** operator.

```go
// Buffered channel of strings. 
buffered := make(chan string, 10) 
// Send a string through
buffered <- "Gopher"
```

## Some bad aspects

### Debugging

### Lack of generics?

### Error handling

Go does not have an exception facility in the conventional sense, that is, there is no control structure associated with error handling. (Go does provide mechanisms for handling exceptional situations such as division by zero. A pair of built-in functions called panic and recover allow the programmer to protect against such things. However, these functions are intentionally clumsy, rarely used, and not integrated into the library the way, say, Java libraries use exceptions.)



## Summary

Several big user-facing services use it, including youtube.com and dl.google.com (the download server that delivers Chrome, Android and other downloads), as well as our own golang.org. And of course many small ones do, mostly built using Google App Engine's native support for Go.

Many other companies use Go as well; the list is very long, but a few of the better known are:

- BBC Worldwide
- Canonical
- Heroku
- Nokia
- SoundCloud


Futher, some interesting aspects:

- Clear dependencies
- Clear syntax
- Clear semantics
- Composition over inheritance
- Simplicity provided by the programming model (garbage collection, concurrency)
- Easy tooling (the go tool, gofmt, godoc, gofix)

If you haven't tried Go already, we suggest you do.

http://golang.org
