[1]: http://rosettacode.org/wiki/Here_document

# [Here document][1]

Heredocs in Perl 6 use the `:to` modifier to a quoting operator,
such as `q` or `qq`.

```perl
my $color = 'green';
my $text = qq :to 'EOT';
some line
color: $color
last line
EOT
```

#### Output:
```
some line
color: green
last line
```


(Note that the quotes around the "EOT" are not magic --- the marker is just a regular string; it's the \`q\` or \`qq\` that decides whether or not the heredoc interpolates.)



Multiple here docs may be stacked on top of each other.

```perl
my $contrived_example = 'Dylan';
sub freewheelin() {
        print q :to 'QUOTE', '-- ', qq :to 'AUTHOR';
          I'll let you be in my dream,
            if I can be in yours.
        QUOTE
                Bob $contrived_example
                AUTHOR
}
 
freewheelin;
```

#### Output:
```
  I'll let you be in my dream,
    if I can be in yours.
-- Bob Dylan
```


Both q and qq are specialised forms of [Q](http://design.perl6.org/S02.html#Q\_forms) which comes with many adverbs. Here a heredoc that only interpolates @-sigils. The lowest level of indentation is removed from every line.

```perl
 
my $s = Q :array :to 'EOH';
    123 \n '"`
        @a$bc
        @a[]
    EOH
 
dd $s; # OUTPUT«Str $var = "123 \\n '\"`\n    \@a\$bc\n    1 2 3 4\n"»
```