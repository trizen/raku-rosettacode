[1]: https://rosettacode.org/wiki/Here_document

# [Here document][1]

Heredocs in Perl 6 use the `:to` modifier to a quoting operator,
such as `q` or `qq`.
The indentation of the end marker is removed from every line.

```perl
my $color = 'green';
say qq :to 'END';
    some line
    color: $color
    another line
    END
```

#### Output:
```
some line
color: green
another line
```


Note that the quotes around the "END" are not magic --- the marker is just a regular string; it's the \`q\` or \`qq\` that decides whether or not the heredoc interpolates.



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


Both q and qq are specialised forms of [Q](http://design.perl6.org/S02.html#Q_forms) which comes with many adverbs. Here a heredoc that only interpolates @-sigils.

```perl
my @a = <1 2 3 4>;
say Q :array :to 'EOH';
    123 \n '"`
        @a$bc
        @a[]
    EOH
```

#### Output:
```
123 \n '"`
    @a$bc
    1 2 3 4
```