# Mutsu
Mutsu is a framework for testing Dyalog APL projects.

## Getting Started:
You need Tatin to add the Mutsu package to your projects.

Add the Mutsu package to your project, e.g. *myProj*:  
` ]CIDER.AddTatinDependencies path\name	`

This adds the CiderConfig and Mutsu name spaces to your project space.  
You can place your test functions anywhere in your project folder.  
For a manageable structur you can create a name space for your tests, e.g:  
`)ns #.myProj.Tests`  
where you add your test functions.

To run your test functions start Mutsu's RunTests on your Tests space.  
`#.myProj.Mutsu.RunTests myProj.Tests`  
This will also place the three helper functions in your project space.

A default configuration is set up by the function NewConfiguration.    
To setup your own configuration create a function *myRunTests* in your   
root directory where you specify your settings.  
In this function you first get the default config, then you make your   
changes and finally run the Mutsu.RunTests passing your modified config.  

Example:  
```myRunTests; cfg; res  
 cfg←Mutsu.NewConfiguration  
 cfg.TestSpaces←#  
 cfg.TestIncludePatter←'_test$'  
 cfg.ReportType←Mutsu.REPORTTYPE.TXT  
 cfg.ReportPath←'.\reports\'  
 res←Mutsu.RunTests cfg
```

## The Settings:
**Debug**

default is 0, all tests are executed without stopping at error detection  
set to 1 , execution is stopped when errors encountered.

**TestSpaces**
 
default is empty vector  
Assign your cretatd NS for your tests to TestSpaces,  
or you can assign with root '#' to search all your   
project space for test functions.  

**TestIncludePattern**

default is a regex to search for functions starting with 'Test' or 'test'  
Possible to modify to any regex filter.

**CleanupIncludePattern**

default is a regex to search for functions starting with 'Cleanup' or 'cleanup'  
Possible to modify to any regex filter.  
Cleanup functions are executed always after tests are done.  

**SetupIncludePattern**

default is a regex to search for functions starting with 'Setup' or 'setup'   
Possible to modify to any regex filter.  
Setup functions are executed always before running the tests.  

If some test doesn't pass then cleanup and setup functions are  
executed before continuing eith testfunctions.   

**ReportPath**

default is an empty vector  
assign it with the path where you want to save your reports, e.g:  
ReportPath←'.\reports\'  

**ReportType**

default is reporting to the session: *API.REPORTTYPE.SESSION*  
To save the report as txt file assign the ReportType by value 1    
or *Mutsu.REPORTTYPE.TXT*  
For  xml reports select value 2 or *Mutsu.REPORTTYPE.JUNIT*

**ReportName**

default is the name 'testreport'  



## Validation helper functions:

**Assert**

Using Assert in your test functions returns 1 if the tested code  
performs as expected, anything else than 1 is assumed as error.   

**Skip**

is used in test functions if your test function for some reason is  
not complted yet but you want to view in your report that it exists.  

**Try**

is an operator to check if a tested function returns the expected    
error code or error message.    
`(11 Try #.myProj.CalcSum_test) 'a'`  
this test will pass as CalcSum generates a Domain Error.  



