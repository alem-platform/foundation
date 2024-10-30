# own-redis

## Learning Objectives

- TCP/IP
- UDP protocol
- Basic networking concepts

## Abstract

This project involves developing a custom key-value database that enables users to store and retrieve data using unique keys.
The database operates in RAM for quick access, utilizing the UDP protocol for communication between the storage and the client.
Key operations include SET, GET, and PING. This project aims to deepen understanding of networking and database principles while providing practical experience in building a key-value store

## Context

In the programming world, there is often a need to store unstructured key-value data without explicit relationships. In such cases, key-value databases come to the rescue. The principle of their work is quite simple: by a given key, they directly store the value in RAM. Due to their simplicity, such databases work very quickly

Examples of such databases are:
- [Redis](https://redis.io/)
- [Memcached](https://memcached.org/)

Basic operations of inserting and retrieving a value by key in the above mentioned databases work in constant time O(1)

## General Criteria

- Your code MUST be written in accordance with [gofumpt](https://github.com/mvdan/gofumpt). If not, you will be graded `0` automatically.
- Your program MUST be able to compile successfully.
- Your program MUST not exit unexpectedly (any panics: `nil-pointer dereference`, `index out of range` etc.). If so, you will be get `0` during the defence.
- Only built-in packages are allowed. If not, you will get `0` grade.
- The project MUST be compiled by the following command in the project's root directory:
```shell
$ go build -o own-redis .
```
- The default port for the program should be `8080`, but it can be changed with the optional argument `--port`.
```shell
$ ./own-redis --port 8080
```

## Mandatory Part

Writing a key-value store via REST API would be too easy, wouldn't it? Let's make it a bit more complicated and let the client and your application communicate using the UDP protocol, i.e. each request and response is a single UDP packet. In our key-value store implementation you have to implent three methods SET, GET and PING. SET puts a key-value and GET gets and returns the given value back to the client. The PING command verifies that the storage is working.

NOTE:
- Command names, command arguments are  case-insensitive. So `PING`, `ping` and `Ping` are all valid and denote the same command

### PING

`PING` is one of the simplest Redis commands. It's used to check whether a Redis server is healthy. The response for the `PING` command is `PONG`
```sh
$ nc 0.0.0.0 8080
PING
PONG
```

### SET

Any request that has the string SET as the first argument in its message will be considered an insert request

Example:
- `SET foo bar` will insert a key `foo` with value “`bar`”.
- `SET foo bar baz` will insert a key `foo` with value “`bar baz`”.

If the number of arguments is not enough to save the key, the server should return an error

Example:
- `SET KEYVAL` will return error with text “`(error) ERR wrong number of arguments for 'SET' command`”
- `SET` will return error with text “`(error) ERR wrong number of arguments for 'SET' command`”.

SET should return `OK`

```sh
$ nc 0.0.0.0 8080
SET Foo Bar
OK
```
#### Options
The `SET` command supports option `PX` that modify its behavior:
- `PX` _milliseconds_ - Set the specified expire time, in milliseconds (a positive integer)
Example:
```sh
$ nc 0.0.0.0 8080
SET foo bar px 10000
OK
GET foo
bar
```
A request within 10000 milliseconds will produce a bar response, but once the time is up, the server should clear the value and should return `(nil)` when client attempting to retrieve the value
```sh
$ nc 0.0.0.0 8080
GET foo
(nil)
```
### GET

A GET request is any request in which the first argument contains the `GET` command. When attempting to query an existing key, the server must return the previously stored value in response

Example:
```sh
$ nc 0.0.0.0 8080
SET Foo Bar
OK
GET Foo
Bar
```

Otherwise, if the key is not in the storage, the server should return a `(nil)` message

Example:
```sh
$ nc 0.0.0.0 8080
GET RandomKey
(nil)
```

If the client tries to insert a value into an existing key, your application should update the value to the last value specified by the client (new)

Example:
```sh
$ nc 0.0.0.0 8080
SET Foo Bar
OK
GET Foo
Bar
SET Foo Buz
OK
GET Foo
Buz
```

Note:
- Your program will be tested with parallel/competitive requests. To avoid data races, you need to use synchronization primitives from the [sync](https://pkg.go.dev/sync) package

### Usage
Your program must be able to print usage information.

Outcomes:

- Program prints usage text.

```shell
$ ./own-redis --help
Own Redis

Usage:
  own-redis [--port <N>]
  own-redis --help

Options:
  --help       Show this screen.
  --port N     Port number.
```

## Guidelines from Author

For debugging and testing your application, it is better to use the built-in linux utility [netcat](https://www.commandlinux.com/man-page/man1/nc.1.html)

## Author

This project has been created by:

Askaruly Nurislam, alumni of Alem school

Contacts:
- Email: [askaruly@hotmail.com](mailto:askaruly@hotmail.com)
- [GitHub](https://github.com/darwin939/)