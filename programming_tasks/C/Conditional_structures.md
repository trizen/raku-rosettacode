[1]: http://rosettacode.org/wiki/Conditional_structures

# [Conditional structures][1]

```perl
if won() -> $prize {
    say "You won $prize.";
}
```


Switch structures are done by topicalization and by smartmatching in Perl 6. They are somewhat orthogonal, you can use a <tt>given</tt> block without <tt>when</tt>, and vice versa. But the typical use is:

```perl
given lc prompt("Done? ") {
    when 'yes' { return }
    when 'no'  { next }
    default    { say "Please answer either yes or not." }
}
```


<tt>when</tt> blocks are allowed in any block that topicalizes <tt>$\_</tt>, including a
<tt>for</tt> loop (assuming one of its loop variables is bound to <tt>$\_</tt>)
or the body of a method (if you have declared the invocant as <tt>$\_</tt>)." See [Synopsis 4](http://perlcabal.org/syn/S04.html#Switch_statements).



There are also statement modifier forms of all of the above.



The [ternary operator](http://en.wikipedia.org/wiki/ternary_operator) looks like this:

```perl
$expression ?? do_something !! do_fallback
```


<tt>and</tt>, <tt>or</tt>, <tt>&amp;&amp;</tt>, <tt>||</tt> and <tt>//</tt> work as in Perl 5.