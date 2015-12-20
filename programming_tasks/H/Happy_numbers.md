[1]: http://rosettacode.org/wiki/Happy_numbers

# [Happy numbers][1]

```perl6
sub happy (Int $n is copy --> Bool) {
  loop {
      state %seen;
      $n = [+] $n.comb.map: { $_ ** 2 }
      return True  if $n == 1;
      return False if %seen{$n}++;
  }
}
 
say join ' ', grep(&happy, 1 .. *)[^8];
```

#### Output:
```
1 7 10 13 19 23 28 31
```


Here's another approach that uses a different set of tricks including lazy lists, gather/take, repeat-until, and the cross metaoperator X.

```perl6
my @happy = lazy gather for 1..* -> $number {
    my %stopper = 1 => 1;
    my $n = $number;
    repeat until %stopper{$n}++ {
        $n = [+] $n.comb X** 2;
    }
    take $number if $n == 1;
}
 
say ~@happy[^8];
```


Output is the same as above.



Here is a version using a subset and an anonymous recursion (we cheat a little bit by using the knowledge that 7 is the second happy number):

```perl6
subset Happy of Int where sub ($n) {
    $n == 1 ?? True  !!
    $n < 7  ?? False !!
    &?ROUTINE([+] $n.comb »**» 2);
}
 
say (grep Happy, 1 .. *)[^8];
```


Again, output is the same as above. It is not clear whether this version returns in finite time for any integer, though.



There's more than one way to do it...