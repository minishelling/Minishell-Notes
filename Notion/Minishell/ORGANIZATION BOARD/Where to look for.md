---
Status: Not started
---
  
# Where to look for?
### SHLVL
- We can start at Michelle’s. We still have to see if she handles signals well.
  
### ENVP
- Repositories with linked lists
  
### $() - Command Substitution
- Martijn’s
  
# Where NOT to look for?
### ; - Command Separator
- Bote’s and Tycho’s
  
### $() - Command Substitution
- Bote’s and Tycho’s
- Michelle’s
- Victor’s
  
### $(()) - Arithmetic Expansion
- Tycho’s and Bote’s
  
### Quotes
Victor
Bote
  
### CD
- Michelle’s segfaults when HOME is unset.
- Martijn’s - Interesting error message though
    
    ```Bash
    lisandro@Lisandro-Win:~/Github/mini/martijns$ ./minishell
    🐙> cd ..
    🐙> cd martijns/
    🐙> unset PWD
    🐙> cd ..
    marsh: (malloc) error current_working_dir in execute_cd
    ```
    
  
### CTRL D in a heredoc
Jiajia