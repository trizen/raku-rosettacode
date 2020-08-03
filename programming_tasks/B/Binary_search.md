[1]: https://rosettacode.org/wiki/Binary_search

# [Binary search][1]

With either of the below implementations of `binary_search`, one could write a function to search any object that does `Positional` this way:

```raku
sub search (@a, $x --> Int) {
    binary_search { $x cmp @a[$^i] }, 0, @a.end
}
```


**Iterative**

```raku
sub binary_search (&p, Int $lo is copy, Int $hi is copy --> Int) {
    until $lo > $hi {
        my Int $mid = ($lo + $hi) div 2;
        given p $mid {
            when -1 { $hi = $mid - 1; } 
            when  1 { $lo = $mid + 1; }
            default { return $mid;    }
        }
    }
    fail;
}
```


**Recursive**

```raku
sub binary_search (&p, Int $lo, Int $hi --> Int) {
    $lo <= $hi or fail;
    my Int $mid = ($lo + $hi) div 2;
    given p $mid {
        when -1 { binary_search &p, $lo,      $mid - 1 } 
        when  1 { binary_search &p, $mid + 1, $hi      }
        default { $mid                                 }
    }
}
```