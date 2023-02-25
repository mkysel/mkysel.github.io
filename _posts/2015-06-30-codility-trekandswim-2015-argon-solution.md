---
title: "Codility 'TrekAndSwim' 2015 Argon Solution"
date: "2015-06-30"
categories: 
  - "codility"
  - "coding-challenge"
  - "php"
---

##### Short Problem Definition:

Find a longest slice of a binary array that can be split into two parts: in the left part, 0 should be the leader; in the right part, 1 should be the leader.

##### Link

[Trek and Swim](https://codility.com/programmers/challenges/argon2015)

##### Complexity:

expected worst-case time complexity is O(N)

expected worst-case space complexity is O(N)

##### Execution:

The PHP solution is by Rastislav Chynoransky. The Python code and an execution explanation will follow shortly.

##### Solution:

```php

/**
 * @author Rastislav Chynoransky
 */
function solution($A) {
    $sum = 0;
    for ($i = 0; $i < count($A); $i++) {
        $sum += !$A[$i];
        $zeros[] = $sum;
    }

    $sum = 0;
    for ($i = count($A); $i--;) {
        $sum += $A[$i];
        $ones[$i] = $sum;
    }

    for ($i = count($A) - 1; $i--;) {
        $res[$i] = ($zeros[$i] + $ones[$i]) * ($A[$i + 1] - $A[$i]);
    }

    $max = max($res);

    if ($max <= 0) {
        return 0;
    }

    $index = array_search($max, $res);

    $right = 0;
    $sum = 0;
    for ($i = $index + 1; $i < count($A); $i++) {
        $sum += $A[$i];
        if (2 * $sum / ($i - $index) > 1) {
            $right = $i - $index;
        }
    }

    $left = 0;
    $sum = 0;
    for ($i = $index; $i >= 0; $i--) {
        $sum += !$A[$i];
        if (2 * $sum / ($index - $i + 1) > 1) {
            $left = $index - $i + 1;
        }
    }

    return $left + $right;
}
```
