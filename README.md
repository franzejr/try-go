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

´´´go
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
´´´



## Channels


Channels are the pipes that connect concurrent goroutines. You can send values into channels from one goroutine and receive those values into another goroutine.
