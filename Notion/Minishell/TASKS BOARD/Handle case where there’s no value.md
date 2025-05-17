---
Status: Not started
Parent item:
  - "[[Environment]]"
---
  
While creating a new environment node, there are 2 cases where there could be no value:
- When for instance we export `KEY=""` or `KEY=` . These are the same.
- And the other case is when we export `KEY` .
  
When there’s just a key, the command `env` doesn’t list that node, but `export` does.
Therefore to differentiate between these cases in the environment nodes, we can say in the first case that the environment has both a `char *key` and a `char *keyvalue` and the value can either be `char *value = NULL` or `char *value = "\0"` ; and in the second case we can have that only `char *key` has a value, and the other 2 strings don’t. This should make it easier to print the right nodes when calling `env` and `export` .