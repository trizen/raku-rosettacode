[1]: http://rosettacode.org/wiki/Find_limit_of_recursion

# [Find limit of recursion][1]

Maximum recursion in Perl 6 is implementation dependent and subject to change as development proceeds.

```perl
my $x = 0;
recurse;
 
sub recurse () {
   say ++$x;
   recurse;
}
```


Using Rakudo 2011.01 this yields:


#### Output:
```
1
2
...
...
971
972
maximum recursion depth exceeded
```


This is because the Parrot VM currently imposes a limit of 1000. On the other hand, the niecza implementation has no limit, subject to availability of virtual memory. In any case, future Perl&#160;6 is likely to require tail call elimination in the absence of some declaration to the contrary.