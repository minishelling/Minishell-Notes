---
Status: Not started
---
A "simple command" is a sequence of optional variable assignments and redirections, in any sequence, optionally followed by  
words and redirections, terminated by a control operator.  
When a given simple command is required to be executed (that is, when any conditional construct such as an AND-OR list or a  
**case** statement has not bypassed the simple command), the following expansions, assignments, and redirections shall all be  
performed from the beginning of the command text to the end:  
1. The words that are recognized as variable assignments or redirections according to [Shell Grammar  
    Rules  
    ](https://pubs.opengroup.org/onlinepubs/9699919799/utilities/V3_chap02.html\#tag_18_10_02)are saved for processing in steps 3 and 4.
2. The words that are not variable assignments or redirections shall be expanded. If any fields remain following their expansion,  
    the first field shall be considered the command name and remaining fields are the arguments for the command.  
    
3. Redirections shall be performed as described in [Redirection](https://pubs.opengroup.org/onlinepubs/9699919799/utilities/V3_chap02.html#tag_18_07).
4. Each variable assignment shall be expanded for tilde expansion, parameter expansion, command substitution, arithmetic expansion,  
    and quote removal prior to assigning the value.  
    
In the preceding list, the order of steps 3 and 4 may be reversed if no command name results from step 2 or if the command name  
matches the name of a special built-in utility; see  
[Special Built-In Utilities](https://pubs.opengroup.org/onlinepubs/9699919799/utilities/V3_chap02.html#tag_18_14).
Variable assignments shall be performed as follows:
- If no command name results, variable assignments shall affect the current execution environment.
- If the command name is not a special built-in utility or function, the variable assignments shall be exported for the execution  
    environment of the command and shall not affect the current execution environment except as a side-effect of the expansions  
    performed in step 4. In this case it is unspecified:  
    - Whether or not the assignments are visible for subsequent expansions in step 4
    - Whether variable assignments made as side-effects of these expansions are visible for subsequent expansions in step 4, or in the  
        current shell execution environment, or both  
        
- If the command name is a standard utility implemented as a function (see XBD _[Utility](https://pubs.opengroup.org/onlinepubs/9699919799/basedefs/V1_chap04.html#tag_04_22)_), the effect of variable assignments shall be as if the utility was not  
    implemented as a function.  
    
- If the command name is a special built-in utility, variable assignments shall affect the current execution environment. Unless  
    the _[set](https://pubs.opengroup.org/onlinepubs/9699919799/utilities/V3_chap02.html#set)_ **a** option is on (see [set](https://pubs.opengroup.org/onlinepubs/9699919799/utilities/V3_chap02.html#tag_18_25)), it is unspecified:
    - Whether or not the variables gain the _export_ attribute during the execution of the special built-in utility
    - Whether or not _export_ attributes gained as a result of the variable assignments persist after the completion of the  
        special built-in utility  
        
- If the command name is a function that is not a standard utility implemented as a function, variable assignments shall affect  
    the current execution environment during the execution of the function. It is unspecified:  
    - Whether or not the variable assignments persist after the completion of the function
    - Whether or not the variables gain the _export_ attribute during the execution of the function
    - Whether or not _export_ attributes gained as a result of the variable assignments persist after the completion of the  
        function (if variable assignments persist after the completion of the function)  
        
If any of the variable assignments attempt to assign a value to a variable for which the _readonly_ attribute is set in the  
current shell environment (regardless of whether the assignment is made in that environment), a variable assignment error shall  
occur. See  
[Consequences of Shell Errors](https://pubs.opengroup.org/onlinepubs/9699919799/utilities/V3_chap02.html#tag_18_08_01) for the consequences of these errors.
If there is no command name, any redirections shall be performed in a  
subshell environment; it is unspecified whether this  
subshell environment is the same one as that used for a command  
substitution within the command. (To affect the current execution  
environment, see the  
[exec](https://pubs.opengroup.org/onlinepubs/9699919799/utilities/V3_chap02.html#tag_18_20)  
special built-in.) If any of the redirections performed in the current  
shell  
execution environment fail, the command shall immediately fail with an  
exit status greater than zero, and the shell shall write an  
error message indicating the failure. See  
[Consequences of Shell Errors](https://pubs.opengroup.org/onlinepubs/9699919799/utilities/V3_chap02.html#tag_18_08_01) for the consequences of these  
failures on interactive and non-interactive shells.  
If there is a command name, execution shall continue as described in [Command Search and Execution](https://pubs.opengroup.org/onlinepubs/9699919799/utilities/V3_chap02.html#tag_18_09_01_01)  
. If there is no command name, but the command contained a command  
substitution, the command shall complete with the exit status of  
the last command substitution performed. Otherwise, the command shall  
complete with a zero exit status.