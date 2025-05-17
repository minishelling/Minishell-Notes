---
Status: Not started
---
Brainstorming:
|   |   |   |
|---|---|---|
|ls \| invalid_cmd1 \| invalid_cmd2 && ls \| invalid_cmd10 \| invalid_cmd20|Repeat it multiple times.|Purpose: to make it print nonsense|
|diff <(echo "hello") <(echo "world")|||
||||
||||
Simple tests:
  
|==**Test**==|==**Is it valid for minishell?**==|==**Output (STDOUT)**==|==**Error Message (STDERR)**==|==**Exit Status**==|==**Comments**==|
|---|---|---|---|---|---|
|`inv1 && inv2`|Bonus||`inv1: command not found\n`|127||
|`echo $((2 + 3))`||5||0||
|`echo {1.5}`||{1.5}||||
|`echo {1..5}`||1 2 3 4 5||0||
|`echo {1...5}`||{1…5}||||
|`echo ~`||||||
|`ls *.c`||||||
|`diff <(echo “hello”) < (echo “world”)`||||||
|`ls \| cat -e`|Bonus||||Bash handles well, only for bonus|
|`ls \| cat -e`|||``bash: syntax error near unexpected token `\|'``|2|Syntax error|
|`‘’`|||`Command '' not found, but can be installed with:`|127|Command '' not found, but can be installed with…|
|`<`|||``bash: syntax error near unexpected token `newline'``|||
|`ls “-a” -l`||YES||0|should work|
|`&`|||``bash: syntax error near unexpected token `&'``|2||
|`< valid_dir`||||||
|`< invalid_dir`||||||
|`$?`||||||
|`echo $(bash -c "echo it works")`||||||
|`unset TERM && clear`|||`TERM environment variable not set.`|1||
|`$(ls)`|||`Command 'Backup' not found, did you mean:`...|||
|`(ls)`||YES||||
|`(unset PATH && ls)`|Bonus||`bash: ls: No such file or directory`|||
|`(unset PATH; ls)`|||`bash: ls: No such file or directory`|||
|`#ls`|||||Prints nothing|
|`<< EOF \| cat`|||||The heredoc functions as an input for the command that follows (in this case none), but it wouln’t function as an input for a pipe.|
|`lshiewq > outfile`||||||
|`echo $USER`||username||0||
|`echo '$USER'`||$USER||0||
|`echo "$USER"`||username||0||
|`echo '"$USER"'`||“$USER”||0||
|`echo "'$USER'"`||‘username’||0||
|`(cat \|)`||||||
||||Syntax error|||
|||||||
|||||||
|||||||
|||||||
|||||||
|||||||
## Series infile and infile redirection tests
---
Files:
- `infile1`
    
    $$abc$$
    
- `infile2`
    
    $$def$$
    
- `infile3`
    
    $$ghi$$
    
- `infile1_nr` - Stands for no reading permissions.
    
    $$abc$$
    
- `infile2_nr`
    
    $$def$$
    
- `infile3_nr`
    
    $$ghi$$
    
- `outfile1_nw` - Stands for no writing permissions.
- `outfile2_nw`
- `outfile3_nw`
  
|**Test**|Output Stream|Error Stream|Return Value|Comments|
|---|---|---|---|---|
|INPUT AFTER THE COMMAND||||Nothing before the cat command,  <br>after the command are:|
|==`cat infile1 infile2 infile3`==|abcdefghi||0|Only filenames: will cat all of them|
|`cat <infile1 <infile2 <infile3`|ghi||0|Only infile redirections: **will cat the last one**|
|`cat <infile1 <infile2 <infile3 infile1 infile2 infile3`|abcdefghi||0|Both filenames and infile redirections: will cat only all filenames|
|INPUT BEFORE THE COMMAND||||File redirections before the cat command,  <br>after the command are:|
|`infile1 infile2 infile3 cat`||`infile1: command not found`|127|XD, for consistency|
|`<infile1 <infile2 <infile3 cat`|ghi||0|Nothing: **will cat the last redirection**|
|`<infile1 <infile2 <infile3 cat <infile1 <infile2`|def||0|File redirections: **will cat the last redirection after the command**|
|`<infile1 <infile2 <infile3 cat infile1 infile2 infile3`|abcdefghi||0|Only filenames: will cat only all filenames|
|`<infile1 <infile2 <infile3 cat <infile1 <infile2 <infile3 infile1 infile2 infile3`|abcdefghi||0|Both filenames and infile redirections: will cat only all filenames|
|OUTPUT REDIRECTION|||||
||||||
|`echo hello >outfile1 \| cat`|nothing|||outfile1: hello|
|`echo hello >outfile2 hi >outfile1`|nothing|||outfile2:  <br>outfile1: hello hi|
|`echo hello > {infile1, infile2, infile3}`||||{infile1,: hello infile2, infile3)|
|`echo hello > infile1 > infile2 >> infile3`||||infile3: hello|
|`echo hello > infile1 >> infile2 > infile3`||||infile3: hello|
|`echo HI > outfile; hiuwhfhiwe > outfile`||||outfile: (empty)  <br>  <br>Whether the command is valid or not, it would truncate the outfile regardless.|
|`echo HI > outfile; echo BYE > outfile`||||outfile: BYE|
||||||
||||||
||||||
NEW REDIRECTION TESTS
|**Test**|Output Stream|Error Stream|Return Value|Comments|
|---|---|---|---|---|
|`cat infile1 infile2`|abcdef|||Prints both infile1 and infile2|
||||||
|`<< hdoc <infile1 cat`|abc||0|Reads heredoc but ignores the input given to it.  <br>  <br>Opens the last infile redirection.|
|`<infile1 <<hdoc cat`|(hdoc)|||Reads heredoc and ignores the infile.  <br>  <br>Opens the last infile redirection.|
|`<< hdoc cat infile1`|abc||0|Now infile1 looks like an infile, but it’s an argument. And that takes precedence over hdoc or infiles (which have the same precedence).|
||||||
|`<< hdoc <infile1 <infile2 cat`|def||0|Reads heredoc but ignores the input given to it, and ignores infile1.  <br>  <br>Counts the last infile redirection.|
|`<infile1 <infile2 cat <<hdoc`|(hdoc)|||Counts the last infile/hdoc redirection.|
|`<< hdoc <infile1 cat infile2`|def||0|The argument takes precedence, and it ignores the infile/hdoc redirections.|
|`<infile1 cat infile2 <<hdoc`|def||0|Reads (or rather processes) heredoc, but ignores both infile and hdoc contents, and prints the infile corresponding to the arg.|
||||||
|`<< hdoc1 << hdoc2 cat`|(hdoc2)||0|Reads hdoc1, then reads hdoc2, and it ignores the input in hdoc1.|
|`cat <<hdoc1 <<hdoc2`|(hdoc2)|||Same as above|
|`<< hdoc1 cat <<hdoc2`|(hdoc2)|||Same as above|
|`<<hdoc2 cat <<hdoc1`|(hdoc1)|||Processes both heredocs but prints the last one.|
||||||
|`<infile1 cat infile2`|def|||Ignores infile1|
|`<infile1 <infile2 cat`|def|||Ignores infile1|
|`<infile3 <infile2 cat <infile1`|abc|||Ignores infile3 and infile2.  <br>Always prints the last infile, no matter the position relative to the command.|
|OUTPUT REDIRECTIONS|||||
|`cat infile1 > outfile1 > outfile2`||||Creates outfile1.  <br>Creates outfile2 and concatenates output.|
||||||
|`cat infile1 > outfile1 \| cat`||||Creates outfile1 and concatenates output.  <br>The cat after the pipe doesn’t wait for input (since it receives it from the pipe).|
|`cat infile1 > outfile1 \| cat >outfile2`||||Creates outfile1 and concatenates output.  <br>Creates outfile2.|
||||||
|`cat infile1 > outfile1 \| <infile2 cat`|def|||Creates outfile1 and concatenates output.  <br>After the pipe the infile2 takes precedence over the input from the pipe.|
|`cat infile1 \| <infile2 cat`|def|||Redirects output to pipe, but infile2 takes precedence over the input from the pipe.|
|`cat infile1 \| << hdoc2 cat`|(hdoc1)|||Redirects output to pipe, but hdoc2 takes precedence over the input from the pipe.|
|`cat infile1 \| << hdoc2 <infile2 cat`|def|||Hdoc2 takes precedence over the pipe, but infile2 takes precedence over hdoc2.|
|`cat infile1 \| << hdoc2 <infile2 cat infile3`|ghi|||Hdoc2 takes precedence over the pipe, infile2 takes precedence over hdoc2, but infile3 (which is an argument) takes precedence over infile2.|
||||||
|`cat infile1 > outfile1 > outfile2`||||Creates outfile1 and outfile2 and concatenates into outfile2.|
|`cat infile1 infile2 > outfile1 > outfile2`||||Creates outfile1 and outfile2 and concatenates both, infile1 and infile2, into outfile2. (Truncates)|
|`cat infile1 infile2 >> outfile1 > outfile2`||||Same as above|
|`cat infile1 infile2 >> outfile1 >> outfile2`||||Creates outfile1 and outfile2 and concatenates both, infile1 and infile2, into outfile2. (Appends)|
||||||
||||||
||||||
||||||
  
I’m thinking how to do the unit tests for output redirection.
`cat infile1 > infile2 > infile3 && cat infile1 && cat infile2 && cat infile3`  
  
<infile1 cat >outfile1 >outfile2 ? we can then cat outfile1 and outfile2 and diff them? Yes  
  
### PRINTF tests
  
|**TEST**|**OUTPUT STREAM**|**ERROR STREAM**|**EXIT STATUS**|**COMMENTS**|
|---|---|---|---|---|
|`printf "%s %i\n" "abc""1"`|abc1 0||0||
|`printf "%s %i\n" "abc”`|abc 0||||
|`printf "%i\n”`|0||||
|`printf "%s”`|NO OUTPUT||0||
|`printf "%s\n" "abc" "def”`|abc  <br>def||||
|`printf "%s\n" "abc""def”`|abcdef||||
|`printf "%s %d\n" "abc" "def”`|abc 0|bash: printf: def: invalid number||First prints the error stream, and then the output stream.|
|`printf "%s %s\n" "abc" "def”`|abc def||||
|`printf "%s-" "abc" "def”`|abc-def-||||
||||||
||||||
||||||
||||||
### AWK tests
  
`awk '{print}' infile1 infile2 infile3`
  
### ENVP tests
  
|**TEST**|**OUTPUT STREAM**|**ERROR STREAM**|**EXIT STATUS**|**COMMENTS**|
|---|---|---|---|---|
|`export CODAM=`|None|None|0|It adds `CODAM=` to env|
|`export CODAM=\`||||It waits for the remaining input. It interprets \ as a escape character.|
|`export CODAM=\\`||||The value for CODAM=\|
|`export CODAM=' '`||||The value for CODAM would still be an empty string, regardless of the amount of new lines passed.|
|`export CODAM=' jfoewfw fwfwe '`||||The value for CODAM would be:  <br>  <br>`jfoewfw fwfwe` So it adds a space between the words.|
|tfeuer@f1r4s13:~$ export KEY="  <br>"  <br>tfeuer@f1r4s13:~$ env \| grep KEY \| cat -e|KEY= $||||
|`export VAR1="export VAR2" export $VAR2=$VAR1`|||||
||||||
||||||
||||||
  
### Signals tests
  
|**TEST**|**OUTPUT STREAM**|**ERROR STREAM**|**EXIT STATUS**|**COMMENTS**|
|---|---|---|---|---|
|cat  <br>> ctrl-\|^\Quit (core dumped)||131||
|cat  <br>> ctrl-\  <br>$ctrl-C|||131||
||||||
||||||
||||||
||||||
||||||
  
### METACHARACTERS tests
  
|**TEST**|**OUTPUT STREAM**|**ERROR STREAM**|**EXIT STATUS**|**COMMENTS**|
|---|---|---|---|---|
|`ls$(echo " ")-a`|(same as `ls -a`)||0||
|`ls$(echo -e "\v")-a`||`ls -a: command not found`|127|`\v` is NOT a metacharacter, therefore it is supposed to give an error.|
|`ls$(echo -e "\t")-a`|(same as `ls -a`)|||**Command Substitution**|
|`ls$(echo -e "\n")-a`||`ls-a: command not found`||`\n` IS a metacharacter, but in bash it’s not separating the words apparently.|
||||||
  
### Built-ins Tests
  
### EXPORT tests
  
|Test|Output Stream|Error Stream|Exit Status|Comments|
|---|---|---|---|---|
|`ls \| export $PATH`||||Shouldn’t export anything.|
|`export 123 lisandro=`||||Should print an error for 123, but it should export lisandro|
|`export lisandro`||||Should add it to the export list as just the key, but not to the env list.|
  
  
### ECHO tests
  
Another interesting command:
`echo $(< infile1)`
For this specific one = cat infile1 but if:
`echo $(< infile1 < infile2)` then no. Cat prints “def” and echo instead stops printing content (only the new line).
  
### CD tests
|Test|Output Stream|Error Stream|Exit status|Comments|
|---|---|---|---|---|
|`cd ~ /`||`bash: cd: too many arguments`|1||
|`mkdir testABC cd testABC`|||0|When changing into a directory that doesn’t come from CDPATH, there’s no output stream.|
|`unset HOME && cd`||`bash: cd: HOME not set`|1||
|`cd ~ mkdir -p testA/mini testB/mini export CDPATH=~/testB cd ~/testA cd mini`|`/home/user/testB/mini`||0|If there is a path defined in the CDPATH env var, and if the concatenation of the directory (provided as argument) and that path is valid, it will first try to chdir there. In other words, it will take precedence over PWD.|
|`cd ~ mkdir -p testA/mini testB/mini cd ~/testA export CDPATH=:~/testB`  <br>  <br>  <br>`cd mini`|||0|Now `cd` will move into `testA/mini` because the first value in `CDPATH` is `NULL`, and when that happens `cd` concatenates the given directory with `./` and tries to change in there. This happens before the next value in `CDPATH`.|
|`cd ~ mkdir -p testA/mini testB testC/mini cd ~/testA export CDPATH=~/testB:~/testC`  <br>  <br>  <br>`cd mini`|`/home/lisandro/testC/mini`||0|In this case `testB` and `testC` were included in `CDPATH`. `testB` however didn’t have the mini dir. When trying to `cd` from `testA`, it will first try `testB`, and then `testC` before trying on `testA`.|
|`cd ~ mkdir -p testA/mini testB/mini testC cd ~/testC export CDPATH=::~/testB:~/testA`  <br>  <br>  <br>`cd mini`|`/home/lisandro/testB/mini`||0||
|`cd ~`  <br>  <br>  <br>`mkdir -p testA/mini testB testC/mini`  <br>  <br>  <br>`cd ~/testA`  <br>  <br>  <br>`cd mini/../../testB/mini`|||0|For paths with double dots, cd should parse and remove:  <br>- the double dots,  <br>- the previous directory if it is not double dot or root,  <br>- and any slash characters separating the double dots from the previous or the following dir.|
|`cd home/invaliduser/../`||`bash: cd: /home/invaliduser/../: No such file or directory`|1||
|`cd ..////`|||0|After this string is trimmed, the string is empty. When you pass an empty string or a `NULL` string to `chdir` it returns -1. But `cd` doesn’t.|
|`export HOME=fiowqejfowe cd`||`bash: cd: fiowqejfowe: No such file or directory`|1||
|`cd " "`||`cd: no such file or directory:`|1||
|`cd -`|Prints `$OLDPWD`||0|Equivalent to `$OLDPWD`|
|`unset OLDPWD cd -`||`bash: cd: OLDPWD not set`|1||
|`cd --`|||0|Equivalent to `$HOME`|
|`unset HOME cd --`||`bash: cd: HOME not set`|1||
|`unset PWD cd cd -`||`bash: cd: OLDPWD not set`|1||
|`cd ~ unset PWD cd ~/Github/mini cd -`||`bash: cd: OLDPWD not set`|1|Apparently when unsetting `PWD`, then changing directories, and then trying to use `cd -` that doesn’t work.  <br>It seems  <br>`OLDPWD` is linked to the value of `PWD` in the environment.|
|`export PWD=/home/ cd ~ cd -`|`/home/`||0|The previous comment is confirmed:  <br>  <br>`OLDPWD` is taken from the env `PWD` variable.  <br>However after using it, the  <br>`PWD` value gets updated with the function value (`getcwd`).|
|`export PWD=/home//// cd ~ cd -`|`/home////`||0|Similar to the previous, but also notice that the path doesn’t get trimmed.|
  
### Command substitution tests - Not needed
  
### Env var expansion
|**TEST**|**OUTPUT STREAM**|**ERROR STREAM**|**EXIT STATUS**|**COMMENTS**|
|---|---|---|---|---|
|VAR="cat\|"; <Makefile $VAR cat >>out||cat: \|: No such file or directory  <br>cat: cat: No such file or directory|||
|echo "dd$TERMff$TERMpp”|dd||||
||||||
|echo $TERMdd||||Should not expand.|
|echo $TERMdd$HOME|/home/tfeuer||||
||||||
||||||
|tfeuer@f0r3s17:~$ cat infile && cat infile2 \| cat infile3  <br>cat: infile: No such file or directory  <br>I'm 3  <br>tfeuer@f0r3s17:~$ cat infile && (cat infile2 \| cat infile3)  <br>cat: infile: No such file or directory|||||
||||||
### Arithmetic expansion tests - Not needed but good for learning
|**TEST**|**OUTPUT STREAM**|**ERROR STREAM**|**EXIT STATUS**|**COMMENTS**|
|---|---|---|---|---|
|`echo ((3+2))`||bash: syntax error near unexpected token `('|2||
|`echo (((3+2)))`||bash: syntax error near unexpected token `3+2'|2||
|`echo $((3+2))`|5||0||
||||||
||||||
||||||
||||||
  
### Quoting tests
f0r3s17% ./a.out "abc""def"  
2%  
  
  
---
  

> [!important] **More complex tests**
  
TEST 1
Have a file with a DEL ascii char. And then cat -e the file, which should print: ^?$
AR="cat|"; <Makefile $VAR cat >>out  
bash: cat|: command not found  
Arithmetic expansion tests - Not needed but good for learning
Arithmetic expansion tests - Not needed but good for learning
  
|   |   |   |   |
|---|---|---|---|
|PRECEDENCE||||
|true \| true && false|1|true \| (true && false)|1|
|true \| false && true|1|||
|true \| true && true|1|||
|true \| false && false|1|||
|false \| true && false|1,2,3|false \| (true && false)|1,2,3|
|false \| false && true|1,2|false \| (false && true)|1,2|
|false \| true && true|1,2,3|false \| (true && true)||
|false \| false && false|1,2|||
|||||
|true && true \| true|1|||
|true && true \| false|1|||
|true && false \| true|1|||
|true && false \| false|1|||
|false && true \| true|1,3|||
|false && true \| false|1,3|||
|false && false \| true|1,3|||
|false && false \| false|1,3|||
|||||
|||||
|||||
  
|   |   |   |
|---|---|---|
|Case|PRECEDENCE|Expected output|
|1|true && (true && true) && true||
|2|true && (true && true) && false||
|3|||
||||
||||
||||
||||
||||
||||
||||