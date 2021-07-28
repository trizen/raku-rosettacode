[1]: https://rosettacode.org/wiki/Sort_the_letters_of_string_in_alphabetical_order

# [Sort the letters of string in alphabetical order][1]

### Semi-realistic version

```perl
sub sort_within_string ( $_ is copy ) {
    constant @lexographic_order = sort *.fc, map &chr, 1..255;
 
    return join '', gather for @lexographic_order -> $l {
        my $count = s:g/$l//;
        take $l x $count;
        LAST { warn "Original string had non-ASCII chars: {.raku}" if .chars }
    }
}
say trim .&sort_within_string for q:to/END/.lines;
The quick brown fox jumps over the lazy dog, apparently
Now is the time for all good men to come to the aid of their country.
END
 
```

#### Output:
```
,aaabcdeeeefghhijkllmnnoooopppqrrrsTttuuvwxyyz
.aaccddeeeeeeffghhhiiiillmmmNnnooooooooorrrstttttttuwy
```


### Following the actual task title / description



Following a strict interpretation of the actual task title and description.



Sorted output is wrapped in double guillemots to make it easier to see where it starts and ends.

```perl
sub moronic-sort ($string is copy) {
    my $chars = $string.chars;
    loop {
        for ^$chars {
            if ($string.substr($_, 1).fc gt $string.substr($_ + 1, 1).fc and $string.substr($_ + 1, 1) ~~ /<:L>/)
               or $string.substr($_, 1) ~~ /<:!L>/ {
                $string = $string.substr(0, $_) ~ $string.substr($_ , 2).flip ~ $string.substr($_ + 2 min $chars);
            }
        }
        last if $++ >= $chars;
    }
    $string
}
 
sub wrap ($whatever) { '»»' ~ $whatever ~ '««' }
 
 
# Test sort the exact string as specified in the task title.
say "moronic-sort 'string'\n" ~ wrap moronic-sort 'string';
 
 
# Other tests demonstrating the extent of the stupidity of this task.
say "\nLonger test sentence\n" ~ 
wrap moronic-sort q[This is a moronic sort. It's only concerned with sorting letters, so everything else is pretty much ignored / pushed to the end. It also doesn't much care about letter case, so there is no upper / lower case differentiation.];
 
 
say "\nExtended test string:\n" ~ my $test = (32..126)».chr.pick(*).join;
say wrap moronic-sort $test;
```

#### Output:
```
moronic-sort 'string'
»»ginrst««

Longer test sentence
»»aaaaaaabccccccccddddddeeeeeeeeeeeeeeeeeeeeeeeeeffggghhhhhhhhiiiIiiiiiIiiiillllllmmmnnnnnnnnnnnnoooooooooooooooopppprrrrrrrrrrrrrrssssssssssssssssTtttttttttttttttttttuuuuuvwwyyy    ,      /   .    . '     ,        /    .   ' ««

Extended test string:
!kjyxAa+,LGh_8?3lXEwW-D]Ku|SY[@VF\.op{=q>MT 1tJ/$nN(Z*%&9^v57")`PCiOHQe'RUb<gs;6}#cfmrzd42B~0I:
»»AabBCcDdEeFfGghHiIjJkKLlMmnNoOpPqQRrSsTtuUVvwWxXyYZz[@\.{=> 1/$(*%&9^57")`'<;6}#42~0:!+,_8?3-]|««
```
