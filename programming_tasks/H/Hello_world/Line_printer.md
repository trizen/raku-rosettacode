[1]: http://rosettacode.org/wiki/Hello_world/Line_printer

# [Hello world/Line printer][1]

```perl6
given open '/dev/lp0', :w { # Open the device for writing as the default
    .say('Hello World!');              # Send it the string
    .close;
#   ^ The prefix "." says "use the default device here"
}
```