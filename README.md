# try-go


# 3 - Packaging and tooling


# 4 - Arrays, Slices and Maps
An array in Go is a fixed length data type that contains a contiguous block of elements of the same type. 
This could be a built-in type such as integers and strings, or it can be a struct type.

## Slice Internals

A slice is a data structure that provides a way for us to work with and manage collections of data. 
Slices are built around the concept of dynamic arrays that can grow and shrink as we see fit.


# 6 - Concurrency

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



## Channels


Channels are the pipes that connect concurrent goroutines. You can send values into channels from one goroutine and receive those values into another goroutine.
