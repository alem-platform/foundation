# HTTP Client URL

## Learning Objectives

- TCP
- HTTP
- Command-line interface

## Abstract

In this project, you will recreate some functionalities of `curl` using standard library excluding `net/http`.  

`curl` is a highly popular and widely-used command-line tool that allows users to transfer data to and from servers over various protocols, primarily HTTP and HTTPS, making it an essential utility for web communication, API testing, and file downloads across numerous platforms.

This project teaches you how to build a low-level HTTP client using raw TCP sockets in Go, enhancing your understanding of networking, HTTP protocol, and error handling, while developing problem-solving and debugging skills.

## Context

curl is a simple yet powerful tool for communicating with servers. Here are some common applications:
* Fetching Web Pages: You can easily retrieve web pages by running a command like `curl https://example.com`, which will display the HTML content directly in your terminal.
* Testing APIs: curl is frequently used to send requests to RESTful APIs. For instance, to fetch user information, you might use `curl -X GET https://api.example.com/users/1`.
* Uploading Files: You can upload files to a server using the POST method. For example: `curl -X POST -F "file=@/path/to/file" https://upload.example.com`.
* Debugging: You can check server responses and headers with commands like `curl -I https://example.com`, which retrieves only the HTTP headers.

In this project, your primary task is to implement several functionalities found in curl. 
Your program should be capable of:
* Communicating with servers using the HTTP protocol
* Sending GET requests
* Sending POST requests with additional data
* Handling redirections
* Allowing users to customize headers
* Handling errors
* Reading options from a plain text configuration file

When implementing the project, consider handling options similarly to how curl does. You can find more information about options [here](https://everything.curl.dev/cmdline/options/index.html). 

Additionally, you should map errors to the appropriate exit codes. More details on exit codes can be found [here](https://everything.curl.dev/cmdline/exitcode.html). 
## Resources

- [An overview of HTTP - HTTP | MDN](https://developer.mozilla.org/en-US/docs/Web/HTTP/Overview)
- [Command line HTTP - everything curl](https://everything.curl.dev/http/index.html)
- [curl - Documentation Overview](https://curl.se/docs/)
- [net package](https://pkg.go.dev/net)
## General Criteria

- Your code MUST be written in accordance with [gofumpt](https://github.com/mvdan/gofumpt). If not, you will be graded `0` automatically.
- Your program MUST be able to compile successfully.
- Your program MUST not exit unexpectedly (any panics: `nil-pointer dereference`, `index out of range` etc.). If so, you will be get `0` during the defence.
- Only built-in packages from the Go standard library are allowed, excluding `net/http`.

The project MUST be compiled by the following command in the project's root directory:

```bash
$ go build -o http-curl .
```

## Mandatory Part

### Usage

Your program should mimic the basic behavior of `curl`, allowing users to perform various HTTP operations from the command line. For example:

```bash
./http-curl http://example.com
```

This command will perform a default GET request to the given URL.

The program must operate exclusively with the HTTP protocol, without relying on the `net/http` package or external HTTP libraries. Requests should be handled through raw TCP connections.

### --help 

When running `./http-curl --help`, the output should list and describe the supported options:

```
Usage: ./http-curl [options] [URL]

Options:
  -X [GET|POST]         Specify the HTTP request method (GET or POST)
  -d "data"             Send data with POST request
  -H "header"           Set custom HTTP headers (e.g., "User-Agent: MyClient")
  -v                    Enable verbose mode to display detailed request and response information
  -c [config file]      Read options from a plain text configuration file
  -h, --help            Display this help message and exit
```


### Sending GET requests

The program should handle GET requests. For example:

```bash
./http-curl -X GET http://example.com
```

This retrieves the content from the given URL.

>When the method is not specified, the program should send GET request

### Sending POST requests

The program should handle POST requests and support sending data. For example:

```bash
./http-curl -X POST -d "name=John" http://example.com
```

This sends `"name=John"` as a POST request.

When the method is not specified, but option `-d` is given, the program should send POST request.
### Handling redirections

The program should automatically follow HTTP redirection responses (3xx status codes) up to a limit of 5 redirections.

### Customizing headers

Your program should allow for custom HTTP headers to be set. For example:

```bash
./http-curl -X GET -H "User-Agent: MyClient" http://example.com
```

This sends a GET request with a custom `User-Agent` header.

### Handling errors

Error handling should map failures to appropriate exit codes, following `curl` conventions. For instance, a connection failure might map to a specific exit code. Reference the `curl` error codes guide [here](https://everything.curl.dev/cmdline/exitcode.html).

### Reading options from a configuration file

Your program should support reading options from a plain text configuration file. The file could look like:

```
method=GET
url=http://example.com
headers=User-Agent: MyClient
```

You should be able to run the program like this:

```bash
./http-url -c config.txt
```

More about usage of a config file can be found [here](https://everything.curl.dev/cmdline/configfile.html)

### Supported options

The following options should be supported:

- **`-X [GET|POST]`**: Specify the HTTP request method.
- **`-d`** or **`--data`**: Provide data for a POST request
- **`-F`** or **`--form`**
- **`-H`** or **`--header`**: Set custom HTTP headers.
- **`-v`** or **`--verbose`**: Display detailed information about the HTTP transaction.
- **`-K`** or **`--config`**: Read options from a configuration file.
- **`-h` or `--help`**: Display help information and explain available options.
- **`-f`** or **`--fail`** : Fail silently on server errors, returning exit code 22 on HTTP errors

### Example of `-v` usage

When using the `-v` (verbose) option, the program should display detailed information about the HTTP request and response, similar to:

```bash
./http-curl -X GET -v http://example.com
```

This will output:

```
> GET / HTTP/1.1
> Host: example.com
> User-Agent: your_program/1.0
> Accept: */*
< HTTP/1.1 200 OK
< Content-Type: text/html
< Content-Length: 1024
...
```

This helps in debugging and seeing exactly what’s happening during the HTTP transaction.
## Support

Start by implementing basic HTTP functionality like GET and POST requests. Add more features gradually, such as URL redirection and custom headers. When stuck, refer to the `curl` documentation for guidance. Test your program by comparing its behavior with previous projects to ensure correctness. Build iteratively and validate each step before moving on to the next.

## Guidelines from Author

### 1. Plan Before You Code

Before jumping into writing code, take a step back and plan out what you're going to do. Think about the idea first—code comes second. Having a clear plan makes coding smoother and helps avoid problems later on.

### 2. Don’t Be Afraid to Rewrite

As you work on your project, you’ll understand it better with each step. If you have time, try rewriting parts of it. Every time you rewrite, you learn something new and improve your skills. It’s all part of the learning process.

### 3. Get It Working First, Then Improve

Focus on making your program work before you worry about making it perfect. Once it’s running, you can clean up the code, make it more efficient, and add complexity. Start simple, then improve.

### 4. Add Cool Extras if You Can

If you have extra time, why not add some bonus features? Try adding a little something extra just for fun or to learn more. It’s a great way to push yourself and make your project stand out

## Author

This project has been created by:

Shakhanov Darkhan, Software Engineer at Cyberlabs.kz

Contacts:
* Email: darkhan.shakhanov@gmail.com
* [GitHub](https://github.com/DarkhanShakhan)
* [LinkedIn](https://www.linkedin.com/in/darkhan-shakhanov)