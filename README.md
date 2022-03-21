# Go Reference
---
## Table of Contents

- [Go Reference](#go-reference)
  * [Table of Contents](#table-of-contents)
  * [Overview of Golang](#overview-of-golang)
  * [Syntax](#syntax)
  * [Operators](#operators)
    + [Arithmetic](#arithmetic)
    + [Comparison](#comparison)
    + [Logical](#logical)
    + [Pointer](#pointer)
    + [Channels](#channels)
  * [Declarations](#declarations)
  * [Functions](#functions)
    + [Basic Functions](#basic-functions)
    + [Function Closures](#function-closures)
    + [Functions with arbitrary input lengths](#functions-with-arbitrary-input-lengths)
  * [Types](#types)
  * [Type Conversion](#type-conversion)
  * [Packages](#packages)
  * [Control Structures](#control-structures)
    + [If](#if)
    + [Loops](#loops)
    + [Switch](#switch)
  * [Arrays, Slices, Range](#arrays--slices--range)
    + [Arrays](#arrays)
    + [Slices](#slices)
    + [Array and Slice operations](#array-and-slice-operations)
  * [Maps](#maps)
  * [Structs](#structs)
    + [Anonymous Structs](#anonymous-structs)
  * [Pointers](#pointers)
  * [Interfaces](#interfaces)
  * [Embedding](#embedding)
  * [Errors](#errors)
  * [Concurrency](#concurrency)
    + [Goroutines](#goroutines)
    + [Channels](#channels-1)
    + [Channel Axioms](#channel-axioms)
  * [Printing](#printing)
  * [Reflection](#reflection)
    + [Type Switching](#type-switching)

<small><i><a href='http://ecotrust-canada.github.io/markdown-toc/'>Table of contents generated with markdown-toc</a></i></small>


---

## Overview of Golang
+ Imperative
+ Static Typing
+ Compiled, not Interpreted
+ Made by Google
+ No Classes, but use structs
+ Type embedding, not inheritance
+ Simple Syntax
+ Functions first class
+ Closures
+ Pointers without pointer arithmetic
+ Strong Concurrency
+ Fast Language

## Syntax

```Go
// main.go
package main

import "fmt"

func main() {
	fmt.Println("Hello World.")
}
```
+ Regular Running of a Go Program
```Bash
go run main.go
```
+ Detect Race Conditions when running a Go Program
```Bash
go run -race main.go
```
+ Build your Go project
```Bash
go build main.go
```
+ Test your Go project
```Bash
go test main.go
```
+ Install a Go package into your go.mod
```Bash
go get {package}
```

+ Your package must be defined
+ All imports must be used
+ No Semi-colons, but curly braces for block scoping
+ // and / * * / for comments
+ You can put flags like -race in front of any of the bash commands listed

## Operators

### Arithmetic
|  Operator | Description |
| -----------| -------------|
| + | add |
| - | subtract |
| * | multiply |
| / | divide |
| % | modulus |
| >> | right shift bits|
| << | left shift bits|
| & | bitwise and |
| &#124; | bitwise or |
| ^ | bitwise xor |

### Comparison

|  Operator | Description |
| -----------| -------------|
| == | equality |
| != | not equal |
| < | less than |
| > | greater than |
| <= | less than or equal to |
| >= | greater than or equal to|
| << | left shift bits|

### Logical

|  Operator | Description |
| -----------| -------------|
| && | logical and |
| &#124; &#124; | logical or |
| ! | logical not |

### Pointer

|  Operator | Description |
| -----------| -------------|
| & | address of / create pointer |
| * | dereference pointer (value of pointer) |

### Channels

|  Operator | Description |
| -----------| -------------|
| <- | send / recieve operator |


## Declarations

```Go
// different ways to declare variables
var foo int // foo is type int, initial value is 0
var foo int = 42
var foo, bar int = 42, 100
var foo = 42 // type inferred by Go
foo := 42
const constant = "This is a constant"

const (
	_ = iota // 0
	a // 1
	b // 2
	c = 1 << iota // 2^3
	d // d = 1 << 4 == 2^4
)

```

+ All variables have initial values as the 0 value for that type, no variable can be undefined.
+ If you don't specify a type, Go will use type inference to determine the type at run time, however, it can't be used as another type later in the program.
+ := is shorthand that only applies within function bodies, not outside
+ const variables can only be primitives and can't be changed
+ in the const block, Go starts counting from iota onwards using pattern matching

## Functions

### Basic Functions
```Go
func function() {}
func function(param string, param int) {}
func function(param1 , param2 int) {}
func function() int { return 1}
func multiFunction() int, string { return 1, "fuck this"}
x,str := multiFunction()

```

### Function Closures
```Go
func main(){
	add := func(a, b int) int {
		return a + b
	}
}
func scope() func() int {
	outer := 2
	foo :- func() int {return outer}
	return foo
}

// Closure
func outer() (func() int, int) {
	outer := 2
	inner := func() int {
		outer += 99
		return outer
	}
	inner()
	return inner, outer
}
```

### Functions with arbitrary input lengths
```Go
func adder(args ...int) int{
	total := 0
	for _, v := range args {
		total += v
	}
	return total
}
```

## Types

|  type | Description |
| -----------| -------------|
| bool | true/false |
| string | slice of runes, which itself is a byte array |
| int, int8, int16, int32, int64| integer type from -range to +range |
| uint, uint8, uint16, uint32, uint64| integer type from 0 to +range |
| byte | alias for uint8 --> 010101 stuff |
| rune | alias for int32 --> characters / unicode |
| float32, float64 | numbers with decimals |
| complex64, complex128 | complex numbers --> (a x real + b x i) |

+ Operations between types is not natively supported. If you try (int + int32) then it will be a compilation error. You must convert types to the same type to perform operations on them.



## Type Conversion

```Go
// One way of type converting
var i int = 42
var f float64 = float(i)
var u uint = uint(f)

// Alternate way of type converting
i := 42
f := float64(i)
u := uint(f)
```

## Packages

+ When making a Go project, define a file called "go.mod" with the following bash command if you are not planning on sharing it. 
```bash
go mod init example.com/{your project file path}
```
+ If you want to share your go module, then it will then be the following.
```bash
go mod init {your github repo.com}/{your project file path}
```

+ All executable files are in package main
+ __Upper case function names => exported__
+ __Lower case function names => private__
+ __go.mod__ file contains all your dependencies for the Go project
+ __go.sum__ file is a file that has checksums to make sure you don't have to update dependencies

## Control Structures

### If
```Go
if x > 1{
	return 1
} else if x < 1 {
	return 2
} else {
	return 3
}
```

### Loops
```Go
// Go has no While loops, only for loops

// Regular for loop
for i := 1; i < 10; i++ {}

// While loop
for ; i < 10; {}

// While loop, version 2
for i < 10 {}

```

+ __break__ keyword will break out of the loop and go to the next higher loop
+ __continue__ keyword will skip over that part and keep going

### Switch
```Go
// switch
switch condition {
	case "1":
		fmt.Println("pass")
	case "2":
		fmt.Println("pass")
	default:
		fmt.Println("pass")
}
// switch version 2
switch condition {
	case "1","2":
		fmt.Println("pass")
	default:
		fmt.Println("pass")

}
```
+ You can have expressions in the case, not just primitives
+ You can list multiple conditions that lead to the same outcome on one line

## Arrays, Slices, Range

### Arrays
+ Arrays are simply a list of memory addresses that are addressed to one thing
```Go
// declare, then set
var a[10] int // declare
a[3] = 42 // set
i := a[3] // read

// declare and set
var a = [2]int{1,2}

// declare and set shortcut
a := [2]int{1,2}

// declare and set shortcut, alternative
a := [...]int{1,2}

```

### Slices
+ Slices are simply arrays that automatically resize.. In less covoluted terms, it is a __List__

```Go
// declare, then initialize
var a []int
var a = []int {1,2,3}

// declare and initialize shortcut
a := []int{1,2,3}

// "slices" support array slicing operations
b := a[1:2]

// slices have append method
a = append(a,17,3) // append 17 and 3 to a
c := append(a,b...) // add slice a to slice b

// make slice with make
a = make([]byte, 5,5) // 1st is arg len, 2nd is initial capacity
a = make([]byte, 5) // capacity is optional
```

### Array and Slice operations

+ __len(a)__ is a built-in function that returns the length of an array or slice

```Go

// looping over array, method 1
for i,e := range a {
	// i is index, e is element
}

// looping over array, method 2
for _,e := range a {
	// e is element
}

// looping over array, method 3
for i := range a {
	// i is index
}

```

## Maps

+ Maps are hash maps, __O(1)__ look up time
```Go
// make a map
m := make(map[string]int)
m["key"] = 42

// delete a key in a map
delete(m, "key")

// test if key "key" is in map and return it, if there is
elem, ok := m["key"]

// map literal
var m = map[string]Vertex{
	"Key 1": {1,2},
	"Key 2": {3,4},
}

// iterate over map
for key, value := range m {
	// logic
}

```

## Structs
+ Go has no classes, only structs. Structs can have methods.

```Go
// A struct is a collection of fields

// Declaration
type V struct {
	X, Y int
}

// Creating
var v = V{1,2}
var v = V{X:1, Y:2}
var v = []V{{1,2},{3,4},{5,6}}

// Accessing
v.X = 4

// Declaring Struct Methods
// The struct is copied on each method call
func (v V) Abs() float64 {
	return math.Sqrt(v.X*v.X v.Y*v.Y)
}

// Calling Methods
v.Abs()

// To mutate Methods, use pointers
// Using this, the struct value will not be copied, it will be updated with pointers
func (v *V) add (n float64){
	v.X += n
	v.Y += n
}
```

### Anonymous Structs
+ Cheaper and safer than using a __map[string]interface{}__
```Go
point := struct {
	X,Y int
}{1,2}
```

## Pointers

+ Pointers point to a memory address of a variable

```Go
p := V{1,2} // p is a V
q := &p     // q is a pointer to p
r := &V{1,2}// r is also a pointer

var s *V = new(V) // creates a new pointer to a new struct instance
```

## Interfaces

```Go
// declaration
type A interface {
	A() string
}

// types do not declare to implement interfaces
type Foo struct {}

// types implicity satisfy an interface if they implement ALL of the required methods
func (foo Foo) A() string {
	return "A"
}
```

## Embedding
+ Embedding is Go's abberation for Sub-Classes.
```Go
type ReadWriter interface {
	Reader
	Writer
}

// Server exposes all methods that Logger has
type Server struct {
	Host string
	Port int
	*log.Logger
}

// init the embedded type the usual way
server := &Server{"localhost", 80, log.New(...)}

// methods implemented on the embedded struct are passed through
sever.Log(...) // calls server.Logger.Log(...)

var logger *log.Logger = server.Logger
```

## Errors
+ Go has no execption handling. Functions just return an __Error__ as an extra parameter.

```Go
type error interface {
	Error() string
}
```

Example: 

```Go
func sqrt(x float64) (float64, error) {
	if x < 0 {
		return 0, errors.New("negative value")
	}
	return math.Sqrt(x), nil
}

func main() {
	val, err := sqrt(-1)
	if err != nil {
		// handle error
		fmt.Println(err) // negative value
		return
	}
	// All is good, use `val`.
	fmt.Println(val)
}
```

## Concurrency
### Goroutines
Goroutines are lightweight threads managed by Go, not the Operating System. 

```Go
func do(s string){}

func main(){
	go do("hey") // Named go routine function

	go func(x int){}(1) // Anonymous go routine function
}
```

### Channels
Channels are Go's way of letting threads communicate to each other.

```Go
ch := make(chan int) // create a channel of type int
ch <- 42             // Send a value to the channel ch.
v := <-ch            // Receive a value from ch

// Non-buffered channels block. Read blocks when no value is available, write blocks until there is a read.

// Create a buffered channel. Writing to a buffered channels does not block if less than <buffer size> unread values have been written.
ch := make(chan int, 100)

close(ch) // closes the channel (only sender should close)

// read from channel and test if it has been closed
v, ok := <-ch

// if ok is false, channel has been closed

// Read from channel until it is closed
for i := range ch {
    fmt.Println(i)
}

// select blocks on multiple channel operations, if one unblocks, the corresponding case is executed
func doStuff(channelOut, channelIn chan int) {
    select {
    case channelOut <- 42:
        fmt.Println("We could write to channelOut!")
    case x := <- channelIn:
        fmt.Println("We could read from channelIn")
    case <-time.After(time.Second * 1):
        fmt.Println("timeout")
    }
}
```

### Channel Axioms

+ Sending to a nil channel blocks forever
```Go
var c chan string
c <- "Hello, World!"
// fatal error: all goroutines are asleep - deadlock!
```
+ Receiving from a nil channel blocks forever
```Go
var c chan string
fmt.Println(<-c)
// fatal error: all goroutines are asleep - deadlock!
```
+ Sending to a closed channel panics
```Go
var c = make(chan string, 1)
c <- "Hello, World!"
close(c)
c <- "Hello, Panic!"
// panic: send on closed channel
```
+ Receiving from a closed channel returns the 0 value 
```Go
var c = make(chan int, 2)
c <- 1
c <- 2
close(c)
for i := 0; i < 3; i++ {
    fmt.Printf("%d ", <-c)
}
// 1 2 0
```

## Printing

```Go
fmt.Println("Hello, 你好, नमस्ते, Привет, ᎣᏏᏲ") // basic print, plus newline
p := struct { X, Y int }{ 17, 2 }
fmt.Println( "My point:", p, "x coord=", p.X ) // print structs, ints, etc
s := fmt.Sprintln( "My point:", p, "x coord=", p.X ) // print to string variable

fmt.Printf("%d hex:%x bin:%b fp:%f sci:%e",17,17,17,17.0,17.0) // c-ish format
s2 := fmt.Sprintf( "%d %f", 17, 17.0 ) // formatted print to string variable

hellomsg := `
 "Hello" in Chinese is 你好 ('Ni Hao')
 "Hello" in Hindi is नमस्ते ('Namaste')
` // multi-line string literal, using back-tick at beginning and end
```

## Reflection

### Type Switching
A type switch is a switch statement for __types__.

```Go
func do(i interface{}) {
	switch v := i.(type) {
	case int:
		fmt.Printf("Twice %v is %v\n", v, v*2)
	case string:
		fmt.Printf("%q is %v bytes long\n", v, len(v))
	default:
		fmt.Printf("I don't know about type %T!\n", v)
	}
}

func main() {
	do(21)
	do("hello")
	do(true)
}
```
