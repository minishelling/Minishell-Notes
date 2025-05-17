---
Status: Not started
---
- Files:
    
    We would have the following files. `nrp`, `nwp`, `nxp` stands for no reading, no writing and no execution permissions respectively. The reason why outfiles have some content is to check if the contents are overwritten or not.
    
    - infile: 123
    - infile_nrp: 123
    - infile_nwp: 123
    - infile_nxp: 123
    - outfile: abc
    - outfile_nrp: abc
    - outfile_nwp: abc
    - outfile_nxp: abc
  
- |**Test**|Output Stream|Output Destination|Error Stream|Exit Code|Comments|
    |---|---|---|---|---|---|
    |INFILES||||||
    |`<infile_nrp cat`|||`bash: infile_nrp: Permission denied`|1||
    |`<infile_nrp <infile cat`|||bash: infile_nrp: Permission denied|1||
    |`<infile <infile_nrp cat`|||bash: infile_nrp: Permission denied|1||
    |OUTFILES||||||
    |`<infile cat >outfile_nwp`||||||
    |`<infile cat >outfile_nwp >outfile`|||||It doesn’t remove the contents of any outfile.|
    |`<infile cat >outfile >outfile_nwp`|||||It truncates the contents any outfile before outfile_nwp.|
    |`<infile cat >outfile_nxp >outfile_nrp >outfile`|123|outfile||0|It truncates every file, but writes only to the last outfile.|
    |||||||
    |||||||
    |||||||
    |BOTH||||||
    |||||||
    |ORDER MIXED UP||||||
    |HEREDOCS||||||
    |`<infile_nrp <<hdoc cat`|||bash: infile_nrp: Permission denied|1|Heredoc opens, accepts input, and after EOF is signaled it prints the error stream.|
    |ARGUMENTS||||||
    |`<infile cat infile infile_nrp >outfile_nwp`||||||
    |PIPES||||||
    |||||||
    
  
Conclusions:
- The first redirection that fails, exits and prints the error.
- We traverse the redirections list and we try to open all them except for the heredocs (are already open). If one fails we exit.
- For the read files we check with access(R_OK for perror) because maybe we don’t have to open them,
  
  
  
  
  
  
  
  
  
  
  
  
  
  
|   |   |   |   |   |   |   |
|---|---|---|---|---|---|---|
||||||||
||||||||
||||||||
||||||||
||||||||
||||||||
||||||||
||||||||
||||||||
||||||||
||||||||
||||||||