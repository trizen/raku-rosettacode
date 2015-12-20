[1]: http://rosettacode.org/wiki/Naming_conventions

# [Naming conventions][1]

Perl 6 is written in Unicode, and has consistent Unicode semantics regardless of the underlying text representations. By default Perl 6 presents Unicode in "NFG" formation, where each grapheme counts as one character. A grapheme is what the novice user would think of as a character in their normal everyday life, including any diacritics.



Built-in object types start with an uppercase letter. This includes immutable types (e.g. Int, Num, Complex, Rat, Str, Bit, Regex, Set, Block, Iterator), as well as mutable (container) types, such as Scalar, Array, Hash, Buf, Routine, Module, and non-instantiable Roles such as Callable and Integral. The names may extend to CamelCase for compound words: IntStr, CaptureCursor, BagHash, SoftRoutine.



Non-object (native) types are lowercase: int, num, complex, rat, buf, bit.



Nearly all built-in subroutines, functions, methods and pragmas included in Perl 6 CORE are lowercase or lower kebab-case. (Compound words joined with hyphens rather than underscores or camelCase.) .grep, .pairs, .log, .defined, .subst-rw. The few notable exceptions are those which can radically change behaviour of the executing code. They are in all-cap/kebab-case to make them stand out: EVAL, MONKEY-TYPING.



All upper case names are semi-reserved. You are free to use them, but are warned that you may encounter future collisions with internal usage. Upper case names are used for pseudo-packages: MY, OUR, CORE, GLOBAL, etc., for relative scope identifiers: CALLER, OUTER, SETTING, PARENT, etc. and other things.



Variables in Perl 6 CORE tend be lower kebab-case for lexical variables and upper case for special or package globals. They have an attached, prefix sigil (or twigil) to indicate what type of object they hold and what methods are available to operate on them.



In user space, there are very few restrictions on how things are named. Identifers of any type can not contain white space. Subroutines must start with a letter character, any unicode character that has a "letter" property. Variable names can't contain any of the sigil, twigil or comment characters ($, @,&#160;%, \*,&#160;?, =,&#160;:, #). Outside of those few restrictions, it's pretty much a free-for-all.



That being said, there are some community conventions which are encouraged, though not enforced. Descriptivness is favoured over terseness, though this should be scaled to the scope of the object. It is perfectly fine to name an index variable in a three line loop, $i. An object in global scope with dozens of methods may be better off with a more descriptive name. It is encouraged to name subroutines for what they do to make it easier for others to follow your logic. Nouny things should have nouny names. Verby things should be verby. If you *aren't* going to follow convention, at least be consistent.