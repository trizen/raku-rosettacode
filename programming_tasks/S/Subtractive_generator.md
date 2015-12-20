[1]: http://rosettacode.org/wiki/Subtractive_generator

# [Subtractive generator][1]

```perl6
sub bentley_clever($seed) {
    constant $mod = 1_000_000_000;
    my @seeds = ($seed % $mod, 1, (* - *) % $mod ... *)[^55];
    my @state = @seeds[ 34, (* + 34 ) % 55 ... 0 ];
 
    sub subrand() {
        push @state, (my $x = (@state.shift - @state[*-24]) % $mod);
        $x;
    }
 
    subrand for 55 .. 219;
 
    &subrand ... *;
}
 
my @sr := bentley_clever(292929);
.say for @sr[^10];
```


Here we just make the seeder return the random sequence as a lazy list.


#### Output:
```
467478574
512932792
539453717
20349702
615542081
378707948
933204586
824858649
506003769
380969305
```