# Javascript

Javascript was invented in 1995 by Brandon Eich, in order to automate HTML presentations in Netscape Navigator. Since then, Javascript interpreters have been built into virtually every internet browser. The language has been standardized by a committee, and receives enhancements and improvements to this day.

One of the interesting aspects of the Javascript programming language is a data-structure known of colloquially as a **dictionary**. We'll look closely at the dictionary, and then use it to investigate a topic in Mathematics, called a **permutation group**.

## Getting Started

### Opening The Command Line

A Javascript **command line** is a text interface for giving commands to a Javascript interpreter. The interpreter reads the command, evaluates the order, prints its results, and then readies itself for a new line. You can use the command line to quickly test small bits of code for correctness.

To open the Javascript command line in Chrome, go to **View -> Developer -> Javascript Console**. You will be presented with a blank page and a blinking cursor. The page will hold your old commands as you type them.

### Warming Up

Let's try a few commands. Try entering the number `17` on the command line, and then hit enter. The result should look like.

```
> 17
< 17
```

The right caret means that `17` went in and the left caret below means that `17` came out. Let's enter a decimal, like `10.23`

```
> 10.23
< 10.23
```

We see that Javascript reflects numbers back at us.

Let's type a word...

```
> zardoz
< Uncaught ReferenceError: zardoz is not defined
```

Javascript has given us an error. The error has to do with references and defining the word zardoz.

### Definitions

To explain better the error we have received, I will explain how to define a reference. In this case, the reference we want to define is the word zardoz. It does not matter what we define it to be, so I will use the number 77. To define zardoz as a reference to the number 77, I type

```
> zardoz = 77
< 77
```

Now, if I type `zardoz`, I get:

```
> zardoz
< 77
```

and there is no error! We see that words can be defined, and that our error meant we had not defined the word we intended to use.


## Dictionaries

So let's say I define a bunch of words to be a bunch of numbers. In the end, I produce my own language for referring to numbers. If I want to transmit this language to another person, I have to show them my definitions.

This object, a collection of definitions, is the dictionary, and in Javascript it looks like:

```
{ zardoz: 77, foobar: 1.13, foobarbaz: 3 }
```

Sometimes we break after the commas, like so:

```
{ zardoz: 77,
  foobar: 1.13,
  foobarbaz: 3 }
```

We can even define words to be references to dictionaries:

```
english_decimal = {
  zero: 0,
  one: 1,
  two: 2,
  three: 3,
  four: 4,
  five: 5,
  six: 6,
  seven: 7,
  eight: 8,
  nine: 9 }
```

Now we have a dictionary of the ten digits in the English decimal system.

### Looking up a definition.

Let's say we have another dictionary:

```
portuguese_decimal = {
  zero: 0,
  um: 1,
  dois: 2,
  três: 3,
  quatro: 4,
  cinco: 5,
  seis: 6,
  sete: 7,
  oito: 8,
  nove: 9 }
```

Suppose I am asked to pick "três" onions from the store. I can lookup the definition of "três" like so:

```
> portuguese_decimal["três"]
< 3
```

I wrote the name of the dictionary, followed by an `[`, an `"`, the name of the term I'm looking for, and then closing with an `"` and a `]`. The `[` and `]` are called **square brackets**.

The quotation marks are important here. A word not encapsulated in quotes is expected to reference, or **signify** some other, underlying value (like the number 77 was signified by the word `zardoz`). However, when we refer to the word, "zardoz", we do not mean 77. Instead, we mean the *literal word*, "zardoz". Quotes are a way of referring to the **signifier** itself.

### Dictionaries of Numbers

Say we want to have the reverse of `portuguese_decimal`, that is, a dictionary that lets us look up the Portuguese word for a particular digit. Then we should write something like

```
decimals_in_portuguese = {
  0: "zero",
  1: "um",
  2: "dois",
  3: "três",
  4: "quatro",
  5: "cinco",
  6: "seis",
  7: "sete",
  8: "oito",
  9: "nove" }
```

Now, when we need the Portuguese word for 8, we type:

```
< decimals_in_portuguese[8]
> "oito"
```
The above can be read: "Look up the number 8 in the dictionary of decimals in portuguese." The response is of course, `"oito"`.


The `decimals_in_portuguese` dictionary is special. Because its terms are the integers, starting from zero, and running consecutively, we can save some keystrokes by using a slightly different structure:

```
decimals_in_portuguese_array = [
  "zero",
  "um",
  "dois",
  "três",
  "quatro",
  "cinco",
  "seis",
  "sete",
  "oito",
  "nove" ]
```

This is an **array**, made with square brackets instead of curly brackets, and omitting the digits from the left-hand side of the definition. The result is less typing, and actually, less overall memory usage!

The array behaves exactly like its dictionary counterpart; Typing `decimals_in_portuguese_array[8]` also yields `"oito"`. We can assert this in the command line by typing

```
< decimals_in_portuguese[8] == decimals_in_portuguese_array[8]
> true
```


Now we can use these tools to explore a foundational topic in Mathematics, i.e. Group Theory.


## Symmetries of the Square.





