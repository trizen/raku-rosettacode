[1]: https://rosettacode.org/wiki/Longest_increasing_subsequence

# [Longest increasing subsequence][1]

### Dynamic programming



Straight-forward implementation of the algorithm described in the video.

```raku
sub lis(@d) {
    my @l = [].item xx @d;
    @l[0].push: @d[0];
    for 1 ..^ @d -> $i {
        for ^$i -> $j {
            if @d[$j] < @d[$i] && @l[$i] < @l[$j] + 1 {
                @l[$i] = [ @l[$j][] ]
            }
        }
        @l[$i].push: @d[$i];
    }
    return max :by(*.elems), @l;
}
 
say lis([3,2,6,4,5,1]);
say lis([0,8,4,12,2,10,6,14,1,9,5,13,3,11,7,15]);
```

#### Output:
```
[2 4 5]
[0 2 6 9 11 15]
```


### Patience sorting

```raku
sub lis(@deck is copy) {
    my @S = [@deck.shift() => Nil].item;
    for @deck -> $card {
        with first { @S[$_][*-1].key > $card }, ^@S -> $i {
            @S[$i].push: $card => @S[$i-1][*-1] // Nil
        } else {
            @S.push: [ $card => @S[*-1][*-1] // Nil ].item
        }
    }
    reverse map *.key, (
        @S[*-1][*-1], *.value ...^ !*.defined
    )
}
 
say lis <3 2 6 4 5 1>;
say lis <0 8 4 12 2 10 6 14 1 9 5 13 3 11 7 15>;
```

#### Output:
```
[2 4 5]
[0 2 6 9 11 15]
```