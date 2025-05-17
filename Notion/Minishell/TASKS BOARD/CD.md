---
Status: Done
Parent item:
  - "[[Built-ins]]"
---
# `cd`

> [!important] With only a relative path or an absolute path.
  
**Manual**: [https://man7.org/linux/man-pages/man1/cd.1p.html](https://man7.org/linux/man-pages/man1/cd.1p.html)
## Overview
  
An **absolute path** is a full path from the root directory ‘ `/` ’ to the target directory.
A **relative path** specifies the location of a directory in relation to the current working directory. It does not begin with a ‘ `/` ’.
  
`cd` has interaction with `HOME`, `PWD`, `OLDPWD` and `CDPATH` environment variables.
It also interacts with the functions: `chdir` && `getcwd`.
  
The manual has a detailed step by step guide into how the execution happens.
  
### Implementations:
  
- Current code of execute_cd
    
    ```C
    t_ecode	execute_cd(t_env *env, char *directory)
    {
    	char	*curpath;
    	char	*cwd;
    	void	*temp;
    	int		i;
    	int		no_cdpath_flag;
    
    	if (!directory) //First step
    	{
    		temp = env_find_node("HOME");
    		if (!temp || !temp.key)
    			return (ERR_CD_NO_HOME);
    		else //Second step
    		{
    			if (chdir(temp.key) == -1)
    				return (ERR_CD_CHDIR_ERROR); //Errno is set.
    		}	
    	}
    	else if (directory[0] == '/' || directory[0] == '.'
    		|| (directory[0] == '.' && directory[1] == '.')) //Steps 3 and 4
    		curpath = directory;
    	else //Step 5 - Which was wrong, because it has to set and then proceed to step 6 outside of the else statement.
    	{
    		temp = (t_env *)env_find_node("CDPATH"); //Look for CDPATH in env. Retrieve the node. - Testing the use of (void *).
    		i = 0;
    		while (i < env_count_values(env, "CDPATH")) //A value can be an empty string. 
    		{
    			curpath = ft_strdup(temp.values[i]); //Dups so I can use ft_strjoin_fs1 or simply ft_free on curpath. - Protect malloc.
    			if (curpath == NULL && no_cdpath_flag == 0) //If the string is empty or is NULL (strdup takes care of handling empty strings, converting them to NULL),
    			{//Then execute step 4
    				curpath = ft_strjoin("./", directory); //Append current directory and the directory provided.
    				if (access(curpath, F_OK) == -1) //Why would I replace access with a dir function?
    				{
    					ft_free(&curpath);
    					i++;
    					continue ;
    				}
    				if (chdir(curpath) == -1)
    				{
    					ft_free(&curpath); //Repeated functionality... How can I optimize this?
    					i++;
    					continue ;
    				}
    				no_cdpath_flag = 1; //Flags this occurrence so it doesn't do it again.
    			}
    			else if (curpath == NULL && no_cdpath_flag == 1) //New addition that I forgot in the initial implementation.
    			{
    				i++;
    				continue ;
    			}
    			if (curpath[len -1] != '/') //Continues execution when the node found in CDPATH is true.
    				curpath = ft_strjoin_fs1(curpath, "/"); //Appends a slash if the path in CDPATH didn't end with one. - Protect the malloc.
    			curpath = ft_strjoin_fs1(curpath, directory); //Appends the path in CDPATH with the directory.
    			if (access(curpath, F_OK) == -1) //Checks if it is a valid dir.
    			{
    				ft_free(&curpath);
    				i++;
    				continue ;
    			}
    			if (chdir(curpath) == -1) //Checks if changing to that dir fails.
    			{
    				ft_free(&curpath);
    				i++;
    				continue ;
    			}
    			else //Forgot to add the action for when it succeeds. So adding it now:
    			{
    				ft_free(&curpath);
    				//Update pwd and oldpwd.
    				return (SUCCESS);
    			}
    			i++;
    		}
    	}
    	curpath = directory; //Step 6
    	if (shell->pwd[len - 1] != '/') //Here I can use either PWD from the shell struct (not from the env), or the getcwd command.
    		shell->pwd = ft_strjoin_fs1(shell->pwd, "/"); //Protect mallocs.
    
    	//Shouldn't I be concatenating PWD with curpath here?
    
    	curpath = cd_trim_curpath(shell, &curpath); //Return a trimmed path that is ready to be used.
    	// Check if (ft_strlen(curpath) + 1 > PATH_MAX)
    	status = chdir(curpath);
    	free(curpath); //The end of the journey.
    	if (status == -1) //Status could be called ret, becasue after all it has to return a value.
    		return (ERR_CD_CHDIR_ERROR);
    	return (SUCCESS);
    }
    ```
    
### Trim curpath - Option A
- Create an array with all the directories using `ft_split(curpath, ‘/’)`
- Create a binary tree with all the directories and free the array.
- Iterate through the directories.
    - If it is a dot, skip it.
    - If it is a double dot, check the curpath up to the previous dir. If it is valid, then remove it (remove it from the list and create a new catted curpath).
        
        - figure out how to remove the node: to store it provisionally in a different pointer, to set head to the previous and next nodes correctly.
        
          
        
  
  
  
### Trim curpath - Option B
- Create array with dirs.
- Iterate through array. If it is a valid dir to add the the list, then add it.
- NOTES
    - `cd` by itself should go into HOME.
        - If HOME is unset then it should print an error message:
            - `bash: cd: HOME not set`
            - The exit code is 1.
            - and it shouldn’t segfault.
    - It updates the PWD and OLDPWD env variables
        - PWD or OLDPWD when changing into a new directory they get reassigned.
            - If they were unset then they get a new value.
            - If they weren’t unset then their value gets reassigned.
        - If it is an invalid path, either because the directory is invalid or because home is unset, then these variables don’t get updated.
    - When given an invalid path the exit code is 1 and the error message is `bash: cd: [path]: No such file or directory`
    - If the first component of the directory operand is dot or dot-dot, set **curpath** to the _directory_ operand and skip the next step.
    - If the CDPATH env variable is defined and is not null:
        - Starting with the first pathname in the <colon>-separated pathnames of CDPATH if the pathname is non-null, test if the concatenation of that pathname, a <slash> character if that pathname did not end with a <slash> character, and the directory operand names a directory. If the pathname is null, test if the concatenation of dot, a <slash> character, and the operand names a directory. In either case, if the resulting string names an existing directory, set **curpath** to that string.
    - If curpath does not begin with a <slash> character, set curpath to the string formed by the concatenation of the value of PWD, a <slash> character if the value of PWD did not end with a <slash> character, and curpath.
- Old Pseudocode
    
    ```C
    	
    t_ecode execute_cd (char *directory) {
    temp = NULL;
    curpath = NULL;
    
    if (!directory)
    {
    	if (!home)
    		NO_HOME_ERR_MSG && return;
    	chdir(home);
    	check chdir status;
    	return;
    }
    else if (dir[0] == '/' || dir[0] == '.' || (dir[0] == '.' && dir[1] == '..'))
    	curpath = directory;
    else
    {
    	while (i < keys_in_CDPATH)
    	{
    		curpath = cdpath[i];
    		if (curpath == NULL && no_path_flag == 0)
    		{
    			curpath = ft_strjoin("./", directory);
    			if (access(curpath, F_OK) == -1)
    			{
    				free(curpath);
    				continue ;
    			}
    			if (chdir(curpath) == -1)
    			{
    				free(curpath);
    				continue;
    			}
    			no_path_flag = 1;
    		}
    		i++;
    	}
    	curpath = directory;
    	
    	//In this step #7, pwd refers not to the environment variable
    	//but to the function getcwd (or bash's pwd command).
    	if (curpath[0] != '/')
    	{
    		cwd = getcwd(cwd, PATH_MAX);
    		if (cwd != NULL)
    		{
    			if (cwd[len - 1] != '/')
    			{
    				temp = ft_strjoin(cwd, "/");
    				curpath = ft_strjoin_fs1(temp, curpath);
    			}
    			else
    				curpath = ft_strjoin(cwd, curpath);
    	}
    	curpath = parse_curpath(&curpath); //As in step 8
    	//Skipping step 9 - At least for now.
    	//It would be interesting to make a test for that.
    	status = chdir(curpath);
    	if (status == -1)
    		return err_msg;
    	env.pwd = getcwd(env.pwd, PATH_MAX);
    	env.oldwd = cwd;
    	
    	
    	
    		
    			 
    				
    			
    	
    ```
    
### Edge cases
### Removing parent directory and `cd`ing into it
When you remove a parent directory in bash, and try to cd into it (from one of the subdirectories), at first bash tells you:
`cd: error retrieving current directory: getcwd: cannot access parent directories: No such file or directory`