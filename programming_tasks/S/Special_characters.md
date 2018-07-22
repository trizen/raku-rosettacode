[1]: https://rosettacode.org/wiki/Special_characters

# [Special characters][1]

Technically speaking, all characters are special in Perl 6, since
they're all just the result of particular mixes of parse rules.
Nevertheless, some characters may appear to be more special than
others. (However, we will not document any operators here, which
contain plenty of odd characters in Perl 6.)



Sigils:


#### Output:
```
    $   Item
    @   Positional
    %   Associative
    &   Callable
```


Twigils may occur only after a sigil, and indicate special scoping:


#### Output:
```
    *   Dynamically scoped
    ?   Compile-time constant
    ^   Positional placeholder
    :   Named placeholder
    !   Private attribute
    .   Public attribute/accessor
    ~   Slang
    =   Pod data
    <   Named match from $/
```


Quote characters:


#### Output:
```
    ''  Single quotes
    ""  Double quotes
    //  Regex
    ｢｣  Quotes that allow no interpolation at all
    <>  Quote words
    «»  Shell-style words
```


Escapes within single quotes:


#### Output:
```
    \\  Backslash
    \'  Quote char
```


Escapes within double quotes:


#### Output:
```
    {}  Interpolate results of block
    $   Interpolate item
    @   Interpolate list (requires postcircumfix)
    %   Interpolate hash (requires postcircumfix)
    &   Interpolate call (requires postcircumfix)
    \\  Backslash
    \"  Quote char
    \a  Alarm
    \b  Backspace
    \c  Decimal, control, or named char
    \e  Escape
    \f  Formfeed
    \n  Newline
    \o  Octal char
    \r  Return
    \t  Tab
    \x  Hex char
    \0  Null
```


Escapes within character classes and translations include most of the double-quote backslashes, plus:


#### Output:
```
    ..  range
```


Escapes within regexes include most of the double-quote escapes, plus:


#### Output:
```
    :   Some kind of declaration
    <>  Some kind of assertion
    []  Simple grouping
    ()  Capture grouping
    {}  Side effect block
    .   Any character
    \d  Digit
    \w  Alphanumeric
    \s  Whitespace
    \h  Horizontal whitespace
    \v  Vertical whitespace
    ''  Single quoted string
    ""  Double quoted string
    «   Word initial boundary
    »   Word final boundary
    ^   String start
    ^^  Line start
    $   String end
    $$  Line end
```


Note that all non-alphanumeric characters are reserved for escapes and operators in Perl 6 regexes.



Any lowercase backslash escape in a regex may be uppercased to negate it, hence `\N` matches anything that is not a newline.