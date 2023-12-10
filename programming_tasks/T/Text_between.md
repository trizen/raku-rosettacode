[1]: https://rosettacode.org/wiki/Text_between

# [Text between][1]





It seems somewhat pointless to write a special purpose routine to do text matching as built-in primitives can do so more flexibly and concisely, but whatever.



This version doesn't use strings for meta indexes ('start' and 'end'), rather it accepts regex assertions which are parsed differently from strings. This allows much more robust and fine grained control over what does and doesn't match. (and allows delimiter strings of 'start' and 'end' incidentally.) See the 11th example below which will confound nearly all of the current string-only based implementations.

```perl
sub text-between ( $text, $start, $end ) {
    return $/»[0]».Str if $text ~~ m:g/ $start (.*?) $end /;
    []
}

# Testing
my $text = 'Hello Rosetta Code world';

# String start and end delimiter
put '1> ', $text.&text-between( 'Hello ', ' world' );

# Regex string start delimiter
put '2> ', $text.&text-between( rx/^/, ' world' );

# Regex string end delimiter
put '3> ', $text.&text-between( 'Hello ',  rx/$/ );

# End delimiter only valid after start delimiter
put '4> ', '</div><div style="chinese">你好嗎</div>'\
    .&text-between( '<div style="chinese">', '</div>' );

# End delimiter not found, default to string end
put '5> ', '<text>Hello <span>Rosetta Code</span> world</text><table style="myTable">'\
    .&text-between( '<text>', rx/'<table>' | $/ );

# Start delimiter not found, return blank string
put '6> ', '<table style="myTable"><tr><td>hello world</td></tr></table>'\
    .&text-between( '<table>', '</table>' );

# Multiple end delimiters, match frugally
put '7> ', 'The quick brown fox jumps over the lazy other fox'\
    .&text-between( 'quick ', ' fox' );

# Multiple start delimiters, match frugally
put '8> ', 'One fish two fish red fish blue fish'\
    .&text-between( 'fish ', ' red' );

# Start delimiter is end delimiter
put '9> ', 'FooBarBazFooBuxQuux'\
    .&text-between( 'Foo', 'Foo' );

# Return all matching strings when multiple matches are possible
put '10> ', join ',', $text.&text-between( 'e', 'o' );

# Ignore start and end delimiter string embedded in longer words
put '11> ', 'Soothe a guilty conscience today, string wrangling is not the best tool to use for this job.'\
    .&text-between( rx/«'the '/, rx/' to'»/ );
```

#### Output:
```
1> Rosetta Code
2> Hello Rosetta Code
3> Rosetta Code world
4> 你好嗎
5> Hello <span>Rosetta Code</span> world</text><table style="myTable">
6> 
7> brown
8> two fish
9> BarBaz
10> ll,tta C, w
11> best tool
```
