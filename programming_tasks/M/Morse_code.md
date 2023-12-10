[1]: https://rosettacode.org/wiki/Morse_code

# [Morse code][1]





Here we use the user as the audio device.  
Just read the output, leaving extra pauses where indicated 
by either whitespace or underscore.

```perl
my %m = ' ', '_ _ ',
|<
    !	---.
    "	.-..-.
    $	...-..-
    '	.----.
    (	-.--.
    )	-.--.-
    +	.-.-.
    ,	--..--
    -	-....-
    .	.-.-.-
    /	-..-.
    :	---...
    ;	-.-.-.
    =	-...-
    ?	..--..
    @	.--.-.
    [	-.--.
    ]	-.--.-
    _	..--.-
    0	-----
    1	.----
    2	..---
    3	...--
    4	....-
    5	.....
    6	-....
    7	--...
    8	---..
    9	----.
    A	.-
    B	-...
    C	-.-.
    D	-..
    E	.
    F	..-.
    G	--.
    H	....
    I	..
    J	.---
    K	-.-
    L	.-..
    M	--
    N	-.
    O	---
    P	.--.
    Q	--.-
    R	.-.
    S	...
    T	-
    U	..-
    V	...-
    W	.--
    X	-..-
    Y	-.--
    Z	--..
>.map: -> $c, $m is copy {
    $m.=subst(rx/'-'/, 'BGAAACK!!! ', :g);
    $m.=subst(rx/'.'/, 'buck ', :g);
    $c => $m ~ '_';
}

say prompt("Gimme a string: ").uc.comb.map: { %m{$_} // "<scratch> " }
```


Sample run:



Gimme a string: **Howdy, World!**

buck buck buck buck \_ BGAAACK!!! BGAAACK!!! BGAAACK!!! \_ buck BGAAACK!!! BGAAACK!!! \_ BGAAACK!!! buck buck \_ BGAAACK!!! buck BGAAACK!!! BGAAACK!!! \_ BGAAACK!!! BGAAACK!!! buck buck BGAAACK!!! BGAAACK!!! \_ \_ \_ buck BGAAACK!!! BGAAACK!!! \_ BGAAACK!!! BGAAACK!!! BGAAACK!!! \_ buck BGAAACK!!! buck \_ buck BGAAACK!!! buck buck \_ BGAAACK!!! buck buck \_ BGAAACK!!! BGAAACK!!! BGAAACK!!! buck \_
