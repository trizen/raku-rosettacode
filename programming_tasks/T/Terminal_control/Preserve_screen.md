[1]: https://rosettacode.org/wiki/Terminal_control/Preserve_screen

# [Terminal control/Preserve screen][1]



```perl
print "\e[?1049h\e[H";
say "Alternate buffer!";

for 5,4...1 {
    print "\rGoing back in: $_";
    sleep 1;
}

print "\e[?1049l";
```
