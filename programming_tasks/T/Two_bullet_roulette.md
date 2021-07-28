[1]: https://rosettacode.org/wiki/Two_bullet_roulette

# [Two bullet roulette][1]

```perl
unit sub MAIN ($shots = 6);
 
my @cyl;
 
sub load () {
    @cyl.=rotate(-1) while @cyl[1];
    @cyl[1] = 1;
    @cyl.=rotate(-1);
}
 
sub spin () { @cyl.=rotate: (^@cyl).pick }
 
sub fire () { @cyl.=rotate; @cyl[0] }
 
sub LSLSFSF {
    @cyl = 0 xx $shots;
    load, spin, load, spin;
    return 1 if fire;
    spin;
    fire
}
 
sub LSLSFF {
    @cyl = 0 xx $shots;
    load, spin, load, spin;
    fire() || fire
}
 
sub LLSFSF {
    @cyl = 0 xx $shots;
    load, load, spin;
    return 1 if fire;
    spin;
    fire
}
 
sub LLSFF {
    @cyl = 0 xx $shots;
    load, load, spin;
    fire() || fire
}
 
my %revolver;
my $trials = 100000;
 
for ^$trials {
    %revolver<LSLSFSF> += LSLSFSF;
    %revolver<LSLSFF>  += LSLSFF;
    %revolver<LLSFSF>  += LLSFSF;
    %revolver<LLSFF>   += LLSFF;
}
 
say "{.fmt('%7s')}: %{(%revolver{$_} / $trials × 100).fmt('%.2f')}"
  for <LSLSFSF LSLSFF LLSFSF LLSFF>
```

#### Output:
```
LSLSFSF: %55.37
 LSLSFF: %58.30
 LLSFSF: %55.42
  LLSFF: %50.29
```


Though if you go and look at the [ Wikipedia article for the 1895 Nagant revolver](https://en.wikipedia.org/wiki/Nagant_M1895) mentioned in the task reference section, you'll see it is actually a <u>**7**</u> shot revolver... so, run again with 7 chambers:



`raku roulette.raku 7`


#### Output:
```
LSLSFSF: %49.29
 LSLSFF: %51.14
 LLSFSF: %48.74
  LLSFF: %43.08
```


Or, how about a [Ruger GP100 10 round revolver](https://en.wikipedia.org/wiki/Ruger_GP100#Specifications)?



`raku roulette.raku 10`


#### Output:
```
LSLSFSF: %36.00
 LSLSFF: %37.00
 LLSFSF: %36.13
  LLSFF: %29.77
```


Doesn't change the answers, B (LSLSFF) is definitely the <strike>worst</strike> most likely choice in all cases.
