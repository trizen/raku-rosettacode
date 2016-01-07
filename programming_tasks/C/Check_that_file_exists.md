[1]: http://rosettacode.org/wiki/Check_that_file_exists

# [Check that file exists][1]

```perl
'input.txt'.IO ~~ :e;
'docs'.IO ~~ :d;
'/input.txt'.IO ~~ :e;
'/docs'.IO ~~ :d
run ('touch', "Abdu'l-Bahá.txt");
"Abdu'l-Bahá.txt".IO ~~ :e;
```