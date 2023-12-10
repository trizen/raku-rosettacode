[1]: https://rosettacode.org/wiki/Terminal_control/Hiding_the_cursor

# [Terminal control/Hiding the cursor][1]



```perl
say 'Hiding the cursor for 5 seconds...';
run 'tput', 'civis';
sleep 5;
run 'tput', 'cvvis';
```
