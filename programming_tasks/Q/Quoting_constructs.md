[1]: https://rosettacode.org/wiki/Quoting_constructs

# [Quoting constructs][1]

The Perl philosophy, which Raku thoroughly embraces, is that "There Is More Than One Way To Do It" (often abbreviated to TIMTOWDI). Quoting constructs is an area where this is enthusiastically espoused.



Raku has a whole quoting specific sub-language built in called Q. Q changes the parsing rules inside the quoting structure and allows extremely fine control over how the enclosed data is parsed. Every quoting construct in Raku is some form of a Q syntactic structure, using adverbs to fine tune the desired behavior, though many of the most commonly used have some form of "shortcut" syntax for quick and easy use. Usually, when using an adverbial form, you may omit the Q: and just use the adverb.



In general, any and all quoting structures have theoretically unlimited length, in practice, are limited by memory size, practically, it is probably better to limit them to less than a gigabyte or so, though they can be read as a supply, not needing to hold the whole thing in memory at once. They can hold multiple lines of data. How the new-line characters are treated depends entirely on any white-space adverb applied. The Q forms use some bracketing character to delimit the quoted data. Usually some Unicode bracket ( [], {}, &lt;&gt;, ⟪⟫, whatever,) that has an "open" and "close" bracketing character, but they may use any non-indentifier character as both opener and closer. ||, //,&#160;??, the list goes on. The universal escape character for constructs that allow escaping is backslash "\".



The following exposition barely scratches the surface. For much more detail see the **[Raku documentation for quoting constructs](https://docs.raku.org/language/quoting)** for a comprehensive list of adverbs and examples.















*Every adverbial form has both a q and a qq variant to give the 'single quote' or "double quote" semantics. Only the most commonly used are listed here.*

































There are other adverbs to give precise control what interpolates or doesn't, that may be applied to any of the above constructs. See **[the doc page for details](https://docs.raku.org/language/quoting#Literal_strings:_Q)**. There is another whole sub-genre dedicated to **[quoting regexes](https://docs.raku.org/language/regexes#Anonymous_regex_definition_syntax)**.
