[1]: https://rosettacode.org/wiki/Hickerson_series_of_almost_integers

# [Hickerson series of almost integers][1]

We'll use [FatRat](http://doc.perl6.org/type/FatRat) values, and a series for an [approximation of ln(2)](http://mathworld.wolfram.com/NaturalLogarithmof2.html).

```raku
constant ln2 = [+] (1/2.FatRat, */2 ... *) Z/ 1 .. 100;
constant h = [\*] 1/2, |(1..*) X/ ln2;
 
use Test;
plan *;
 
for h[1..17] {
    ok m/'.'<[09]>/, .round(0.001) 
}
```

#### Output:
```
ok 1 - 1.041
ok 2 - 3.003
ok 3 - 12.996
ok 4 - 74.999
ok 5 - 541.002
ok 6 - 4683.001
ok 7 - 47292.999
ok 8 - 545834.998
ok 9 - 7087261.002
ok 10 - 102247563.005
ok 11 - 1622632572.998
ok 12 - 28091567594.982
ok 13 - 526858348381.001
ok 14 - 10641342970443.085
ok 15 - 230283190977853.037
not ok 16 - 5315654681981354.513

# Failed test '5315654681981354.513'
# at hickerson line 8
not ok 17 - 130370767029135900.458

# Failed test '130370767029135900.458'
# at hickerson line 8
```