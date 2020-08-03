[1]: https://rosettacode.org/wiki/Exceptions

# [Exceptions][1]

The Perl 6 equivalent to Perl 5's eval {...} is try {...}. A try block by default has a CATCH block that handles all fatal exceptions by ignoring them. If you define a CATCH block within the try, it replaces the default CATCH. It also makes the try keyword redundant, because any block can function as a try block if you put a CATCH block within it. The inside of a CATCH functions as a switch statement on the current exception.

```perl
try {
    die "Help I'm dieing!";
    CATCH {
        when X::AdHoc { note .Str.uc; say "Cough, Cough, Aiee!!" }
        default { note "Unexpected exception, $_!" }
    }
}
 
say "Yay. I'm alive.";
 
die "I'm dead.";
 
say "Arrgh.";
 
CATCH {
    default { note "No you're not."; say $_.Str; }
}
```

#### Output:
```
HELP I'M DIEING!
Cough, Cough, Aiee!!
Yay. I'm alive.
No you're not.
I'm dead.
```


Perl 6 comes with [phasers](http://design.perl6.org/S04.html#Phasers), that are called when certain conditions in the life of a program, routine or block are met. `CATCH` is one of them and works nicely together with `LEAVE` that is called even if an exception would force the current block to be left immediately. It's a nice place to put your cleanup code.

```perl
sub f(){
        ENTER { note '1) f has been entered' }
        LEAVE { note '2) f has been left' }
        say '3) here be dragons';
        die '4) that happend to be deadly';
}
 
f();
say '5) am I alive?';
 
CATCH {
        when X::AdHoc { note q{6) no, I'm dead}; }
}
```

#### Output:
```
1) f has been entered
3) here be dragons
6) no, I'm dead
2) f has been left
```