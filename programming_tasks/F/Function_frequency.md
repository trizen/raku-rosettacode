[1]: https://rosettacode.org/wiki/Function_frequency

# [Function frequency][1]


Here we just examine the ast of the Raku compiler (which is written in Raku) to look for function calls.

```perl
my $text = qqx[raku --target=ast @*ARGS[]];
my %fun;
for $text.lines {
    %fun{$0}++ if / '(call &' (.*?) ')' /
}

for %fun.invert.sort.reverse[^10] { .value.say }
```


Here we run it on the strand sort RC entry.  Note how Raku considers various operators to really be function calls underneath.


```
$ ./morefun strand
pop
postcircumfix:<[ ]>
unshift
succeed
splice
prefix:<->
push
infix:<,>
infix:<..>
infix:<->
```
