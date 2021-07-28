[1]: https://rosettacode.org/wiki/Tau_function

# [Tau function][1]

Yet more tasks that are tiny variations of each other. <strong class="selflink">Tau function</strong>, [Tau number](https://rosettacode.org/wiki/Tau_number), [Sum of divisors](https://rosettacode.org/wiki/Sum_of_divisors) and [Product of divisors](https://rosettacode.org/wiki/Product_of_divisors) all use code with minimal changes. What the heck, post 'em all.

```perl
use Prime::Factor:ver<0.3.0+>;
use Lingua::EN::Numbers;
 
say "\nTau function - first 100:\n",        # ID
(1..*).map({ +.&divisors })[^100]\          # the task
.batch(20)».fmt("%3d").join("\n");          # display formatting
 
say "\nTau numbers - first 100:\n",         # ID
(1..*).grep({ $_ %% +.&divisors })[^100]\   # the task
.batch(10)».&comma».fmt("%5s").join("\n");  # display formatting
 
say "\nDivisor sums - first 100:\n",        # ID
(1..*).map({ [+] .&divisors })[^100]\       # the task
.batch(20)».fmt("%4d").join("\n");          # display formatting
 
say "\nDivisor products - first 100:\n",    # ID
(1..*).map({ [×] .&divisors })[^100]\       # the task
.batch(5)».&comma».fmt("%16s").join("\n");  # display formatting
```

#### Output:
```
Tau function - first 100:
  1   2   2   3   2   4   2   4   3   4   2   6   2   4   4   5   2   6   2   6
  4   4   2   8   3   4   4   6   2   8   2   6   4   4   4   9   2   4   4   8
  2   8   2   6   6   4   2  10   3   6   4   6   2   8   4   8   4   4   2  12
  2   4   6   7   4   8   2   6   4   8   2  12   2   4   6   6   4   8   2  10
  5   4   2  12   4   4   4   8   2  12   4   6   4   4   4  12   2   6   6   9

Tau numbers - first 100:
    1     2     8     9    12    18    24    36    40    56
   60    72    80    84    88    96   104   108   128   132
  136   152   156   180   184   204   225   228   232   240
  248   252   276   288   296   328   344   348   360   372
  376   384   396   424   441   444   448   450   468   472
  480   488   492   504   516   536   560   564   568   584
  600   612   625   632   636   640   664   672   684   708
  712   720   732   776   792   804   808   824   828   852
  856   864   872   876   880   882   896   904   936   948
  972   996 1,016 1,040 1,044 1,048 1,056 1,068 1,089 1,096

Divisor sums - first 100:
   1    3    4    7    6   12    8   15   13   18   12   28   14   24   24   31   18   39   20   42
  32   36   24   60   31   42   40   56   30   72   32   63   48   54   48   91   38   60   56   90
  42   96   44   84   78   72   48  124   57   93   72   98   54  120   72  120   80   90   60  168
  62   96  104  127   84  144   68  126   96  144   72  195   74  114  124  140   96  168   80  186
 121  126   84  224  108  132  120  180   90  234  112  168  128  144  120  252   98  171  156  217

Divisor products - first 100:
               1                2                3                8                5
              36                7               64               27              100
              11            1,728               13              196              225
           1,024               17            5,832               19            8,000
             441              484               23          331,776              125
             676              729           21,952               29          810,000
              31           32,768            1,089            1,156            1,225
      10,077,696               37            1,444            1,521        2,560,000
              41        3,111,696               43           85,184           91,125
           2,116               47      254,803,968              343          125,000
           2,601          140,608               53        8,503,056            3,025
       9,834,496            3,249            3,364               59   46,656,000,000
              61            3,844          250,047        2,097,152            4,225
      18,974,736               67          314,432            4,761       24,010,000
              71  139,314,069,504               73            5,476          421,875
         438,976            5,929       37,015,056               79    3,276,800,000
          59,049            6,724               83  351,298,031,616            7,225
           7,396            7,569       59,969,536               89  531,441,000,000
           8,281          778,688            8,649            8,836            9,025
 782,757,789,696               97          941,192          970,299    1,000,000,000
```
