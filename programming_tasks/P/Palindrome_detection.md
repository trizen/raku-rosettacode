[1]: https://rosettacode.org/wiki/Palindrome_detection

# [Palindrome detection][1]



```perl
subset Palindrom of Str where {
    .flip eq $_ given .comb(/\w+/).join.lc
}
 
my @tests = q:to/END/.lines;
    A man, a plan, a canal: Panama.
    My dog has fleas
    Madam, I'm Adam.
    1 on 1
    In girum imus nocte et consumimur igni
    END
 
for @tests { say $_ ~~ Palindrom, "\t", $_ }
```

#### Output:
```
True    A man, a plan, a canal: Panama.
False   My dog has fleas
True    Madam, I'm Adam.
False   1 on 1
True    In girum imus nocte et consumimur igni
```
