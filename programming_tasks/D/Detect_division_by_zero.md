[1]: http://rosettacode.org/wiki/Detect_division_by_zero

# [Detect division by zero][1]

```perl
sub div($a, $b){
    my $r;
    try {
        $r = $a / $b;
        CATCH {
           say "tried to divide by zero !" if $! ~~ "Divide by zero";
        }
    }
    return $r // fail;
}
 
say div(10,2); # 5
say div(1,0); # Inf, 1/0 constants are substituted for Infinity
say div(1, sin(0)); # undef, and prints "tried to divide by zero"
 
```
```perl
multi div($a, $b){ return $a / $b }
multi div($a, $b where { $b == 0 }){ 
    say 'lolicheatsyou'; 
    return 1; 
}
 
say div(1, sin(0)); # prints "lolicheatsyou" newline "1"
 
```