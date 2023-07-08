# Mentoring

## Problems and Chalenges

## Reasonable Solution

**If expression**

```pyret
fun score(x, y):
  distance = num-sqrt(num-expt(x, 2) + num-expt(y, 2))

  if distance <= 1:
    10
  else if distance <= 5:
    5
  else if distance <= 10:
    1
  else:
    0
  end
end
```

or

**Ask expression**

```pyret
fun score(x, y):
  distance = num-sqrt(num-expt(x, 2) + num-expt(y, 2))

  ask:
  | distance <= 1 then: 10
  | distance <= 5 then: 5
  | distance <= 10 then: 1
  | otherwise: 0
  end
end
```

## Talking Points

### Minimizing comparisons

A student may check the upper and lower limit for each ring (see below). However, this isn't necessary.
For a given ring, the lower limit check is the negation of the previous ring's upper limt check.
So we can assume the distance is above the lower limit because otherwise we would have satisified
the previous ring's checks.

```pyret
fun score(x, y):
  distance = num-sqrt(num-expt(x, 2) + num-expt(y, 2))

  ask:
  | (0 <= distance) and (distance <= 1) then: 10
  | (1 < distance) and (distance <= 5) then: 5
  | (5 < distance) and (distance <= 10) then: 1
  | otherwise: 0
  end
end
```

### Squared distance checks

`num-sqrt` and `num-expt` aren't strictly necessary in solving the exercise. It's perfectly okay to use squared values for the distance checks.

```pyret
fun score(x, y):
  distance = (x * x) + (y * y)

  ask:
  | distance <= 1 then: 10
  | distance <= 25 then: 5
  | distance <= 100 then: 1
  | otherwise: 0
  end
end
```

### Using iteration

To avoid repeating the distance checks, we can store the upper limit and scores in a `List` of tuples representing each ring, iterating over that.
In terms of dictionaries, Pyret only has the immutable `StringDict` or mutable `MutableStringDict`, both of which use string keys.
However, a student might find it more readable to attach label keys to each tuple. For our purposes here, iterating over either dictionary type is identical.
The `now` in `get-value-now` and `keys-now` for `MutableStringDict` simply reflects that those represents snapshots. For a particular copy of `StringDict`, `get-value` and `keys` will return the same results. Since we don't need to mutate the rules, `StringDict` should be preferred over `MutableStringDict`. However, we shouldn't require `StringDict` over a `List` of tuples or vice versa.

**List of tuples**

```pyret
RULES = [list: {1;10}, {5;5}, {10;1}] 

fun score(x, y):
  distance = (x * x) + (y * y)

  find(lam(rule): distance <= rule.{0} end, RULES).and-then(lam(rule): rule.{1} end).or-else(0)
end
```

**String dictionary**

```pyret
include string-dict
  
RULES = [string-dict: "inner", {1;10}, "middle", {5;5}, "outer", {10;1}] 

fun score(x, y):
  distance = (x * x) + (y * y)

  rules = RULES.keys().to-list().map(
    lam(key): RULES.get-value(key) end)
  
  find(lam(rule): distance <= rule.{0} end, rules).and-then(lam(rule): rule.{1} end).or-else(0)
end
```

**Mutable string dictionary**

```pyret
include string-dict
  
RULES = [mutable-string-dict: "inner", {1;10}, "middle", {5;5}, "outer", {10;1}] 

fun score(x, y):
  distance = (x * x) + (y * y)

  rules = RULES.keys-now().to-list().map(
    lam(key): RULES.get-value-now(key) end)
  
  find(lam(rule): distance <= rule.{0} end, rules).and-then(lam(rule): rule.{1} end).or-else(0)
end
```
