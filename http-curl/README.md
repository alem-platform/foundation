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
* Sending requests with any HTTP method
* Handling redirections
* Allowing users to customize headers
* Handling errors
* Using a configurable timeout
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

### Baseline

By default, your program must send a GET request to the specified URL if no additional options are provided.

Outcomes:

- Prints the response from the server

Notes: 
- All requests must be handled via direct TCP connections.

- If no method is specified, GET is the default.
- If data is provided with the `-d` or `--data` option and no method is specified, the program must send a POST request.
- It must automatically follow up to 5 HTTP redirections (3xx status codes)

Constraints:

- If an error occurs (e.g. connection issues, invalid input), the program must map it to a specific exit code. Reference the `curl` error codes guide [here](https://everything.curl.dev/cmdline/exitcode.html)

Examples:
```bash
$ ./http-curl http://example.com
```


### Help documentation

When running `./http-curl --help`, the output must print usage example and list and describe the supported options:

```
Usage: ./http-curl [options] [URL]

Options:
  -X  "method"                    Specify the HTTP request method
  -d, --data "data"               Send data with POST request
  -H, --header "header"           Set custom HTTP headers (e.g., "User-Agent: MyClient")
  -K, --config [config file]      Read options from a plain text configuration file
  -h, --help                      Display this help message and exit
```

### HTTP methods

The program must be able to send requests with any arbitrary HTTP method. The desired method can be specified using the `-X` option.


For example: 
```bash
$ ./http-curl -X GET http://example.com
```

or 

```bash
$ ./http-curl -X POST -d "name=John" http://example.com
```

or

```bash
$ ./http-curl -X DELETE http://example.com
```

### Customizing headers

Your program must allow users to customize HTTP headers. The header must be customized using `-H` or `--header`.

```bash
$ ./http-curl -X GET -H "User-Agent: MyClient" http://example.com
``` 

Notes: 
- Several headers must be customized by using `-H` or `--header` for each header. 

```bash 
$ ./http-curl -X GET -H "User-Agent: MyClient" -H "Authorization: Bearer token123" http://example.com
```

Constraints:
- The program must validate header format: `Header-Name: Header-Value`.


### Timeout

The program must process requests within a default timeout of 30 seconds. This timeout can be adjusted using the `--max-time` or `-m` option.

Notes:
* The timeout value is specified in seconds.
* The timeout applies to the entire duration of the request, including connection setup and data transfer.
* If the specified timeout is reached before the request completes, the program should terminate the connection and return an error.
* If no timeout is set, the default 30 seconds will be used to ensure the program does not hang indefinitely.
  
Examples:

```bash
$ ./http-curl --max-time 10 http://example.com
```

### Configuration file

The program must be able to read options from a plain text configuration file using `-K` or `--config` option.

Notes:
- Options must be specified on separate lines.
- The url must be specified using `--url` or `url`.
- The lines starting with `#` must be treated as comments.
- Long or short command-line options can be used, and arguments can be separated by spaces, colons (:), or equals signs (=).
- Long options can be used without leading dash lines.

Constraints:
- The program must validate the configuration file format. If it is invalid, the program should display an error and show the correct usage.

Examples:
```bash
$ ./http-curl -K config.txt
```

#### config.txt

```
-X POST
# this is comment
url = http://example.com
header: "User-Agent: MyClient"
```

More about usage of a config file can be found [here](https://everything.curl.dev/cmdline/configfile.html)

## Support

Start by implementing basic HTTP functionality like GET and POST requests. Add more features gradually, such as URL redirection and custom headers. When stuck, refer to the `curl` documentation for guidance. Test your program by comparing its behavior with previous projects to ensure correctness. Build iteratively and validate each step before moving on to the next.

## Guidelines from Author

### 1. Plan Before You Code

Before jumping into writing code, take a step back and plan out what you're going to do. Think about the idea first — code comes second. Having a clear plan makes coding smoother and helps avoid problems later on.

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