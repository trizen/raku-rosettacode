[1]: http://rosettacode.org/wiki/Huffman_coding

# [Huffman coding][1]

This version uses nested `Array`s to build a tree [like shown in this diagram](https://commons.wikimedia.org/wiki/File:HuffmanCodeAlg.png), and then recursively traverses the finished tree to accumulate the prefixes.

```perl
sub huffman (%frequencies) {
    my @queue = %frequencies.map({ [.value, .key] }).sort;
    while @queue > 1 {
        given @queue.splice(0, 2) -> ([$freq1, $node1], [$freq2, $node2]) {
            @queue = (|@queue, [$freq1 + $freq2, [$node1, $node2]]).sort;
        }
    }
    hash gather walk @queue[0][1], '';
}
 
multi walk ($node,            $prefix) { take $node => $prefix; }
multi walk ([$node1, $node2], $prefix) { walk $node1, $prefix ~ '0';
                                         walk $node2, $prefix ~ '1'; }
```


This version uses an `Array` of `Pair`s to implement a simple priority queue. Each value of the queue is a `Hash` mapping from letters to prefixes, and when the queue is reduced the hashes are merged on-the-fly, so that the last one remaining is the wanted Huffman table.

```perl
sub huffman (%frequencies) {
    my @queue = %frequencies.map: { .value => (hash .key => '') };
    while @queue > 1 {
        @queue.=sort;
        my $x = @queue.shift;
        my $y = @queue.shift;
        @queue.push: ($x.key + $y.key) => hash $x.value.deepmap('0' ~ *),
                                               $y.value.deepmap('1' ~ *);
    }
    @queue[0].value;
}
```
```perl
for huffman 'this is an example for huffman encoding'.comb.Bag {
    say "'{.key}' : {.value}";
}
```

#### Output:
```
'x' : 11000
'p' : 01100
'h' : 0001
'g' : 00000
'a' : 1001
'e' : 1101
'd' : 110011
's' : 0111
'f' : 1110
'c' : 110010
'm' : 0010
' ' : 101
'n' : 010
'o' : 0011
'u' : 10001
't' : 10000
'i' : 1111
'r' : 01101
'l' : 00001
```


To demonstrate that the table can do a round trip:

```perl
my $original = 'this is an example for huffman encoding';
 
my %encode-key = huffman $original.comb.Bag;
my %decode-key = %encode-key.invert;
my @codes      = %decode-key.keys;
 
my $encoded = $original.subst: /./,      { %encode-key{$_} }, :g;
my $decoded = $encoded .subst: /@codes/, { %decode-key{$_} }, :g;
 
.say for $original, $encoded, $decoded;
```

#### Output:
```
this is an example for huffman encoding
1000000011111011110111110111101100101010111011100010010010011000000111011011110001101101101000110001111011100010100101010111010101100100011110011111101000000
this is an example for huffman encoding
```
