[1]: http://rosettacode.org/wiki/Input/Output_for_Pairs_of_Numbers

# [Input/Output for Pairs of Numbers][1]

```perl
for ^get() { say [+] get.words }
```


This does more than the task asks. It will sum as many numbers as you care to put on each line, and the numbers need not be integers, but may also be a mix of rational, floating-point, or complex numbers. More subtly, <tt>get</tt> can read from a file specified as a command-line argument, but defaults to taking STDIN if no filename is specified.