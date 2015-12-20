[1]: http://rosettacode.org/wiki/Jump_anywhere

# [Jump anywhere][1]

Perl 6 supports various forms of controlled jumps out of loops, switches, functions, and blocks. In addition, <tt>goto</tt> can jump to any label within the current lexical scope, or in the lexical scope associated with any surrounding dynamic scope. If the declaration of the label is not visible lexically, then the label must enclosed in quotes. (A computed label is therefore allowed, but discouraged.)