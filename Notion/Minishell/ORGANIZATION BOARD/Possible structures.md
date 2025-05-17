---
Status: Not started
---
  
```C
typedef struct s_env
{
char             *keyvalue;
char             *key;
char             *value;
struct s_env     *next;
}  t_env;
```
```C
typedef enum e_token_type
{
TOKEN_IDENTIFIER,  
TOKEN_LESS, 
TOKEN_GREATER,
TOKEN_DOUBLE_LESS, 
TOKEN_DOUBLE_GREATER, 
TOKEN_PIPE, 
TOKEN_OPEN_PAREN,
TOKEN_CLOSE_PAREN,
TOKEN_AND, 
TOKEN_OR, 
TOKEN_NEWLINE,
TOKEN_COUNT
}  t_token_type;
```
  
```C
typedef struct s_token
{
t_token_type		type;
char				*value;
struct s_token		*next;
struct s_token		*prev;
}	t_token;
```
  
```C
typedef struct s_minishell
{
char			*line;
t_token		*tokens;
}	t_minishell;
```
  
```C
typedef struct s_node
{
t_node_type			type;
t_io_node			*io_list;
char				*args;
char				**expanded_args;
struct s_node		*left;
struct s_node		*right;
}	t_node;
```
  
```C
typedef enum e_node_type
{
N_PIPE,
N_AND,
N_OR,
N_CMD
}	t_node_type;
```