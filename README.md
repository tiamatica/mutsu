# Mutsu
Mutsu is a framework for testing Dyalog APL projects.  

## Installation  
Add the Mutsu package to your project:  
```APL
]TATIN.LoadPackages Mutsu
```  

## Getting Started  
For more info how to use Mutsu, see [Usage](./docs/Usage.md).
    
Create your test functions with names that either start with `Test` or end with `_Test`(case-insensitive).  
Run Mutsu's RunTests   
`Mutsu.RunTests #`  

This will run all test functions in your workspace by  
default  configuration and show a test result report  
in the session window.  
