[1]: http://rosettacode.org/wiki/File_input/output

# [File input/output][1]

```perl
my $in = open "input.txt";
my $out = open "output.txt", :w;
for $in.lines -> $line {
	$out.say($line);
}
```


or, using the new slurp operator:

```perl
(open "output.txt", :w).print(slurp "input.txt")
```