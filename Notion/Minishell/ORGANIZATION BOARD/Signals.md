---
Status: Not started
---
|Signal|Status|Action|Exit code|
|---|---|---|---|
|ctrl-C|interactive mode|In interactive mode, if `Ctrl + C` is pressed, it interrupts the currently running command or operation. This may include stopping a command that is waiting for user input. The shell remains active, and the exit code for the interrupted command is `130`. The user is returned to the prompt.|GPT says 0  <br>I say 1|
|ctrl-C|during a running command|When a command is running in the foreground and `Ctrl + C` is pressed, it sends the `SIGINT` signal to the process, causing it to terminate. The shell does not exit; it continues running. The exit code for the terminated command is`130`. The user will see the prompt after the command is interrupted.|130|
|ctrl-C|in heredoc|When `Ctrl + C` is pressed while typing inside a heredoc, it terminates the heredoc input immediately and stops the current command. The shell aborts the command associated with the heredoc, but it does not exit the shell itself. The exit code for the command that was running or waiting for heredoc input is `130` (calculated as `128 + 2`, where `2` is the signal number for `SIGINT`). After pressing `Ctrl + C`, a new prompt is displayed.|130|
|||||
|ctrl-D|interactive mode|In an **interactive shell**, pressing `Ctrl + D` signals the end of input (EOF) as well. If the command line is empty, `Ctrl + D` causes the shell to exit. If there is text in the command line, it simply closes the current input and attempts to execute whatever has been typed so far. You can implement custom behavior to prevent the shell from exiting immediately by checking whether the input is empty before allowing the shell to close. If no input is present, you print an "exit" message and exit with code `0` to signify successful closure of the shell.|0|
|ctrl-D|waiting for user input|When using `Ctrl + D` during the execution of a command like `cat` or another process reading from standard input, it signals the end of input. Commands like `cat` interpret this as no more data being available and finish their execution normally, exiting with code `0` (success), or another code depending on the command's logic. The `^D` symbol is not typically shown when sending EOF this way, and no special exit code handling is required.|0|
|ctrl-D|in heredoc|In the case of a **heredoc (**`**<<**`**)**, `Ctrl + D` indicates the end of the input for the heredoc. The shell treats this as the end of the heredoc content, closes it, and proceeds to execute the command that requires the input. No special handling or exit code is usually needed, and the heredoc content is passed to the command as expected.|0|
|||||
|ctrl-\|interactive mode|When `Ctrl + \` is pressed in an interactive shell with no running process, the shell itself will terminate, and the exit code will again be `131`. The shell will print a message like `Quit (core dumped)` if a core dump is generated, but this depends on the system settings and whether core dumps are enabled.|131|
|ctrl-\|waiting for user input|If you're running a command that is reading from the standard input, pressing `Ctrl + \` sends the `SIGQUIT` signal to that process. The process will terminate and, unlike `Ctrl + C`, it might also generate a core dump. The exit code is also `131`. This can be useful for debugging, as a core dump provides more detailed information about the state of the process at the time it was terminated|131|
|ctrl-\|in heredoc|If `Ctrl + \` is pressed while you're typing the heredoc input, it immediately interrupts the input and terminates the shell process that is waiting for the heredoc input. This behavior is more forceful than `Ctrl + C`, as `SIGQUIT` typically causes a process to terminate and create a core dump. The exit code of the shell process in this case is `131`, which represents `128 + 3` (128 is the standard signal offset, and 3 is the signal number for `SIGQUIT`)|131|
|||||
==SIGINT==
==void handle_sigint(int sig) //interactive mode  
{  
(void)sig;  
rl_replace_line("", 0);  
write(1, "\n", 1); // Display ^C and return to a new line  
rl_forced_update_display(); // Redisplay the prompt  
}  
==
  
==For a running command (e.g.,== ==`cat`====,== ==`sleep`====), we don't need a custom handler because the default behavior of== ==`SIGINT`== ==works fine:  
The process is terminated.  
The shell displays====`^C`====.  
Exit code is set to  
====`130`====.==
  
==void handle_sigint_heredoc(int sig) //heredoc mode  
{  
(void)sig;  
write(1, "\n", 1); // Move to a new line  
// Optionally clean up heredoc-specific resources here.  
// Set a flag or exit heredoc mode  
}  
==
==QUIT==
==EOF==
  
  
man 7 signal for a list of what each signal does