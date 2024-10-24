# Mutsu

Mutsu is an easy-to-use and flexible test framework for Dyalog APL.

## How to use it 

Write tests that either start with *'Test'* or end with *'_test'* (case insensitive).

```apl
function_test←{'easy'≡'easy'}   
```

If you place your tests inside a namespace (e.g., *Tests*) you can pass that namespace as an argument to Mutsu's test function 
`Mutsu.RunTests`. This will run all tests in that namespace and give you a test result report:

```apl
      Mutsu.RunTests Tests
Test run on 2024-10-24T09:34:54
Total suites:     1            
Total assertions: 0            
Total tests:      1            
Total errors:     0            
Total failures:   0            
Total skipped:    0 
```

There are three helper functions that you can use inside your tests: `Assert`, `Skip` and `Try`. Calling `Mutsu.RunTests` will 
place these functions inside your test namespaces before invoking your tests.

```apl
Assert 2=addOne 1 ⍝ Assert that result matches expected value.

Skip 'This test isn''t ready yet.'

2 (11 Try {⍺÷⍵}) 0 ⍝ We expected a Domain error and we got it
```

For more examples on how to use Mutsu see [Usage examples](./docs/UsageExamples.md).

You can of course name your functions however you like, save the report as a text file, stop on errors, add a setup function, 
and in general modify the behaviour. See [Configuration](./docs/Configuration.md) for default settings and how you can configure 
them.

## Installation
Mutsu is a Tatin package and can be loaded or installed via Tatin:  

```apl
]TATIN.LoadPackages tiamatica-Mutsu
```  
