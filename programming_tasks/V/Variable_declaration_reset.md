[1]: https://rosettacode.org/wiki/Variable_declaration_reset

# [Variable declaration reset][1]

By default, Raku variables need a prefix sigil indicating the storage / interface, and a scope declarator to indicate the variables' accessibility. The vast majority of the time, variables are declared with a "my" scope declarator that constrains them to the present block and any enclosed sub blocks. When a 'my' variable is declared inside a loop (block), a new independent instance of the variable is instantiated every time through.

```perl
my @s = 1, 2, 2, 3, 4, 4, 5;
loop (my $i = 0; $i < 7; $i += 1) {
    my $curr = @s[$i];
    my $prev;
    if $i > 1 and $curr == $prev {
        say $i;
    }
    $prev = $curr;
}
```

#### Output:
```
Use of uninitialized value of type Any in numeric context
  in block <unit> at var.p6 line 5
Use of uninitialized value of type Any in numeric context
  in block <unit> at var.p6 line 5
Use of uninitialized value of type Any in numeric context
  in block <unit> at var.p6 line 5
Use of uninitialized value of type Any in numeric context
  in block <unit> at var.p6 line 5
Use of uninitialized value of type Any in numeric context
  in block <unit> at var.p6 line 5
```


Lots of warnings but nothing else. If we suppress the warnings:

```perl
my @s = 1, 2, 2, 3, 4, 4, 5;
quietly loop (my $i = 0; $i < 7; $i += 1) {
    my $curr = @s[$i];
    my $prev;
    if $i > 1 and $curr == $prev {
        say $i;
    }
    $prev = $curr;
}
```


No output.


#### Output:
```

```


We can however, declare the variable with an "our" scope, which effectively makes it a package global. Use of 'our' scoping is discouraged except in a few very specific situations. It "works" (for some value of works), but pollutes the namespace. The 'our' variable will trample any other instance of a variable with that name anywhere in the program in any other scope.

```perl
my @s = 1, 2, 2, 3, 4, 4, 5;
loop (my $i = 0; $i < 7; $i += 1) {
    my $curr = @s[$i];
    our $prev;
    if $i > 1 and $curr == $prev {
        say $i;
    }
    $prev = $curr;
}
```

#### Output:
```
2
5
```


A better solution is to declare a state variable. A 'state' variable is essentially scoped similar to a 'my' variable (visible only inside the block), but is persistent across calls.

```perl
my @s = 1, 2, 2, 3, 4, 4, 5;
loop (my $i = 0; $i < 7; $i += 1) {
    my $curr = @s[$i];
    state $prev;
    if $i > 1 and $curr == $prev {
        say $i;
    }
    $prev = $curr;
}
```

#### Output:
```
2
5
```


If you really want to run fast and loose, and bypass all the protections Raku builds in, you can turn off strict scoping with a pragma. This is ***heavily*** discouraged. Anyone trying to release code with strictures turned off will receive some degree of side-eye... but just because something is a bad idea in general doesn't mean Raku forbids it. The Raku mindset is "Make it difficult to make casual mistakes but make it possible to bypass those protections if you have the need."



No scope declarators at all. Every variable is a global. Bad idea. Do not do this casually.

```perl
no strict;
@s = 1, 2, 2, 3, 4, 4, 5;
loop ($i = 0; $i < 7; $i += 1) {
    $curr = @s[$i];
    if $i > 1 and $curr == $prev {
        say $i;
    }
    $prev = $curr;
}
```

#### Output:
```
2
5
```
