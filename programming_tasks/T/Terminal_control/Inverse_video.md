[1]: https://rosettacode.org/wiki/Terminal_control/Inverse_video

# [Terminal control/Inverse video][1]

```perl
say "normal";
run "tput", "rev";
say "reversed";
run "tput", "sgr0";
say "normal";
```