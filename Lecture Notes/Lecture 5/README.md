# Session 5

Created: Mar 4, 2021 6:58 PM
Reviewed: No

### Date: 5 Feb 2021

### Recall

**KEYWORD**

- 

**RELEVANT QUESTION**

### Notes

In the last lecture, the notion of **handles** was formalized.

A handle is a string that can be reduced and also allows further restrictions back to the start symbol. (using particular production at a specific spot). We only want to reduce at handles.

## Shift-reduce parsing

Its a scheme to implement handle pruning. 

Shift-reduce parser uses a stack and an input buffer

1. Initialize stack with $
2. Repeat until the top of stack is the start symbol and the input token is $

    (a) find the handle. If we don't have a handle on top of stack, shift an input symbol onto the stack.

    (b) prune the handle. If we have a handle A→B on the stack reduce

    (i) pop B symbols off the stack 

    (ii) push A onto the stack

![Session%205%2016c759aa9782470f8895b679c5a9aea8/Untitled.png](Session%205%2016c759aa9782470f8895b679c5a9aea8/Untitled.png)

Important: In shift-reduce parsing, handles appear only at the top of the stack, never inside.

Why?

- Informal reduction
    - True initially, stack is empty
    - Immediately after reducing a handle (Rightmost non-terminal on top of stack)

        Next handle must be to the right of rightmost nonterminal, because this is a rightmost derivation

        Sequence of shift moves reaches next handle

**Conflict in shift-reduce**

Generically if there is a handle on top of the stack, **reduce** otherwise **shift**. 

**But what if there is a choice?**

- If it is legal to shift or reduce, there is a **shift-reduce conflict**.
- If it is legal to reduce by two different productions, there is a **reduce-reduce conflict**.

Ambiguous grammars always cause conflicts.

![Session%205%2016c759aa9782470f8895b679c5a9aea8/Untitled%201.png](Session%205%2016c759aa9782470f8895b679c5a9aea8/Untitled%201.png)

 In the step, where stack is **$E*E** and input is *+int,* we can shift or reduce by **E → E* E**

(Grammar can be rewritten to enforce precedence)

## **LR Parsing**

- LR(k) parsing
    - L: Scan input left to right
    - R: produce rightmost derivation
    - k: tokens of lookahead.
- LR(0)
    - Zero tokens of lookahead
- SLR
    - Simple LR: Like LR(0) but uses FOLLOW sets to build more "precise" parsing tables.

Similar to LL Parsing we have table driven implementation.

- Grammatical knowledge is encoded in two tables: ACTION and GOTO
    - They encode the handle-finding automation

Why two tables?

- Reduce needs more information than shift
- GOTO holds the extra information

![Session%205%2016c759aa9782470f8895b679c5a9aea8/Untitled%202.png](Session%205%2016c759aa9782470f8895b679c5a9aea8/Untitled%202.png)

 

![Session%205%2016c759aa9782470f8895b679c5a9aea8/Untitled%203.png](Session%205%2016c759aa9782470f8895b679c5a9aea8/Untitled%203.png)

Key:

R is reduce 

S is shift 

![Session%205%2016c759aa9782470f8895b679c5a9aea8/Untitled%204.png](Session%205%2016c759aa9782470f8895b679c5a9aea8/Untitled%204.png)

Using this parsing algorithm and the table from above, we run it on the input **int * int + int $**

![Session%205%2016c759aa9782470f8895b679c5a9aea8/Untitled%205.png](Session%205%2016c759aa9782470f8895b679c5a9aea8/Untitled%205.png)

### **LR(0) states**

- So what are the states in the parsing table?
- LR(0) States
    - Describe states in which the parser can be
    - As LR parser makes shift-reduce decision by maintaining states to keep track of where we are in a parser
- LR(0) state = set of LR(0) items

**LR(0) item**

An item is a production with a "." somewhere on the right side, denoting a focus point.

- An LR(0) item [ X → α.β ] says that
    - The parser is looking for an X
    - It has an  α on top of the stack
    - Expects to find input string derived from β
- The items for T→ (E) are
    - T → .(E)
    - T → (.E)
    - T → (E.)
    - T → (E).
- [ **X → α.aβ** ] means that a is on the input, it can be shifted (resulting **αa.β** ) That is
    - **a** is a correct token to see on the input
    - Shifting **a** would not "over-shift"
- [ **X → α. ]** means that we could reduce **α to X**
- The only item for **X → eps** is **X → .**

## Constructing SLR states

- LR(0) automaton
    - encodes all strings that are valid on the stack
    - Each valid string corresponds to a state of the LR(0) automaton
    - Each state tells us what to do (shift or reduce?)
- What are the valid handles that can appear at the front of the input?
- Begin with item S' → .Start, calculate related items (closure)
- Determine following states (what states can be reached on a single terminal or nonterminal)
- Construct closure of each resulting state

**Closure of a set of items**

![Session%205%2016c759aa9782470f8895b679c5a9aea8/Untitled%206.png](Session%205%2016c759aa9782470f8895b679c5a9aea8/Untitled%206.png)

![Session%205%2016c759aa9782470f8895b679c5a9aea8/Untitled%207.png](Session%205%2016c759aa9782470f8895b679c5a9aea8/Untitled%207.png)

- Closure results in a new state

![Session%205%2016c759aa9782470f8895b679c5a9aea8/Untitled%208.png](Session%205%2016c759aa9782470f8895b679c5a9aea8/Untitled%208.png)

## New states - the GOTO function

- To determine possible states reachable from existing state, use GOTO function
- GOTO(state, stack element) is the closure of the set of items that result from shifting stack element in state

![Session%205%2016c759aa9782470f8895b679c5a9aea8/Untitled%209.png](Session%205%2016c759aa9782470f8895b679c5a9aea8/Untitled%209.png)

![Session%205%2016c759aa9782470f8895b679c5a9aea8/Untitled%2010.png](Session%205%2016c759aa9782470f8895b679c5a9aea8/Untitled%2010.png)

![Session%205%2016c759aa9782470f8895b679c5a9aea8/Untitled%2011.png](Session%205%2016c759aa9782470f8895b679c5a9aea8/Untitled%2011.png)

### **Constructing an SLR parsing table**

- Add an extra production S' → Start to the grammar
- Construct set of LR(0) states, the initial state is the one containing **S' → .Start**

![Session%205%2016c759aa9782470f8895b679c5a9aea8/Untitled%2012.png](Session%205%2016c759aa9782470f8895b679c5a9aea8/Untitled%2012.png)

![Session%205%2016c759aa9782470f8895b679c5a9aea8/Untitled%2013.png](Session%205%2016c759aa9782470f8895b679c5a9aea8/Untitled%2013.png)

**SLR Parsing table**

- Empty table items are errors
- Grammar is not SLR if more than one entry for any table item

**Limits of SLR parsing** 

- SLR parsing uses the FOLLOW set to decide reductions
- LR(1) is more powerful
    - Build lookahead into the items
    - An LL(1) item is pair (LR(0) item x lookahead)
    - **[ T → int*T.,$ ]** means after seeing **T → int*T** reduce if lookahead is **$**
    - More accurate than just using follow set

---

# Syntax error handling

Syntax error handler should

- report errors accurately and clearly
- recover from error quickly
- Not slow down compilation of valid code
- Good error handling is not easy

**Approaches to syntax error recovery**

- For simple to complex
    - Panic mode
    - Error productions
    - Automatic local or global correction

## Panic mode

Simples, most popular method

- When an error is detected
    - Discard tokens until one with a clear role is found
    - Continue from there
- Such tokens are called synchronizing tokens

## Error Productions

- Idea: specify in the grammar known common mistakes
- Essentially promotes common errors to alternative syntax
- **Example**
    - Write 5x instead of 5*x
    - Add the production E → ... | EE
    - Detects the error when the error production is used during parsing
- Disadvantage: complicates the error

## Local and global correction

- Idea: find a correct "nearby" program
    - Try token insertions and deletions
    - Exhaustive search
- Disadvatages:
    - Hard to implement
    - Nearby is not necessarily the "intended" program

Today: Panic mode seems enough 

## Abstract Syntax Trees

- AST is a better structure for later compilation phases
- Like parse trees but ignore some details
- AST is condensed form of a parse tree
    - Operators appear at internal nodes, not at leaves
    - "Chains" of single productions are collapsed
    - Lists are "flattened"
    - Syntactic details are omitted

**Comparison with Parse tree** 

![Session%205%2016c759aa9782470f8895b679c5a9aea8/Untitled%2014.png](Session%205%2016c759aa9782470f8895b679c5a9aea8/Untitled%2014.png)

![Session%205%2016c759aa9782470f8895b679c5a9aea8/Untitled%2015.png](Session%205%2016c759aa9782470f8895b679c5a9aea8/Untitled%2015.png)

## Semantic actions

- This is what we will use to construct ASTs
    - Semantic actions can also be used for type checking, code generation
- Syntax-direction translation is done by extending the context-free grammar
- Each grammar symbol may have attributes
    - For terminal symbols, attributes can be calculated by the lexer
- Each production may have an action
    - Written as X → Y1...Yn { action }
    - That can refer to or compute symbol attributes

![Session%205%2016c759aa9782470f8895b679c5a9aea8/Untitled%2016.png](Session%205%2016c759aa9782470f8895b679c5a9aea8/Untitled%2016.png)

![Session%205%2016c759aa9782470f8895b679c5a9aea8/Untitled%2017.png](Session%205%2016c759aa9782470f8895b679c5a9aea8/Untitled%2017.png)

## Constructing an AST

![Session%205%2016c759aa9782470f8895b679c5a9aea8/Untitled%2018.png](Session%205%2016c759aa9782470f8895b679c5a9aea8/Untitled%2018.png)

 

![Session%205%2016c759aa9782470f8895b679c5a9aea8/Untitled%2019.png](Session%205%2016c759aa9782470f8895b679c5a9aea8/Untitled%2019.png)

![Session%205%2016c759aa9782470f8895b679c5a9aea8/Untitled%2020.png](Session%205%2016c759aa9782470f8895b679c5a9aea8/Untitled%2020.png)

---

# Semantic Analysis

- Last front end phase of compilation
- Catches all remaining errors

## Classes of errors

- Lexical
- Syntactic
- Static semantic - can be detected in parser or in separate semantic analysis passes
    - Input can be parsed by CFG but some non context-free error
    - Examples: Multiply declared variables, undeclared variables, type mismatch
- Semantic - may be detected at compile time, may also add checks in code for runtime
    - Examples: division by 0, null pointer
    - can include code to check for conditions and not allow them to occur
    - Advantage: error message instead of error behavior
    - Disadvantage: object code is longer and slower
- Logical - hardest to detect

## Static semantic Analyzer

- Works in two phases
    - traverses the AST created by the parser
    - 1. for each scope in the program
        - Process the declarations = add new entries to the symbol table and report and variables that are multiply declared
        - Process the statements = find uses of undeclared variables, and update the "ID" nodes of the AST to point the appropriate symbol table entry
    - 2. Process all the statements in program again
        - Use the symbol table information to determine the type of each expression, and find type errors.

### Symbol table = set of entries

Purpose:

- Keep track of names declared in the program
- Names of variables, classes, fields, methods

Symbol table entry

- Associate a name with a set of attributes
    - Kind of name (variable, class, field)
    - Type (int, float, etc)
    - Nesting level
    - Memory location

### Scoping

- Symbol table design influenced by what kind of scoping is used by the compiled language
- In most languages, the same name can be declared multiple times
    - If its declarations occur in different scopes, and or involve different kinds of names

![Session%205%2016c759aa9782470f8895b679c5a9aea8/Untitled%2021.png](Session%205%2016c759aa9782470f8895b679c5a9aea8/Untitled%2021.png)

**The scope rule of a language**

- determine which declaration of a named object corresponds to use of the object
- scoping rules map uses of objects to their declarations

C++ and Java use static scoping

- Mapping from uses to declarations is made at compile time
- C++ uses the "most closely nested" rule

![Session%205%2016c759aa9782470f8895b679c5a9aea8/Untitled%2022.png](Session%205%2016c759aa9782470f8895b679c5a9aea8/Untitled%2022.png)

### Dynamic Scoping

Not all languages use static scoping

- A use of a variable that has no corresponding declaration in the same function corresponds to the declaration in the most-recently-called still active function (Lisp, SNOBOL)

Static vs dynamic soping

- Dynamic scoping can make a program difficult to understand
    - A single use of a variable can correspond to many different declarations with different types!
- Can a name be used before they are defined?
    - Java: a method or field name can be used before the definition
    - Not variables

    ## Symbol table implementations

    - Symbol table will be used to answer two questions
    1. Given a declaration of a name, is there already a declaration of the same name in the current scope (is it multiply declared?)
    2. Given a use of a name, to which declaration does it correspond or is it undeclared?

    We make some assumptions

    - use static scoping
    - require all names be declared before use
    - do not allow multiple declarations in same scope
    - use the same scope for a method's parameters and for local variables declared at the beginning of the method

    ### What operations do we need?

    Given the assumptions:

    1. Look up a name in the current scope only to check if it is multiply declared
    2. Look up a name in the current and enclosing scopes to check for a use of an undeclared name and to link a use with the corresponding symbol table entry
    3. Insert a new name into symbol table with its attributes
    4. Do what must be done when a new scope is entered 
    5. Do what must be done when a scope is exited 

    ![Session%205%2016c759aa9782470f8895b679c5a9aea8/Untitled%2023.png](Session%205%2016c759aa9782470f8895b679c5a9aea8/Untitled%2023.png)

**Method 1: List of hash tables**

The idea is

- Symbol table = a list of hash tables
- One hash table for each currently visible scope

![Session%205%2016c759aa9782470f8895b679c5a9aea8/Untitled%2024.png](Session%205%2016c759aa9782470f8895b679c5a9aea8/Untitled%2024.png)

![Session%205%2016c759aa9782470f8895b679c5a9aea8/Untitled%2025.png](Session%205%2016c759aa9782470f8895b679c5a9aea8/Untitled%2025.png)

---

Summary:

- In shift-reduce parsing, handles always appear at the top of the stack
- Handles are never to the left of the rightmost nonterminal
    - Therefore, shift-reduce moves are sufficient
- Bottom-up parsing algorithms are based on recognizing handles
    - Use heuristics to guess which stacks are handles
    - On some CFGs, the heuristics always guess correctly