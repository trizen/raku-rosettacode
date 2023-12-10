[1]: https://rosettacode.org/wiki/Special_variables

# [Special variables][1]





It is probably useful to briefly explain normal variables in Raku before tackling special variables.



Variables in Raku have a prefix sigil to distinguish them from named subroutines, functions, classes, and so on. There is a system of sigils to mark the fundamental structural type of the variable:

```perl
 $foo   scalar (object)
 @foo   ordered array
 %foo   unordered hash (associative array)
 &foo   code/rule/token/regex
 ::foo  package/module/class/role/subset/enum/type/grammar
```


Sigils indicate overall interface, not the exact type of the bound object. Different sigils imply different minimal abilities. Ordinary sigils indicate normally scoped variables, either lexical or package scoped. Oddly scoped variables include a secondary sigil (a twigil) that indicates what kind of strange scoping the variable is subject to:

```perl
 $foo                # ordinary scoping
 $.foo               # object attribute public accessor
 $^foo               # self-declared formal positional parameter
 $:foo               # self-declared formal named parameter
 $*foo               # dynamically overridable global variable
 $?foo               # compiler hint variable
 $=foo               # POD variable
 $<foo>              # match variable, short for $/{'foo'}
 $!foo               # object attribute private storage
 $~foo               # the foo sublanguage seen by the parser at this lexical spot
```


A selection (not comprehensive) of Raku's automatically set and/or pre-defined compile-time and run-time variables.

```perl
# Lexical variables

 $_                  # implicit variable lexically scoped to the current block
 $!                  # current Exception object
 $/                  # last match
 $0, $1, $2...       # captured values from match: $/[0], $/[1], $/[2] ...

# Compile-time variables

$?PACKAGE            # current package
$?CLASS              # current class
$?MODULE             # current module
$?ROLE               # current role
$?DISTRIBUTION       # which OS distribution am I compiling under
$?FILE               # current filename of source file
$?LINE               # current line number in source file
&?ROUTINE            # current sub or method (itself)
&?BLOCK              # current block (itself)

# Dynamic variables

$*USAGE              # value of the auto-generated USAGE message
$*PROGRAM-NAME       # path to the current executable
$*PROGRAM            # location (in the form of an IO::Path object) of the Raku program being executed
@*ARGS               # command-line arguments
$*ARGFILES           # the magic command-line input handle
$*CWD                # current working directory
$*DISTRO             # which OS distribution am I running under
%*ENV                # system environment variables
$*ERR                # standard error handle
$*EXECUTABLE-NAME    # name of the Raku executable that is currently running
$*EXECUTABLE         # IO::Path absolute path of the Raku executable that is currently running
$*IN                 # standard input handle; is an IO object
$*KERNEL             # operating system running under
$*OUT                # standard output handle
$*RAKU               # Raku version running under
$*PID                # system process id
$*TZ                 # local time zone
$*USER               # system user id

# Run-time variables

$*COLLATION          # object that can be used to configure Unicode collation levels
$*TOLERANCE          # used by the =~= operator to decide if two values are approximately equal
$*DEFAULT-READ-ELEMS # affects the number of bytes read by default by IO::Handle.read
```


Also, not really a variable but...

```perl
 *  # A standalone term that has no fixed value, instead it captures the notion of "Whatever",
    # the meaning of which is decided lazily by whatever it is an argument to.
    # See the docs on Whatever: https://docs.raku.org/type/Whatever
```
