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
,"2SOMHz","400MHz","Lines o f "
,"RlOOOO","Pentium I I " , " source code "
C,0.36 s e c , 0 . 3 0 sec.150
lava , 4.9 .9 .2 , 10
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

This function should read a new line from a CSV file.

```go
func (с CSV) ReadCSVLine(file *os.File) ([]string, error) {
    // Implementation goes here
}
```

- Reads one line from open input file
- Returns pointer to line, with terminator removed, or nil if EOF occurred
- Calling ReadCSVLine in a loop allows you to sequentially read each line from the file, continuing until the end of the file is reached.
- Assumes that input lines are terminated by \r, \n, \r\n, or EOF
- Return nil if memory limit exceeded.
- The retured line should include the `\n`, if there is one at the end.

> **Note:**
> What if you will read a file with 1000000 (a lot) entries?
> Each time ReadCSVLine is called, you should read only until newline. Avoid reading the entire file at once and then processing each line.

#### GetCSVField

This function should return the nth field.

```go
func (с CSV) GetCSVField(n int) (string, error) {
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
func (с CSV) GetNumberOfFields() int {
    // Implementation goes here
}
```

- Returns number of fields on last line read by ReadCSVLine
- Behavior undefined if called before ReadCSVLine is called

> **Note:**
> Make sure that `GetNumberOfFields` and `GetCSVField` fuctions behaves well if they are called after ReadCSVLine has encountered EOF.

> **Note:**
> Working with files and parsing can be complicated. Make sure to test well your library before submitting the project. For example try different file types, what happens when you pass a binary file?

### Support

<!--
    Tip: leave this section unchanged.
    This is a static text, which student must read in every project.
-->

If you get stuck, try your code with the example inputs from the project. You should get the same results. If not, read the description again. Maybe you missed something, or your code is wrong. After the examples work, but your answer is still wrong, make some test cases you can check by hand. See if they work with your code. Use the complete example input.

If you are still stuck, ask a friend for help or take a break and come back later.

### Guidelines from Author

Starting a new project can be challenging. We are unlikely to get the design of a library or interface right on the first attempt. Here are some steps from where to begin:

- Understand the CSV Format.
- Write the ReadCSVLine Function:
  - This function should read lines from a file byte-by-byte without using bufio. Focus on handling different line lengths and termination characters.
  - Implement the GetCSVField Method:
  - Ensure it correctly handles fields separated by commas and quoted fields.
- Handle File Operations.
- Add Error Handling.
- Implement the Basic Structure:
  - Start by defining the Parser interface.
- Test Your Implementation:
  - Write test cases to validate your CSV parser.
  - Use various CSV formats to ensure your parser handles edge cases correctly.

Focus on ensuring your code handles edge cases like quoted fields with commas.

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
