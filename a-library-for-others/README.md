<!-- > **Note:**


    Tip: project name here
-->

# A Library for Others

### Learning Objectives

- File Handling in Go
- CSV Parsing
- Error Handling
- Interfaces

### Abstract

Welcome to your next project. In this assignment, you will delve into the world of CSV files — a ubiquitous format for storing and sharing tabular data. This project is not merely an exercise in coding; it is an essential step in your journey to becoming a software engineer.

You will learn how to:

- Develop an Intuitive Library: You are tasked with designing a library that simplifies the process of handling CSV data.
- Master Resource Management: Learn to manage resources efficiently, ensuring optimal performance of your code.
- Implement Robust Error Handling: Develop the ability to anticipate, identify, and resolve errors, enhancing the reliability of your software.

### Context

    "In programming, the hard part isn’t solving problems, but deciding what problems to solve."

    — Paul Graham

Comma-separated values, or CSV, is a simple and widely used format for representing tabular data. Each row in a CSV file corresponds to a line of text, with individual fields separated by commas. Here’s an example:

```csv
John,Doe,120 jefferson st.,Riverside, NJ, 08075
Jack,McGinnis,220 hobo Av.,Phila, PA,09119
"John ""Da Man""",Repici,120 Jefferson St.,Riverside, NJ,08075
Stephen,Tyler,"7452 Terrace ""At the Plaza"" road",SomeTown,SD, 91234
,Blankman,,SomeTown, SD, 00298
"Joan ""the bone"", Anne",Jet,"9th, at Terrace plc",Desert City,CO,00123
```

CSV files are commonly read and written by programs like spreadsheets, making them a standard format for data exchange and storage. When viewed in a table, CSV data might look like this:

| Name      | Age | Occupation |
| --------- | --- | ---------- |
| John Doe  | 28  | Engineer   |
| Jane Doe  | 32  | Designer   |
| Sam Smith | 24  | Developer  |

Why CSV?

- Simplicity: CSV files are plain text, making them easy to create, read, and edit using any text editor.
- Portability: Because they are simple text files, CSV files can be easily shared across different systems and platforms without compatibility issues.
- Efficiency: CSV files are compact and efficient for storing large datasets without the overhead associated with more complex file formats.
- Compatibility: CSV is supported by a wide range of applications, including spreadsheet programs like Microsoft Excel and Google Sheets, databases, and various data processing tools.

Why handle CSV Files?

- Data Exchange: CSV is a universal format for data exchange between different systems and applications.
- Data Storage: It provides a lightweight and easy-to-read method for storing large datasets.
- Data Analysis: CSV files are frequently used in data analysis and machine learning projects due to their simplicity and compatibility with various tools.

This project will provide hands-on experience in file manipulation and parsing, essential skills for any developer. You will learn to manually process CSV data, enhancing your understanding of fundamental programming concepts.

### Resources

<!-- Tip: useful resources here -->

- [Go File I/O](https://golang.org/pkg/os/)
- [CSV Format](https://tools.ietf.org/html/rfc4180)
- [Go Interfaces](https://golang.org/doc/effective_go.html#interfaces)

### General Instructions

<!--
    Tip: general instructions here
    You MUST change this points to align with your project.
-->

- Your code MUST be written in accordance with [gofumpt](https://github.com/mvdan/gofumpt). If not, you will be graded `0` automatically.
- Your program MUST be able to compile successfully.
- Your program MUST not exit unexpectedly (any panics: `nil-pointer dereference`, `index out of range` etc.). If so, you will be get `0` during the defence.
- Only built-in packages are allowed. If not, you will get `0` grade.
- The project MUST be compiled by the following command in the project's root directory:

```sh
$ go build -o csvparser .
```

### Mandatory Part

You will build a CSV library in Go. Implement following functions:

#### ReadCSVLine

interface:

```go
type CSV interface  {
    ReadCSVLine(file *os.File) ([]string, error)
    GetCSVField(n int) (string, error)
    GetNumberOfFields() int
}
```

This function should read a new line from a CSV file.

```go
func ReadCSVLine(file *os.File) ([]string, error)
    // Implementation goes here
```

- Reads one line from open input file
- Returns pointer to line, with terminator removed, or nil if EOF occurred
- Calling ReadCSVLine in a loop allows you to sequentially read each line from the file, continuing until the end of the file is reached.
- Assumes that input lines are terminated by `\r`, `\n`, `\r\n`, or `EOF`
- Return nil if memory limit exceeded.
- The retured line should include the `\n`, if there is one at the end.
- Using `bufio` is forbidden

> **Note:**
> What if you will read a file with 1000000 (a lot) entries?
> Each time ReadCSVLine is called, you should read only until newline. Avoid reading the entire file at once and then processing each line.

#### GetCSVField

This function should return the nth field.

```go
func GetCSVField(n int) (string, error) {
    // Implementation goes here
}

```

- Returns n-th field from last line read by ReadCSVLine;
- Returns nil if n < 0 or beyond last field
- Fields are separated by commas
- Fields may be surrounded by "..."; such quotes are removed
- There can be an arbitrary number of fields of any length
- Returns nil if memory limit exceeded

#### GetNumberOfFields

```go
func GetNumberOfFields() int {
    // Implementation goes here
}
```

- Returns number of fields on last line read by ReadCSVLine
- Behavior undefined if called before ReadCSVLine is called

> **Note:**
> Make sure that `GetNumberOfFields` and `GetCSVField` fuctions behaves well if they are called after ReadCSVLine has encountered EOF.

> **Note:**
> Working with files and parsing can be complicated. Make sure to test well your library before submitting the project. For example try different file types, what happens when you pass a binary file?

- You can create as many util functions as you need.

- Global variables are forbidden

- Allowed packages: "fmt", "os"

#### Test

Here is an example on how your functions will be tested:

```go
func main() {
    file, err := os.Open("example.csv")
    if err != nil {
        fmt.Println("Error opening file:", err)
        return
    }
    defer file.Close()

    for {
        fields, err := ReadCSVLine(file)
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

### Support

<!--
    Tip: leave this section unchanged.
    This is a static text, which student must read in every project.
-->

If you get stuck, try your code with the example inputs from the project. You should get the same results. If not, read the description again. Maybe you missed something, or your code is wrong. After the examples work, but your answer is still wrong, make some test cases you can check by hand. See if they work with your code. Use the complete example input.

If you are still stuck, ask a friend for help or take a break and come back later.

### Guidelines from Author

Starting a new project can be challenging, and designing a library or interface correctly is unlikely on the first attempt. Here are some detailed guidelines to help you get started:

- Understand the CSV Format:

  - Study the CSV format, including how fields are separated by commas and how quoted fields can contain commas. Recognize that double quotes within a field are represented by doubled quotes.
  - Ensure you understand the RFC 4180 standard for CSV files, which describes the format in detail.

- Implement the Basic Structure:
  - Start by defining the CSV struct and its associated methods.
  - Hide implementation details to ensure users do not rely on internal behavior that may change. Use unexported (private) variables and functions to keep details encapsulated within the implementation file.

### Author

This project has been created by:

<!-- Tip: type here author's name, position and company -->

Adilyam Tilegenova, Software Developer at Doodocs.kz

Contacts:

<!--
    Tip: list of contacts to reach the author.
    It can be email, linkedin, telegram, instagram, etc.
-->

    Email: adilyamt@gmail.com
    GitHub: https://github.com/Adilyam
    LinkedIn: https://www.linkedin.com/in/adilyam/
