[1]: https://rosettacode.org/wiki/Compare_length_of_two_strings

# [Compare length of two strings][1]

So... In what way does this task differ significantly from [String length](https://rosettacode.org/wiki/String_length)? Other than being horribly under specified?



In the modern world, string "length" is pretty much a useless measurement, especially in the absence of a specified encoding; hence Raku not even having an operator: "length" for strings.

```perl
say 'Strings (👨‍👩‍👧‍👦, 🤔🇺🇸, BOGUS!) sorted: "longest" first:';
say "$_: characters:{.chars},  Unicode code points:{.codes},  UTF-8 bytes:{.encode('UTF8').bytes},  UTF-16 bytes:{.encode('UTF16').bytes}" for <👨‍👩‍👧‍👦 BOGUS! 🤔🇺🇸>.sort: -*.chars;
```

#### Output:
```
Strings (👨‍👩‍👧‍👦, 🤔🇺🇸, BOGUS!) sorted: "longest" first:
BOGUS!: characters:6,  Unicode code points:6,  UTF-8 bytes:6,  UTF-16 bytes:12
🤔🇺🇸: characters:2,  Unicode code points:3,  UTF-8 bytes:12,  UTF-16 bytes:12
👨‍👩‍👧‍👦: characters:1,  Unicode code points:7,  UTF-8 bytes:25,  UTF-16 bytes:22
```
