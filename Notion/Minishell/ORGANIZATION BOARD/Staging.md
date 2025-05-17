---
Status: Not started
---
# TO DO
- $? expansion
- Error messages
    - prompt, cmd: to use later on in perror (otherwise consider strerror).
    - cmd, message: for cmd not found.
    - prompt, message: for tokenization errors.
- Leaks
- Signals
- Clean_nicely function
- There are cases where we just clean nicely (case 1), and others where we clean nicely and exit process (case 2), and others where clean nicely and exit minishell (case 3).
    - Malloc: case 3
    - Dup: case 3
    - System call failures: case 3
    - Syntax: case 1
    - Parsing in execution:
        - env variable name: case 2
        
    
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
# Parsing
When the shell reads input, it proceeds through a  
sequence of operations. If the input indicates the beginning of a  
comment, the shell ignores the comment symbol (‘\#’), and the rest  
of that line.  
Otherwise, roughly speaking, the shell reads its input and  
divides the input into words and operators, employing the quoting rules  
to select which meanings to assign various words and characters.  
The shell then parses these tokens into commands and other constructs,  
removes the special meaning of certain words or characters, expands  
others, redirects input and output as needed, executes the specified  
command, waits for the command’s exit status, and makes that exit status  
available for further inspection or processing.  
  
**Tokenization** using meta chars and special chars ($, ‘ , “)
  
Check token **syntax** (take care of ambiguous redirects?)
**"**Ambiguous redirects" in the context of shell scripting typically refers to situations where the shell encounters redirection symbols (`<`, `>`, `>>`, `<<`) in commands but is unable to determine the intended target or source for the redirection.
  
**Expansion** is performed on the command line after it has been split into tokens.  
There are seven kinds of expansion performed:  
- brace expansion
- tilde expansion
- parameter and variable expansion
- command substitution
- arithmetic expansion
- word splitting
- filename expansion
The order of expansions is:  
brace expansion; tilde expansion, parameter and variable expansion, arithmetic expansion,  
and command substitution (done in a left-to-right fashion); word splitting; and filename expansion.  
On systems that can support it, there is an additional expansion  
available: _process substitution_.  
This is performed at the  
same time as tilde, parameter, variable, and arithmetic expansion and  
command substitution.  
After these expansions are performed, quote characters present in the  
original word are removed unless they have been quoted themselves  
(_quote removal_).
Only brace expansion, word splitting, and filename expansion  
can increase the number of words of the expansion; other expansions  
expand a single word to a single word.  
The only exceptions to this are the expansions of  
  
`"$@"` and `$*` (see [Special Parameters](https://www.gnu.org/software/bash/manual/bash.html#Special-Parameters)), and  
  
`"${name[@]}"` and `${name[*]}`  
(see  
[Arrays](https://www.gnu.org/software/bash/manual/bash.html#Arrays)).
  
In out case: variable expansion, squotes, dquotes handling (inside dquotes will not expand)
**Injection**: Inserting the expanded values back into the original context to form the final command string.
**Input and output redirection**
__________________________________________________
---
---
---
---
---
**Simple commands**
A simple command is the kind of command encountered most often.  
It’s just a sequence of words separated by  
`blank`s, terminated  
by one of the shell’s control operators (see  
[Definitions](https://www.gnu.org/software/bash/manual/bash.html#Definitions)). The  
first word generally specifies a command to be executed, with the  
rest of the words being that command’s arguments.  
The return status (see [Exit Status](https://www.gnu.org/software/bash/manual/bash.html#Exit-Status)) of a simple command is  
its exit status as provided  
by the POSIX 1003.1  
`waitpid` function, or 128+n if  
the command was terminated by signal n.  
  
**Pipelines**
A `pipeline` is a sequence of one or more commands separated by  
one of the control operators ‘|’ or ‘|&’.  
The format for a pipeline is
`[time [-p]] [!] command1 [ | or |& command2 ] …`
  
**A list**
A `list` is a sequence of one or more pipelines separated by one  
of the operators ‘;’, ‘&’, ‘&&’, or ‘||’,  
and optionally terminated by one of ‘;’, ‘&’, or a  
  
`newline`.  
  
  
  
  
Expander States:  
- SCANNING (default) = 'normal' parsing of characters  
- state changes:  
- after $: if quote state != IN_SQUOTE: expander state -> READING_VAR_NAME  
- creating expanded string:  
- squotes are only added if quote state == IN_DQUOTE  
- dquotes are only added if quote state == IN_SQUOTE  
- dollar symbol are only added if quote state == IN_SQUOTE  
- other chars, incl. blanks are always added  
- READING_VAR_NAME = $ has been read  
- state changes:  
- state changes back to SCANNING after squote, dquote, or <blank> char  
- creating expanded string:  
- after '\0', $, single quote, double quote, blank -> variable is expanded  
- in case the $ char is directly followed by $, blank, or '\0' ->  
the $ char is literally printed  
  
---
## To discuss - ☑️
  
---
## Discussed - ✅
- How to approach functions
    
    I’m thinking of 2 ways of designing our functions, and wanted to discuss with you what should we go for.  
    One approach would be for instance to pass everywhere we can the structure that holds all the malloc-ed stuff (our shell struct), and if something fails within the function we can exit from that point. Another point of this approach is that we can simply assign values to variables for our struct directly inside the function, and therefore the function itself can return something else or nothing at all.  
    A different approach is to pass only the necessary information for that function, and make the function return what it does, and if we have to exit we do so from the function that called it, and not from within the function itself. I think this approach makes the functions re-usable, and might be better coding practice, but I’m not certain.  
    
  
tokenize
check parenthesis, check quotes
read heredocs
reading from beginning until we reach an logical operator that is either not in parenthesis
try to open all outfiles read and try to open all infiles read
execute that part
save the exit code somewhere
look if the part beyond the logical operator needs to be run
if it does, nice. if not skip to the next logical operator that is outside parenthesis
  
---
  
  
- The previous backbone section is here
    
      
    
    - Possibility 1
        
        - Sources
            - builtin
            - log
            - executor
            - expander
            - parser
            - utils
        - Libft
            - get_next_line
            - ft_printf
            - other c files
        
        - Tester
        - Include
        
          
        
    
      
    
    ---
    
      
    
    ## Built-ins
    
    - `cd`
        
        Changes the working directory of the current shell execution environment and updates the environment variables `PWD` and `OLDPWD`.  
        Without arguments it changes the working directory to the home directory.  
          
        `-` changes the directory to the `OLDPWD`.
        
    - `echo`
        
        Displays a line of text.
        
        `-n` does not output the trailing new line.
        
    - `env`
        
        Displays the environment variables.
        
    - `exit`
        
        Terminates the shell.  
          
        `n` sets the exit status to `n`.
        
    - `export`
        
        Accepts the arguments `name[=value]`.  
        Adds  
        `name` to the environment. Set’s the value of `name` to `value`.  
        If no argument is given, it displays a list of the exported variables.  
        
    - `pwd`
        
        Shows the current directory as an absolute path.
        
    - `unset`
        
        Accepts the argument `name`.  
        Removes the variable  
        `name` from the environment.
        
          
        
    
    ## Binary Operators
    
    ### Arithmetic (mathematical) operators
    
    - Addition ( + )
    - Subtraction ( - )
    - Multiplication ( * )
    - Division ( / )
    - Modulo ( % )
    - Power ( ** )
    
      
    
    ### Logical operators
    
    - Non-exclusive OR ( || )
    - AND ( && )