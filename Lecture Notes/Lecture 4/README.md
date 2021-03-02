# Session 4

Created: Feb 26, 2021 12:03 AM
Reviewed: No

### Date: 5 Feb 2021

### Recall

**KEYWORD**

- Context-Free Grammars
- Recursive Descent Parsers

**RELEVANT QUESTION**

What makes a grammar "context free"?

### Notes

**Eliminating left recursion**

Example:

![Session%204%206d68f59568fc4b0680cfd902955b7f50/Untitled.png](Session%204%206d68f59568fc4b0680cfd902955b7f50/Untitled.png)

In general:

![Session%204%206d68f59568fc4b0680cfd902955b7f50/Untitled%201.png](Session%204%206d68f59568fc4b0680cfd902955b7f50/Untitled%201.png)

Algorithm for eliminating left recursion: (copied from the Dragon textbook)

![Session%204%206d68f59568fc4b0680cfd902955b7f50/Untitled%202.png](Session%204%206d68f59568fc4b0680cfd902955b7f50/Untitled%202.png)

---

**Predictive Parsing** (Another kind of top down parser)

- recursive descent might need to backtrack. The alternative is to look ahead in the input and pick the correct production
- But the question arises how much lookahead is needed for CFG?
- Fortunately, large subclasses of cfgs can be parsed with limited lookahead. (including most programming lang)
- Predictive parsing accept LL(k) grammars
    - L means left to right scan of input
    - Second L means leftmost derivation
    - k means predict based on k tokens of lookahead
    - In practice LL(1) is used
- In LL(1) at each step there is only one choice of production as compared to recursive descent. There is a unique production to use based on one lookahead or no production to use in which its an error state.

**LL(1) Properties**

- Function First(a) → a is the string of grammar symbols, to be the set of terminals that begin strings derived from a.

![Session%204%206d68f59568fc4b0680cfd902955b7f50/Untitled%203.png](Session%204%206d68f59568fc4b0680cfd902955b7f50/Untitled%203.png)

- Function Follow(A), for nonterminal A, to be the set of terminals a such that there exists a derivation of the form

    ![Session%204%206d68f59568fc4b0680cfd902955b7f50/Untitled%204.png](Session%204%206d68f59568fc4b0680cfd902955b7f50/Untitled%204.png)

    - A non terminal's Follow set specifies the tokens that can legally appear after it.

    ![Session%204%206d68f59568fc4b0680cfd902955b7f50/Untitled%205.png](Session%204%206d68f59568fc4b0680cfd902955b7f50/Untitled%205.png)

- If the grammar is not LL(1), can we transform it into LL(1)?
    - In some cases, YES
    - If we pull the common prefix into a separate production, we may make the grammar LL(1) : **left factoring**

![Session%204%206d68f59568fc4b0680cfd902955b7f50/Untitled%206.png](Session%204%206d68f59568fc4b0680cfd902955b7f50/Untitled%206.png)

**Skeleton parser of LL(1) Parser**

- Method similar to recursive descent except:
    - for leftmost non-terminal S, we look at input a
    - choose the production shown at [S, a]
- A stack records frontier of parse tree

![Session%204%206d68f59568fc4b0680cfd902955b7f50/Untitled%207.png](Session%204%206d68f59568fc4b0680cfd902955b7f50/Untitled%207.png)

**LL(1) Parsing table construction:**

Takes as input Grammar G and outputs a parsing table T.

How do we get to the parsing table?

- For each production A → a in G:
    - For each **terminal t belonging to FIRST(a)** do **T[A, t]** = **a** (Intuition: **a** can derive **t** in the first position.
    - If **epsilon belongs to FIRST(a)**, for each **t belonging to FOLLOW(a)** do **T[A, t]** = **a** (Intuition: A cannot derive t, but t can follow A)
    - If **epsilon belongs to FIRST(a)** and **$ belongs to FOLLOW(A)** do **T[A,$] = a**
- If any entry is multiply defined, then G is not LL(1)

![Session%204%206d68f59568fc4b0680cfd902955b7f50/Untitled%208.png](Session%204%206d68f59568fc4b0680cfd902955b7f50/Untitled%208.png)

![Session%204%206d68f59568fc4b0680cfd902955b7f50/Untitled%209.png](Session%204%206d68f59568fc4b0680cfd902955b7f50/Untitled%209.png)

RECALL: If any entry is multiply defined then G is not LL(1)

- if G is ambiguous
- if G is left recursive
- if G is not left factored
- The only way to test grammar for LL(1) Parsing is to apply the two properties: FIRST and FOLLOW (and generate parsing table)
- Most programming languages are not LL(1).
- There are tools that build LL(1) tables.

---

# Bottom-up Parsing

- Start at the leaves and grow toward root
- An input is consumed, encode possibilities in an internal state.
- Start in a state valid for legal first tokens
- We can make the process deterministic
- Bottom up parsers can recognize a large class of grammars than top down parsers.
- Bottom up parsers dont need left-refactored grammars
- The idea behind bottom-up parsing reduces a string to the start symbol by inverting productions.

![Session%204%206d68f59568fc4b0680cfd902955b7f50/Untitled%2010.png](Session%204%206d68f59568fc4b0680cfd902955b7f50/Untitled%2010.png)

Observations:

- Read the productions in reverse (bottom to top)
- This is a reverse rightmost derivation

![Session%204%206d68f59568fc4b0680cfd902955b7f50/Untitled%2011.png](Session%204%206d68f59568fc4b0680cfd902955b7f50/Untitled%2011.png)

- A bottom up parser traces a rightmost derivation in reverse.

Defining **handles**

- We want bottom up parser to reduce only if the result can be reduced to the start symbol
- Why? Because we saw this mistake:

![Session%204%206d68f59568fc4b0680cfd902955b7f50/Untitled%2012.png](Session%204%206d68f59568fc4b0680cfd902955b7f50/Untitled%2012.png)

- Handle is a string that can be reduced and also allows further reductions back to the start symbol.
- We want to handle at handles only.
- How do we find handles? Its like magic right now.
- The process to construct a bottom-up parser is called **handle pruning**.
- 

![Session%204%206d68f59568fc4b0680cfd902955b7f50/Untitled%2013.png](Session%204%206d68f59568fc4b0680cfd902955b7f50/Untitled%2013.png)

[]()

---

Summary:

- Recursive descent parser is simple and general.
    - Left recursion must be eliminated first but that can be done automatically.
- Historically its unpopular because of backtracking. In practice, backtracking is eliminated by restricting the grammar.
- Top-down parsers build parsing tree from root to leaves
    - left recursion causes non-termination in top-down parsers. (transformation to eliminate left recursion)
- Two top-down parsers:
    - Recursive descent
    - Predictive parsing LL(1)
        - FIRST and FOLLOW are used to construct parsing tables.
        - Skeleton parsers based on parsing table.