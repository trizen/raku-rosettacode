[1]: http://rosettacode.org/wiki/Find_duplicate_files

# [Find duplicate files][1]

This implementation takes a starting directory (defaults to the current directory) and has a few flags to set behaviour: --minsize, minimum file size to look at, defaults to 5 bytes; and --recurse, recurse into the directory structure, default True. It finds files of the same size, calculates hashes to compare, then reports files that hash the same. Uses the very fast but cryptographically poor xxHash library to hash the files.

```perl
use Digest::xxHash;
 
sub MAIN( $dir = '.', :$minsize = 5, :$recurse = True ) {
    my %files;
    my @dirs = $dir.IO.absolute.IO;
    while @dirs {
        my @files = @dirs.pop;
        while @files {
            for @files.pop.dir -> $path {
                %files{ $path.s }.push: $path if $path.f and $path.s >= $minsize;
                @dirs.push: $path if $path.d and $path.r and $recurse
            }
        }
    }
 
    for %files.sort( +*.key ).grep( *.value.elems > 1)».kv -> ($size, @list) {
        my %dups;
        @list.map: { %dups{ xxHash( $_.slurp :bin ) }.push: $_.Str };
        for %dups.grep( *.value.elems > 1)».value -> @dups {
            say sprintf("%9s : ", scale $size ),  @dups.join(', ');
        }
    }
}
 
sub scale ($bytes) {
    given $bytes {
        when $_ < 2**10 {  $bytes                    ~ ' B'  }
        when $_ < 2**20 { ($bytes / 2**10).round(.1) ~ ' KB' }
        when $_ < 2**30 { ($bytes / 2**20).round(.1) ~ ' MB' }
        default         { ($bytes / 2**30).round(.1) ~ ' GB' }
    }
}
```


Passing in command line switches: --minsize=0 --recurse=False /home/me/p6


#### Output:
```
     0 B : /home/me/p6/vor.ppm, /home/me/p6/ns.txt
   190 B : /home/me/p6/scrub(copy).t, /home/me/p6/scrub.t
  1.3 KB : /home/me/p6/coco.p6, /home/me/p6/coc.p6
 80.5 KB : /home/me/p6/temp.txt, /home/me/p6/temp.html
279.6 KB : /home/me/p6/pentaflake.svg, /home/me/p6/5nflake.svg
```