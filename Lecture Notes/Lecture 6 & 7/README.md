# Session 6 and 7

Created: Apr 1, 2021 1:56 AM
Reviewed: No

### Date: 26 March

### Recall

**KEYWORD**

- 

**RELEVANT QUESTION**

- What is a Type?
- 

### Notes

### Introduction to Type Systems

The appropriate formalism for type checking is logical rules of inference having the form:

if (hypothesis) is true then (conclusion) is true

From English to Inference Rule

 Symbols of (^) and (⇒) use in inference 

- if e1 has type int and e2 has type int, then e1 + e2 has type int
- (e1 : int ^ e2: int) ⇒ e1 + e2 : int
- This is a special case of hypo1 ^ hypo2 ^ ..... hypo(n) ⇒ conclusion

![Session%206%20and%207%20689cd39c17314a93b802e2317a8794bf/Untitled.png](Session%206%20and%207%20689cd39c17314a93b802e2317a8794bf/Untitled.png)

![Session%206%20and%207%20689cd39c17314a93b802e2317a8794bf/Untitled%201.png](Session%206%20and%207%20689cd39c17314a93b802e2317a8794bf/Untitled%201.png)

Note that hypotheses state facts about the sub-expressions and conclusion state facts about the entire expression.

What about variables without types?

An environment gives types for free variables:

- a var is free if an expr is not defined within the expression; otherwise its bound
- an environment is a function from variables to type:

    lambda x:t ⇒ (x+y)

![Session%206%20and%207%20689cd39c17314a93b802e2317a8794bf/Untitled%202.png](Session%206%20and%207%20689cd39c17314a93b802e2317a8794bf/Untitled%202.png)

Under the environment A, it is provable that x has type A(x)

**Rules for functions**

![Session%206%20and%207%20689cd39c17314a93b802e2317a8794bf/Untitled%203.png](Session%206%20and%207%20689cd39c17314a93b802e2317a8794bf/Untitled%203.png)

Under the environment A, if it is proven that e has type t', then the function lambda x of type t yields e of type function t → t' 

 We have a problem though? x is a free variable. we need its type.

**The fix** 

![Session%206%20and%207%20689cd39c17314a93b802e2317a8794bf/Untitled%204.png](Session%206%20and%207%20689cd39c17314a93b802e2317a8794bf/Untitled%204.png)

**Rule for call**

![Session%206%20and%207%20689cd39c17314a93b802e2317a8794bf/Untitled%205.png](Session%206%20and%207%20689cd39c17314a93b802e2317a8794bf/Untitled%205.png)

![Session%206%20and%207%20689cd39c17314a93b802e2317a8794bf/Untitled%206.png](Session%206%20and%207%20689cd39c17314a93b802e2317a8794bf/Untitled%206.png)

![Session%206%20and%207%20689cd39c17314a93b802e2317a8794bf/Untitled%207.png](Session%206%20and%207%20689cd39c17314a93b802e2317a8794bf/Untitled%207.png)

![Session%206%20and%207%20689cd39c17314a93b802e2317a8794bf/Untitled%208.png](Session%206%20and%207%20689cd39c17314a93b802e2317a8794bf/Untitled%208.png)

Soundness of the above system:

- e0 is guaranteed to be a bool
- e1 and e2 are guaranteed to be of the same type

These type of guarantees are characteristics of sound type system.

A type system is sound:

- Whenever expr e has type t *and*
- then e evaluates to a value v in t

Soundness is extremely useful

- Program type-checks ⇒ no errors at run-time
- Verify absence of a class of errors

These offer a very strong guarantee:

- verified property holds in all executions
- "well-typed programs cant go wrong"

![Session%206%20and%207%20689cd39c17314a93b802e2317a8794bf/Untitled%209.png](Session%206%20and%207%20689cd39c17314a93b802e2317a8794bf/Untitled%209.png)

As shown by the example above without the side constraints we have an unsound program. when the if condition is satisfied the return kind is int → int otherwise its int. 

## Algorithms for type checking

### 1. Global Checking

![Session%206%20and%207%20689cd39c17314a93b802e2317a8794bf/Untitled%2010.png](Session%206%20and%207%20689cd39c17314a93b802e2317a8794bf/Untitled%2010.png)

- Step 1 requires the overall environment A (global analysis needs to know the env A before proceeding)
    - only then we can analyze the subexpressions
- This is global program
    - requires the entire program
    - or constructing a model of the environment

### 2. Local Checking

![Session%206%20and%207%20689cd39c17314a93b802e2317a8794bf/Untitled%2011.png](Session%206%20and%207%20689cd39c17314a93b802e2317a8794bf/Untitled%2011.png)

- First analyze subexpressions and infer needed environments
- Since the separately computed environments might not agree, constraint them to be equal to get a valid analysis for the entire expression

### Global vs Local Analysis

Global

- Usually technically simpler than local
- may need extra work to model environments for unfinished programs

Local

- more flexible in application
- technically harder: may need to allow unknown parameters, more side conditions

---

# Run-time Environments

Run-time resources:

- Execution of a program is initially under the control of the OS
- When a program is invoked
    - The OS allocates space for the program
    - The code is loaded into part of the space
    - OS jumps to the entry point (i.e main)

![Session%206%20and%207%20689cd39c17314a93b802e2317a8794bf/Untitled%2012.png](Session%206%20and%207%20689cd39c17314a93b802e2317a8794bf/Untitled%2012.png)

What is Other Space?

- Hold all the data for the program
- Other Space = Data Space
- Compiler is responsible for
    - Generating code
    - Orchestrating use of data area

**Code Generation Goals**

- Correctness
- Speed

**Activations**

- An invocation of a procedure P is an activation of P
- The lifetime of an activation of P is
    - all the steps to execute P
    - including all the steps in procedures P calls

**Lifetimes of variables**

Lifetime of variable x is the portion of execution in which x is defined (lifetime is dynamic concept)

The activation tree depends on the run-time behavior.

It may be different for every program input.

Since activations are properly nested, a stack can track currently active procedures.

![Session%206%20and%207%20689cd39c17314a93b802e2317a8794bf/Untitled%2013.png](Session%206%20and%207%20689cd39c17314a93b802e2317a8794bf/Untitled%2013.png)

This introduction of stack revises our memory into:

![Session%206%20and%207%20689cd39c17314a93b802e2317a8794bf/Untitled%2014.png](Session%206%20and%207%20689cd39c17314a93b802e2317a8794bf/Untitled%2014.png)

**Activation Records**

The information needed to manage one procedure activation is called activation record or frame.

If procedure F calls G, then G's activation record contains a mix of information about F and G.

So what is G's Activation Record when F calls G?

- F is "suspended" until G completes, at which point F resumes. G's AR contains information needed to resume execution of F.
- G's AR may also contain
    - G's return value (needed by F)
    - Parameters to G
    - Space for G's local variables

    ![Session%206%20and%207%20689cd39c17314a93b802e2317a8794bf/Untitled%2015.png](Session%206%20and%207%20689cd39c17314a93b802e2317a8794bf/Untitled%2015.png)

Illustration of stack after two calls to f

![Session%206%20and%207%20689cd39c17314a93b802e2317a8794bf/Untitled%2016.png](Session%206%20and%207%20689cd39c17314a93b802e2317a8794bf/Untitled%2016.png)

(**) and (***) are return addresses of the invocations of f 

The main point is:

The compiler must determine at compile time the layout of activation records and generate code that correctly accesses locations in the activation record.

The **advantage** of placing the return value 1st in a activation record is that the caller can find it at a fixed offset from its own activation record. 

There is though nothing magical about this organisation.

- can rearrange order of frame elements
- can divide caller/callee responsibilities differently
- An org is better if it improves execution speed or simplifies code generation.

**Globals**

All references to a global variable point to the same object.

- cannot store a global in activation record.

Globals are assigned a fixed address once

- Variables with fixed addresses are "statically allocated"

Depending on the language, there may be other statically allocated vars.

![Session%206%20and%207%20689cd39c17314a93b802e2317a8794bf/Untitled%2017.png](Session%206%20and%207%20689cd39c17314a93b802e2317a8794bf/Untitled%2017.png)

**Heap Storage**

A value that outlives the procedure that creates it cannot be kept in the AR.

- Bar foo() { return new Bar}
- The Bar value must survive deallocation of foo's AR

Languages with dynamically allocated data use a heap to store dynamic data.

**Note that**

- The code area contains object code
    - for many languages, fixed size & read only
- The static area contains data (not code) with fixed addresses (eg global data)
    - fixed size, may be readable/writable
- The stack contains an AR for each currently active procedure
    - each AR usually fixed size, contains locals
- Heap contains all other data

But the problem is

- both the heap and stack grow
- must make sure they dont grow into each other

Solution

- Start heap and stack at opposite ends of memory and let them grow into each other

![Session%206%20and%207%20689cd39c17314a93b802e2317a8794bf/Untitled%2018.png](Session%206%20and%207%20689cd39c17314a93b802e2317a8794bf/Untitled%2018.png)

Have to keep in mind this memory layout when the discussion comes on code generation.

---

# Stack Machines

- A simple evaluation model
- No variables or registers
- A stack of values for intermediate results
- Each instruction
    - Take its operands from top of the stack
    - remove those operands from the stack
    - computes the required operation on them
    - pushes the result on the stack

Why use a stack machine?

- Each operation takes operands from the same place and puts results in the same place (Top of the stack)
- This means a uniform compilation scheme and therefore a simpler compiler
- Location of the operands in implicit
    - Always on top of stack
- No need to specify operands explictly
- No need to specify the location of the result
- Instruction "add" as opposed to "add r1, r2, r3"
    - Smaller encoding of instructions
    - More compact programs
- This is one reason why Java Bytecode use a stack evaluation model

**Optimizing the stack machine**

 The add op does 3 memory operations

- two reads and one write to the stack
- the top of the stack is frequently accessed

Idea: keep the top of the stack in a register (called accumulator) 

- register accesses are faster

The add instruction is now

- acc ← acc + top_of_stack

Invariants of stack machine

- the result in an expr is in the accumulator
- for op(e1,..., en) push the accumulator on the stack after computing e1, ....., en-1 (after the operation pops n-1 values)
- Expression evaluation preserves the stack (very important invariant)

![Session%206%20and%207%20689cd39c17314a93b802e2317a8794bf/Untitled%2019.png](Session%206%20and%207%20689cd39c17314a93b802e2317a8794bf/Untitled%2019.png)

![Session%206%20and%207%20689cd39c17314a93b802e2317a8794bf/Untitled%2020.png](Session%206%20and%207%20689cd39c17314a93b802e2317a8794bf/Untitled%2020.png)

Notes

It is v important evaluation of a subexpression preserves the stack

- Stack before the evaluation of 7+5 is 3, <init>
- Stack after the evaluation of 7+5 is 3, <init>
- The first operand is on top of the stack

## From stack machines to MIPS

- The compiler generates code for a stack machine with accumulator
- We want to run the resulting code on a real machine
    - eg the MIPS processor
- We simulate stack machine instructions using MIPS instructions and registers

### Simulating a stack machine

- The acc is kept in MIPS register $a0
- The stack is kept in memory
    - The stack grows towards lower addresses
    - Standard convention on the MIPS architecture
- The address of the next location on the stack is kept in MIPS register $sp
    - The top of the stack is at address $sp + 4 (32 bit machine)

**MIPS Assembly**

- MIPS architecture
    - Prototypical Reduced Instruction Set Computer (RISC) architecture
    - Arithmetic operations use registers for operands and results
    - Must use load and store instructions to use operands and results in memory
    - 32 general purpose registers (32 bits each): we will use $sp, $a0, and $t1 (temp register)

**A sample of MIPS instructions**

- lw reg1 offset(reg2)
    - Load 32 bit word from address reg2+offset into reg1
- add reg1 reg2 reg3
    - reg1 ← reg2 + reg3
- sw reg1 offset(reg 2)
    - store 32-bit word in reg1 at address reg2+offset
- addiu reg1 reg2 imm
    - reg1 ← reg2 + imm
    - "u" means overflow is not checked
- li reg imm (load immediate)
    - reg ← imm

![Session%206%20and%207%20689cd39c17314a93b802e2317a8794bf/Untitled%2021.png](Session%206%20and%207%20689cd39c17314a93b802e2317a8794bf/Untitled%2021.png)

Note: stack pointer starts and ends at the same location.

---

**Summary**:

- Computing types of programs using inference rules
- Properties of type checking
    - Global vs Local
    - Soundness
- Introduction to code generation: stack machine and MIPS architecture