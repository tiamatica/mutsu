# Mutsu
Mutsu is a framework for testing Dyalog APL projects.

## Getting Started:
You need Tatin to add the Mutsu package to your projects.

If you use Cider to manage your project,   
you can install Mutsu as a development tool using  
the `-development` switch:  
`]CIDER.AddTatinDependencies Mutsu -development`  
This adds the Mutsu namespaces to your project space. 

You can place your test functions anywhere in your project folder.  
For a manageable structure you can create a namespace for your tests, e.g.:  
`)ns #.myProj.Tests`  
where you add your test functions, e.g.: `TestMyFunction`  


Before beeing able to run your test functions you **have to**   
run Mutsu's RunTests once.  
`#.myProj.Mutsu.RunTests myProj.Tests`    
The arg of RunTests is either a configuration space (NewConfiguration),   
a namespace  or a vector of namespaces to scan for tests.  
If arg is empty it will scan calling space for tests.  
It'll return a result space.  
The initial execution of RunTests will also place helper functions  
in each namespace containing tests.

## Your configuration
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

## The Settings:
### Debug  
Default: `0`  

If `0` all tests are executed without stopping at error detection.    
set to 1 , execution is stopped when errors are encountered.

### TestSpaces
 
default: empty vector    
  
The `TestSpaces` can be a single namespace, a vector of namespaces or the complete project space when assigning it `#`.  

### TestIncludePattern  
default: a regex to search for functions starting with 'Test' or 'test'  

Possible to modify to any regex filter.

### CleanupIncludePattern  
default: a regex to search for functions starting with 'Cleanup' or 'cleanup'  

Possible to modify to any regex filter.  
Cleanup functions are executed always after tests are done.  

### SetupIncludePattern

default: a regex to search for functions starting with 'Setup' or 'setup'  

Possible to modify to any regex filter.  
Setup functions are executed always before running the tests.  
If some test doesn't pass then cleanup and setup functions are  
executed before continuing with testfunctions.   

### ReportPath  

default: empty vector  

assign it with the path where you want to save your reports, e.g:  
`ReportPath←'.\reports\' ` 

### ReportType

default: reporting to the session: *API.REPORTTYPE.SESSION*  

To save the report as txt file assign the ReportType by value 1    
or *Mutsu.REPORTTYPE.TXT*  
For  xml reports select value 2 or *Mutsu.REPORTTYPE.JUNIT*

### ReportName

default: the name 'testreport'  



## Validation helper functions:

### Assert

Using Assert in your test functions returns 1 if the tested code  
performs as expected, anything else than 1 is assumed as error.  
`Assert 'your expected result'≡Function to test`  

### Skip

If your test function for some reason is not completed yet  
but you want to view  in your report that it exists,    
then use Skip in test function.  
`Skip 'awaiting'`

### Try

is an operator to check if a tested function returns the expected    
error code or error message.    
`(11 Try #.myProj.CalcSum_test) 'a'`  
this test will pass as CalcSum generates a Domain Error.