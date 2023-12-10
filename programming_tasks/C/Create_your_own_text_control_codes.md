[1]: https://rosettacode.org/wiki/Create_your_own_text_control_codes

# [Create your own text control codes][1]

This, in general would be a bad idea. It isn't smart to add a lot of overhead to core functions, especially for so little gain. That being said, something being a bad idea never stopped us before.



printf already has so many directives, most of the good mnemonics are already taken. Add a "commas" directive as %y and an "invert" directive as %z.



Some languages already have a commas directive as that one is actually useful. I doubt if any language has an "invert" directive.



This is *really* basic and sketchy. It only modifies printf, not sprintf, so probably isn't terribly useful as is... but it satisfies the task requirements. It actually *does* add new, non-standard directives to the core printf function, not just implement a separate formatting function to pre-format a string which is then passed to the printing function.

```perl
use Lingua::EN::Numbers;
use Acme::Text::UpsideDown;

sub printf (Str $format is copy, *@vars is copy) {
    my @directives = $format.comb(/ <?after <-[%]>|^> '%' <[ +0#-]>* <alpha>/);
    for ^@directives {
        if @directives[$_] eq '%y' {
            $format.=subst('%y', '%s');
            @vars[$_].=&comma;
        } elsif @directives[$_] eq '%z' {
            $format.=subst('%z', '%s');
            @vars[$_].=&upsidedown;
        }
    }
    &CORE::printf($format, @vars)
}

printf "Integer %d with commas: %y\nSpelled out: %s\nInverted: %z\n",
       12345, 12345, 12345.&cardinal, 12345.&cardinal;
```

#### Output:
```
Integer 12345 with commas: 12,345
Spelled out: twelve thousand, three hundred forty-five
Inverted: ǝʌᴉɟ-ʎʇɹoɟ pǝɹpunɥ ǝǝɹɥʇ ‘puɐsnoɥʇ ǝʌꞁǝʍʇ
```
