# Ruby under a microscope

## Tokenization and Parsing
* Ruby transforms the code 3 times before running it
* `Code -> Tokenize -> Parse -> Compile -> YARV instructions`
* Parsing rips the stream of ruby code and converts into tokens and puts back tokens together in order
* gem ripper
* ripper.lex
* Lex tokenizing
* Ruby tokenizer was entirely written by hand
* Tokenizer does not check if the syntax is valid

* Parser
* `Parser generator` takes in a set of grammar rules and produces parser
* Ruby uses Yacc and Bison
* LALR - Look ahead Left Reversed Rightmost parsing algorithm
* ruby -y simple.rb - displays debug info everytime parser jumps tokens
* Parser builds an AST (abstract syntax tree)
* ripper.sexp
* Tree of tokens representing the valid syntax and how the ruby will execute it

