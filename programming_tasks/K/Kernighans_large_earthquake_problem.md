[1]: https://rosettacode.org/wiki/Kernighans_large_earthquake_problem

# [Kernighans large earthquake problem][1]

Pass in a file name, or use default for demonstration purposes.

```raku
$_ = @*ARGS[0] ?? @*ARGS[0].IO !! q:to/END/;
    8/27/1883    Krakatoa            8.8
    5/18/1980    MountStHelens       7.6
    3/13/2009    CostaRica           5.1
    END
 
map { .say if .words[2] > 6 }, .lines;
```