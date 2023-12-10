[1]: https://rosettacode.org/wiki/Determine_if_a_string_is_squeezable

# [Determine if a string is squeezable][1]



```perl
map {
    my $squeeze = $^phrase;
    sink $^reg;
    $squeeze ~~ s:g/($reg)$0+/$0/;
    printf "\nOriginal length: %d <<<%s>>>\nSqueezable on \"%s\": %s\nSqueezed length: %d <<<%s>>>\n",
      $phrase.chars, $phrase, $reg.uniname, $phrase ne $squeeze, $squeeze.chars, $squeeze
},  
  '', ' ', 
  '"If I were two-faced, would I be wearing this one?" --- Abraham Lincoln ', '-',
  '..1111111111111111111111111111111111111111111111111111111111111117777888', '7',
  "I never give 'em hell, I just tell the truth, and they think it's hell. ", '.',
  '                                                    --- Harry S Truman  ', ' ',
  '                                                    --- Harry S Truman  ', '-',
  '                                                    --- Harry S Truman  ', 'r'
```

#### Output:
```
Original length: 0 <<<>>>
Squeezable on "SPACE": False
Squeezed length: 0 <<<>>>

Original length: 72 <<<"If I were two-faced, would I be wearing this one?" --- Abraham Lincoln >>>
Squeezable on "HYPHEN-MINUS": True
Squeezed length: 70 <<<"If I were two-faced, would I be wearing this one?" - Abraham Lincoln >>>

Original length: 72 <<<..1111111111111111111111111111111111111111111111111111111111111117777888>>>
Squeezable on "DIGIT SEVEN": True
Squeezed length: 69 <<<..1111111111111111111111111111111111111111111111111111111111111117888>>>

Original length: 72 <<<I never give 'em hell, I just tell the truth, and they think it's hell. >>>
Squeezable on "FULL STOP": False
Squeezed length: 72 <<<I never give 'em hell, I just tell the truth, and they think it's hell. >>>

Original length: 72 <<<                                                    --- Harry S Truman  >>>
Squeezable on "SPACE": True
Squeezed length: 20 <<< --- Harry S Truman >>>

Original length: 72 <<<                                                    --- Harry S Truman  >>>
Squeezable on "HYPHEN-MINUS": True
Squeezed length: 70 <<<                                                    - Harry S Truman  >>>

Original length: 72 <<<                                                    --- Harry S Truman  >>>
Squeezable on "LATIN SMALL LETTER R": True
Squeezed length: 71 <<<                                                    --- Hary S Truman  >>>
```
