[1]: http://rosettacode.org/wiki/Scope_modifiers

# [Scope modifiers][1]

Perl 6 has a system of declarators that introduce new names into various scopes.

```perl
my $lexical-variable;
our $package-variable;
state $persistent-lexical;
has $.public-attribute;
```


Lexically scoped variables, declared with <tt>my</tt>, are the norm.
Function definitions are intrinsically lexical by default, but allow for forward references, unlike any other declaration.



Package variables, declared with <tt>our</tt>, are de-emphasized. Unlike in Perl 5, almost no built-ins use package declarations for anything other than type names, and most of Perl 5's global punctuational variables become dynamic variables instead, with the final recourse in the GLOBAL and PROCESS packages. The per-interpreter GLOBAL package is mainly for users; all predefined process-wide information is stored in the PROCESS symbol table instead. The <tt>our</tt> declarator actually just declares an alias to a variable of the same name in the current package, and is in a sense just permission to use that global variable in the current scope as if it were a lexical. Type and constant declarations, including enums, are intrinsically considered "our" declarations, since Perl 6 considers constants to be degenerate types.



State variables, declared with <tt>state</tt>, are persistent lexicals that are not re-initialized on each function entry, but retain their previous value. An initializer is run only the first time through. State variables are similar but not identical to C static variables; in Perl each closure clone gets its own state variable, since such closures are really a form of generic code.



The <tt>has</tt> declarator is for declaring items in object scope.
Method declarations are implicitly in "has" scope.



In Perl 5, dynamic scoping is done via "local" to temporarily change the value of a global variable. This mechanism is still specced for Perl 6, albeit with a different keyword, <tt>temp</tt>, that better reflects what it's doing. None of the implementations yet implement <tt>temp</tt>, since Perl 6 does dynamic scoping via a more robust system of scanning up the call stack for the innermost dynamic declaration, which actually lives in the lexical scope of the function declaring it. We distinguish dynamic variables syntactically by introducing a "twigil" after the sigil. The twigil for dynamic variables is <tt>\*</tt> to represent that we don't know how to qualify the location of the variable.

```perl
sub a {
    my $*dyn = 'a';
    c();
}
sub b {
    my $*dyn = 'b';
    c();
}
sub c {
    say $*dyn;
}
a();  # says a
b();  # says b
```


The standard IO filehandles are dynamic variables $\*IN, $\*OUT, and $\*ERR, which allows a program to easily redirect the input or output from any subroutine and all its children. More generally, since most process-wide variables are accessed via this mechanism, and only look in the PROCESS package as a last resort, any chunk of code can pretend to be in a different kind of process environment merely by redefining one or more of the dynamic variables in question, such as&#160;%\*ENV.



This mechanism automatically produces thread-local storage if you declare your dynamic variable inside the lexical scope of the thread.