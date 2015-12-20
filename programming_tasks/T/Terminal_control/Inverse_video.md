[1]: http://rosettacode.org/wiki/Terminal_control/Inverse_video

# [Terminal control/Inverse video][1]

```perl6
say "normal";
run "tput", "rev";
say "reversed";
run "tput", "sgr0";
say "normal";
```