# Using Mutsu
When adding the Mutsu package to your project, `myProj`,  
it will add the Mutsu namespace to your project space.


You can place your test functions anywhere in your project folder.  
For a manageable structure you can create a namespace for your tests, e.g.:  
`)ns #.myProj.Tests`  
where you add your test functions, e.g.: `TestMyFunction`  


Before beeing able to run your test functions you **have to**  
run Mutsu's RunTests once.  
`#.myProj.Mutsu.RunTests myProj.Tests`  
The arg of RunTests is either a configuration space (NewConfiguration),  
a namespace  or a vector of namespaces to scan for tests.  
If arg is empty it will scan the calling space for tests.  
It'll return a result space.  
The initial execution of RunTests will also place helper functions  
in each namespace containing tests.

## Configuration
A default configuration is set up by the function `NewConfiguration`.  
To setup your own configuration create a function *myRunTests* in your  
root directory where you specify your settings.  
In this function you first get the default config, then you make your  
changes and finally run the Mutsu.RunTests passing your modified config.  



Example:  
``` APL
∇myRunTests; cfg; res  
 cfg←Mutsu.NewConfiguration  
 cfg.TestSpaces←#  
 cfg.TestIncludePatter←'_test$'  
 cfg.ReportType←Mutsu.REPORTTYPE.TXT  
 cfg.ReportPath←'.\reports\'  
 res←Mutsu.RunTests cfg
∇
```

## The Settings
### Debug  
Default: `0`  

If `0` all tests are executed without stopping at error detection.  
set to 1 , execution is stopped when errors are encountered.

### TestSpaces 
Default: `⍬`  
  
The `TestSpaces` can be a single namespace or a vector of namespaces. To scan the entire workspaceset it to `#`.  

### TestIncludePattern  
Default: '^[tT]est' 

Possible to modify to any regex filter.

### CleanupIncludePattern  
Default: '^[cC]leanup'  

Possible to modify to any regex filter.  
Cleanup functions are executed always after tests are done.  

### SetupIncludePattern

Default: '^[sS]etup' 

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



## Validation helpers
Helper functions are copied into any space containing test functions  
and can be used in your test functions.

### Assert
Use Assert in your test functions to check that  
your expressions are true . 
`Assert 'your expected result'≡Function to test`  

### Skip
If your test function for some reason is not completed yet  
but you want to view in your report that it exists,  
then use Skip in test function.  
`Skip 'awaiting'`

### Try
is an operator to check if a tested function returns the expected  
error code or error message.  
`(11 Try #.myProj.CalcSum_test) 'a'`  
this test will pass as CalcSum generates a Domain Error.