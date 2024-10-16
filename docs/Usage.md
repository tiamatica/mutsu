# Using Mutsu
When adding the Mutsu package to your project, `myProj`,  
it will add the Mutsu namespace to your project space .  
`#.myProj.Mutsu`


You can place your test functions anywhere in your project folder.  
For a manageable structure you can create a namespace for your tests, e.g.:  
`)ns #.myProj.Tests`  
where you create your test functions with names that either start with  
`Test` or end with `_Test`(case-insensitive). Then call the RunTests  
function with the reference to that namespace as right argument.  
`#.myProj.Mutsu.RunTests myProj.Tests`  
It'll return a result space, a test report, that will be saved  
according to the settings of 
`ReportType`.   


The argument of RunTests is either a configuration space (NewConfiguration),  
a namespace  or a vector of namespaces to scan for tests.  
If arg is empty it will scan the calling space for tests.  
The initial execution of RunTests will also place helper functions  
in each namespace containing tests.

## Configuration
A default configuration is set up by the function `NewConfiguration`.  
To setup your own configuration create a function *myRunTests* in your  
root directory where you specify your settings.  
In this function you first get the default config, then you make your  
changes and finally run the Mutsu.RunTests passing your modified config.  
For further info about the settings, see: [Configs](Configs.md). 


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
## Validation helpers
Helper functions are copied into any space containing test functions  
and can be used in your test functions.
If you write test functions 
which return a value, you will not need these helpers.  
Make sure to set `SuccessCode` to the value of your choice  
in your `myRunTests` function.

### Assert
Use Assert in your test functions to check that  
your expressions are true.   
`Assert 'your expected result'≡Function to test`   
e.g.: `Assert 1 ≡ {4=2+2}`  
The total number of the calls to the the functions `Assert` and `Try`  
is displayed in the test report.

### Skip
If your test function for some reason is not completed yet  
but you want to view in your report that it exists,  
then use Skip in test function.  
`Skip testMyFn`

### Try
is an operator to check if a tested function returns the expected  
error code or error message.  
`(11 Try #.myProj.CalcSum_test) 'a'`  
this test will pass as CalcSum generates a Domain Error as w expected.  

## Test report

An example of the test report:  
```
Test run on 2024-10-14T14:54:39                                                                                        
Total suites:     1                                                                                                    
Total assertions: 2                                                                                                    
Total tests:      2                                                                                                    
Total errors:     0                                                                                                    
Total failures:   1                                                                                                    
Total skipped:    0                                                                                                    
                                                                                                                       
Testsuite: #.Mutsu.Tests                                                                                
 #.Mutsu.Tests.testFail   ⍝ failure:   Expected exception [VALUE] got [DOMAIN ERROR]
 ``` 
