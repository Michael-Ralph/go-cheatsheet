# Golang Cheatsheet

## Table of Contents
- [Basic Syntax](#basic-syntax)
- [Data Types](#data-types)
- [Variables](#variables)
- [Constants](#constants)
- [Operators](#operators)
- [Control Structures](#control-structures)
- [Functions](#functions)
- [Packages](#packages)
- [Arrays and Slices](#arrays-and-slices)
- [Maps](#maps)
- [Structs](#structs)
- [Methods](#methods)
- [Interfaces](#interfaces)
- [Error Handling](#error-handling)
- [Concurrency](#concurrency)
- [File I/O](#file-io)
- [Testing](#testing)
- [Common Packages](#common-packages)

## Basic Syntax

### Hello World
```go
package main

import "fmt"

func main() {
    fmt.Println("Hello, World!")
}
```

### Comments
```go
// Single line comment

/*
   Multi-line
   comment
*/
```

## Data Types

### Basic Types
```go
bool        // true or false
string      // UTF-8 encoded text

// Integer types
int, int8, int16, int32, int64
uint, uint8, uint16, uint32, uint64, uintptr

// Floating point types
float32, float64

// Complex types
complex64, complex128

// Other types
byte        // alias for uint8
rune        // alias for int32 (Unicode code point)
```

### Type Conversion
```go
i := 42
f := float64(i)
u := uint(f)
```

## Variables

### Variable Declaration
```go
// Single variable
var x int
var x int = 10
var x = 10

// Short declaration (only inside functions)
x := 10

// Multiple variables
var x, y int
var x, y int = 10, 20
var x, y = 10, "hello"
x, y := 10, "hello"
```

### Zero Values
```go
var i int       // 0
var f float64   // 0.0
var b bool      // false
var s string    // ""
var p *int      // nil
```

## Constants

### Constant Declaration
```go
const PI = 3.14
const (
    StatusOK = 200
    StatusNotFound = 404
)

// iota for incrementing constants
const (
    Monday = iota // 0
    Tuesday       // 1
    Wednesday     // 2
    Thursday      // 3
    Friday        // 4
)
```

## Operators

### Arithmetic Operators
```go
+    // addition
-    // subtraction
*    // multiplication
/    // division
%    // modulus
```

### Comparison Operators
```go
==   // equal to
!=   // not equal to
<    // less than
>    // greater than
<=   // less than or equal to
>=   // greater than or equal to
```

### Logical Operators
```go
&&   // logical AND
||   // logical OR
!    // logical NOT
```

### Bitwise Operators
```go
&    // bitwise AND
|    // bitwise OR
^    // bitwise XOR
&^   // bit clear (AND NOT)
<<   // left shift
>>   // right shift
```

### Assignment Operators
```go
=    // simple assignment
+=   // add and assign
-=   // subtract and assign
*=   // multiply and assign
/=   // divide and assign
%=   // modulus and assign
&=   // bitwise AND and assign
|=   // bitwise OR and assign
^=   // bitwise XOR and assign
&^=  // bit clear and assign
<<=  // left shift and assign
>>=  // right shift and assign
```

## Control Structures

### If Statement
```go
if x > 0 {
    // code
} else if x < 0 {
    // code
} else {
    // code
}

// With initialization statement
if x := getValue(); x > 0 {
    // code (x is only in scope in this block)
}
```

### For Loop
```go
// Standard for loop
for i := 0; i < 10; i++ {
    // code
}

// While-like loop
for x < 10 {
    // code
}

// Infinite loop
for {
    // code
    if condition {
        break
    }
}

// Range loop for arrays, slices, maps, channels
for index, value := range collection {
    // code
}

// Skip index or value
for _, value := range collection {
    // code
}
for index, _ := range collection {
    // code
}
```

### Switch Statement
```go
switch value {
case 1:
    // code
case 2, 3, 4:
    // code for multiple values
default:
    // code
}

// With initialization statement
switch x := getValue(); x {
case 1:
    // code
}

// Type switch
switch v := interface{}.(type) {
case string:
    // v is a string
case int:
    // v is an int
default:
    // unknown type
}

// Condition switch (like if-else-if)
switch {
case x > 0:
    // code
case x < 0:
    // code
default:
    // code
}
```

## Functions

### Function Declaration
```go
func add(x int, y int) int {
    return x + y
}

// Simplified parameter list
func add(x, y int) int {
    return x + y
}

// Multiple return values
func divide(x, y float64) (float64, error) {
    if y == 0 {
        return 0, errors.New("division by zero")
    }
    return x / y, nil
}

// Named return values
func split(sum int) (x, y int) {
    x = sum * 4 / 9
    y = sum - x
    return // naked return
}

// Variadic functions
func sum(nums ...int) int {
    total := 0
    for _, num := range nums {
        total += num
    }
    return total
}
```

### Anonymous Functions and Closures
```go
// Anonymous function
func() {
    fmt.Println("Anonymous function")
}()

// Function variable
add := func(x, y int) int {
    return x + y
}
result := add(3, 4)

// Closure
func adder() func(int) int {
    sum := 0
    return func(x int) int {
        sum += x
        return sum
    }
}
```

### Defer, Panic, and Recover
```go
// Defer (executes after surrounding function returns)
func readFile() {
    file := openFile()
    defer file.Close() // Will be executed when readFile() returns
    // rest of the function
}

// Panic (runtime error)
func divide(a, b int) int {
    if b == 0 {
        panic("division by zero")
    }
    return a / b
}

// Recover (handle panic)
func safeOperation() {
    defer func() {
        if r := recover(); r != nil {
            fmt.Println("Recovered from:", r)
        }
    }()
    
    // code that might panic
    panic("something went wrong")
}
```

## Packages

### Package Declaration
```go
package main  // Executable package
package utils // Library package
```

### Importing Packages
```go
import "fmt"
import "math/rand"

// Multiple imports
import (
    "fmt"
    "math/rand"
    "time"
)

// Import with alias
import (
    "fmt"
    r "math/rand"
)

// Importing for side effects only
import _ "image/png"
```

### Exporting
```go
// Exported (public) names start with a capital letter
var Public int
var private int

func ExportedFunc() {}
func privateFunc() {}
```

## Arrays and Slices

### Arrays
```go
// Declaration
var a [5]int

// Declaration with initialization
a := [5]int{1, 2, 3, 4, 5}

// Size inferred from initialization
a := [...]int{1, 2, 3, 4, 5}

// Access elements
a[0] = 10
fmt.Println(a[1])

// Length
len(a)

// Multi-dimensional arrays
var matrix [3][3]int
```

### Slices
```go
// Declaration
var s []int

// Create slice with make
s := make([]int, 5)      // len=5, cap=5
s := make([]int, 3, 5)   // len=3, cap=5

// Create slice from array
a := [5]int{1, 2, 3, 4, 5}
s := a[1:4]              // s = [2, 3, 4]

// Slice literals
s := []int{1, 2, 3, 4, 5}

// Slice operations
len(s)                   // length
cap(s)                   // capacity
s = append(s, 6)         // append element
s = append(s, 7, 8, 9)   // append multiple elements
s = append(s, anotherSlice...) // append another slice

// Copying slices
dst := make([]int, len(src))
copy(dst, src)
```

## Maps

### Map Declaration and Initialization
```go
// Declaration
var m map[string]int

// Initialization with make
m = make(map[string]int)

// Declaration and initialization
m := map[string]int{
    "one": 1,
    "two": 2,
}
```

### Map Operations
```go
// Set value
m["three"] = 3

// Get value
value := m["one"]

// Check if key exists
value, exists := m["four"]
if exists {
    // key exists
}

// Delete key
delete(m, "two")

// Length
len(m)

// Iterate over map
for key, value := range m {
    fmt.Println(key, value)
}
```

## Structs

### Struct Declaration and Initialization
```go
// Declaration
type Person struct {
    Name     string
    Age      int
    Address  string
}

// Initialization
var p Person
p = Person{Name: "John", Age: 30, Address: "123 Main St"}
p = Person{"John", 30, "123 Main St"} // order matters

// Pointer to struct
ptr := &Person{Name: "John", Age: 30}
```

### Accessing Struct Fields
```go
p.Name = "Jane"
fmt.Println(p.Age)

// With pointer
fmt.Println(ptr.Name) // Same as (*ptr).Name
```

### Anonymous Structs
```go
point := struct {
    X, Y int
}{
    X: 10,
    Y: 20,
}
```

### Nested Structs
```go
type Address struct {
    Street  string
    City    string
    Country string
}

type Person struct {
    Name    string
    Age     int
    Address Address
}
```

## Methods

### Method Declaration
```go
type Rect struct {
    Width, Height float64
}

// Method with receiver
func (r Rect) Area() float64 {
    return r.Width * r.Height
}

// Method with pointer receiver
func (r *Rect) Scale(factor float64) {
    r.Width *= factor
    r.Height *= factor
}
```

### Method Usage
```go
r := Rect{Width: 10, Height: 5}
area := r.Area()

// With pointer receiver
r.Scale(2)  // r is modified
```

## Interfaces

### Interface Declaration
```go
type Shape interface {
    Area() float64
    Perimeter() float64
}
```

### Implementing Interfaces
```go
// Circle implements Shape
type Circle struct {
    Radius float64
}

func (c Circle) Area() float64 {
    return math.Pi * c.Radius * c.Radius
}

func (c Circle) Perimeter() float64 {
    return 2 * math.Pi * c.Radius
}

// Usage
var s Shape
s = Circle{Radius: 5}
fmt.Println(s.Area())
```

### Empty Interface
```go
// Can hold any type
var i interface{}
i = 42
i = "hello"

// Type assertion
value, ok := i.(string)
if ok {
    // i holds a string
}

// Type switch
switch v := i.(type) {
case int:
    fmt.Println("Integer:", v)
case string:
    fmt.Println("String:", v)
default:
    fmt.Println("Unknown type")
}
```

## Error Handling

### Error Interface
```go
type error interface {
    Error() string
}
```

### Creating and Returning Errors
```go
// Using errors.New
import "errors"
func divide(a, b float64) (float64, error) {
    if b == 0 {
        return 0, errors.New("division by zero")
    }
    return a / b, nil
}

// Using fmt.Errorf
import "fmt"
func divide(a, b float64) (float64, error) {
    if b == 0 {
        return 0, fmt.Errorf("division by %v not allowed", b)
    }
    return a / b, nil
}

// Custom error type
type DivisionError struct {
    Dividend float64
    Divisor  float64
}

func (e *DivisionError) Error() string {
    return fmt.Sprintf("cannot divide %f by %f", e.Dividend, e.Divisor)
}

func divide(a, b float64) (float64, error) {
    if b == 0 {
        return 0, &DivisionError{a, b}
    }
    return a / b, nil
}
```

### Handling Errors
```go
result, err := divide(10, 0)
if err != nil {
    fmt.Println("Error:", err)
    return
}
fmt.Println("Result:", result)

// Type assertion for custom errors
if divErr, ok := err.(*DivisionError); ok {
    fmt.Printf("Tried to divide %f by %f\n", divErr.Dividend, divErr.Divisor)
}
```

## Concurrency

### Goroutines
```go
// Start a goroutine
go func() {
    // code runs concurrently
    fmt.Println("Hello from goroutine")
}()

// With function
func say(s string) {
    fmt.Println(s)
}
go say("Hello")
```

### Channels
```go
// Create a channel
ch := make(chan int)      // Unbuffered channel
bch := make(chan int, 10) // Buffered channel with capacity 10

// Send value to channel
ch <- 42

// Receive value from channel
value := <-ch

// Close channel
close(ch)

// Check if channel is closed
value, ok := <-ch
if !ok {
    // channel is closed
}

// Range over channel
for value := range ch {
    fmt.Println(value)
}

// Select statement
select {
case v := <-ch1:
    // received from ch1
case ch2 <- x:
    // sent x to ch2
case <-time.After(time.Second):
    // timeout after 1 second
default:
    // executed if no other case is ready
}
```

### sync Package
```go
// WaitGroup
import "sync"

var wg sync.WaitGroup
wg.Add(1)  // Add counter
go func() {
    defer wg.Done() // Decrease counter when done
    // code
}()
wg.Wait()  // Wait for counter to reach 0

// Mutex
var mu sync.Mutex
mu.Lock()
// critical section
mu.Unlock()

// RWMutex
var rwmu sync.RWMutex
rwmu.RLock()   // Read lock
// read operations
rwmu.RUnlock() // Release read lock

rwmu.Lock()    // Write lock
// write operations
rwmu.Unlock()  // Release write lock

// Once
var once sync.Once
once.Do(func() {
    // code executed only once
})
```

## File I/O

### Reading Files
```go
// Read entire file
import "os"
import "io/ioutil"

data, err := ioutil.ReadFile("file.txt")
if err != nil {
    // handle error
}
fmt.Println(string(data))

// Read with File
file, err := os.Open("file.txt")
if err != nil {
    // handle error
}
defer file.Close()

buffer := make([]byte, 1024)
count, err := file.Read(buffer)
if err != nil {
    // handle error
}
fmt.Println(string(buffer[:count]))

// Scanner for line-by-line
import "bufio"

file, err := os.Open("file.txt")
if err != nil {
    // handle error
}
defer file.Close()

scanner := bufio.NewScanner(file)
for scanner.Scan() {
    line := scanner.Text()
    fmt.Println(line)
}
if err := scanner.Err(); err != nil {
    // handle error
}
```

### Writing Files
```go
// Write entire file
data := []byte("Hello, World!")
err := ioutil.WriteFile("file.txt", data, 0644)
if err != nil {
    // handle error
}

// Write with File
file, err := os.Create("file.txt")
if err != nil {
    // handle error
}
defer file.Close()

data := []byte("Hello, World!")
_, err = file.Write(data)
if err != nil {
    // handle error
}

// Buffered writing
file, err := os.Create("file.txt")
if err != nil {
    // handle error
}
defer file.Close()

writer := bufio.NewWriter(file)
_, err = writer.WriteString("Hello, World!")
if err != nil {
    // handle error
}
writer.Flush()
```

## Testing

### Basic Test
```go
// In file: math_test.go
package math

import "testing"

func TestAdd(t *testing.T) {
    got := Add(2, 3)
    want := 5
    if got != want {
        t.Errorf("Add(2, 3) = %d; want %d", got, want)
    }
}

// Run tests
// go test
// go test -v
// go test -cover
```

### Table-Driven Tests
```go
func TestAdd(t *testing.T) {
    tests := []struct {
        a, b, want int
    }{
        {2, 3, 5},
        {0, 0, 0},
        {-1, 1, 0},
    }
    
    for _, tt := range tests {
        got := Add(tt.a, tt.b)
        if got != tt.want {
            t.Errorf("Add(%d, %d) = %d; want %d", tt.a, tt.b, got, tt.want)
        }
    }
}
```

### Benchmarks
```go
func BenchmarkAdd(b *testing.B) {
    for i := 0; i < b.N; i++ {
        Add(2, 3)
    }
}

// Run benchmarks
// go test -bench=.
```

### Example Tests
```go
func ExampleAdd() {
    fmt.Println(Add(2, 3))
    // Output: 5
}
```

## Common Packages

### fmt
```go
fmt.Println("Hello")
fmt.Printf("Value: %v\n", value)
fmt.Sprintf("Value: %v", value)
fmt.Fprintf(writer, "Value: %v", value)
```

### time
```go
now := time.Now()
future := now.Add(time.Hour * 24)
duration := future.Sub(now)
time.Sleep(time.Second * 2)
```

### strings
```go
s := "hello, world"
strings.Contains(s, "hello")
strings.HasPrefix(s, "he")
strings.HasSuffix(s, "world")
strings.Split(s, ", ")
strings.Join([]string{"hello", "world"}, ", ")
strings.ToUpper(s)
strings.ToLower(s)
strings.TrimSpace(" hello ")
strings.Replace(s, "hello", "hi", -1)
```

### strconv
```go
i, err := strconv.Atoi("123")
s := strconv.Itoa(123)
b, err := strconv.ParseBool("true")
f, err := strconv.ParseFloat("3.14", 64)
```

### math
```go
math.Abs(-10.5)
math.Ceil(10.1)
math.Floor(10.9)
math.Round(10.5)
math.Max(10, 20)
math.Min(10, 20)
math.Pow(2, 3)
math.Sqrt(9)
```

### os
```go
args := os.Args
env := os.Getenv("PATH")
os.Setenv("KEY", "VALUE")
dir, err := os.Getwd()
info, err := os.Stat("file.txt")
err := os.Remove("file.txt")
err := os.Rename("old.txt", "new.txt")
```

### json
```go
import "encoding/json"

// Struct to JSON
type Person struct {
    Name string `json:"name"`
    Age  int    `json:"age"`
}
p := Person{Name: "John", Age: 30}
data, err := json.Marshal(p)
prettyData, err := json.MarshalIndent(p, "", "  ")

// JSON to struct
var p Person
err := json.Unmarshal(data, &p)
```
