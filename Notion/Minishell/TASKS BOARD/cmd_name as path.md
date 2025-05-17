---
Status: Not started
Parent item:
  - "[[Execution]]"
---
  
If the cmd_name starts with ./ then that takes priority.
Otherwise it checks on the PATH variable
If path was NULL, then it checks the cmd_name as a path (even if it doesnt start with ./)  
  
Otherwise it returns NULL.