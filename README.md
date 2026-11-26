# Assignment 1: for and _do-while_, by desugaring
## Group 404: Francesco Bertaiola, Serena Federici, Maria Eduarda Tosetti Guerra

### Solution
Both loops are desugared in the parser (rwetac/src/parser.rs), so the resolver, typechecker and later phases are
unchanged and no new AST node was added.
The semicolons separating the clauses in a _for_ statement are mandatory to avoid ambiguities. An extra semicolon
after c and parentheses around the clause list are optional. The compiler also supports the omission of each clause of
the _for_ loop. When the condition b is omitted, the compiler interprets it as true. The _do-while_ also allows optional
parentheses around the guard.

To implement the _for_ loop, we used the existing _while_ loop construct. The condition b was placed as the guard of
the _while_ loop, the increment c was added after the body, and the statement a was placed immediately before the _while_ loop. Similarly, the _while_ loop construct was used to implement the _do-while_ loop. To execute the body once before evaluating the condition, a variable _\$do\_cond_ was placed as the guard of the while loop and is initially set to true, ensuring that the first iteration of the loop is always executed. At the end of each iteration, the variable _\$do\_cond_ is updated to the value of the _do-while_ guard.

The bodies of both the _for_ and _do-while_ loops are enclosed within an if true. The typechecker would otherwise reject any body that had a return statement inside. Moreover, to ensure that any variable declared inside remains local to the loop, both constructs were wrapped within a block.

### Files changed
* rwetac/src/token.rs: we introduced two new tokens, _Do_ and _For_.
* rwetac/src/parser.rs: we introduced the functions _parse\_do\_while_ and _parse\_for_, which are called by
parse statement and contain the desugaring logic. Moreover, we moved the existing logic for parsing identifier-led simple statements (procedure calls, local declarations, and assignments) into the new function _parse\_simple\_stmt (terminated: bool)_, which, depending on the value of terminated, either requires or omits the ending semicolon. This function was necessary since the syntax of the _for_ loop did not include a semicolon after c, yet c required a trailing semicolon to be parsed.
* tests/: the 6 tests requested by the assignment were addes in this folder

### Lessons learned
The main one: **everything comes at a price**. More specifically:
* Desugaring before the AST forced us to build and design the _for_ and _do-while_ using many existing constructs.
* Initially, we thought about duplicating the body of the _do-while_, but this would have increased the number of
nodes in the AST and duplicated any errors present in body (bad for error reporting). Moreover, in practice,
this would have required adding a Clone trait for the whole AST and we thought that the consequences would
be mostly negative, non-maintainable and non-scalable.
* Our implementation does not work for one case: when the only return statement that is always executed is inside
the _do-while_. Since the body is inside a _while_, the compiler cannot know that the first iteration will always be executed. We discussed many solutions, but they either required cloning the body or updating the typechecker,
which we thought would not be consistent with the concept of desugaring.
