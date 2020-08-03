[1]: https://rosettacode.org/wiki/Deepcopy

# [Deepcopy][1]

Perl 6 doesn't currently provide a proper mechanism for deep copies, but depending on your requirements you could use one of these work-arounds:





**1) Use `.deepmap(*.clone)`:**



`.deepmap` constructs a copy of the data structure, and `.clone` makes a shallow copy of each leaf node. Limitations:

```raku
my %x = foo => 0, bar => [0, 1];
my %y = %x.deepmap(*.clone);
 
%x<bar>[1]++;
say %x;
say %y;
```

#### Output:
```
{bar => [0 2], foo => 0}
{bar => [0 1], foo => 0}
```




**2) Use `.perl.EVAL`:**



`.perl` serializes the data structure to Perl 6 code, and `.EVAL` deserializes it. Limitations:

```raku
use MONKEY-SEE-NO-EVAL;
 
my %x = foo => 0, bar => [0, 1];
my %y = %x.perl.EVAL;
 
%x<bar>[1]++;
say %x;
say %y;
```

#### Output:
```
{bar => [0 2], foo => 0}
{bar => [0 1], foo => 0}
```