# Compiler Construction (Session 1)

### Date: 22 Jan 2021

### Recall

**KEYWORD**

Compilers

Interpreters

**RELEVANT QUESTION**

Why do we need IR?

Is lexer as easy as it sounds?

FORTRAN rule: whitespace is ignored

- VAR I same as VA RI

### Notes

- What is a compiler?

    Ans: A compiler is a program that can read a program in one language (source lang) and translate it into an equivalent language (target lang)

- An important role of the compiler is to report and errors in the source program that it detects during the translation process.
- Interpreter is another common kind of language processor.

![lecture01/Untitled.png](lecture01/Untitled.png)

- Java Bytecode acts as an interpreter (intermediate program). Virtual machines takes intermediate program and input and outputs (like Python).
- The structure of a compiler:
    - **Two pass Compiler**
        - Front end maps legal code into IR (recognize legal procedure and produce errors)
        - Back end maps IR onto target machine (choose instructions for each IR operation decide what to keep in registers at each point ensure conformance with system interfaces)
    - **Three pass compiler**
        - Middle end (code optimization) - analyzes and changes IR, goal is to reduce runtime. Can produce many IRs (from one IR to another)

        ![lecture01/Untitled%201.png](lecture01/Untitled%201.png)

- Phases of a compiler
    - Lexical: breaks the code into tokens. Keeps the variables into symbol table.
    - Syntax Analyzer: Takes the tokens and makes sense of it. Parses the tokens and makes a tree. An AST.
    - Semantic Analyzer: tries to understood the meaning of the statement. Compilers perform limited semantic analysis to catch inconsistencies. Programming languages define strict rules to avoid such ambiguities. (Type checking)
    - Intermediate Code Generator: generates IR

        Why do we need IR? 

        - IRs are generally ordered in descending level of abstraction (highest is source lowest is assembly)
        - they are useful because lower levels expose features hidden by higher levels i.e registers, memory layouts *but* lower levels obscure high-level meaning i.e classes, loops.
    - Code optimizer:
    - Code generation

![lecture01/Untitled%202.png](lecture01/Untitled%202.png)

- Now optimizations dominates all other phases, lexing and parsing are well understood, earlier on these were most complex. We need to focus on optimizations.

### **Tokens, Patterns and Lexemes**

- Common tokens in programming languages:
    - One token for each keyword. The pattern for a keyword is the same as the keyword itself.
    - Tokens for the operators, either individually or in classes such as the token "comparison"
    - One token representing all identifiers.
    - 

    ![lecture01/Untitled%203.png](lecture01/Untitled%203.png)

    What are tokens for?

    - Classify program substrings according to role
    - Lexical analysis produces a stream of tokens (which is input to parser)

    ### Designing a lexical analyzer

    Step 1

    - Define a finite set of tokens (identifiers, integers, keywords)
    - Choice of tokens depends on: language, design of parser

    Step 2

    - Describe which strings belong to each token

    An implementation must do two things:

    1. Classify each substring as a token
    2. Return the lexeme of this token 

    So lexer returns a combinations of (lexers, tokens)

    Coming to the question of is lexer as easy as it sounds as we saw in the fortran example.

    Two important points:

    - The goal is to partition the string. This is implemented by reading left to right recognizing one token at a time.
    - "Lookahead" may be required to decide where one token ends and the next token begins. ( ***=*** vs ***==*** )

## Summary

**SUMMARY: Goal of lexical analysis is partition input into lexemes, identify token of each lexeme.  Left-to-right scan ⇒ lookahead needed sometimes.**