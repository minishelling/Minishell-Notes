---
Status: Not started
---
  
- Do not interpret unclosed quotes or special characters such as \ (backslash) or ; (semicolon).
- Handle `'`(single quote) which should prevent the shell from interpreting the metacharacters in the quoted sequence.
- Handle `"` (double quote) which should prevent the shell from interpreting the metacharacters in the quoted sequence except for `$` (dollar sign).
- Implement redirections:  
    ◦  
    `<` should redirect input.  
    ◦  
    `>` should redirect output.  
    ◦  
    `<<` should be given a delimiter, then read the input until a line containing the  
    delimiter is seen. However, it doesn’t have to update the history!  
    ◦  
    `>>` should redirect output in append mode.
- Implement pipes (`|` character). The output of each command in the pipeline is connected to the input of the next command via a pipe.
- Handle environment variables (`$` followed by a sequence of characters) which  
    should expand to their values.  
    
- Handle `$?` which should expand to the exit status of the most recently executed foreground pipeline.
Bonus:
1. `&&` and `||` with parentheses for priorities.
2. Wildcards `*` should work for the current working directory.
  
  
Control operators:
is a token that performs a control funcion.  
‘||’  
‘&&’
‘&’,
‘;’
‘;;’
‘;&’,
‘;;&’  
‘|’  
‘|&’
‘(’,
‘)’
  
Redirection operators:
‘>’ Redirecting input
‘<’
‘>>’
‘<<’
‘&>’
‘<&’
‘>&’
‘&>>’
‘<<<’
  
f0r3s17% ./a.out "abc""def"  
2%  
  
bote’s problem:  
$>ls” “-la  
token: ls" "-la  
Minishell: "ls "-l": command not found