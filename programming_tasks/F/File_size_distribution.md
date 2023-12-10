[1]: https://rosettacode.org/wiki/File_size_distribution

# [File size distribution][1]





By default, process the current and all readable sub-directories, or, pass in a directory path at the command line.

```perl
sub MAIN($dir = '.') {
    sub log10 (Int $s) { $s ?? $s.log(10).Int !! 0 }
    my %fsize;
    my @dirs = $dir.IO;
    while @dirs {
        for @dirs.pop.dir -> $path {
            %fsize{$path.s.&log10}++ if $path.f;
            @dirs.push: $path if $path.d and $path.r
        }
    }
    my $max = %fsize.values.max;
    my $bar-size = 80;
    say "File size distribution in bytes for directory: $dir\n";
    for 0 .. %fsize.keys.max {
          say sprintf( "# Files @ %5sb %8s: ", $_ ?? "10e{$_-1}" !! 0, %fsize{$_} // 0 ),
              histogram( $max, %fsize{$_} // 0, $bar-size )
    }
    say %fsize.values.sum, ' total files.';
}

sub histogram ($max, $value, $width = 60) {
    my @blocks = <| ▏ ▎ ▍ ▌ ▋ ▊ ▉ █>;
    my $scaled = ($value * $width / $max).Int;
    my ($end, $bar) = $scaled.polymod(8);
    (@blocks[8] x $bar * 8) ~ (@blocks[$end] if $end) ~ "\n"
}
```

#### Output:
```
File size distribution in bytes for directory: /home

# Files @     0b      989: ▏

# Files @  10e0b     6655: ████████

# Files @  10e1b    31776: ████████████████████████████████████████

# Files @  10e2b    63165: ████████████████████████████████████████████████████████████████████████████████

# Files @  10e3b    19874: ████████████████████████▏

# Files @  10e4b     7730: ████████▏

# Files @  10e5b     3418: ▌

# Files @  10e6b     1378: ▏

# Files @  10e7b      199:

# Files @  10e8b       45:

135229 total files.
```
