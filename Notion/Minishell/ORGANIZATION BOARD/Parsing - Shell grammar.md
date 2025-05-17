---
Status: Not started
---
### Precedence Climbing
(As opposed to recursive descent)
[https://eli.thegreenplace.net/2012/08/02/parsing-expressions-by-precedence-climbing](https://eli.thegreenplace.net/2012/08/02/parsing-expressions-by-precedence-climbing)
  
👁👁 [https://www.youtube.com/watch?v=fiJR4_059HA&t=4s](https://www.youtube.com/watch?v=fiJR4_059HA&t=4s)
[https://m4nnb3ll.medium.com/minishell-building-a-mini-bash-a-42-project-b55a10598218](https://m4nnb3ll.medium.com/minishell-building-a-mini-bash-a-42-project-b55a10598218)
  
Check input before parsing
- ‘“ ‘
- ‘\’’
- ‘>’
- ‘<’
- ‘&’
- ‘;’
- ‘(’
- ‘)’
Split pipe wise
- char **pipe_wise_splitted;
First initialization
- allocate data->group
number of groups is dependent on the number of pipes
Make token
- special chars ‘>’, ‘<’, ‘$’, ‘<<’, ‘>>’
    
    expand/ heredoc/ redirect
    
- quoted word extract ‘”’, ‘\’’
- normal word extract
Free
  
  
### Command substitution
changes formatting: all white spaces turn into a space.
  
### Here doc
2 possible ways:
1. Keeping track of all files paths to unlink if necessary
2. going through the whole line, checking for here doces. If ones fails we don’t do anything.
  
  
Backus - Naur, Recursive Descent  
Formal grammar:  
```Plain
<input> ::= <command-list> | <background-command-list>
<background-command-list> ::= <command-list> <nullspace> <bg>
<bg> ::= "&"
<command-list> ::= <command> | <piped-list-of-commands>
<command> ::= <simple-command> | <compound-command>
<piped-list-of-commands> ::= <command> | <command> <nullspace> <pipe> <piped-list-of-commands>
<nullspace> ::= "" | <sp>
<pipe> ::= "|"
<sp> ::= <whitespace> | <whitespace> <sp>
<whitespace> ::= " " | "\t"
<simple-command> ::= <arg-list>
<arg-list> ::= <word> | <word> <sp> <arg-list>
<word> ::= <char> | <char> <word>
<char> ::= any character (ascii or unicode) that are not in the set {'|', ' ', '\t', '<', '>', '&'}
<compound-command> ::= <possible-io-list> <nullspace> <simple-command> <nullspace> <possible-io-list>
<possible-io-list> ::= "" | <io-list>
<io-list> ::= <io-op> | <io-op> <nullspace> <io-list>
<io-op> ::= <input-redirection> | <output-redirection>
<input-redirection> ::= <in> <nullspace> <word>
<output-redirection> ::= <stdout> | <fd-out>
<stdout> ::= <out> <nullspace> <word>
<fd-out> ::= <fd> <out> <nullspace> <word>
<fd> ::= positive integer
<in> ::= "<"
<out> ::= ">"
```
  
[https://pubs.opengroup.org/onlinepubs/9699919799.2016edition/utilities/V3_chap02.html#tag_18_10](https://pubs.opengroup.org/onlinepubs/9699919799.2016edition/utilities/V3_chap02.html#tag_18_10)
[https://hal.science/hal-01890044/document](https://hal.science/hal-01890044/document)
\n:
- A token
- Part of a comment - ignored
- \ \n line continuation
- End of phrase marker
  
  
hero.codam.nl/piscines/2024/6