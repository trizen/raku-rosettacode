[1]: https://rosettacode.org/wiki/Make_directory_path

# [Make directory path][1]

There is a built-in function for this:

```perl
mkdir 'path/to/dir'
```


Alternatively, a custom solution (as per task description) that only uses the built-in `mkdir` non-recursively. The "triangle reduce" meta-operator `[\ ]` is used get the intermediate results of a left fold with the comma operator on the list of path elements.

```perl
for [\,] $*SPEC.splitdir("../path/to/dir") -> @path {
    mkdir $_ unless .e given $*SPEC.catdir(@path).IO;
}
```