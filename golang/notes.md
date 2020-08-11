## Zero values

Go initialize variables with some zero values:

- number types with 0
- boolean with false
- pointer with nil

## Blank identifier

Go has "black hole" where you can assign unused variable:

'''go
r, _ := some_func_with_two_return_values()
'''