[1]: https://rosettacode.org/wiki/Largest_number_divisible_by_its_digits

# [Largest number divisible by its digits][1]

### Base 10



The number can not have a zero in it, that implies that it can not have a 5 either since if it has a 5, it must be divisible by 5, but the only numbers divisible by 5 end in 5 or 0. It can't be zero, and if it is odd, it can't be divisible by 2, 4, 6 or 8. So that leaves 98764321 as possible digits the number can contain. The sum of those 8 digits is not divisible by three so the largest possible integer must use no more than 7 of them (since 3, 6 and 9 would be eliminated). Strictly by removing possibilities that cannot possibly work we are down to at most 7 digits.



We can deduce that the digit that won't get used is one of 1, 4, or 7 since those are the only ones where the removal will yield a sum divisible by 3. It is *extremely* unlikely be 1, since EVERY number is divisible by 1. Removing it reduces the number of digits available but doesn't gain anything as far as divisibility. It is unlikely to be 7 since 7 is prime and can't be made up of multiples of other numbers. Practically though, the code to accommodate these observations is longer running and more complex than just brute-forcing it from here.



In order to accommodate the most possible digits, the number must be divisible by 7, 8 and 9. If that is true then it is automatically divisible by 2, 3, 4, &amp; 6 as they can all be made from the combinations of multiples of 2 and 3 which are present in 8 &amp; 9; so we'll only bother to check multiples of 9 \* 8 \* 7 or 504.



All these optimizations get the run time to well under 1 second.

```raku
my $magic-number = 9 * 8 * 7;                        # 504
 
my $div = 9876432 div $magic-number * $magic-number; # largest 7 digit multiple of 504 < 9876432
 
for $div, { $_ - $magic-number } ... * -> $test {    # only generate multiples of 504
    next if $test ~~ / <[05]> /;                     # skip numbers containing 0 or 5
    next if $test ~~ / (.).*$0 /;                    # skip numbers with non unique digits
    next unless all $test.comb.map: $test %% *;      # skip numbers that don't divide evenly by all digits
 
    say "Found $test";                               # Found a solution, display it
    for $test.comb {
        printf "%s / %s = %s\n", $test, $_, $test / $_;
    }
    last
}
```

#### Output:
```
Found 9867312
9867312 / 9 = 1096368
9867312 / 8 = 1233414
9867312 / 6 = 1644552
9867312 / 7 = 1409616
9867312 / 3 = 3289104
9867312 / 1 = 9867312
9867312 / 2 = 4933656
```


### Base 16



There are fewer analytical optimizations available for base 16. Other than 0, no digits can be ruled out so a much larger space must be searched. We'll start at the largest possible permutation (FEDCBA987654321) and work down so as soon as we find **a** solution, we know it is **the** solution.

```raku
my $hex = 'FEDCBA987654321';        # largest possible hex number
my $magic-number = [lcm] 1 .. 15;   # find least common multiple
my $div = :16($hex) div $magic-number * $magic-number;
 
for $div, { $_ - $magic-number } ... 0 -> $num {  # Only generate multiples of the lcm 
    my $test = $num.base(16);
 
    next if $test ~~ / 0 /;        # skip numbers containing 0
    next if $test ~~ / (.).*$0 /;  # skip numbers with non unique digits
 
    say "Found $test";             # Found a solution, display it
    say ' ' x 12, 'In base 16', ' ' x 36, 'In base 10';
    for $test.comb {
        printf "%s / %s = %s  |  %d / %2d = %19d\n",
          $test, $_, ($num / :16($_)).base(16),
          $num, :16($_), $num / :16($_);
    }
    last
}
```

#### Output:
```
Found FEDCB59726A1348
            In base 16                                    In base 10
FEDCB59726A1348 / F = 10FDA5B4BE4F038  |  1147797065081426760 / 15 =   76519804338761784
FEDCB59726A1348 / E = 1234561D150B83C  |  1147797065081426760 / 14 =   81985504648673340
FEDCB59726A1348 / D = 139AD2E43E0C668  |  1147797065081426760 / 13 =   88292081929340520
FEDCB59726A1348 / C = 153D0F21EDE2C46  |  1147797065081426760 / 12 =   95649755423452230
FEDCB59726A1348 / B = 172B56538F25ED8  |  1147797065081426760 / 11 =  104345187734675160
FEDCB59726A1348 / 5 = 32F8F11E3AED0A8  |  1147797065081426760 /  5 =  229559413016285352
FEDCB59726A1348 / 9 = 1C5169829283B08  |  1147797065081426760 /  9 =  127533007231269640
FEDCB59726A1348 / 7 = 2468AC3A2A17078  |  1147797065081426760 /  7 =  163971009297346680
FEDCB59726A1348 / 2 = 7F6E5ACB93509A4  |  1147797065081426760 /  2 =  573898532540713380
FEDCB59726A1348 / 6 = 2A7A1E43DBC588C  |  1147797065081426760 /  6 =  191299510846904460
FEDCB59726A1348 / A = 197C788F1D76854  |  1147797065081426760 / 10 =  114779706508142676
FEDCB59726A1348 / 1 = FEDCB59726A1348  |  1147797065081426760 /  1 = 1147797065081426760
FEDCB59726A1348 / 3 = 54F43C87B78B118  |  1147797065081426760 /  3 =  382599021693808920
FEDCB59726A1348 / 4 = 3FB72D65C9A84D2  |  1147797065081426760 /  4 =  286949266270356690
FEDCB59726A1348 / 8 = 1FDB96B2E4D4269  |  1147797065081426760 /  8 =  143474633135178345
```