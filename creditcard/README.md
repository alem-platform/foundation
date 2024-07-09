# Warm Welcome

> I think that it's extraordinarily important that we in computer science keep **fun** in computing.
>
> — Alan J. Perlis. Structure & Interpretation of Computer Programs

Welcome to the Foundation branch!

Congratulations on completing your bootcamp journey. This significant achievement shows your dedication and commitment to becoming a Software Engineer. Yet, the path ahead is filled with more challenges that requires you to stay consistent to reach your goals.

As you dive deeper into Programming, it is important to keep fun and curiosity alive throughout your career. Programming is a field where you need to keep learning and adapting.

Let's explore what you will learn during our Foundation branch:
- Bits and bytes  
- Algorithms and Data Structures  
- I/O: File, TCP/IP
- Data Representation: Binary, CSV, JSON, Base64
- Internet: HTTP, REST API
- Basic networking concepts  
- Concurrency and parallelism basics
- Basic software design principles  
- Testing and test-driven development

Stay curious, stay engaged, and keep the joy of discovery in your programming journey. Welcome once again, and let's start this exciting adventure together!

# creditcard

| Learning Objectives   |
| --------------------- |
| `Algorithms`          |
| `I/O`                 |
| `Data Representation` |

> Every computer program is a model, hatched in the mind, of a real or mental process.
>
> — Alan J. Perlis. Structure & Interpretation of Computer Programs

Credit cards are used to pay for goods and services. Each card has a unique number that helps identify the cardholder and the issuing bank. Credit card numbers are long to ensure each card is unique. For example:

- Visa uses 13- and 16-digit numbers.
- MasterCard uses 16-digit numbers.
- American Express uses 15-digit numbers.

These numbers are not random. They follow specific patterns:

- Visa numbers start with 4.
- MasterCard numbers start with 51, 52, 53, 54, or 55.
- American Express numbers start with 34 or 37.

Credit card numbers also include a "checksum" that helps detect errors. This is done using Luhn's Algorithm, a simple math formula that checks if the number is valid.

In this project, you will create a tool called `creditcard` to:

- Validate credit card numbers.
- Generate possible card numbers.
- Get information about card brands and issuers.
- Issue new card numbers.

### Allowed

Only built-in packages are allowed.

### Resources

- [Anatomy of a Credit Card: The Luhn Algorithm Explained](https://www.groundlabs.com/blog/anatomy-of-a-credit-card/)

### Feature: Validate

The `validate` feature checks if a credit card number is valid using Luhn's Algorithm.

Requirements:

- The number must be at least 13 digits long.
- If valid, print `OK` to stdout and exit with status 0.
- If invalid, print `INCORRECT` to stderr and exit with status 1.
- Support passing multiple entries.
- Support `--stdin` flag to pass number from stdin.

```sh
$ ./creditcard validate "4400430180300003"
OK
$ ./creditcard validate "4400430180300002"
INCORRECT
$ ./creditcard validate "4400430180300003" "4400430180300011"
OK
OK
$ echo "4400430180300003" | ./creditcard validate --stdin
OK
$ echo "4400430180300003" "4400430180300011" | ./creditcard validate --stdin
OK
OK
```

### Feature: Generate

The `generate` feature creates possible credit card numbers by replacing asterisks (*) with digits.

Requirements:

- Replace up to 4 asterisks (*) with digits. If more - it's an error.
- Print the generated numbers to stdout.
- Numbers must be printed in ascending order.
- Exit with status 1 if there is any error.
- Support `--pick` flag to randomly pick a single entry.

```sh
$ ./creditcard generate "440043018030****"
4400430180300003
4400430180300011
4400430180300029
...
4400430180309988
4400430180309996
$ ./creditcard generate --pick "440043018030****"
44004301809521
```

In case of an error:

```sh
$ ./creditcard generate --pick "44004301803*****"
$ echo $?
1
```

### Feature: Information

The `information` feature provides details about the card based on data in `brands.txt` and `issuers.txt`.

Requirements:
- Output the card number, validity, brand, and issuer.
- Support `--stdin` flag to pass number from stdin.
- Support passing multiple entries.

```sh
$ ./creditcard information --brands=brands.txt --issuers=issuers.txt "4400430180300003"
4400430180300003
Correct: yes
Card Brand: VISA
Card Issuer: Kaspi Gold
```

In case of an incorrect card number:

```
4400450180300003
Correct: no
Card Brand: -
Card Issuer: -
```

Example content of `brands.txt`:

```
VISA:4
MASTERCARD:51
MASTERCARD:52
MASTERCARD:53
MASTERCARD:54
MASTERCARD:55
AMEX:34
AMEX:37
```

Example content of `issuers.txt`:

```
Kaspi Gold:440043
Forte Black:404243
Forte Blue:517792
Halyk Bonus:440563
Jusan Pay:539545
```

Notice, that both `brands.txt` and `issuers.txt` enumerate different kinds of a card, where each line consists of two parts. First part is the name of the kind and the second is the number prefix (i.e. numbers the card starts with). The parts are separated by `:` (two dots) symbol.

### Feature: Issue

The `issue` feature generates a random valid credit card number for a specified brand and issuer.

Requirements:

- Pick a random number for the specified brand and issuer.
- Exit with status 1 if there is any error.

```sh
$ ./creditcard issue --brands=brands.txt --issuers=issuers.txt --brand=VISA --issuer="Kaspi Gold"
4400430180300003
```

## Support

If you get stuck, try your code with the example inputs from the project. You should get the same results. If not, read the description again. Maybe you missed something, or your code is wrong. After the examples work, but your answer is still wrong, make some test cases you can check by hand. See if they work with your code. Use the complete example input.

If you are still stuck, ask a friend for help or take a break and come back later.
