## Zero values
Go initialize variables with zero values:
- number types with 0
- boolean with false
- pointer with nil

## Blank identifier
Go has "black hole" where you can assign unused variable:
```go
r, _ := some_func_with_two_return_values()
```

## Constants
- constants can be untyped, which means they be converted in place like literals
- iota - constant generator for ordered constants

## Other
- If you can assign sth than you can compare with it
- Incdec statement cannot be used as expression like in C
