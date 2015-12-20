[1]: http://rosettacode.org/wiki/Benford's_law

# [Benford's law][1]

```perl
sub benford(@a) { bag +« flat @a».comb: /<( <[ 1..9 ]> )> <[ , . \d ]>*/ }
 
sub show(%distribution) {
    printf "%9s %9s  %s\n", <Actual Expected Deviation>;
    for 1 .. 9 -> $digit {
        my $actual = %distribution{$digit} * 100 / [+] %distribution.values;
        my $expected = (1 + 1 / $digit).log(10) * 100;
        printf "%d: %5.2f%% | %5.2f%% | %.2f%%\n",
          $digit, $actual, $expected, abs($expected - $actual);
    }
}
 
multi MAIN($file) { show benford $file.IO.lines }
multi MAIN() { show benford ( 1, 1, 2, *+* ... * )[^1000] }
```


**Output:** First 1000 Fibonaccis


#### Output:
```
   Actual  Expected  Deviation
1: 30.10% | 30.10% | 0.00%
2: 17.70% | 17.61% | 0.09%
3: 12.50% | 12.49% | 0.01%
4:  9.60% |  9.69% | 0.09%
5:  8.00% |  7.92% | 0.08%
6:  6.70% |  6.69% | 0.01%
7:  5.60% |  5.80% | 0.20%
8:  5.30% |  5.12% | 0.18%
9:  4.50% |  4.58% | 0.08%
```


**Extra credit:** Square Kilometers of land under cultivation, by country / territory. First column from Wikipedia: [Land use statistics by country](http://en.wikipedia.org/wiki/Land\_use\_statistics\_by\_country" class="extiw" title="wp:Land use statistics by country).


#### Output:
```
   Actual  Expected  Deviation
1: 33.33% | 30.10% | 3.23%
2: 18.31% | 17.61% | 0.70%
3: 13.15% | 12.49% | 0.65%
4:  8.45% |  9.69% | 1.24%
5:  9.39% |  7.92% | 1.47%
6:  5.63% |  6.69% | 1.06%
7:  4.69% |  5.80% | 1.10%
8:  5.16% |  5.12% | 0.05%
9:  1.88% |  4.58% | 2.70%
```