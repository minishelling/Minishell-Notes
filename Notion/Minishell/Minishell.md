> [!important] **Project page:**
> 
> [https://projects.intra.u42.fr/42cursus-minishell/lprieri](https://projects.intra.42.fr/42cursus-minishell/lprieri)

> [!important] **GitHub Page**
> 
> : `[https://github.com/minishelling/minishell](https://github.com/minishelling/minishell)`

> [!important] **Repository SSH Link (for cloning)**
> 
> : `git@github.com:minishelling/minishell.git`

> [!important] **Codam’s Repository**
> 
> : `git@vogsphere-v2.codam.nl:vogsphere/intra-uuid-817a162b-08e4-4b37-bec2-6fe6c68c5101-5700185`

> [!important] Testing with:
> 
> [https://github.com/LucasKuhn/minishell_tester.git](https://github.com/LucasKuhn/minishell_tester.git)

> [!important] **Deadline**
> 
> : 16th of November
  
#### ORGANIZATION BOARD
| Name                                                        | Assign | Date | Status      |
| ----------------------------------------------------------- | ------ | ---- | ----------- |
| [[Staging]]                                                 |        |      | Not started |
| [[Unit tests]]                                              |        |      | Not started |
| [[Subject]]                                                 |        |      | Not started |
| [[Parsing - Shell grammar]]                                 |        |      | Not started |
| [[Expansions]]                                              |        |      | Not started |
| [[Allowed functions]]                                       |        |      | Not started |
| [[Possible structures]]                                     |        |      | Not started |
| [[Tester resources]]                                        |        |      | Not started |
| [[Where to look for]]                                       |        |      | Not started |
| [[Signals]]                                                 |        |      | Not started |
| [[Syntax]]                                                  |        |      | Not started |
| [[Commenting]]                                              |        |      | Not started |
| [[Makefile]]                                                |        |      | Not started |
| [[Minishell/ORGANIZATION BOARD/Execution\|Execution]]       |        |      | Not started |
| [[Git branches]]                                            |        |      | Not started |
| [[Input Syntax]]                                            |        |      | Not started |
| [[Minishell repositories]]                                  |        |      | Not started |
| [[Minishell/ORGANIZATION BOARD/Heredoc\|Heredoc]]           |        |      | Not started |
| [[Bonus]]                                                   |        |      | Not started |
| [[Minishell/ORGANIZATION BOARD/Redirections\|Redirections]] |        |      | Not started |
| [[To discuss with Lisandro]]                                |        |      | Not started |
| [[Memory allocated]]                                        |        |      | Not started |
| [[Minishell/ORGANIZATION BOARD/README\|README]]             |        |      | Not started |
| [[Untitled]]                                                |        |      | Not started |
  
  
  
#### TASKS BOARD
|Name|Assign|Status|
|---|---|---|
|[[Built-ins]]|Lisandro|In progress|
|[[Environment]]|LisandroTTamar|Done|
|[[Take care of syntax error- . If there are 3 - it’s considered to be a pipe, if it is more it is considered to be an or]]|TTamar|Done|
|[[Handle $]]||Not started|
|[[echo -e 't’ seg fault]]||Done|
|[[echo -e t]]||Done|
|[[Execution]]|Lisandro|In progress|
|[[CD]]||Done|
|[[Take care of && becoming a word token]]||Done|
|[[Echo]]||Done|
|[[PWD]]||Done|
|[[Export]]||Done|
|[[Unset]]||Done|
|[[Env]]||Done|
|[[Exit]]||In progress|
|[[Handle case where there’s no value]]||Not started|
|[[Don’t print backslashes]]||Done|
|[[Add “declare -x” at the beginning of each line when printing the export list]]||Done|
|[[PWD, OLDPWD and _ still need to be updated at the end of execution]]||Not started|
|[[Implement Signals]]||In progress|
|[[Parse for invalid identifiers]]||Done|
|[[Interactive mode]]||Done|
|[[Non-interactive mode]]||Done|
|[[Heredoc]]||Not started|
|[[Redirections]]||In progress|
|[[Exit execution if there’s a redir with no permission]]||Not started|
|[[Handle execution permissions]]||Not started|
|[[If a directory in PATH doesn’t have execution permissions, it acts as if the cmd wasn’t found]]||Not started|
|[[Handle $_]]||Not started|
|[[Handle relative paths to commands]]||Not started|
|[[Shell levels]]||Not started|
|[[cmd_name as path]]||Not started|
  
  
  
---
### Bash resources:
[https://www.man7.org/linux/man-pages/man1/bash.1.html](https://www.man7.org/linux/man-pages/man1/bash.1.html)
[https://blog.devgenius.io/lets-build-a-linux-shell-part-i-954c95911501](https://blog.devgenius.io/lets-build-a-linux-shell-part-i-954c95911501)
[https://tldp.org/LDP/Bash-Beginners-Guide/html/index.html](https://tldp.org/LDP/Bash-Beginners-Guide/html/index.html)
[https://tldp.org/HOWTO/Bash-Prog-Intro-HOWTO.html](https://tldp.org/HOWTO/Bash-Prog-Intro-HOWTO.html)
[https://tldp.org/LDP/abs/html/index.html](https://tldp.org/LDP/abs/html/index.html)
[https://www.udemy.com/course/bash-scripting/](https://www.udemy.com/course/bash-scripting/)
[https://www.gnu.org/software/bash/manual/bash.html](https://www.gnu.org/software/bash/manual/bash.html)
[https://github.com/bminor/bash](https://github.com/bminor/bash)
  
---
### Github resources:
Git manual: [https://git-scm.com/](https://git-scm.com/)
[https://www.udemy.com/course/git-and-github-bootcamp/learn/lecture/24911528#overview](https://www.udemy.com/course/git-and-github-bootcamp/learn/lecture/24911528#overview)
  
### Problems seen in other people’s evaluations
[https://projects.intra.42.fr/42cursus-minishell/scale_teams](https://projects.intra.42.fr/42cursus-minishell/scale_teams)
- Yesim and Fra, very nice work so far! Unfortunately some little mistakes are left. Good luck!  
    Crash case: /bin/ls/asdfasf Minor things: - SHLVL increases with steps of 2 echo $TERM_PROGRAM -> expander / parser in combination with _ gives problem echo $_ From root: cd sources ../minishell -> Should run minishell from a relative path and hence increase SHLVL unset PATH PATH=/Users/yzaim/.brew/bin:/usr/local/bin:/usr/bin:/bin:/usr/sbin:/sbin:/usr/local/munki:/opt/X11/bin we would expect commands like ls to work with env path operators in quotes should be treated literally: echo '>' hello2  
    
- signal cntrl D, segfault when input is spaces. Tabs also gives a weird response
- unfortunate seg fault when utilizing multiple heredocs, check relative paths, especially when the PATH variable is unset. other things to know about is the export differences and be able to defend that choice
- -nnnn doesn't work but is justified xD. CTRL+\ is weird. Heredoc twice works weird with signals and termination, unsure if in scope of the project. Something weird with the prompt. Fix single quotes. Fix the parsing so that it works with the weird export TEST="l -s" l$TEST. Also fix the prompt and the piping with waiting. Also nice that the weird mkdir a; mkdir b; cd a/b; rm -r ../../a works
- issues found: echo -nnn -n: are not catcing export without arguments does not sort the env list cd is not updating PWD and OLDPWD OLDPWD is there in env list from the launch of the minishell spaces before the commands are stopping command from execution
- some really small things like prompt missing with a file without \n at the end of it, or prompting when no prompt should be shown, cat | cat | ls is not fully as in bash as it never quits.
- Thanks for the explanation of all the functionalities. Unfortunately there were still some problems: • Test only spaces or tabs. • Ctrl-\ in a prompt after you wrote some stuff should not do anything. • minishell$$asf • [1] 59310 segmentation fault ./minishell echo $$. (Maybe not required) CTRL+C should not clear the command, just ignore it and give a new prompt minishell$echo "asdfasdf" | cat | cat | cat
- c: exit -300 > exit status of 212 c: unset a c > should unset both env vars c heredoc > doesnt stop on delimiter Nice project, unfortunately we found some mistakes. We logged them, you should do fine next time! Good luck!
- Hello Adri and Mirjam, Overall, nice work! Unfortunately there are a few errors left: e.g with relative path and redirections. It seems like something stays in the buffer between calls. Also, I'm not too sure about the construction with functions maintain_prompt and reset_data calling each other. Better luck next time! [minishell]: touch "vreemd" [minishell]: rm vreemd [minishell]: ls ———— [minishell]: gcc test.c [minishell]: ls clang: error: no such file or directory: 'ls' ———— ➜ eval_ch git:(mirjam) ✗ ./minishell [minishell]: ./a.out ———— [minishell]: ./wc -> should not execute wc from path bash-3.2$ ./wc bash: ./wc: No such file or directory Same with /wc ———— echo "hello world" >> file echo "hello world" >> file cat file
- Unfortunately, we found a few minor issues which prevent me from giving you full marks. The return value of a signaled process needs to be set using WTERMSIG, export and unset need to be able to set multiple variables in one go, and it appears as if variables and quotes do not always play nice together (echo $ vs echo "$", echo "$USER'").
- ok so nice minishell, some of the edge cases that often fail have been handled. However, there are some errors. redirection permissions are wrong, "echo "bip | bip ; coyotte > <"" fails, " export TEST=" HALLO? XDDD 'we zijn hier' misschien, athans... "" and then "echo $TEST" fails as well(because of too early expansion? idk). when there is no command found when unset PATH, should say so. Otherwise, good work, good stuff. Be sure to fix it, be sure to test them thoroughly, enjoy your vacation. Really nice image at startup and "sin"tax error.
- Well done! I'm very impressed by the structure of your repo and code, it looks very clean and organized. We've talked about lots of different details about bash and its quirks, and which ones are and aren't worth implementing in a project like minishell. In the end, we found out whitespace does not get interpreted literally inside quotes, and there was a minor concern about heredocs on different commands overriding each other, but to me, you have more than done your job here. Keep up the good work!
- unfortunate seg fault when utilizing multiple heredocs, check relative paths, especially when the PATH variable is unset.  
    other things to know about is the export differences and be able to defend that choice, Good luck with the fixes, shouldn't take too long!  
    
- echo -n option, exit a b, echo "$9PWDlol", export test=, export 9tmp=test, export lol  
    Amazing job on your minishell! The subject is incredibly unclear so there are obviously very small edge cases you missed, yet the whole project is amazing and very well executed. Great job and I'm glad I got to be the last evaller for it. Good luck on your next projects <3  
    
- Test only spaces or tabs: tabs is not like on bash but I feel like this is not exactly shell auto-complete.  
    There was a a leak with echo + `` but it should not be tested anyway, all good. Also we sorted it out on the go.  
    Auto complete cd with too many options + ctrl-c before seeing the whole list of options gives a bug -> random characters printed + cant got to the prompt for a while.  
    Good work, its a very difficult one. good luck on the next one!  
    
- Super friendly and explained everything well, just had a slight issue with export alone, but was able to fix it in 30 sec. Super impressive.
- This are some small details to fix:  
    isspace  
    flags for echo  
    echo "'$USER'" '"$USER"'  
    export cccc!="asdasd"  
    printf hereheredoc.txt  
    Otherwise looks good! Good luck on next evals!  
    
- great Minishell couple of things. heredoc doesn't support ctrl-D signal (EOF) also signals like CTRL-/ are not turned back on. for the use of the command. (./) this means that you enforce to check current dir first not $PATH. also you normally check current working directory first but that needs to be $PATH (this has been tested by created a program like ls) if you want you can do the foodo magic exit edge case (exit 234 234 {Doesn't exit]) but further more great work and goodluck on the next eval! Probs on finishing it in 1.5 Months crazy :p
- Some things are not quite there yet, but great job! check the env stuff and the single quotes with same env '$USER'$USER
- local variable $HOME$HOME doesn't work correctly
- execve(get_path(all_paths(pg->envp), arg[0]),  
    arg, list_to_arr(pg->envp));  
    If execve fails you lose the pointers of the arrays inside the execve function, this leads to memory leaks in a very specific case. —> Really nice eval, everything was checked with fsanitize. We didn't know that if execve fails it would not free all allocated memory, so there was a leak for execution file with executable rights. In a future, we promise to handle this issue properly. Thank you so much!  
    
- Good job on the project, I did my best to test your minishell and found a few minor mistakes. UNSET has a few problems when unsetting too many variables. Also when I started changing $PATH your shell didn't correctly behave as it should.
- Crashed at "export HOME=" after "unset HOME". Good luck :D
- ok so nice minishell, some of the edge cases that often fail have been handled. However, there are some errors. redirection permissions are wrong, "echo "bip | bip ; coyotte > <"" fails, " export TEST=" HALLO? XDDD 'we zijn hier' misschien, athans... "" and then "echo $TEST" fails as well(because of too early expansion? idk). when there is no command found when unset PATH, should say so. Otherwise, good work, good stuff.
- Test only spaces or tabs: tabs is not like on bash but I feel like this is not exactly shell auto-complete. Auto complete cd with too many options + ctrl-c before seeing the whole list of options gives a bug -> random characters printed + cant got to the prompt for a while. Test only spaces or tabs: tabs is not like on bash but I feel like this is not exactly shell auto-complete.
- copy paste gives weird prompts. unset path errors local binary. echo -n segfault (and some minor things with the -n flag edge cases). export doesnt differentiate between emtpy string and non set variable. no truncate in file redirects. Heredoc requirtes expansion (works slightly different from normal expansion). segfault on only "  
    <nonexist". Consider more static functions, makes code faster where applicable.  
    
- echo -n option, exit a b, echo "$9PWDlol", export test=, export 9tmp=test, export lol
- Double free for exit builtin, Signal in child process : ctrl+/, cat | cat | ls is sadly didn't go as we wished;(. Export should updates even there is no value. But not in the env list. No LEAKS! very remarkable. except cat | cat | ls, everything will be easy to fix. Good luck!
- The minishell looks good, all cases work with one small exception < okqoqk < okwqdo kw | ls >> out1 not writing
- couldn't explain the problem of copying ""cat << test.rxt >> test.tst" it was keeping a part of the string in memory
- Great work! Your minishell works good. There's one case where the Variable doesnt expand. ("$USER"$USER) Also check if you want to implement the SHLLVL (+1 when starting the shell). And if you want to update the OLDPWD when using cd -. Good luck with the other evals!
- Project and code looks good, some really interesting approaches, such as using lookup tables for lexing and validating user input. Everything worked as intended, even the "weirder" things in bash. Learned about the "lambda operator" as well.
- cat | cat | cat | ls, the pipes don't close perfectly  
    echo -nnnn hdada, the -nn shouldnt be there and there shouldt be a new_line  
    otherwise a very good minishell, no leaks, global was good. good luck guys! (mademir)  
    
- apsolute paths as /bin/ls doesn't work, and signals isn't handled correctly. should be easy fixes. (vincent and abbas’s)
- echo "$iets" geeft een extra spatie  
    grep hello > ctrl \ exit niet  
    printf en ft_printf niet door elkaar halen  
    oh en al je files pushen gl  
    
- Program leaks when given an absolute path to a file that doesn't exist,  
    When expanding environment variables it doesn't only interpret alphanumeric characters and underscores but also for example colons e.g.  
    `export PATH=$PATH:$PWD` sets PATH to PWD instead of $PATH:$PWD,  
      
    `echo '"'$USER'"'`didn't expand to `"username"` as it should,  
    ^\ didn't send sigterm to the child process and pressing ^C in a child minishell printed the prompts multiple times,  
    technically the global is not the signal number, and fork is not checked for errors.  
    Other than that everything worked as expected. Good luck!  
    
[[Command substitution tests - Not needed]]