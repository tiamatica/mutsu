# Configuration

The function `Mutsu.NewConfiguration` sets up a default configuration. To customize this, first retrieve the default 
configuration, then adjust it with your desired settings. 


## The Settings
### Debug  
Default: `0`  

`0`: All tests are executed without stopping at error detection.  
`1`: Execution is stopped when errors are encountered.

### TestSpaces 
Default: `⍬`  
  
The TestSpaces can be either a single namespace or a vector of namespaces. To scan the entire workspace, set it to `#`.  
If it is empty, Mutsu will scan the calling space (the namespace that `Mutsu.RunTests` was invoked from) for any tests.

### PatternIgnoreCase
Default: `1`

Flag for whether or not to ignore case when applying the search patterns (specified below) for tests, cleanup and setup functions.

`0`: Case-sensitive match.
`1`: Case-insensitive match.

### TestIncludePattern  
Default: `'^test|_test$'`

All function names that match the expression will be run when calling `RunTests`. This must be a valid regex expression.

### CleanupIncludePattern  
Default: `'^cleanup'`

Each test-suite (namespace with test functions) may contain cleanup functions. These are executed after all tests in a 
namespace have been run. In addition, if an unexpected error is signaled during the execution of a test, all cleanup functions 
are executed, followed by any setup functions before resuming the run of tests.

This must be a valid regex expression.

### SetupIncludePattern
Default: `'^setup'`

Possible to modify to any regex filter.  
Setup functions are always executed before running the tests. If a test fails, then cleanup and setup functions are executed 
before continuing with test functions.  

### ReportPath  
Default: `''`

The path where you want to save your reports, e.g.:  
`ReportPath←'.\reports\' ` 

### ReportType
Default: `REPORTTYPE.SESSION`

The type of report to generate, must be one of the types defined in `Mutsu.REPORTTYPE`.

| Type    | Description | 
| ---     | ---         |
| SESSION | Summary report printed to session log |
| TXT     | Summary report written to file        |
| JUNIT   | Detailed JUnit report in XML format written to file |
| GITHUB  | Detailed report posted to GitHub as a Check run |

For `GITHUB` type, Mutsu expects to find the following parameters provided as environment variables. If any of them is missing, 
it will fallback to generate a SESSION report.

| Variable          | Description | 
| ---               | ---         |
| GITHUB_TOKEN      | An authentication token for GitHub |
| GITHUB_REPOSITORY | The account owner and name of the repository (e.g. `tiamatica/Mutsu`) that this report should be sent to |
| GITHUB_SHA        | The SHA of the commit |

### ReportName
Default: `'testreport'`  

The name of the test result report without extension (`.xml` and `.txt` will be appended for `JUNIT` and `TXT` types respectively).

### SuccessCode
Default: `1`

Defines what result to expect from a result returning test function to consider it a success. This will only be used and taken 
into account if a test function does return a result (shy results are used). For tests that don't return a result, the test is 
considered successful if it doesn't signal an error.
