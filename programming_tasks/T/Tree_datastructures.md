[1]: https://rosettacode.org/wiki/Tree_datastructures

# [Tree datastructures][1]

Code golf is a entertaining passtime, even if it isn't appropriate for this site. To a large extent, I agree with [Hout](https://rosettacode.org/mw/index.php?title=User:Hout&amp;action=edit&amp;redlink=1), I am not really on board with mocking anybody, especially espousing it as an official RosettaCode position. So, feel free to mark this incorrect.

```raku
#`(
Sort of vague as to what we are trying to accomplish here. If we are just
trying to transform from one format to another, probably easiest to just
perform string manipulations.
)
 
my $level = '  ';
 
my $trees = q:to/END/;
    RosettaCode
      encourages
        code
          diversity
          comparison
      discourages
        golfing
        trolling
        emphasising execution speed
    code-golf.io
      encourages
        golfing
      discourages
        comparison
    END
 
sub nested-to-indent { $^str.subst: / ^^ ($($level))* /, -> $/ { "{+$0} " }, :g }
sub indent-to-nested { $^str.subst: / ^^ (\d+) \s* /, -> $/ { "{$level x +$0}" }, :g }
 
say $trees;
say my $indent = $trees.&nested-to-indent;
say my $nest = $indent.&indent-to-nested;
 
use Test;
is($trees, $nest, 'Round-trip equals original');
 
#`(
If, on the other hand, we want perform more complex transformations; better to
load it into a native data structure which will then allow us to manipulate it
however we like.
)
 
# Import outline paragraph into native data structure
sub import (Str $trees, $level = '  ') {
    my $forest;
    my $last = -Inf;
 
    for $trees.lines -> $branch {
        $branch ~~ / ($($level))* /;
        my $this = +$0;
        $forest ~= do {
            given $this cmp $last {
                when More { "\['{esc $branch.trim}', " }
                when Same { "'{esc $branch.trim}', " }
                when Less { "{']' x $last - $this}, '{esc $branch.trim}', " }
            }
        }
        $last = $this;
    }
 
    sub esc { $^s.subst( /(<['\\]>)/, -> $/ { "\\$0" }, :g) }
 
    $forest ~= ']' x 1 + $last;
    use MONKEY-SEE-NO-EVAL;
    $forest.EVAL;
}
 
my $forest = import $trees;
 
say "\nNative data structure:\n", $forest.perl;
 
{
    use JSON::Fast;
    say "\nJSON:\n", $forest.&to-json;
}
 
{
    use YAML;
    say "\nYAML:\n", $forest.&dump;
}
```

#### Output:
```
RosettaCode
  encourages
    code
      diversity
      comparison
  discourages
    golfing
    trolling
    emphasising execution speed
code-golf.io
  encourages
    golfing
  discourages
    comparison

0 RosettaCode
1 encourages
2 code
3 diversity
3 comparison
1 discourages
2 golfing
2 trolling
2 emphasising execution speed
0 code-golf.io
1 encourages
2 golfing
1 discourages
2 comparison

RosettaCode
  encourages
    code
      diversity
      comparison
  discourages
    golfing
    trolling
    emphasising execution speed
code-golf.io
  encourages
    golfing
  discourages
    comparison

ok 1 - Round-trip equals original

Native data structure:
$["RosettaCode", ["encourages", ["code", ["diversity", "comparison"]], "discourages", ["golfing", "trolling", "emphasising execution speed"]], "code-golf.io", ["encourages", ["golfing"], "discourages", ["comparison"]]]

JSON:
[
  "RosettaCode",
  [
    "encourages",
    [
      "code",
      [
        "diversity",
        "comparison"
      ]
    ],
    "discourages",
    [
      "golfing",
      "trolling",
      "emphasising execution speed"
    ]
  ],
  "code-golf.io",
  [
    "encourages",
    [
      "golfing"
    ],
    "discourages",
    [
      "comparison"
    ]
  ]
]

YAML:
---
- RosettaCode
- - encourages
  - - code
    - - diversity
      - comparison
  - discourages
  - - golfing
    - trolling
    - emphasising execution speed
- code-golf.io
- - encourages
  - - golfing
  - discourages
  - - comparison
...
```
