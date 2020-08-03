[1]: https://rosettacode.org/wiki/Jump_anywhere

# [Jump anywhere][1]

### Label-based jumps

```raku
    outer-loop: loop {
        inner-loop: loop {
            # NYI # goto inner-loop if rand > 0.5; # Hard goto
            next inner-loop if rand > 0.5; # Next loop iteration
            redo inner-loop if rand > 0.5; # Re-execute block
            last outer-loop if rand > 0.5; # Exit the loop
            ENTER { say "Entered inner loop block" }
            LEAVE { say "Leaving inner loop block" }
        }
        ENTER { say "Entered outer loop block" }
        LEAVE { say "Leaving outer loop block" }
        LAST  { say "Ending outer loop" }
    }
```


Produces random output, but here's a representative run:


#### Output:
```
 Entered outer loop block
 Entered inner loop block
 Leaving inner loop block
 Entered inner loop block
 Leaving inner loop block
 Entered inner loop block
 Leaving inner loop block
 Leaving outer loop block
 Ending outer loop
```


### Continuation-based execution



Continuations in Perl 6 are currently limited to use in generators via the gather/take model:

```raku
    my @list = lazy gather for ^100 -> $i {
        if $i.is-prime {
            say "Taking prime $i";
            take $i;
        }
    }
 
    say @list[5];
```


This outputs:


#### Output:
```
 Taking prime 2
 Taking prime 3
 Taking prime 5
 Taking prime 7
 Taking prime 11
 Taking prime 13
 13
```


Notice that no further execution of the loop occurs. If we then asked for the element at index 20, we would expect to see 15 more lines of "Taking prime..." followed by the result: 73.



### Failures and exceptions



Exceptions are fairly typical in Perl6:

```raku
    die "This is a generic, untyped exception";
```


Will walk up the stack until either some \`CATCH\` block intercepts the specific exception type or we exit the program.



But if a failure should be recoverable (e.g. execution might reasonably continue along another path) a failure is often the right choice. The fail operator is like "return", but the returned value will only be valid in boolean context or for testing definedness. Any other operation will produce the original exception with the original exception's execution context (e.g. traceback) along with the current context.

```raku
    sub foo() { fail "oops" }
    my $failure = foo;
    say "Called foo";
    say "foo not true" unless $failure;
    say "foo not defined" unless $failure.defined;
    say "incremented foo" if $failure++; # exception
```


Produces:


#### Output:
```
 Called foo
 foo not true
 foo not defined
 oops
   in sub foo at fail.p6 line 1
   in block <unit> at fail.p6 line 2
 Actually thrown at:
   in any  at gen/moar/m-Metamodel.nqp line 3090
   in block <unit> at fail.p6 line 6
```


However, an exception can \`.resume\` in order to jump back to the failure point (this is why the stack is not unwound until after exception handling).

```raku
  sub foo($i) {
      if $i == 0 {
          die "Are you sure you want /0?";
      }
      say "Dividing by $i";
      1/$i.Num + 0; # Fighting hard to make this fail
  }
 
  for ^10 -> $n {
      my $recip = foo($n);
      say "1/{$n} = {$recip.perl}";
  }
 
  CATCH {
      when ~$_ ~~ m:s/Are you sure/ { .resume; #`(yes, I'm sure) }
  }
```


This code raises an exception on a zero input, but then resumes execution, divides be zero and then raises a divide by zero exception which is not caught:


#### Output:
```
 Dividing by 0
 Attempt to divide 1 by zero using /
   in sub foo at fail.p6 line 6
   in block <unit> at fail.p6 line 10
 
 Actually thrown at:
   in sub foo at fail.p6 line 6
   in block <unit> at fail.p6 line 10
```