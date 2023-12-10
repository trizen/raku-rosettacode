[1]: https://rosettacode.org/wiki/Conditional_structures

# [Conditional structures][1]





### if/else

```perl
if won() -> $prize {
    say "You won $prize.";
}
```


### given/when



Switch structures are done by topicalization and by smartmatching in Raku.  They are somewhat orthogonal, you can use a `given` block without `when`, and vice versa.  But the typical use is:

```perl
given lc prompt("Done? ") {
    when 'yes' { return }
    when 'no'  { next }
    default    { say "Please answer either yes or no." }
}
```


`when` blocks are allowed in any block that topicalizes `$_`, including a
`for` loop (assuming one of its loop variables is bound to `$_`)
or the body of a method (if you have declared the invocant as `$_`)." See more at: [https://docs.raku.org/language/control#index-entry-switch_(given)](https://docs.raku.org/language/control#index-entry-switch_(given))



There are also statement modifier forms of all of the above.



### Ternary operator



The [ternary operator](https://en.wikipedia.org/wiki/ternary_operator) looks like this:

```perl
$expression ?? do_something !! do_fallback
```


### Other short-circuiting operators



`and`, `or`, `&&`, `||` and `//` work as in Perl 5.
