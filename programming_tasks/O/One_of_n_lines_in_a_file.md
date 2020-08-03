[1]: https://rosettacode.org/wiki/One_of_n_lines_in_a_file

# [One of n lines in a file][1]

```perl
sub one_of_n($n) {
    my $choice;
    $choice = $_ if .rand < 1 for 1 .. $n;
    $choice - 1;
}
 
sub one_of_n_test($n = 10, $trials = 1_000_000) {
    my @bins;
    @bins[one_of_n($n)]++ for ^$trials;
    @bins;
}
 
say one_of_n_test();
```


Output:


#### Output:
```
100288 100047 99660 99773 100256 99633 100161 100483 99789 99910
```