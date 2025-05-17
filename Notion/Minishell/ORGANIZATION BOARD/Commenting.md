---
Status: Not started
---
This is for learning about Doxygen’s commenting style.
  
---
  
A block comment goes on top of a function declaration. I think it looks nicer in a header, but that’s debatable.
Here’s a link to special ‘commands’:  
  
[https://www.doxygen.nl/manual/commands.html#cmdbrief](https://www.doxygen.nl/manual/commands.html#cmdbrief)
But to list some:
- `@brief` : This is to give a briefing of the function’s purpose.
- `@param` : This is to list parameters. One `@param` for each param.
- `@return` : To indicate the return value of the function.
  
Example:
```C
/**
 * @brief The isalnum() function tests for any character
 * for which isalpha() or isdigit() is true.
 *
 * @param c The character to test.
 * @return 1 if 'c' is an alphanumerical character, 0 otherwise.
 */
int	ft_isalnum(int c)
{
	return (ft_isdigit(c) || ft_isalpha(c));
}
```