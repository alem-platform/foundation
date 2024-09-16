# A Library for Others

## Learning Objectives

- File Handling in Go
- CSV Parsing
- Interfaces
- Error Handling

## Abstract

Welcome to your next project! This assignment is not just about writing code; it's a crucial step in your journey to becoming a software engineer. You will implement methods for a new library, focusing on several key principles of design and development.

Here's what you'll focus on:

- Go Interfaces: Interfaces in Go act as contracts between supplier and customer. You will use them to create a user-friendly library that simplifies handling CSV files. The goal is to provide consistent and convenient services that are easy to use without being too complex.
- Implement Parsing: You will develop the skills needed to parse complex data structures.
- Information Hiding: Determine what information should be visible and what should remain private. A well-designed interface keeps things simple for users by hiding the complex details, so changes can be made without disrupting them.
- Resource Management: Understand who is responsible for managing memory and other limited resources.
- Error Handling: When working with files and parsing, a major challenge could be managing errors. Determine who detects errors, who reports them, and how they are reported. Establish a clear process for recovery when an error is detected.

This project will provide hands-on experience in file manipulation and parsing. You will not only learn to manually process CSV data but also gain experience in designing and working with interfaces.

## Context

> In programming, the hard part isn’t solving problems, but deciding what problems to solve.
> 
>  — Paul Graham

Comma-separated values, or CSV, is a simple and widely used format for representing tabular data. Each row in a CSV file corresponds to a line of text, with individual fields separated by commas. Here’s an example:

```csv
Name,Age,Occupation
John Doe,28,Engineer
Jane Doe,32,Designer
Sam Smith,24,Developer
```

CSV files are commonly read and written by programs like spreadsheets, making them a standard format for data exchange and storage. When viewed in a table, CSV data might look like this:

| Name      | Age | Occupation |
| --------- | --- | ---------- |
| John Doe  | 28  | Engineer   |
| Jane Doe  | 32  | Designer   |
| Sam Smith | 24  | Developer  |

However, working with CSV files can be challenging. Historically, there wasn’t a clear, standardized specification for CSV which lead to many different implementations and parsing issues. Variations in how fields are quoted, how commas within fields are handled, and other inconsistencies can complicate parsing and processing CSV data.

## Resources

- [Go File I/O](https://golang.org/pkg/os/)
- [CSV Format](https://tools.ietf.org/html/rfc4180)
- [Go Interfaces](https://golang.org/doc/effective_go.html#interfaces)

## General Instructions

- Your code MUST be written in accordance with [gofumpt](https://github.com/mvdan/gofumpt). If not, you will be graded `0` automatically.
- Your program MUST be able to compile successfully.
- Your program MUST not exit unexpectedly (any panics: `nil-pointer dereference`, `index out of range` etc.). If so, you will be get `0` during the defence.
- Only `io.Reader` is allowed. If not, you will get `0` grade.
- You can create as many util functions as you need.
- 
## Mandatory Part

You will build a CSV library in Go. Implement following interface methods:

```go
type CSVParser interface  {
    ReadLine(r io.Reader) (string, error)
    GetField(n int) (string, error)
    GetNumberOfFields() int
}

var (
    ErrQuote      = errors.New("excess or missing \" in quoted-field")
    ErrFieldCount = errors.New("wrong number of fields")
)
```

### ReadLine

This function should read a new line from a CSV file.

```go
type CSVParser interface  {
    // ...
    ReadLine(r io.Reader) (string, error)
    // ...
}
```

- Reads one line from open input file
- Returns line, with terminator removed, or `nil` if EOF occurred
- Calling `ReadLine` in a loop allows you to sequentially read each line from the file, continuing until the end of the file is reached.
- Assumes that input lines are terminated by `\r`, `\n`, `\r\n`, or `EOF`
- If the line has a missing or extra quote, it should return an empty string and an `ErrQuote` error.
- The retured line should not include the `\n` at the end.

> **Note:**
> What if you will read a file with 1000000 (a lot) entries?
> Each time `ReadLine` is called, you should read only until newline. Avoid reading the entire file at once and then processing each line.

### GetField

This function should return the nth field.

```go
type CSVParser interface  {
    // ...
    GetField(n int) (string, error)
    // ...
}
```

- Returns n-th field from last line read by `ReadLine`;
- Returns `ErrFieldCount` if `n < 0` or beyond last field
- Fields are separated by commas
- Fields may be surrounded by "..."; such quotes are removed
- There can be an arbitrary number of fields of any length

### GetNumberOfFields

```go
type CSVParser interface  {
    // ...
    GetNumberOfFields() int
    // ...
}
```

- Returns number of fields on last line read by `ReadLine`
- Behavior undefined if called before `ReadLine` is called

> **Note:**
> Make sure that `GetNumberOfFields` and `GetField` fuctions behaves well if they are called after `ReadLine` has encountered EOF.

> **Note:**
> Working with files and parsing can be complicated. Make sure to test well your library before submitting the project. For example try different file types, what happens when you pass a binary file?

### Test

Here is an example on how your functions will be tested:

```go
func main() {
    file, err := os.Open("example.csv")
    if err != nil {
        fmt.Println("Error opening file:", err)
        return
    }
    defer file.Close()

    var csvparser CSVParser = YourCSVParser{}

    for {
        line, err := csvparser.ReadLine(file)
        if err != nil {
            if err == io.EOF {
                break
            }
            fmt.Println("Error reading line:", err)
            return
        }
    }
}
```

## Support

If you get stuck, try your code with the example inputs from the project. You should get the same results. If not, read the description again. Maybe you missed something, or your code is wrong. After the examples work, but your answer is still wrong, make some test cases you can check by hand. See if they work with your code. Use the complete example input.

If you are still stuck, ask a friend for help or take a break and come back later.

## Guidelines from Author

Starting a new project can be challenging, it's not easy to get the design of a library or interface perfect on the first attempt. Don't worry about getting everything right from the start. Improve the code and design step by step. Here is a short list to help you get started:

- Understand the problem you're solving, it is important because it gives you a clear picture of the context.
- Look up information on the CSV format to understand how it works. Knowing the format can help you handle the data correctly.
- Start by defining the `CSV` struct and its associated methods.
- Use private variables and functions to keep details encapsulated.
- Use the `Read(b []byte)` method to read lines from a file.

## Author

This project has been created by:

Adilyam Tilegenova, Software Developer at Doodocs.kz

Contacts:
- Email: adilyamt@gmail.com
- [GitHub](https://github.com/Adilyam)
- [LinkedIn](https://www.linkedin.com/in/adilyam/)
