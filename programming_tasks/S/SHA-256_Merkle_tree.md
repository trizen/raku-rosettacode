[1]: https://rosettacode.org/wiki/SHA-256_Merkle_tree

# [SHA-256 Merkle tree][1]

```perl
use Digest::SHA256::Native;

unit sub MAIN(Int :b(:$block-size) = 1024 × 1024, *@args);

my $in = @args ?? IO::CatHandle.new(@args) !! $*IN;

my @blocks = do while my $block = $in.read: $block-size { sha256 $block };

while @blocks > 1 {
  @blocks = @blocks.batch(2).map: { $_ > 1 ?? sha256([~] $_) !! .[0] }
}

say @blocks[0]».fmt('%02x').join;
```

#### Output:
```
$ sha256tree --block-size=1024 title.png
a4f902cf9d51fe51eda156a6792e1445dff65edf3a217a1f3334cc9cf1495c2c
```
