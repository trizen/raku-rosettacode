[1]: https://rosettacode.org/wiki/Print_debugging_statement

# [Print debugging statement][1]





There isn't anything built-in to do this in Rakudo/Raku, though it's pretty easy to cobble something together piggybacking off of the exception system. It would probably be better to instantiate a specific "DEBUG" exception to avoid interfering with other user instantiated ad-hoc exceptions, but for a quick-and-dirty demo, this should suffice.



This example will report any payload contents passed to the exception. If you want specific information, it will need to be passed in, though some of it may be determinable through introspection. Reports the file name and line number where the "debug" call originated and unwinds the call stack to trace through the subroutine calls leading up to it. Will follow the call chain into included files and modules, though calls to the CORE setting and dispatcher are filtered out here to reduce noise.



Comments with the files line numbers are added here to make it easier to match up the debug output with the file. Typically you would be editing the file in an editor that provides line numbering so that wouldn't be necessary/helpful.

```perl
my &pdb = &die;

CATCH {
    when X::AdHoc {
        my @frames = .backtrace[*];
        say .payload;
        for @frames {
            # Filter out exception handing and dispatcher frames
            next if .file.contains: 'SETTING' or .subname.chars < 1;
            printf "%sfrom file: %s,%s line: %s\n",
              (' ' x $++), .file,
              (my $s = .subname) eq '<unit>' ?? '' !! " sub: $s,", .line;
        }
        say '';
        .resume;
    }
    default {}
}

## Testing / demonstration

# helper subs                #line 22
sub alpha ($a) {             #line 23
    pdb ('a =>', $a + 3);    #line 24
    pdb 'string';            #line 25
    beta(7);                 #line 26
}                            #line 27
sub beta  ($b) { pdb $b    } #line 28
sub gamma ($c) { beta $c   } #line 29
sub delta ($d) { gamma $d  } #line 30
                             #line 31
my $a = 10;                  #line 32
pdb (.VAR.name, $_) with $a; #line 33
alpha($a);                   #line 34
delta("Δ");                  #line 35
.&beta for ^3;               #line 36
```

#### Output:
```
($a 10)
from file: debug.p6, line: 33

(a => 13)
from file: debug.p6, sub: alpha, line: 24
 from file: debug.p6, line: 34

string
from file: debug.p6, sub: alpha, line: 25
 from file: debug.p6, line: 34

7
from file: debug.p6, sub: beta, line: 28
 from file: debug.p6, sub: alpha, line: 26
  from file: debug.p6, line: 34

Δ
from file: debug.p6, sub: beta, line: 28
 from file: debug.p6, sub: gamma, line: 29
  from file: debug.p6, sub: delta, line: 30
   from file: debug.p6, line: 35

0
from file: debug.p6, sub: beta, line: 28
 from file: debug.p6, line: 36

1
from file: debug.p6, sub: beta, line: 28
 from file: debug.p6, line: 36

2
from file: debug.p6, sub: beta, line: 28
 from file: debug.p6, line: 36
```
