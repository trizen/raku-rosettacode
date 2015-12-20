[1]: http://rosettacode.org/wiki/Inverted_index

# [Inverted index][1]

```perl
sub MAIN (*@files) {
    my %norm; 
    do for @files -> $file {
        %norm.push: $file X=> slurp($file).lc.words;
    }
    (my %inv).push: %norm.invert.unique;
 
    while prompt("Search terms: ").words -> @words {
        for @words -> $word {
            say "$word => {%inv.{$word.lc}//'(not found)'}";
        }
    }
}
```