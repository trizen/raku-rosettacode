[1]: http://rosettacode.org/wiki/Walk_a_directory/Recursively

# [Walk a directory/Recursively][1]

Uses File::Find from [File-Tools](http://github.com/tadzik/perl6-File-Tools)

```perl
use File::Find;
 
.say for find(dir => '.').grep(/foo/);
```