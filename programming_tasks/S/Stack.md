[1]: https://rosettacode.org/wiki/Stack

# [Stack][1]

Perl 6 still has the stack functions from Perl 5, but now they also can be accessed by object notation:

```raku
my @stack;          # just a array
@stack.push($elem); # add $elem to the end of @stack
$elem = @stack.pop; # get the last element back
@stack.elems == 0   # true, because the stack is empty
not @stack          # also true because @stack is false
```