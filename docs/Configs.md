# Setup Mutsu  
The default configuration is in the function `NewConfiguration`.  

``` APL
∇r←NewConfiguration; cfg; res  
 r←(⊃⎕RSI).⎕NS''
 r.⎕DF'[Mutsu configuration]'
 r.Debug←0                                 
 r.TestSpaces←⍬                            
 r.PatternIgnoreCase←1                     
 r.TestIncludePattern←'^test|_test$'       
 r.CleanupIncludePattern←'^cleanup'        
 r.SetupIncludePattern←'^setup'            
 r.ReportPath←''                           
 r.ReportType←API.REPORTTYPE.SESSION       
 r.ReportName←'testreport'                 
 r.SuccessCode←1                           
∇
```  
## The Settings
### Debug  
Default: `0`  

If `0` all tests are executed without stopping at error detection.  
set to 1 , execution is stopped when errors are encountered.

### TestSpaces 
Default: `⍬`  
  
The `TestSpaces` can be a single namespace or a vector of namespaces. To scan the entire workspace set it to `#`.  

### PatternIgnoreCase
Default: 1

Case insensitivity flag for the include patterns of test, cleanup and setup. 
Set to 0 for case-sensitive match.

### TestIncludePattern  
Default: '^test|_test$' 

Possible to modify to any regex filter.

### CleanupIncludePattern  
Default: '^cleanup'  

Possible to modify to any regex filter.  
Cleanup functions are executed always after tests are done.  

### SetupIncludePattern

Default: '^setup' 

Possible to modify to any regex filter.  
Setup functions are executed always before running the tests.  
If some test doesn't pass then cleanup and setup functions are  
executed before continuing with testfunctions.  

### ReportPath  

Default: `''`

assign it with the path where you want to save your reports, e.g:  
`ReportPath←'.\reports\' ` 

### ReportType

Default: `API.REPORTTYPE.SESSION`

To save the report as txt file assign the ReportType by value 1  
or `Mutsu.REPORTTYPE.TXT`  
For  xml reports select value 2 or `Mutsu.REPORTTYPE.JUNIT`

### ReportName

Default: `testreport`  

### SuccessCode

Default: `1`

When test functions use a return code to signal success or fail,  
this defines what constitutes a passed test.  
