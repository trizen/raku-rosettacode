[1]: https://rosettacode.org/wiki/Walk_a_directory/Recursively

# [Walk a directory/Recursively][1]

Using the [File::Find](https://github.com/tadzik/File-Find/) module:

```perl
use File::Find;
 
.say for find dir => '.', name => /'.txt' $/;
```


Alternatively, a custom solution that provides the same interface as the built-in (non-recursive) `dir` function, and uses `gather`/`take` to return a lazy sequence:

```perl
sub find-files ($dir, Mu :$test) {
    gather for dir $dir -> $path {
        if $path.basename ~~ $test { take $path }
        if $path.d                 { .take for find-files $path, :$test };
    }
}
 
.put for find-files '.', test => /'.txt' $/;
```


Or if you value performance over portability, here's a function that runs the GNU `find` program and returns a lazy sequence of the files it finds. Parameters are not subjected to shell expansion, and the null-byte (which cannot be present in file paths) is used as the path delimiter, so it should be pretty safe.

```perl
sub find-files ($dir, :$pattern) {
    run('find', $dir, '-iname', $pattern, '-print0', :out, :nl«\0»).out.lines;
}
 
.say for find-files '.', pattern => '*.txt';
```