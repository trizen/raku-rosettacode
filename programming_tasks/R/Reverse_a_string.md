[1]: https://rosettacode.org/wiki/Reverse_a_string

# [Reverse a string][1]





Raku handles graphemes, multi-byte characters and emoji correctly by default.

```perl
say "hello world".flip;
say "as⃝df̅".flip;
say 'ℵΑΩ 駱駝道 🤔 🇸🇧 🇺🇸 🇬🇧‍ 👨‍👩‍👧‍👦🆗🗺'.flip;
```

#### Output:
```
dlrow olleh
f̅ds⃝a
🗺🆗👨‍👩‍👧‍👦 🇬🇧‍ 🇺🇸 🇸🇧 🤔 道駝駱 ΩΑℵ
```
