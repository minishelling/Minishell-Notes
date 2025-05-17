---
Status: In progress
Assign: Lisandro
Sub-item:
  - "[[Handle execution permissions]]"
  - "[[If a directory in PATH doesn’t have execution permissions, it acts as if the cmd wasn’t found]]"
  - "[[Handle relative paths to commands]]"
  - "[[Shell levels]]"
  - "[[cmd_name as path]]"
---
  
# Notes
- For each command called, the environment array has to be re-done.
- Same with the command path.
    
    - This is one of the reasons why I shouldn’t replace the first argument in the cmd_list with the cmd_path. Rather I should use a different variable, that can be freed at the end of execution.
    
      
    
  
  
  
  
  
  
  
REDIRECTIONS
  
- Hdoc and infiles have the same level of precedence
- Whatever comes last, that has the precedence.
- If the argument is a file (F_OK) that has precedence.
  
- Table Template
    
    |Test|Output|Error|Exit Code|Input Redirection|Output Redirection|Comments|
    |---|---|---|---|---|---|---|
    ||||||||
    ||||||||
    ||||||||
    
  
- Simple Input Tests
    
      
    
    There are 4 things to test:
    
    - File as an argument
    - File as an input redirection
    - Heredoc
    - Pipe
    
      
    
      
    
    |Test|Output|Error|Exit Code|Input Redirection|Output Redirection|Comments|
    |---|---|---|---|---|---|---|
    ||||||||
    ||||||||
    ||||||||
    
  
  
|Test|Output|Error|Exit Code|Input Redirection|Output Redirection|Comments|
|---|---|---|---|---|---|---|
|`cat <infile1 infile1_nr`||`cat: infile1_nr: permission denied`|1||||
|`cat <infile1 <infile1_nr`||`bash: infile1_nr: permission denied`|1||||
||||||||
||||||||
||||||||
||||||||
||||||||
||||||||