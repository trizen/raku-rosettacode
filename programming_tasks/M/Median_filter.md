[1]: https://rosettacode.org/wiki/Median_filter

# [Median filter][1]


Clone of Perl 5, for now.

```perl
use PDL:from<Perl5>;
use PDL::Image2D:from<Perl5>;

my $image = rpic 'plasma.png';
my $smoothed = med2d($image, ones(3,3), {Boundary => 'Truncate'});
wpic $smoothed, 'plasma_median.png';
```


Compare offsite images: [plasma.png](https://github.com/SqrtNegInf/Rosettacode-Perl6-Smoke/blob/master/ref/plasma-perl6.png) vs.
[plasma_median.png](https://github.com/SqrtNegInf/Rosettacode-Perl6-Smoke/blob/master/ref/plasma_median.png)
