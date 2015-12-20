[1]: http://rosettacode.org/wiki/Scope/Function_names_and_labels

# [Scope/Function names and labels][1]

First a little hand-wavey exposition. The lines are rather blurry in Perl 6 between subroutines, methods, operators and functions. Methods are associated with an object and are inheritable. Subroutines and operators are not. Other than that though there is a lot of overlap. "A function" doesn't really have a specific definition, but is more of a generic term used when talking about code reference type of things.



Methods don't have a separate scope from the object they are attached to. If the object is in scope, the method will be.



A subroutine is really just another type of object. It has a code reference and has ROUTINE semantics attached to it. The same holds for operators. Operators are really just subroutines with a funny calling convention. That being the case, scoping for subroutines very closely follows scoping rules for any other Perl 6 variable type.



In general, subroutines are "my" variables by default (if you don't specify, the "my" is implicit), meaning scoping is lexical to the enclosing block and flows inward. A subroutine defined within a block will be visible to everything inside that block, even other blocks within that block. However, any inner block can define its own subroutine with the same name and that will be used in preference to the routine from an outer block. That implies you can easily override / redefine core functions from the Perl 6 setting. The setting is the collection of built in functions supplied by Perl 6, typically and somewhat incongruously referred to as "CORE" even though technically it is the outermost scope. ( SKIN? BARK? CRUST? ... oooo! EXOSKELETON!&#160;:-) )



Alternately, subroutines may be declared as an "our" variable making it a package global, visible anywhere in the packages' namespace. That is somewhat discouraged though as it pollutes the namespace and reduces the granularity of control.



There are several ways to modify the relative scope of a subroutine (any item in the symbol table really) by adding modifiers to the name.


#### Output:
```
CALLER    # Contextual symbols in the immediate caller's lexical scope
OUTER     # Symbols in the next outer lexical scope
UNIT      # Symbols in the outermost lexical scope of compilation unit
SETTING   # Lexical symbols in the unit's DSL (usually CORE)
PARENT    # Symbols in this package's parent package (or lexical scope)
```
```perl6
# call a routine before it has been defined
say log();              # prints: outer
 
# define a subroutine that overrides a CORE function
sub log { 'outer' }; 
{
    # redefine the subroutine in this block
    sub log { 'inner' };
    {
        # redefine the subroutine yet again
        sub log { 'way down inside' };
 
        # call it within this block
        say log();                 # prints: way down inside
 
        # call it from the block one level out
        say &OUTER::log();         # prints: inner
 
        # call it from the block two levels out
        say &OUTER::OUTER::log();  # prints: outer
 
        # call it from the outermost block
        say &UNIT::log();          # prints: outer
 
        # call a subroutine that is post declared in outermost scope
        outersub()
    }
 
    {
        # subroutine in an inner block that doesn't redefine it
        # uses definition from nearest enclosing block
        say log();      # prints: inner
    }
    # call it within this block
    say log();          # prints: inner
 
    # call it from the block one level out
    say &OUTER::log();  # prints: outer
}
 
sub outersub{ 
    # call subroutine within this block - gets outer sub
    say log();          # prints: outer
 
    # call subroutine from the scope of the callers block
    say &CALLER::log(); # prints: way down inside
 
    # call subroutine from the outer scope of the callers block
    say &CALLER::OUTER::log(); # prints: inner
 
    # call the original overridden CORE routine
    say &CORE::log(e);  # prints: 1 ( natural log of e )
}
```


Labels are less interesting and are typically useful only for control flow in looping constructs. They generally follow the same scoping rules as "my" variables. There is nearly always easier ways to do control flow though, so they aren't heavily used.