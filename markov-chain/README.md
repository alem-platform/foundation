# markov-chain

## Learning Objectives

- Algorithms
- I/O
- File Handling
- Basic software design principles

## Abstract

In this project you will build a text generator using Markov Chain algorithm.

Similar algorithms are used in your phones. For example, when you type a word in the 
keyboard it suggests you the next the most probable word.

This project teaches you that before starting writing code you should consider what your
data-structures will be, how would you store your data and only then think of your code.

## Context

> Bad programmers worry about the code. Good programmers worry about data structures and their relationships.
>
> — Linus Torvalds.

The challenge we've selected might seem unique, but at its core, it shares a common structure with many other programs: data comes in, data goes out, and the processing involves a bit of creativity.

Our specific task is to generate random English text that reads naturally. If we were to emit random letters or random words, the output would be nonsensical. For instance, a program that randomly picks letters (and spaces to separate words) might generate something like this:

```
tqrhyla kjsbq oewzj xna i smdkt uivbxfytnvplxq
```

Similarly, selecting words at random from a dictionary won't produce coherent text either:

```
almanac threshold griddle 9f zigzag cactus haphazardly blue anchor
```

To achieve more meaningful results, we need a statistical model with greater structure, such as one that considers the frequency of entire phrases. But how can we obtain such statistical data?

**Markov Chain Algorithm**

An intelligent way to handle this type of text generation is with a technique called a Markov chain algorithm. This method views the input as a series of overlapping sequences, breaking each sequence into two parts: a multi-word prefix and a single suffix word that follows the prefix. The algorithm creates new sequences by randomly picking a suffix that follows each prefix, based on patterns in the original text. Using three-word sequences, with a two-word prefix to select the suffix, works well:

```
1. Set w1 and w2 to the first two words of the text.
2. Print w1 and w2.
3. Loop:
    a. Randomly choose w3, a word that can follow w1 and w2 in the text.
    b. Print w3.
    c. Update w1 to w2 and w2 to w3.
    d. Repeat loop.
```

For example, we can generate random text from a few sentences paraphrased from the earlier text, using two-word prefixes:

```
Bad programmers worry about code. Good programmers worry about data-structures and their relationships.
```

Here are word pairs and the words that follow them:

| Input prefix          | Suffix words that follow |
| --------------------- | ------------------------ |
| Bad programmers       | worry                    |
| programmers worry     | about, about             |
| worry about           | code., data-structures   |
| about code.           | Good                     |
| Good programmers      | worry                    |
| about data-structures | and                      |
| data-structures and   | their                    |
| and their             | relationships.           |
| their relationships.  | (end)                    |

A Markov algorithm processing this text will begin by:
- Printing `Bad programmers` and will print `worry`.
- Then current prefix becomes `programmers worry`, which will be followed by `about`.
- The next prefix becomes `worry about`, which then randomly pick either `code.` or `data-structures`.
- If it chooses `code.`, the current prefix becomes `about code.` and the next word will be `Good`.
- If it chooses `data-structures`, the next word will be `and`.

This process continues until sufficient output is produced or an end-marker is reached as a suffix.

Our program will take an English text and use a Markov chain algorithm to create new text based on the frequency of fixed-length phrases. The prefix length, which is two words in our example, can be adjusted as a parameter.

How do we define a word? The simple answer might be a group of letters, but it's better to include punctuation with the words. This way, "words" and "words." are seen as different. Why? It helps make the created text sound more natural by letting punctuation, and as a result, grammar, affect which words are chosen. So, we'll say a "word" is anything found between spaces. This approach works for any language and keeps punctuation connected to the words.

**Data Structure**

Let's consider our program's requirements. We're aiming to handle a book's worth of text - say, 100,000 words or more. Our output should be perhaps hundreds or thousands of words, and we want results in seconds, not minutes. With this scale, we can't afford inefficient algorithms.

The Markov process demands we digest all input before producing output. This means we need a clever way to store and access our data. We're looking for a structure that can represent a prefix and its suffixes efficiently. It should allow quick lookups, handle prefixes of any length, and accommodate a growing list of suffixes.

Our program will work in two stages: first, building this data structure from the input; then using it to generate random output. Both stages need to find prefixes quickly.

Let's call a prefix and all its possible suffixes a "state" - that's the usual term in Markov models. We'll need to add suffixes one by one, not knowing in advance how many we'll encounter. And when it's time to generate output, we must be able to pick a suffix at random.
This approach gives us a clear path forward. By focusing on these key requirements, we can design a solution that's both elegant and efficient.

## Resources

- [the_great_gatsby.txt](./the_great_gatsby.txt)


## General Criteria

- Your code MUST be written in accordance with [gofumpt](https://github.com/mvdan/gofumpt). If not, you will be graded `0` automatically.
- Your program MUST be able to compile successfully.
- Your program MUST not exit unexpectedly (any panics: `nil-pointer dereference`, `index out of range` etc.). If so, you will be get `0` during the defence.
- Only built-in packages are allowed. If not, you will get `0` grade.
- The project MUST be compiled by the following command in the project's root directory:

```sh
$ go build -o markovchain .
```

## Mandatory Part

### Baseline

By default your program must read from stdin the whole text and generate the text according to Markov Chain algorithm.

Outcomes:
- Program prints generated text according to the Markov Chain algorithm.

Notes:
- Suffix length is ALWAYS 1 word.
- Default prefix length is 2 words.
- Default starting prefix is the first N words of the text, where N is the length of the prefix.
- Default number of maximum words is 100.

Constraints:
- If any error print an error message indicating the reason.
- The code should stop generating code after it printed maximum number of words or encountered the very last word in the text.

Examples:

```sh
$ cat the_great_gatsby.txt | ./markovchain | cat -e
Chapter 1 In my younger and more stable, become for a job. He hadn't eat anything for a long, silent time. It was the sound of someone splashing after us over the confusion a long many-windowed room which overhung the terrace. Eluding Jordan's undergraduate who was well over sixty, and Maurice A. Flink and the great bursts of leaves growing on the air now. "How do you want? What do you like Europe?" she exclaimed surprisingly. "I just got here a minute. "Yes." He hesitated. "Was she killed?" "Yes." "I thought you didn't, if you'll pardon my--you see, I carry$
```

```sh
$ cat the_great_gatsby.txt | ./markovchain | wc -w
   100
```

```sh
$ ./markovchain
Error: no input text
```

### Number of words

Your program must be able to accept maximum number of words to be generated.

Outcomes:
- Program prints generated text according to the Markov Chain algorithm limited by the given maximum number of words.

Constraints:
- Given number can't be negative.
- Given number can't be more 10,000.
- If any error print an error message indicating the reason.

```sh
$ cat the_great_gatsby.txt | ./markovchain -w 10 | cat -e
Chapter 1 In my younger and more stable, become for$
```

### Prefix

Your program must be able to accept the starting prefix.

Outcomes:
- Program prints generated text according to the Markov Chain algorithm that starts with the given prefix.

Constraints:
- Given prefix must be present in the original text.
- If any error print an error message indicating the reason.

```sh
$ cat the_great_gatsby.txt | ./markovchain -w 10 -p "to play" | cat -e
to play for you in that vast obscurity beyond the$
```

### Prefix length

Your program must be able to accept the prefix length.

Outcomes:
- Program prints generated text according to the Markov Chain algorithm with the given prefix length.

Constraints:
- Given prefix length can't be negative.
- Given prefix length can't be greater than 5.
- If any error print an error message indicating the reason.

```sh
$ cat the_great_gatsby.txt | ./markovchain -w 10 -p "to something funny" -l 3
to something funny the last two days," remarked Wilson. "That's
```

### Usage

Your program must be able to print usage information.

Outcomes:
- Program prints usage text.

```sh
$ ./markovchain --help
Markov Chain text generator.

Usage:
  markovchain [-w <N>] [-p <S>] [-l <N>]
  markovchain --help

Options:
  --help  Show this screen.
  -w N    Number of maximum words
  -p S    Starting prefix
  -l N    Prefix length
```

## Support

If you get stuck, try your code with the example inputs from the project. You should get the same results. If not, read the description again. Maybe you missed something, or your code is wrong. After the examples work, but your answer is still wrong, make some test cases you can check by hand. See if they work with your code. Use the complete example input.

If you are still stuck, ask a friend for help or take a break and come back later.


## Guidelines from Author

Before diving into code, it's crucial to step back and consider the foundation of your program: the data structures. This project illustrates a fundamental principle of good software design - your choice of data representation often dictates the clarity and efficiency of your code.

Start by asking yourself: How will I organize the information? What structures will best serve the problem at hand? Only after you've carefully thought through these questions should you begin to sketch out your algorithms and write code.

This approach might seem like extra work upfront, but it pays dividends. A well-chosen data structure can make your code simpler, more readable, and often more efficient. It's like choosing the right tools before starting a job - with the proper foundations in place, the rest of the work becomes much more straightforward.

Remember, in programming, as in many things, thoughtful preparation is key to success. Take the time to get your data structures right, and you'll find the coding process smoother and more enjoyable.

To give you some real-life example of such a principle is how git was designed:

> git actually has a simple design, with stable and reasonably well-documented data structures. In fact, I'm a huge proponent of designing your code around the data, rather than the other way around, and I think it's one of the reasons git has been fairly successful […] I will, in fact, claim that the difference between a bad programmer and a good one is whether he considers his code or his data structures more important.

Good data structures are the foundation of clear, efficient code. They often simplify programming more than clever algorithms can. Invest time in designing your data representation first. This approach usually leads to more maintainable and understandable programs, regardless of their size or complexity.

## Author

This project has been created by:

Alimukhamed Tlekbai, Team Lead at Doodocs.kz

Contacts:
- Email: tlekbai@doodocs.kz
- [GitHub](https://github.com/atlekbai/)
- [LinkedIn](https://www.linkedin.com/in/atlekbai/)
