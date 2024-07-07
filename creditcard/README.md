# creditcard


### Validate

- Minimum length 13.
- `OK` should be printed into stdout.
- Exit with status `0`.

```sh
./creditcard validate "4400430180300003"
OK
```

Support passing number from stdin.

```sh
echo "4400430180300003" | ./creditcard validate --stdin
OK
```

- `INCORRECT` should be printed into stderr.
- Exit with status `1`.

```sh
./creditcard validate "4400430180300002"
INCORRECT
```

Support multiple entries.

```sh
./creditcard validate "4400430180300003" "4400430180300011"
OK
OK
```

```sh
echo "4400430180300003" "4400430180300011" | ./creditcard validate --stdin
OK
OK
```

### Generate

- At most 4 asterisks `*`.
- Should be printed into stdout.
- Numeric order - ascending.
- Exit with status `1` if any error.

```sh
./creditcard generate "440043018030****"
4400430180300003
4400430180300011
4400430180300029
...
4400430180309988
4400430180309996
```

Support `--pick` flag to randomly pick a single entry.

```sh
./creditcard generate --pick "440043018030****"
44004301809521
```

Support passing number from stdin.

### Information

Content of `brands.txt`:

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

Content of `issuers.txt`:

```
Kaspi Gold:440043
Forte Black:404243
Forte Blue:517792
Halyk Bonus:440563
Jusan Pay:539545
```

```sh
./creditcard information --brands=brands.txt --issuers=issuers.txt "4400430180300003"
```

```
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

- Support stdin
- Support multiple entries

### Issue

Must pick random number.

```sh
./creditcard issue --brands=brands.txt --issuers=issuers.txt --brand=VISA --issuer="Kaspi Gold"
```

Output:
```
4400430180300003
```
