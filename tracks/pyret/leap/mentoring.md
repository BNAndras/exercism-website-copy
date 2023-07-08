# Mentoring

## Reasonable Solution

This is a pretty concise and efficient solution for this exercise.

```pyret

fun leap(year):
  (num-modulo(year, 4) == 0) and ((num-modulo(year, 100) <> 0) or (num-modulo(year, 400) == 0))
end
```

Students may opt to store the three divisibility checks to variables. That's also fine.

```pyret

fun leap(year):
  divisible-by-4 = num-modulo(year, 4) == 0
  divisible-by-100 = num-modulo(year, 100) == 0
  divisible-by-400 = num-modulo(year, 400) == 0
  divisible-by-4 and (not(divisible-by-100) or divisible-by-400)
end
```

## Talking Points

### Operator Precedence

Pyret doesn't use operator precedence like many other languages. Instead, operator precedence is defined by the use of parentheses.
Parentheses are required if any two different operators are at the same level.

Valid

```pyret
1 + 1 + 1 // 3
1 + 1 - 1 // error because + and - are at the same level
(1 + 1) - 1 // 2

true and true and true // true
true and true or false // error because different operator
true and (true or false) // true

2 > 1 // true
2 > 1 and true // error because different operator
(2 > 1) and true // true
```

Therefore, operators of different type (logical, arithmetic, equality, etc) must be explicitly grouped.
Evaluation is always let to right.
