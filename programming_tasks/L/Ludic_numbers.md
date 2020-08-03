[1]: https://rosettacode.org/wiki/Ludic_numbers

# [Ludic numbers][1]

This implementation has no arbitrary upper limit, since it can keep adding new rotors on the fly. It just gets slower and slower instead... `:-)`

```raku
constant @ludic = gather {
        my @taken = take 1;
        my @rotor;
 
        for 2..* -> $i {
            loop (my $j = 0; $j < @rotor; $j++) {
                --@rotor[$j] or last;
            }
            if $j < @rotor {
                @rotor[$j] = @taken[$j+1];
            }
            else {
                push @taken, take $i;
                push @rotor, @taken[$j+1];
            }
        }
    }
 
say @ludic[^25];
say "Number of Ludic numbers <= 1000: ", +(@ludic ...^ * > 1000);
say "Ludic numbers 2000..2005: ", @ludic[1999..2004];
 
my \l250 = set @ludic ...^ * > 250;
say "Ludic triples < 250: ", gather
    for l250.keys.sort -> $a {
        my $b = $a + 2;
        my $c = $a + 6;
        take "<$a $b $c>" if $b ∈ l250 and $c ∈ l250;
    }
```

#### Output:
```
(1 2 3 5 7 11 13 17 23 25 29 37 41 43 47 53 61 67 71 77 83 89 91 97 107)
Number of Ludic numbers <= 1000: 142
Ludic numbers 2000..2005: (21475 21481 21487 21493 21503 21511)
Ludic triples < 250: (<1 3 7> <5 7 11> <11 13 17> <23 25 29> <41 43 47> <173 175 179> <221 223 227> <233 235 239>)
```