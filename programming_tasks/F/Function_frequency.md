[1]: https://rosettacode.org/wiki/Function_frequency

# [Function frequency][1]

Here we just examine the ast of the Perl 6 compiler (which is written in Perl 6) to look for function calls.

```perl
my $text = qqx[perl6 --target=ast @*ARGS[]];
my %fun;
for $text.lines {
    %fun{$0}++ if / '(call &' (.*?) ')' /
}
 
for %fun.invert.sort.reverse[^10] { .value.say }
```


Here we run it on the strand sort RC entry. Note how Perl 6 considers various operators to really be function calls underneath.


#### Output:
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