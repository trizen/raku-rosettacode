[1]: https://rosettacode.org/wiki/Keyboard_input/Keypress_check

# [Keyboard input/Keypress check][1]

```perl
use Term::ReadKey;
 
react {
    whenever key-pressed(:!echo) {
        given .fc {
            when 'q' { done }
            default { .uniname.say }
        }
    }
}
```