---
Status: Not started
---
### Test cases
  

> `echo 123 || ((sleep && echo 456) || grep)`
```JavaScript
OR_OPR
    CMD |echo| |123|
    OR_OPR
        AND_OPR
            CMD |sleep| |sleep|
            CMD |echo| |456|
        CMD |grep| |grep|
```

> `(echo 123 || (sleep && echo 456) && ls) || grep`
```JavaScript
OR_OPR
    AND_OPR
        OR_OPR
            CMD |echo| |123|
            AND_OPR
                CMD |sleep| |sleep|
                CMD |echo| |456|
        CMD |ls| |ls|
    CMD |grep| |grep|
```

> `(echo 123 || (sleep && echo 456) && ls)`
```JavaScript
AND_OPR
    OR_OPR
        CMD |echo| |123|
        AND_OPR
            CMD |sleep| |sleep|
            CMD |echo| |456|
    CMD |ls| |ls|
```

> `(((ls) || cmd2) && (cmd3))`
```JavaScript
AND_OPR
    OR_OPR
        CMD |ls| |ls|
        CMD |cmd2| |cmd2|
    CMD |cmd3| |cmd3|
```

> `(ls || echo 123) && (pwd)`
```JavaScript
AND_OPR
    OR_OPR
        CMD |ls| |ls|
        CMD |echo| |123|
    CMD |pwd| |pwd|
```

> `(ls) || (pwd)`
```JavaScript
OR_OPR
    CMD |ls| |ls|
    CMD |pwd| |pwd|
```

> `(echo 123 (&& ls -la || sleep) || echo)`
```JavaScript
Mini_shared: Syntax error
```

> `((echo 123 || sleep && ) echo)`
```JavaScript
Mini_shared: Syntax error near unexpected token `)`
```

> `(echo 123 (&& ls -la || sleep) || echo)`
```JavaScript
Mini_shared: Syntax error
```

> `sleep (5)`
```JavaScript
Mini_shared: Syntax error
```

> `( (ls -la || echo 2 ) && ((cmd3)) || cmd4 && cmd5)`
```JavaScript
AND_OPR
    OR_OPR
        AND_OPR
            OR_OPR
                CMD |ls| |-la|
                CMD |echo| |2|
            CMD |((| |))|
        CMD |cmd4| |cmd4|
    CMD |cmd5| |cmd5|
```

> `((cmd1 )) || (cmd2)`
```JavaScript
OR_OPR
    CMD |((| |))|
    CMD |cmd2| |cmd2|
```

> `(((ls) || (echo 123)) && (pwd)) || ((grep && (ls || sleep)) && echo 456) || ((grep && (ls || sleep)) && echo 789)`
```JavaScript
OR_OPR
    OR_OPR
        AND_OPR
            OR_OPR
                CMD |ls| |ls|
                CMD |cat| |cat|
            CMD |pwd| |pwd|
        AND_OPR
            AND_OPR
                CMD |grep| |grep|
                OR_OPR
                    CMD |ls| |ls|
                    CMD |sleep| |sleep|
            CMD |echo| |echo|
    AND_OPR
        AND_OPR
            CMD |grep| |grep|
            OR_OPR
                CMD |ls| |ls|
                CMD |sleep| |sleep|
        CMD |echo| |echo|
```

> `(((echo 1) || (echo 2)) && (echo 3)) && ((echo 4 && (echo 5 || echo 6)) && echo 7) && ((echo 8 && (echo 9 || echo 10)) && echo 11)`
```JavaScript
AND_OPR
    AND_OPR
        AND_OPR
            OR_OPR
                CMD |echo| |1|
                CMD |echo| |2|
            CMD |echo| |3|
        AND_OPR
            AND_OPR
                CMD |echo| |4|
                OR_OPR
                    CMD |echo| |5|
                    CMD |echo| |6|
            CMD |echo| |7|
    AND_OPR
        AND_OPR
            CMD |echo| |8|
            OR_OPR
                CMD |echo| |9|
                CMD |echo| |10|
        CMD |echo| |11| 
```
  

> `(((ls -la) || (cat -e <infile)) && (pwd)) || ((grep && (ls -l -a|| sleep -1)) && echo 123) || ((grep TERM && (ls -la || sleep 5)) && echo <<EOF)`
```C
OR_OPR
    OR_OPR
        AND_OPR
            OR_OPR
                CMD |ls| |-la|
                    Command: ls -la
                CMD |cat| |infile|
                    Command: cat -e
                    Redirection: infile (INFILE)
            CMD |pwd| |pwd|
                Command: pwd
        AND_OPR
            AND_OPR
                CMD |grep| |grep|
                    Command: grep
                OR_OPR
                    CMD |ls| |-a|
                        Command: ls -l -a
                    CMD |sleep| |-1|
                        Command: sleep -1
            CMD |echo| |123|
                Command: echo 123
    AND_OPR
        AND_OPR
            CMD |grep| |TERM|
                Command: grep TERM
            OR_OPR
                CMD |ls| |-la|
                    Command: ls -la
                CMD |sleep| |5|
                    Command: sleep 5
        CMD |echo| |EOF|
            Command: echo
            Redirection: EOF (HEREDOC)
```

> `(((echo 1) || (invalidcmd1 && echo 2)) && (echo 3 || invalidcmd2)) && ((invalidcmd3 && (echo 4 || invalidcmd4)) && echo 5) && ((echo 6 && (invalidcmd5 || echo 7)) && echo 8)`
```C
AND_OPR
    AND_OPR
        AND_OPR
            OR_OPR
                CMD |echo| |1|
                    Command: echo 1
                AND_OPR
                    CMD |invalidcmd1| |invalidcmd1|
                        Command: invalidcmd1
                    CMD |echo| |2|
                        Command: echo 2
            OR_OPR
                CMD |echo| |3|
                    Command: echo 3
                CMD |invalidcmd2| |invalidcmd2|
                    Command: invalidcmd2
        AND_OPR
            AND_OPR
                CMD |invalidcmd3| |invalidcmd3|
                    Command: invalidcmd3
                OR_OPR
                    CMD |echo| |4|
                        Command: echo 4
                    CMD |invalidcmd4| |invalidcmd4|
                        Command: invalidcmd4
            CMD |echo| |5|
                Command: echo 5
    AND_OPR
        AND_OPR
            CMD |echo| |6|
                Command: echo 6
            OR_OPR
                CMD |invalidcmd5| |invalidcmd5|
                    Command: invalidcmd5
                CMD |echo| |7|
                    Command: echo 7
        CMD |echo| |8|
            Command: echo 8
```
  
  
`cmd1 | (cmd2 && cmd3)` :
Maybe if cmd2 is reading from the pipe it will close it so cmd3 will have no input. Otherwise, if cmd2 is not reading from the pipe, cmd3 will have the output of cmd1 as input ??? → Yes
  

> `(echo 1 && echo 2 || echo 3) | (echo 4 && echo 5) && echo 6`
```JavaScript
AND_OPR
    PIPE
        OR_OPR
            AND_OPR
                CMD |echo| |1|
                CMD |echo| |2|
            CMD |echo| |3|
        AND_OPR
            CMD |echo| |4|
            CMD |echo| |5|
    CMD |echo| |6|
```
PIPE is a right child:

> `echo 1 && (echo 2 || echo 3) | (echo 4 && echo 5 && echo 6)`
```JavaScript
AND_OPR
    CMD |echo| |1|
    PIPE
        OR_OPR
            CMD |echo| |2|
            CMD |echo| |3|
        AND_OPR
            AND_OPR
                CMD |echo| |4|
                CMD |echo| |5|
            CMD |echo| |6|
```

> `echo 1 && echo 2 || echo 3 | (echo 4 && echo 5) && echo 6`
```JavaScript
AND_OPR
    OR_OPR
        AND_OPR
            CMD |echo| |1|
            CMD |echo| |2|
        PIPE
            CMD |echo| |3|
            AND_OPR
                CMD |echo| |4|
                CMD |echo| |5|
    CMD |echo| |6|
```

> `echo 1 && (echo 2 || echo 3) | (echo 4 && echo 5) && echo 6`
```JavaScript
AND_OPR
    AND_OPR
        CMD |echo| |1|
        PIPE
            OR_OPR
                CMD |echo| |2|
                CMD |echo| |3|
            AND_OPR
                CMD |echo| |4|
                CMD |echo| |5|
    CMD |echo| |6|
```

> `echo 1 | echo 2 && echo 3 | (echo 4 || echo 5)`
```JavaScript
AND_OPR
    PIPE
        CMD |echo| |1|
        CMD |echo| |2|
    PIPE
        CMD |echo| |3|
        OR_OPR
            CMD |echo| |4|
            CMD |echo| |5|
```

> `ls | (cat && cat -e)`
```JavaScript
PIPE
    CMD |ls| |ls|
    AND_OPR
        CMD |cat| |cat|
        CMD |cat| |-e|
       
```

> `(echo 1 && invalid 2 || echo 3) | (echo 4 && echo 5) && invalid 6`
```C
AND_OPR 0x1162750
    PIPE 0x11627d0
        OR_OPR 0x1162610
            AND_OPR 0x1162710
                CMD start token:|echo|, end token:|1|, tree address: 0x1162650
                CMD start token:|invalid|, end token:|2|, tree address: 0x11626d0
            CMD start token:|echo|, end token:|3|, tree address: 0x1162690
        AND_OPR 0x1162790
            CMD start token:|echo|, end token:|4|, tree address: 0x11625d0
            CMD start token:|echo|, end token:|5|, tree address: 0x1162590
    CMD start token:|invalid|, end token:|6|, tree address: 0x1162810
```

> `( ( ls ) )`
```C
This is an arithmetic expansion.                We don't do that here.
```

> `$(ls)`
```C
This is either a command substitution or an arithmetic expansion. We don't do these here.
```