[1]: https://rosettacode.org/wiki/Deepcopy

# [Deepcopy][1]





Raku doesn't currently provide a proper mechanism for deep copies, but depending on your requirements you could use one of these work-arounds:





**1) Use `.deepmap(*.clone)`:**



`.deepmap` constructs a copy of the data structure, and `.clone` makes a shallow copy of each leaf node. Limitations:

```perl
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




**2) Use `.raku.EVAL`:**



`.raku` serializes the data structure to Raku code, and `.EVAL` deserializes it. Limitations:

```perl
my %x = foo => 0, bar => [0, 1];
my %y = %x.raku.EVAL;

%x<bar>[1]++;
say %x;
say %y;
```

#### Output:
```
{bar => [0 2], foo => 0}
{bar => [0 1], foo => 0}
```
