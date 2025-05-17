---
Status: In progress
Parent item:
  - "[[Built-ins]]"
---
  
|ARGS|STDERR|STDERR|EXIT CODE|Does it exit?|
|---|---|---|---|---|
||exit||0|Yes|
|--|exit||0|Yes|
|255|exit|-|255|Yes|
|256|exit|-|0|Yes|
|10 20|exit|bash: exit: too many arguments|1|No|
|10 jfiewof|exit|bash: exit: too many arguments|1|No|
|uhfiwhefiwe 10|exit|bash: exit: uhfiwhefiwe: numeric argument required|2|Yes|
|10foiewhfw|exit|bash: exit: 10foiewhfw: numeric argument required|2|Yes|
|10hdiwq 3|exit|bash: exit: 10hdiwq: numeric argument required|2|Yes|
|-- 200|exit|-|200|Yes|
|20 --|exit|bash: exit: too many arguments|1|No|
|-4|exit|-|252|Yes|
|-1|exit||255|Yes|
|exit \| cat|exit||0|No (it exits the child)|
|(exit)|||0|No (it exits the subshell)|
||||||
||||||