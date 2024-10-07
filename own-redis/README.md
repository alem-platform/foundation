# Learning Objectives
- TCP/IP
- UDP protocol
- basic networking concept
# Abstract
in this project, you will create you own key-value database to:
- store value by key
- retrieve stored data from given key
# Context

In the programming world, there is often a need to store unstructured key-value data without explicit relationships. In such cases, key-value databases come to the rescue. The principle of their work is quite simple: by a given key, they directly store the value in RAM. Due to their simplicity, such databases work very quickly

Example of such databases are:
- [Redis](https://redis.io/)
- [MongoDB](https://www.mongodb.com/)
- [Dynomydb](https://aws.amazon.com/dynamodb/)

Basic operations of inserting and retrieving a value by key work in constant time O(1)

# General Criteria
- Your code MUST be written in accordance with [gofumpt](https://github.com/mvdan/gofumpt). If not, you will be graded `0` automatically.
- Your program MUST be able to compile successfully.
- Your program MUST not exit unexpectedly (any panics: `nil-pointer dereference`, `index out of range` etc.). If so, you will be get `0` during the defence.
- Only built-in packages are allowed. If not, you will get `0` grade.
- The project MUST be compiled by the following command in the project's root directory:

```shell
$ go build -o own-redis .
```

# Mandatory Part


Writing a key-value store via REST API would be too easy, wouldn't it? Let's make it a bit more complicated and let the client and your application communicate using the UDP protocol, i.e. each request and response is a single UDP packet. In our key-value store implementation there are only two methods, SET and GET. SET puts the value by key, and GET gets and returns the given value back to the client or returns an error if the value was not found in the database.

### SET

Any request that has an equal sign in its message (“`=`”, or ASCII 61) will be considered an insert request.

The equal sign (“`=`”) separates the key (before the sign) and the value (everything after the sign) from each other

Example:
- `foo=bar` will insert a key `foo` with value “`bar`”.
- `foo=bar=baz` will insert a key `foo` with value “`bar=baz`”.
- `foo===` will insert a key `foo` with value “`==`”.

If the client tries to insert a value into an existing key, your application should update the value to the last value specified by the client (new).

SET should not return any message
### GET

A GET request is any request that does not contain an equal sign (“`=`”, or ASCII 61). When attempting to query for an existing key, the server should return a response in the format `key=value`.

Example request:

```
foo
```

Example response:

```
foo=bar
```

Otherwise, if the key is not in the database, the server should return the message `keyDoesNotExists`

Example request:

```
bar
```

Example response:

```
keyDoesNotExists
```


# Guidelines from Author

For debugging and testing your application, it is better to use the built-in linux utility [netcat](https://www.commandlinux.com/man-page/man1/nc.1.html)

# Author
This project has been created by:

Askaruly Nurislam, alumni of Alem school

Contacts:

- Email: [askaruly@hotmail.com](mailto:askaruly@hotmail.com)
- [GitHub](https://github.com/darwin939/)