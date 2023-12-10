[1]: https://rosettacode.org/wiki/Determine_if_a_string_is_collapsible

# [Determine if a string is collapsible][1]





Technically, the task is asking for a boolean. "Determine if a string is collapsible" is answerable with True/False, so return a boolean as well.

```perl
map {
    my $squish = .comb.squish.join;
    printf "\nLength: %2d <<<%s>>>\nCollapsible: %s\nLength: %2d <<<%s>>>\n",
      .chars, $_, .chars != $squish.chars, $squish.chars, $squish
}, lines
 
q:to/STRINGS/;
    
    "If I were two-faced, would I be wearing this one?" --- Abraham Lincoln 
    ..1111111111111111111111111111111111111111111111111111111111111117777888
    I never give 'em hell, I just tell the truth, and they think it's hell. 
                                                        --- Harry S Truman  
    The American people have a right to know if their president is a crook. 
                                                        --- Richard Nixon   
    AАΑAАΑAАΑAАΑAАΑAАΑAАΑAАΑAАΑAАΑAАΑAАΑAАΑAАΑAАΑAАΑAАΑAАΑAАΑAАΑAАΑAАΑAАΑAАΑ
    AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA
    STRINGS
```

#### Output:
```
Length:  0 <<<>>>
Collapsible: False
Length:  0 <<<>>>

Length: 72 <<<"If I were two-faced, would I be wearing this one?" --- Abraham Lincoln >>>
Collapsible: True
Length: 70 <<<"If I were two-faced, would I be wearing this one?" - Abraham Lincoln >>>

Length: 72 <<<..1111111111111111111111111111111111111111111111111111111111111117777888>>>
Collapsible: True
Length:  4 <<<.178>>>

Length: 72 <<<I never give 'em hell, I just tell the truth, and they think it's hell. >>>
Collapsible: True
Length: 69 <<<I never give 'em hel, I just tel the truth, and they think it's hel. >>>

Length: 72 <<<                                                    --- Harry S Truman  >>>
Collapsible: True
Length: 17 <<< - Hary S Truman >>>

Length: 72 <<<The American people have a right to know if their president is a crook. >>>
Collapsible: True
Length: 71 <<<The American people have a right to know if their president is a crok. >>>

Length: 72 <<<                                                    --- Richard Nixon   >>>
Collapsible: True
Length: 17 <<< - Richard Nixon >>>

Length: 72 <<<AАΑAАΑAАΑAАΑAАΑAАΑAАΑAАΑAАΑAАΑAАΑAАΑAАΑAАΑAАΑAАΑAАΑAАΑAАΑAАΑAАΑAАΑAАΑAАΑ>>>
Collapsible: False
Length: 72 <<<AАΑAАΑAАΑAАΑAАΑAАΑAАΑAАΑAАΑAАΑAАΑAАΑAАΑAАΑAАΑAАΑAАΑAАΑAАΑAАΑAАΑAАΑAАΑAАΑ>>>

Length: 72 <<<AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA>>>
Collapsible: True
Length:  1 <<<A>>>
```
