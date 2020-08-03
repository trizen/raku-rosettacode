[1]: https://rosettacode.org/wiki/Include_a_file

# [Include a file][1]

Perl 6 provides a module system that is based primarily on importation of symbols rather than
on inclusion of textual code:

```raku
use MyModule;
```


However, one can evaluate code from a file:

```raku
require 'myfile.p6';
```


One can even do that at compile time:

```raku
BEGIN require 'myfile.p6'
```


None of these are true inclusion, unless the `require` cheats and modifies the current input string of the parser. To get a true textual inclusion, one could define an unhygienic textual macro like this:

```raku
macro include(AST $file) { slurp $file.eval }
include('myfile.p6');
```