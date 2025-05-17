---
Status: Not started
---
Syntax error:
  
|   |   |   |   |   |   |   |   |   |   |   |
|---|---|---|---|---|---|---|---|---|---|---|
|==Token\ Followed by==|==WORD==|==PAR_OPEN==|PAR_CLOSE|IN_REDIR|OUT_REDIR|QUOTE|ENV_VAR|PIPE|AND/OR|NL|
|==WORD==||syntax error near unexpected token <newline or token after PAR_OPEN>|||||||||
|==PAR_OPEN==|syntax error near unexpected token <newline or token after PAR_OPEN>||syntax error near unexpected token `)'|||?||syntax error near unexpected token `\|'|syntax error near unexpected token `&&' or `\|'||
|==PAR_CLOSE==||syntax error near unexpected token `)'||||syntax error near unexpected token `QUOTE'|||||
|IN_REDIR||?|syntax error near unexpected token `)'|||||||syntax error near unexpected token `newline'|
|OUT_REDIR||||?|?|||||syntax error near unexpected token `newline'|
|QUOTE|||||||||||
|ENV_VAR||we are not supposed to handle|syntax error near unexpected token `)'||||||||
|PIPE|||syntax error near unexpected token `\|'|||||syntax error near unexpected token `\|'|syntax error near unexpected token `&&' or `\|'|?|
|AND/OR|||syntax error near unexpected token `)'|||||syntax error near unexpected token `\|'|syntax error near unexpected token `\|' the second log_op|?|
|NL|||||||||||
  
  
  
  
`<(command)` syntax is called process substitution, which allows the output of a command to be used as a file input for another command.
`$(command)` syntax is called Command substitution.  
  
`$((...))` syntax is called arithmetic expansion
  
  
ls (<infile) syntax error
ls | (cat <infile) ok
  
cat (la) >>outfile should
  
( <dsfdsf )