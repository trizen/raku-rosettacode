[1]: http://rosettacode.org/wiki/Create_a_file

# [Create a file][1]

```perl
 
for '.', '' -> $prefix {
    mkdir "$prefix/docs";
    open "$prefix/output.txt", :w;
}
 
```