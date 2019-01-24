# Defining Program Syntax

## Syntax 📖 

*Syntax* - The part of the language definition that says how programs **look**: their **form and structure**.

## Semantics 🧠 

*Semantics* - The part of the language definition that says how programs **act**: their **behavior and meaning**.

## 🌳 English Example 🌳 

A sample grammar for English would look like this:

```
<A> ::= a | the
<N> ::= dog | cat | rat
<NP> ::= <A> <N>
<V> ::= loves | hates | eats
<S> ::= <NP> <VP> NP>
```

We can use these rules to generate **parse trees** 🌳: a set of nodes, starting at a *root node*, that follow the rules laid out by a particular grammar to generate a valid sentence.

## 🤖 Programming Example 🤖 

Programming languages (or *notations*) can also have grammars!

An example is:

```
<exp> ::= <exp> + <exp> | <exp> * <exp> | ( <exp> ) | a | b | c
```

This grammar specifies that an expression (`<exp>`) can be any of the following:

- Two other expressions being added together
- Two other expressions being multiplied together
- One expression enclosed in parentheses
- a
- b
- c

Which allows us to generate expressions such as:

```
a
a + b
a + b * c
((a + b) * c)
```

And any of these derivations can be used to create a parse tree 🌳 



## Parsing

The above processes are called *parsing*

Parsing: given grammatical string --> parse tree



## Grammars 📚 

A *grammar* is composed of four distinct parts:

1. tokens 🥇 
2. non-terminal symbols 🧱 
3. productions 🏠 
4. start symbol
   1. a type OF non-terminal symbol that tells the grammar where to begin the root node of the parse tree

So, really, three.

A **token** is the smallest meaningful unit of a grammar: the building blocks of a language. They can be `cat` or `a`, as in our examples, or keywords like `if` in a real programming notation.

A **production** is simply a rule, consisting of a left-hand side, the `::=` separator, and the right-hand side. The left-hand side is always a *single* non-terminal symbol and the right-hand side can be a list of tokens OR non-terminal symbols.

Grammars are constructed in **Backer-Naus Form (BNF)**, which is just a specific notation for expressing grammars.



## Lexical vs Phrase Structure

This isn't the whole picture though. Sadly, programs cannot be stored as nice, simple little sets of tokens, but rather are stored as text files, as characters, as bytes. This is the difference between **phrase structure** - specifying how to construct parse trees from a set of tokens - and **lexical structure** - specifiying how to construct tokens from program text.

- phrase structure - specifies how to construct parse trees from a set of tokens
- lexical structure - specifies how to construct tokens from program text

In practice, these two concepts are both articulated/expressed in two separate grammars for readability and clarity:

1. **token-level grammar** - defines a program as a sequence of tokens
   1. specifies the phrase structure
2. **character-level grammar** - defines a text file as a sequence tokens and/or white space
   1. specifies the lexical structure

And these grammars are carried out/enforced 🔫🔫🔫 by two *separate* components:

1. **scanner** - text file ➡️ sequence of tokens
2. **parser** - sequence of tokens ➡️ parse tree

So, all in all, a "program" goes from:

```
bytes 	---> 		tokens 	--> 		parse tree
		SCANNNER			PARSER
```

![See slides below](http://slideplayer.com/slide/13292271/79/images/6/Scanning+and+Parsing+Lexical+Structure%3A+The+structure+of+tokens+%28words%29+scanning+phase+%28lexical+analysis%29+%3A+scanner%2Flexer..jpg)

### Lexical Structure Format

We take it for granted now that the column position of characters in a text file has no bearing on the semantics/meaning of the program (i.e. it does not matter if the beginning of a `while` loop starts on column 4 or column 16). However, that was not always the case!

In the days of Fortran, there were **fixed-format languages**: specifications that changed the meaning of the character depending on column number.

- This was a result of the early punch card days 🏷 when you had a physical notion of "columns" and "rows" that is absent today.

Now, most languages - with the notable exception of Python 🐍 - are **free-format languages**: column number does not matter.

[Wiki Link](https://en.wikipedia.org/wiki/Free-form_language)

## Context ⛰ 🏝 🌊 

Introduced here is the vague idea of *context*, that is, the surrounding nodes of a given node in a parse tree and three categories are constructed:

1. Context-free - the children of a given node depend only on the node's non-terminal symbol
   1. Used for phrase structure usually
2. Regular
   1. Less expressive
   2. Used for lexical structure usu.
3. Context-sensitive
   1. More expressive

