[1]: https://rosettacode.org/wiki/Diversity_prediction_theorem

# [Diversity prediction theorem][1]



```perl
sub diversity-calc($truth, @pred) {
    my $ae = avg-error($truth, @pred); # average individual error
    my $cp = ([+] @pred)/+@pred;       # collective prediction
    my $ce = ($cp - $truth)**2;        # collective error
    my $pd = avg-error($cp, @pred);    # prediction diversity
    return $ae, $ce, $pd;
}

sub avg-error ($m, @v) { ([+] (@v X- $m) X**2) / +@v }

sub diversity-format (@stats) {
    gather {
        for <average-error crowd-error diversity> Z @stats -> ($label,$value) {
            take $label.fmt("%13s") ~ ':' ~ $value.fmt("%7.3f");
        }
    }
}

.say for diversity-format diversity-calc(49, <48 47 51>);
.say for diversity-format diversity-calc(49, <48 47 51 42>);
```

#### Output:
```
average-error:  3.000
  crowd-error:  0.111
    diversity:  2.889
average-error: 14.500
  crowd-error:  4.000
    diversity: 10.500
```
