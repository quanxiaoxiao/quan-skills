# Code Complexity Checklist

Before completing a task verify:

* [ ] functions ≤ 40 lines
* [ ] files ≤ 300 lines
* [ ] nesting depth ≤ 3
* [ ] functions have a single responsibility
* [ ] key business logic split into functions that can be unit tested directly
* [ ] pure logic separated from side effects where practical
* [ ] one function does not mix parsing, branching, side effects, and response formatting
* [ ] parameters ≤ 5
* [ ] logic extracted into helpers when needed
* [ ] repeated logic refactored

If any rule is violated, refactor the implementation.
