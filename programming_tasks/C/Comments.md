[1]: https://rosettacode.org/wiki/Comments

# [Comments][1]





**Single-line comments**



A single-line comment starts with # and extends to the end of the line.

```perl
# the answer to everything
my $x = 42;
```


**Multi-line comments**



A multi-line comment starts with #\` and followed by the commented text enclosed by bracketing characters (e.g., (), [], {}, 「」, etc.).

```perl
#`( 
    Comments beginning with a backtick and one or more opening bracketing characters are embedded comments.
    They can span more than one line…
)

my $y = #`{ …or only part of a line. } 3;
```


Multi-line comments can also be embedded into code.

```perl
for #`(each element in) my @array {
    say #`(or print element) $_ #`(with a newline);
}
```


Using more than one bracketing character lets you include an unmatched close bracket, as shown below.

```perl
#`{{
  This close curly brace } won't terminate the comment early.
}}
```


**Pod comments**

```perl
=begin comment

Pod is the successor to Perl 5's POD. This is the simplest way to use it for multi-line comments.
For more about Pod, see Pod: https://docs.perl6.org/language/pod

=end comment
```


Pod also provides declarator blocks which are special comments that attach to some source code and can be extracted as documentation. They are either #| or #= and must be immediately followed by either a space or an opening curly brace. In short, blocks starting with #| are attached to the code after them, and blocks starting with #= are attached to the code before them.

```perl
#| Compute the distance between two points in the plane.
sub distance(
    Rat \x1, #= First point's abscissa,
    Rat \y1, #= First point's ordinate, 
    Rat \x2, #= Second point's abscissa, 
    Rat \y2, #= Second point's ordinate, 
){
    return sqrt((x2 - x1)**2 + (y2 - y1)**2)
}
```
