[1]: http://rosettacode.org/wiki/Terminal_control/Hiding_the_cursor

# [Terminal control/Hiding the cursor][1]

```perl6
run 'tput', 'civis';
sleep 5;
run 'tput', 'cvvis';
```