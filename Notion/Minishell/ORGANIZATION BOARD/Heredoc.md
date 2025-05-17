---
Status: Not started
---
`<< $HOME`
  
All heredocs get read unless there is a cntl-C
cntl-D is sending a EOF to the input
Conclusion : we need to open all heredocs
writing files in/tmp/
  
  
Delimiter not ok:
  
something beginning with a |
something beginning with a &
dito ;  
\ whatever comes after that becomes the delimiter  
redirections  
(
)
Delimiters ok:
anything quoted, DQ and SQ
  
<<EOF; seg faultEOF
<<& now: syntax error should say &