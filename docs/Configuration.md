# Configuration

The function `Mutsu.NewConfiguration` sets up a default configuration. To customize this, first retrieve the default configuration, then adjust it with your desired settings. For instance, you can create a function  `myRunTests`, where you define your custom settings, and then pass the modified configuration to `Mutsu.RunTests`

```apl
∇myRunTests; cfg; res  
 cfg←Mutsu.NewConfiguration  
 cfg.TestSpaces←Tests 
 cfg.TestIncludePattern←'Test_'  
 cfg.ReportType←Mutsu.REPORTTYPE.TXT  
 cfg.ReportPath←'.\reports\'  
 res←Mutsu.RunTests cfg
∇
```

## The Settings
### Debug  
Default: `0`  

`0`: All tests are executed without stopping at error detection.  
`1`: Execution is stopped when errors are encountered.

### TestSpaces 
Default: `⍬`  
  
The TestSpaces can be either a single namespace or a vector of namespaces. To scan the entire workspace, set it to `#`.  

### PatternIgnoreCase
Default: `1`

Flag for whether or not to ignore case when searching for the patterns (specified below) for test, cleanup and setup.

`0`: Case-sensitive match.

### TestIncludePattern  
Default: `'^test|_test$'`

All function names that match the expression will be run when calling `RunTests`. This is possible to modify to any regex filter.

### CleanupIncludePattern  
Default: `'^cleanup'`

Possible to modify to any regex filter.  
Cleanup functions are always executed after tests are done.  

### SetupIncludePattern
Default: `'^setup'`

Possible to modify to any regex filter.  
Setup functions are always executed before running the tests. If a test fails, then cleanup and setup functions are executed before continuing with test functions.  

### ReportPath  
Default: `''`

The path where you want to save your reports, e.g.:  
`ReportPath←'.\reports\' ` 

### ReportType
Default: `API.REPORTTYPE.SESSION`

To save the report as a .txt file, set it to `Mutsu.REPORTTYPE.TXT` or `1`. For XML set it to `Mutsu.REPORTTYPE.JUNIT` or `2`.

### ReportName
Default: `'testreport'`  

The name of the test result report.

### SuccessCode
Default: `1`

Define what is OK for a test function that returns a value.
