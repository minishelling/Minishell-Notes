---
Status: Not started
---
- Approach using a jumptable
    
      
      
    
      
    
Interesting. Tell me all about that
  
I think what I thought about when writing this was using a jumptable for the builtin commands.  
Usually the builtins are dealt with using a lot of if/else if/else statements, but the other approach would be to have an array with all the builtin commands names, and then a jumptable with the function pointers to those builtins.  
  
- Approach going back and forth from parsing to execution