[1]: https://rosettacode.org/wiki/Draw_a_clock

# [Draw a clock][1]



```perl
my ($rows,$cols) = qx/stty size/.words;
my $v = floor $rows / 2;
my $h = floor $cols / 2 - 16;

my @t = < ⡎⢉⢵ ⠀⢺⠀ ⠊⠉⡱ ⠊⣉⡱ ⢀⠔⡇ ⣏⣉⡉ ⣎⣉⡁ ⠊⢉⠝ ⢎⣉⡱ ⡎⠉⢱ ⠀⠶⠀>;
my @b = < ⢗⣁⡸ ⢀⣸⣀ ⣔⣉⣀ ⢄⣀⡸ ⠉⠉⡏ ⢄⣀⡸ ⢇⣀⡸ ⢰⠁⠀ ⢇⣀⡸ ⢈⣉⡹ ⠀⠶⠀>;

loop {
    my @x = DateTime.now.Str.substr(11,8).ords X- ord('0');
    print "\e[H\e[J";
    print "\e[$v;{$h}H";
    print ~@t[@x];
    print "\e[{$v+1};{$h}H";
    print ~@b[@x];
    print "\e[H";
    sleep 1;
}
```

#### Output:
```
⠀⢺⠀ ⢀⠔⡇ ⠀⠶⠀ ⠊⠉⡱ ⠊⣉⡱ ⠀⠶⠀ ⣏⣉⡉ ⡎⢉⢵
⢀⣸⣀ ⠉⠉⡏ ⠀⠶⠀ ⣔⣉⣀ ⢄⣀⡸ ⠀⠶⠀ ⢄⣀⡸ ⢗⣁⡸
```


A simpler version that does not clear the screen:

```perl
constant @t = < ⡎⢉⢵ ⠀⢺⠀ ⠊⠉⡱ ⠊⣉⡱ ⢀⠔⡇ ⣏⣉⡉ ⣎⣉⡁ ⠊⢉⠝ ⢎⣉⡱ ⡎⠉⢱ ⠀⠶⠀>;
constant @b = < ⢗⣁⡸ ⢀⣸⣀ ⣔⣉⣀ ⢄⣀⡸ ⠉⠉⡏ ⢄⣀⡸ ⢇⣀⡸ ⢰⠁⠀ ⢇⣀⡸ ⢈⣉⡹ ⠀⠶⠀>;

signal(SIGINT).tap: { print "\e[?25h\n"; exit }
print "\e7\e[?25l"; # saves cursor position, make it invisible
loop {
    my @x = DateTime.now.Str.substr(11,8).ords X- ord('0');
    put ~.[@x] for @t, @b;
    sleep 1;
    print "\e8"; # restores cursor position
}
```


Finally a more minimalist version that shows three progress bars (hours, minutes, seconds):

```perl
print "\e7";
loop {
    my $time = DateTime.now;
    put '#' x $_ ~ '.' x (24 - $_) given $time.hour.round;
    put '#' x $_ ~ '.' x (60 - $_) given $time.minute.round;
    put '#' x $_ ~ '.' x (60 - $_) given $time.second.round;
    sleep 1;
    print "\e8";
}
END put "\n";
```
