[1]: http://rosettacode.org/wiki/Special_variables

# [Special variables][1]

It is probably useful to briefly explain normal variables in Perl 6 before tackling special variables.



Variables in Perl 6 have a prefix sigil to distinguish them from named subroutines, functions, classes, and so on. There is a system of sigils to mark the fundamental structural type of the variable:

```perl
 $foo   scalar (object)
 @foo   ordered array
 %foo   unordered hash (associative array)
 &foo   code/rule/token/regex
 ::foo  package/module/class/role/subset/enum/type/grammar
```


Sigils indicate overall interface, not the exact type of the bound object. Different sigils imply different minimal abilities. Ordinary sigils indicate normally scoped variables, either lexical or package scoped. Oddly scoped variables include a secondary sigil (a twigil) that indicates what kind of strange scoping the variable is subject to:

```perl
 $foo               # ordinary scoping
 $.foo              # object attribute public accessor
 $^foo              # self-declared formal positional parameter
 $:foo              # self-declared formal named parameter
 $*foo              # dynamically overridable global variable
 $?foo              # compiler hint variable
 $=foo              # Pod variable
 $<foo>             # match variable, short for $/{'foo'}
 $!foo              # object attribute private storage
 $~foo              # the foo sublanguage seen by the parser at this lexical spot
```


Special Variables:



Perl 6 has deprecated most of the "line-noise" variables from Perl 5 in favor of named variables.

```perl
 $_                # The implicit variable lexically scoped to the current block
 @_                # Implicit array of parameters to the current block. Still available but rarely used or needed with the improved sub signatures
 $!                # Current Exception object
 $/                # Last match
 $0, $1, $2...     # Captured values from match: $/[0], $/[1], $/[2] ...
 $?ARCH            # Host architecture
 $?XARCH           # Target architecture
 @*ARGS            # command-line arguments
 $*ARGFILES        # The magic command-line input handle
 &?BLOCK           # current block (itself)
 ::?CLASS          # current class (as package or type name)
 $?CLASS           # current class
 @=COMMENT         # All the comment blocks in the file
 %?CONFIG          # configuration hash 
 $*CWD             # current working directory
 $=DATA            # data block handle (=begin DATA ... =end)
 @=DATA            # Same as above, but array
 %?DEEPMAGIC       # Controls the mappings of magical names to sub definitions
 $?DISTRO          # Which OS distribution am I compiling under
 $*DISTRO          # Which OS distribution am I running under
 $*EGID            # effective group id
 %*ENV             # system environment variables
 $*ERR             # Standard error handle
 $*EUID            # effective user id
 $*EXECUTABLE_NAME # executable name
 $?FILE            # current filename of source file
 $?GRAMMAR         # current grammar
 $*GID             # group id
 $*IN              # Standard input handle; is an IO object
 @*INC             # where to search for user modules (but not std lib!)
 $?KERNEL          # operating system compiled for
 $*KERNEL          # operating system running under
 %?LANG            # What is the current set of interwoven languages?
 $*LANG            # LANG variable from %*ENV that defines what human language is used
 $?LINE            # current line number in source file
 %*META-ARGS       # Meta-arguments
 $?MODULE          # current module
 %*OPTS            # Options from command line
 %*OPT...          # Options from command line to be passed down
 $*OUT             # Standard output handle
 $?PACKAGE         # current package
 $?PERL            # Which Perl am I compiled for?
 $*PERL            # perl version running under
 $*PID             # system process id
 %=POD             # POD
 $*PROGRAM_NAME    # name of the Perl program being executed
 %*PROTOCOLS       # Stores the methods needed for the uri() function
 ::?ROLE           # current role (as package or type name)
 $?ROLE            # current role
 &?ROUTINE         # current sub or method (itself)
 $?SCOPE           # Current "my" scope
 $*TZ              # Local time zone
 $*UID             # system user id
 $?USAGE           # Default usage message generated at compile time
 $?VM              # Which virtual machine am I compiling under
 $?XVM             # Which virtual machine am I cross-compiling for
```


Also, not really a variable but...

```perl
 *  # A standalone term that has no fixed value, instead it captures the notion of "Whatever",
    # the meaning of which is decided lazily by whatever it is an argument to.
    # See the "*" section of http://perlcabal.org/syn/S02.html#Built-In_Data_Types
```