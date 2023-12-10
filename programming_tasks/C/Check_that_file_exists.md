[1]: https://rosettacode.org/wiki/Check_that_file_exists

# [Check that file exists][1]



```perl
my $path = "/etc/passwd";
say $path.IO.e ?? "Exists" !! "Does not exist";

given $path.IO {
    when :d { say "$path is a directory"; }
    when :f { say "$path is a regular file"; }
    when :e { say "$path is neither a directory nor a file, but it does exist"; }
    default { say "$path does not exist" }
}
```


`when` internally uses the smart match operator `~~`, so `when :e` really does `$given ~~ :e` instead of the method call `$given.e`; both test whether the file exists.

```perl
run ('touch', "♥ Unicode.txt");

say "♥ Unicode.txt".IO.e;      # "True"
say "♥ Unicode.txt".IO ~~ :e;  # same
```
