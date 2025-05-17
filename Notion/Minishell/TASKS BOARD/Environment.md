---
Status: Done
Assign: LisandroTTamar
Sub-item:
  - "[[Handle case where there’s no value]]"
---
## To still handle:
- Update `SHLVL`
    - It would be +1 each time minishell is called. So each time env is initialized it should be +1.
- Update the underscore variable `_`
    - `$_` holds the last argument of the previous command.
    - It can also hold the name of the last executed command.
    - It’s always the last variable in the env list.
- ~~Create variable called keys in the ENV struct, that would split keys that are separated by ‘`:`’ (such as `PATH` or `CDPATH`) into new strings stored in this ‘keys’ array.~~
    - It would make the `cd` executable easier to implement.
    - While parsing for it: if the value after the colon is a digit, then the colon is not a separator but instead part of the variable’s value.
- **Create a function to update the keyvalue variable if the value gets updated**.
- Create a function that removes a node from the list (it would also set the previous node to point not to the removed node anymore but to the next).
- Create functions to update values and keyvalues.
  
---