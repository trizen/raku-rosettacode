[1]: http://rosettacode.org/wiki/Test_a_function

# [Test a function][1]

```perl
use Test;
 
my %tests =
    'A man, a plan, a canal: Panama.'           => True,
    'My dog has fleas'                          => False,
    "Madam, I'm Adam."                          => True,
    '1 on 1'                                    => False,
    'In girum imus nocte et consumimur igni'    => True,
    ''                                          => True,
    ;
 
plan %tests.elems;
 
for %tests.kv -> $test, $expected-result {
    is palin($test), $expected-result,
        "\"$test\" is {$expected-result??''!!'not '}a palindrome.";
}
```


Output:


#### Output:
```
1..6
ok 1 - "1 on 1" is not a palindrome.
ok 2 - "My dog has fleas" is not a palindrome.
ok 3 - "A man, a plan, a canal: Panama." is a palindrome.
ok 4 - "" is a palindrome.
ok 5 - "Madam, I'm Adam." is a palindrome.
ok 6 - "In girum imus nocte et consumimur igni" is a palindrome.
```