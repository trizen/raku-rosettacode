[1]: https://rosettacode.org/wiki/Assertions_in_design_by_contract

# [Assertions in design by contract][1]

Most of theses entries seem to have missed the point. This isn't about how to implement or use assertions, it is about how the language uses design-by-contract to help the programmer get correct results.



Perl 6 doesn't have an assert routine in CORE. You could easily add one; several modules geared toward unit testing supply various "assertion" routines, though I'm not aware of any actually specifically named "assert()".



Perl 6 core has subroutine signatures, multi-dispatch and exceptions to implement design-by-contract.



Subroutine signature allow the programmer to make the parameters supplied to a routine be checked to make sure they are the of correct quantity, type, and value and can constrain the returned value(s) to a particular type. See the below snippet for a brief demonstration. Perl 6 does some static analysis to trap errors at compile time, but traps many errors at run time since it is often impossible to tell if a variable is of the correct type before the program is run.



When Perl 6 encounters a design-by-contract violation, it will fail with an error message telling where the problem occurred, what it expected and what it actually got. Some failures are trappable and resumeable. Failures may be trapped by adding a CATCH { } block in the current scope. Failures will transfer execution to the CATCH block where the failure may be analysed and further action taken. Depending on the failure type, execution may be resumable. Some failures cause execution to halt unavoidably.



In this snippet, the routine repeat takes one Integer that must be greater than 1, a String and returns a String. (Note that as written, it is incorrect since it actually returns a boolean.)

```raku
sub repeat ( Int $repeat where * > 1, Str $message, --> Str ) {
    say $message x $repeat;
    True # wrong return type
}
 
repeat( 2, 'A' ); # parameters ok, return type check error
 
repeat( 4, 2 ); # wrong second parameter type
 
repeat( 'B', 3 ); # wrong first (and second) parameter type
 
repeat( 1, 'C' ); # constraint check fail
 
repeat( ); # wrong number of parameters
 
CATCH {
    default {
        say "Error trapped: $_";
        .resume;
    }
}
```

#### Output:
```
AA
Error trapped: Type check failed for return value; expected Str but got Bool (Bool::True)
Error trapped: Type check failed in binding to parameter '$message'; expected Str but got Int (2)
Error trapped: Type check failed in binding to parameter '$repeat'; expected Int but got Str ("B")
Error trapped: Constraint type check failed in binding to parameter '$repeat'; expected anonymous constraint to be met but got Int (1)
Error trapped: Too few positionals passed; expected 2 arguments but got 0
This exception is not resumable
  in block  at contract.p6 line 19
  in block <unit> at contract.p6 line 14
```