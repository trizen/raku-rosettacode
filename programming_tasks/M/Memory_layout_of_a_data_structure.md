[1]: http://rosettacode.org/wiki/Memory_layout_of_a_data_structure

# [Memory layout of a data structure][1]

The following is specced to work, but implementation of shaped arrays is not quite complete.

```perl6
enum T_RS232 <
    carrier_detect
    received_data
    transmitted_data
    data_terminal_ready
    signal_ground
    data_set_ready
    request_to_send
    clear_to_send
    ring_indicator
>;
 
my bit @signal[T_RS232];
 
@signal[signal_ground] = 1;
```


In the absence of shaped arrays, you can do the usual bit-twiddling tricks on a native integer of sufficient size. (Such an integer could presumably be mapped directly to a device register.)

```perl6
$signal +|= 1 +< signal_ground;
```


Using a native int is likelier to work on a big-endian machine in any case. Another almost-there solution is the mapping of C representational types into Perl 6 for native interfaces, but it does not yet support bit fields.