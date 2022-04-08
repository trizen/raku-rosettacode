[1]: https://rosettacode.org/wiki/Distinct_palindromes_within_decimal_numbers

# [Distinct palindromes within decimal numbers][1]

A minor modification of the [Longest palindromic substrings](https://rosettacode.org/wiki/Longest_palindromic_substrings#Raku) task. As such, works for any string, not just integers.

```perl
use Sort::Naturally;
 
sub getpal ($str) {
    my @chars = $str.comb;
    my @pal = flat @chars,
    (1 ..^ @chars).map: -> \idx {
        my @s;
        for 1, 2 {
           my int ($rev, $fwd) = $_, 1;
           loop {
                quietly last if ($rev > idx) || (@chars[idx - $rev] ne @chars[idx + $fwd]);
                $rev = $rev + 1;
                $fwd = $fwd + 1;
            }
            @s.push: @chars[idx - $rev ^..^ idx + $fwd].join if $rev + $fwd > 2;
            last if @chars[idx - 1] ne @chars[idx];
        }
        next unless +@s;
        @s
    }
    @pal.unique.sort({.chars, .&naturally});
}
 
say 'All palindromic substrings including (bizarrely enough) single characters:';
put "$_ => ", getpal $_ for 100..125;
put "\nDo these strings contain a minimum two character palindrome?";
printf "%25s => %s\n", $_, getpal($_).tail.chars > 1 for flat
    9, 169, 12769, 1238769, 123498769, 12346098769, 1234572098769,
    123456832098769, 12345679432098769, 1234567905432098769,
    123456790165432098769, 83071934127905179083, 1320267947849490361205695,
    <Do these strings contain a minimum two character palindrome?>
```

#### Output:
```
All palindromic substrings including (bizarrely enough) single characters:
100 => 0 1 00
101 => 0 1 101
102 => 0 1 2
103 => 0 1 3
104 => 0 1 4
105 => 0 1 5
106 => 0 1 6
107 => 0 1 7
108 => 0 1 8
109 => 0 1 9
110 => 0 1 11
111 => 1 11 111
112 => 1 2 11
113 => 1 3 11
114 => 1 4 11
115 => 1 5 11
116 => 1 6 11
117 => 1 7 11
118 => 1 8 11
119 => 1 9 11
120 => 0 1 2
121 => 1 2 121
122 => 1 2 22
123 => 1 2 3
124 => 1 2 4
125 => 1 2 5

Do these strings contain a minimum two character palindrome?
                        9 => False
                      169 => False
                    12769 => False
                  1238769 => False
                123498769 => False
              12346098769 => False
            1234572098769 => False
          123456832098769 => False
        12345679432098769 => False
      1234567905432098769 => False
    123456790165432098769 => False
     83071934127905179083 => False
1320267947849490361205695 => True
                       Do => False
                    these => True
                  strings => False
                  contain => False
                        a => False
                  minimum => True
                      two => False
                character => True
              palindrome? => False
```
