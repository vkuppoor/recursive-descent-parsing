# recursive-descent-parsing
Recursive Descent Parsing is a straightforward, easy-to-implement method for constructing parsers for grammars. In OCaml, thanks to its functional nature and powerful pattern matching, implementing a Recursive Descent Parser can be quite intuitive. This tutorial will guide you through the process of building such a parser in OCaml. 

## Prerequisites
This tutorial assumes you have some background in Grammars and Functional Programming.

## Understanding Recursive Descent Parsing

Recursive Descent Parsing is a top-down parsing technique that constructs the parse tree from the root and then attempts to transform the tokenized input text into a parse tree (or abstract syntax tree) that captures relationships between these tokens. Each non-terminal in the grammar will be represented by a function in the parser. These functions will call each other recursively to process the input and build a parse tree.

## Setting Up OCaml Environment

Before we start, ensure you have an OCaml environment set up. You can install OCaml from [The OCaml website](https://ocaml.org/docs/install.html). Or feel free to use this [online playground](https://try.ocamlpro.com/).

## Defining the Grammar

A simple integer arithmetic grammar:

```
Expr -> Term ((PLUS | MINUS) Term)
Term -> Factor ((MUL | DIV) Factor)
Factor -> INT | LPAREN Expr RPAREN
```

Here, `Expr`, `Term`, and `Factor` are non-terminals, while `PLUS`, `MINUS`, `MUL`, `DIV`, `INT`, `LPAREN`, and `RPAREN` are terminals.

## Lexer

Before we write our parser, we need a lexer to tokenize the input and place the tokens in a list. For simplicity, we'll define a basic inductive type for our arithmetic expressions:

```ocaml
(* type to represent tokens *)
type token =
  | INT of int
  | PLUS
  | MINUS
  | MUL
  | DIV
  | LPAREN
  | RPAREN
  | EOF

(* Lexer implementation is ommitted. Assume our lexer outputs a valid list of tokens *)
```

## Writing the Parser

We'll create a function for each non-terminal in the grammar.

### Parsing `Factor`

```ocaml
let rec parse_factor tokens =
  match tokens with
  | INT n :: rest -> (* handle INT case *)
  | LPAREN :: rest -> (* handle LPAREN Expr RPAREN case *)
  | _ -> (* handle error *)
```

### Parsing `Term`

```ocaml
let rec parse_term tokens =
  let rec parse_term_tail left tokens =
    match tokens with
    | MUL :: rest -> (* handle MUL case *)
    | DIV :: rest -> (* handle DIV case *)
    | _ -> left, tokens
  in
  let left, tokens = parse_factor tokens in
  parse_term_tail left tokens
```

### Parsing `Expr`

```ocaml
let rec parse_expr tokens =
  let rec parse_expr_tail left tokens =
    match tokens with
    | PLUS :: rest -> (* handle PLUS case *)
    | MINUS :: rest -> (* handle MINUS case *)
    | _ -> left, tokens
  in
  let left, tokens = parse_term tokens in
  parse_expr_tail left tokens
```

### Handling the Input and Errors

For handling the input and potential errors, we can write a function that initiates the parsing process and deals with any unexpected tokens.

```ocaml
let parse input =
  let tokens = lexer input in
  match parse_expr tokens with
  | result, [] -> Some result
  | _ -> None  (* Error handling *)
```

## Testing the Parser

Now, you can test your parser with different input strings:

```ocaml
open Printf

let () =
  let result = parse "1 + 2 * 3" in
  Printf.eprintf result
```

## Conclusion

This tutorial introduced you to Recursive Descent Parsing in OCaml. This method's simplicity makes it an excellent choice for many parsing problems, especially when dealing with simpler languages or subsets of languages. With OCaml's pattern matching and functional style, Recursive Descent Parsers are more intuitive and concise. I use this technique often for constructing simple toy programming languages. Feel free to check out my attempt at creating an [LLVM compiler](https://github.com/vkuppoor/paddle) for Racket also written in OCaml.

Remember, this approach has its limitations, especially with left-recursive grammars and performance for complex grammars. In such cases, other parsing techniques or parser generators might be more suitable. But, then again, why would you ever create a grammar with left recursion?
