[1]: https://rosettacode.org/wiki/Loops/While

# [Loops/While][1]

Here is a straightforward translation of the task description:

```raku
my $n = 1024; while $n > 0 { say $n; $n div= 2 }
```


The same thing with a C-style loop and a bitwise shift operator:

```raku
loop (my $n = 1024; $n > 0; $n +>= 1) { say $n }
```


And here's how you'd <em>really</em> write it, using a sequence operator that intuits the division for you:

```raku
.say for 1024, 512, 256 ... 1
```