# markov-chain

### Learning Objectives

- Algorithms
- I/O
- File Handling
- Basic software design principles

### Abstract

This project will teach you on that thinking first about data-structures first is the best way to design programs.

<!-- 
    Tip: Write a short description of what student
    will do during this project.
-->


In this project, you will create a tool called `creditcard` to:

- Validate credit card numbers.
- Generate possible card numbers.
- Get information about card brands and issuers.
- Issue new card numbers.


### Context

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

Let's consider our program's requirements. We're aiming to handle a book's worth of text - say, 100,000 words or more. Our output should be substantial, perhaps thousands of words, and we want results in seconds, not minutes. With this scale, we can't afford inefficient algorithms.

The Markov process demands we digest all input before producing output. This means we need a clever way to store and access our data. We're looking for a structure that can represent a prefix and its suffixes efficiently. It should allow quick lookups, handle prefixes of any length, and accommodate a growing list of suffixes.

Our program will work in two stages: first, building this data structure from the input; then using it to generate random output. Both stages need to find prefixes quickly.

Let's call a prefix and all its possible suffixes a "state" - that's the usual term in Markov models. We'll need to add suffixes one by one, not knowing in advance how many we'll encounter. And when it's time to generate output, we must be able to pick a suffix at random.
This approach gives us a clear path forward. By focusing on these key requirements, we can design a solution that's both elegant and efficient.

### Resources

<!-- Tip: useful resources here -->
- [Anatomy of a Credit Card: The Luhn Algorithm Explained](https://www.groundlabs.com/blog/anatomy-of-a-credit-card/)

### General Instructions


!!! You have to choose most-appropriate data-structure

<!-- 
    Tip: general instructions here
    You MUST change this points to align with your project.
-->

- Your code MUST be written in accordance with [gofumpt](https://github.com/mvdan/gofumpt). If not, you will be graded `0` automatically.
- Your program MUST be able to compile successfully.
- Your program MUST not exit unexpectedly (any panics: `nil-pointer dereference`, `index out of range` etc.). If so, you will be get `0` during the defence.
- Only built-in packages are allowed. If not, you will get `0` grade.
- Add `AUTHORS.md` file which contains your login followed by a newline. 

```sh
$ cat -e ./AUTHORS.md
author1$
```

- The project MUST be compiled by the following command in the project's root directory:

```sh
$ go build -o creditcard .
```

### Mandatory Part

<!-- 
    Tip: write here what student should do

    Provide project description
    Provide examples
    Provide requirements    
-->


MANDATORY PART HERE

## Support

<!--
    Tip: leave this section unchanged.
    This is a static text, which student must read in every project.
-->

If you get stuck, try your code with the example inputs from the project. You should get the same results. If not, read the description again. Maybe you missed something, or your code is wrong. After the examples work, but your answer is still wrong, make some test cases you can check by hand. See if they work with your code. Use the complete example input.

If you are still stuck, ask a friend for help or take a break and come back later.


## Guidelines from Author

<!--
    Tip: this section is optional. 
    In case if you want to give some guidelines, write it here.
    If no guidelines provided whole section can be removed.
-->

GUIDELINES FROM AUTHOR HERE.

## Author

This project has been created by:

<!-- Tip: type here author's name, position and company -->
<!-- John Doe, DevOps at Google -->

AUTHOR NAME HERE.

Contacts:
<!-- 
    Tip: list of contacts to reach the author.
    It can be email, linkedin, telegram, instagram, etc.
-->
- Email: EMAIL HERE.
- [GitHub](https://github.com/LOGIN_HERE/)
- [LinkedIn](https://www.linkedin.com/in/LOGIN_HERE/)
