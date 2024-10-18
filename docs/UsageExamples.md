# Usage examples

Install Mutsu in your project with Tatin, Dyalog APL's package manager. If you use Cider as a project manager, you can also add Mutsu as a Tatin dependency.

```apl
]TATIN.InstallPackages tiamatica-Mutsu <install-path> 
```

## Mutsu.RunTests

`Mutsu.RunTests` runs all your tests and returns a test result report. It expects a vector of namespaces to scan for tests, or a configuration space, as the right argument. If no configuration is passed, it will use the default configuration. If the argument is empty, it will scan the calling space for tests.

## Test functions
Both tests written as dfns and tradfns are supported. Using the default configuration, a test will pass if it either returns `1` or does not return a result. There are three helper functions available: `Assert`, `Skip` and `Try`, which are described in more detail below.

## Using Mutsu with default configurations
Let's say you want to run the tests in the folder `UnitTests` with the default configuration. Then all you need to do is call `Mutsu.RunTests UnitTests`. This will run all tests that matches the default pattern, which is the case insensitive *test* as prefix, or *_test* as suffix. `RunTests` will also look for any *setup* and *cleanup* functions. The results of the test run will be displayed in the session window.

## Specify your own configurations
See [Configuration](./Configuration.md) for more information about the default configurations and how you can modify them.

Suppose you have two test suites, `UnitTests` and `IntegrationTests`, both located in the folder `Tests`. You want to run all tests in these two suites and save the test result report as a text file in the folder TestReports. You can accomplish this as follows:

```apl
myRunTests; cfg

cfg←Mutsu.NewConfiguration
cfg.TestSpaces←Tests
cfg.ReportType←Mutsu.REPORTTYPE.TXT
cfg.ReportPath←'/Users/sandra/Desktop/Result/'
Mutsu.RunTests cfg
```


## Validation helpers
Mutsu provides three helper functions that you can use in your tests. These functions are copied into any namespace containing test functions when you run `Mutsu.RunTests`.

### Assert
Use `Assert` to check that the result matches what you expect. `Assert` expects a boolean as the right argument, with 1 indicating a pass and 0 a fail. You can also provide a more detailed message as a left argument. 

```apl
expected←2
Assert expected≡addOne 1
``` 

### Skip
Use `Skip` if you have test functions that are not completed yet, but you still want to include them in the test result report.

```apl
Skip 'This test isn't ready yet.'
```

### Try
Use `Try` to check that a function returns the expected  error code or error message. If you do not expect an error, pass `0` as the left operand.

```apl
⍝ All these tests will pass

(11 Try #.myProj.CalcSum_test) 'a' ⍝ We expect a Domain error and get one.

4 (0 Try {⍺÷⍵}) 2 ⍝ We do not expect an error.

4 ('DOMAIN' Try {⍺÷⍵}) 0 ⍝ We can also specify the error as a substring.
```

## Test Result

The result report will display the time when the tests were run, along with the total number of test suites, the number of times that `Assert` and `Try` were called, and the total number of individual tests. If there were any errors, failures or skipped functions, the report will provide additional information. *Failures* are tests that did not pass, *errors* are tests that encountered runtime issues, and *skipped* are those omitted using the `Skip` function.

In the example below, one test suite, containing one test was run. There was one failure, which will display more information. Here we can see that the failure occured in the test suite `Tests`, in the function `function_test`, and that the failure was an *Affirm error*.

```
Test run on 2024-10-18T12:13:54                                               
Total suites:     1                                                           
Total assertions: 1                                                           
Total tests:      1                                                           
Total errors:     0                                                           
Total failures:   1                                                           
Total skipped:    0                                                           
                                                                              
Testsuite: #.Tests                                                            
 #.Tests.function_test  ⍝ failure:   AFFIRM ERROR function_test[1] Assert 1≡2 
 ```
