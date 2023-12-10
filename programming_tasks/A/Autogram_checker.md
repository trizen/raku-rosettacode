[1]: https://rosettacode.org/wiki/Autogram_checker

# [Autogram checker][1]

Slightly fragile, especially for non-letter character counts, but good enough for here.



Using [Text::Wrap](https://raku.land/github:jkramer/Text::Wrap) from the ecosystem. Install command: `zef install -v <a rel="nofollow" class="external free" href="git://github.com/jkramer/p6-Text-Wrap.git">git://github.com/jkramer/p6-Text-Wrap.git</a>`.

```perl
my %nums = :0zero, :1one, :2two, :3three, :4four, :5five, :6six, :7seven, :8eight, :9nine, :10ten, :11eleven,
    :12twelve, :13thirteen, :14fourteen, :15fifteen, :16sixteen, :17seventeen, :18eighteen, :19nineteen,
    :20twenty, :30thirty, :40forty, :50fifty, :60sixty, :70seventy, :80eighty, :90ninety, :100hundred, :1single;

sub whitespace  { $^a.subst(:g, /\s|'.'/, '') }
sub non-letters { $^a.subst(:g, /\W/, '') }

my @tests =
    (&non-letters, "This sentence employs two a's, two c's, two d's, twenty-eight e's, five f's, three g's, eight h's, eleven i's, three l's, two m's, thirteen n's, nine o's, two p's, five r's, twenty-five s's, twenty-three t's, six v's, ten w's, two x's, five y's, and one z."),
    (&non-letters, "This sentence employs two a's, two c's, two d's, twenty eight e's, five f's, three g's, eight h's, eleven i's, three l's, two m's, thirteen n's, nine o's, two p's, five r's, twenty five s's, twenty three t's, six v's, ten w's, two x's, five y's, and one z."),
    (&whitespace,  "Only the fool would take trouble to verify that his sentence was composed of ten a's, three b's, four c's, four d's, forty-six e's, sixteen f's, four g's, thirteen h's, fifteen i's, two k's, nine l's, four m's, twenty-five n's, twenty-four o's, five p's, sixteen r's, forty-one s's, thirty-seven t's, ten u's, eight v's, eight w's, four x's, eleven y's, twenty-seven commas, twenty-three apostrophes, seven hyphens and, last but not least, a single !"),
    (&non-letters, "This pangram contains four as, one b, two cs, one d, thirty es, six fs, five gs, seven hs, eleven is, one j, one k, two ls, two ms, eighteen ns, fifteen os, two ps, one q, five rs, twenty-seven ss, eighteen ts, two us, seven vs, eight ws, two xs, three ys, & one z."),
    (&non-letters, "This sentence contains one hundred and ninety-seven letters: four a's, one b, three c's, five d's, thirty-four e's, seven f's, one g, six h's, twelve i's, three l's, twenty-six n's, ten o's, ten r's, twenty-nine s's, nineteen t's, six u's, seven v's, four w's, four x's, five y's, and one z."),
    (&non-letters, "Thirteen e's, five f's, two g's, five h's, eight i's, two l's, three n's, six o's, six r's, twenty s's, twelve t's, three u's, four v's, six w's, four x's, two y's."),
    (&whitespace,  "Fifteen e's, seven f's, four g's, six h's, eight i's, four n's, five o's, six r's, eighteen s's, eight t's, four u's, three v's, two w's, three x's."),
    (&non-letters, "Sixteen e's, five f's, three g's, six h's, nine i's, five n's, four o's, six r's, eighteen s's, eight t's, three u's, three v's, two w's, four z's.")
;

use Text::Wrap;

say '=' x 100;

for @tests ->  (&filter, $text) {

    # Original
    say wrap-text :100width, $text;

    say "\nFiltering out " ~ &filter.name ~ ':';

    # Semi-bogus and somewhat fragile names to numbers conversion.
    my $str = $text.lc;
    for %nums.kv -> $word, $number { $str ~~ s:g/ <|w> $word <|w> /$number/ }
    $str ~~ s:g/ (\d)<ws>['and'|'-']<ws>(\d)  /$0 $1/;
    $str ~~ s:g/ <|w>(\d ** 2)<ws>(\d ** 2) <|w> /{ $0 × 100 + $1}/;
    $str ~~ s:g/ ( [\d+<ws>]* \d+ ) /{[+] $0.split: ' '}/;

    # Build a hash of claimed characters.
    my %claim = flat $str.lc.match(:g, /\d+ <:ws> ['comma'|'apostrophe'|'hyphen'|.]/)».split(' ')»[1,0]».pairup;
    for <comma , hyphen - apostrophe '> #`['] -> $word, $symbol { %claim{$symbol} = %claim{$word}:delete if %claim{$word} }
    say "\nClaimed character counts:\n" ~ wrap-text :100width, %claim.sort( ~*.key ).map( { sprintf "%s\(%d)", .key, .value } ).join: ' ';

    # And of the actual character counts.
    my %count = &filter($text).lc.comb.Bag.hash;
    say "\nActual:\n" ~ wrap-text :100width, %count.sort( ~*.key ).map( { sprintf "%s\(%d)", .key, .value } ).join: ' ';

    # And compare them
    say "\nAutogram? " ~
    quietly (so all %count.map: { .value == %claim{.key} }) && (so all %claim.map: { .value == %count{.key} });

    say '=' x 100;
}
```

#### Output:
```
====================================================================================================
This sentence employs two a's, two c's, two d's, twenty-eight e's, five f's, three g's, eight h's,
eleven i's, three l's, two m's, thirteen n's, nine o's, two p's, five r's, twenty-five s's,
twenty-three t's, six v's, ten w's, two x's, five y's, and one z.

Filtering out non-letters:

Claimed character counts:
a(2) c(2) d(2) e(28) f(5) g(3) h(8) i(11) l(3) m(2) n(13) o(9) p(2) r(5) s(25) t(23) v(6) w(10) x(2)
y(5) z(1)

Actual:
a(2) c(2) d(2) e(28) f(5) g(3) h(8) i(11) l(3) m(2) n(13) o(9) p(2) r(5) s(25) t(23) v(6) w(10) x(2)
y(5) z(1)

Autogram? True
====================================================================================================
This sentence employs two a's, two c's, two d's, twenty eight e's, five f's, three g's, eight h's,
eleven i's, three l's, two m's, thirteen n's, nine o's, two p's, five r's, twenty five s's, twenty
three t's, six v's, ten w's, two x's, five y's, and one z.

Filtering out non-letters:

Claimed character counts:
a(2) c(2) d(2) e(28) f(5) g(3) h(8) i(11) l(3) m(2) n(13) o(9) p(2) r(5) s(25) t(23) v(6) w(10) x(2)
y(5) z(1)

Actual:
a(2) c(2) d(2) e(28) f(5) g(3) h(8) i(11) l(3) m(2) n(13) o(9) p(2) r(5) s(25) t(23) v(6) w(10) x(2)
y(5) z(1)

Autogram? True
====================================================================================================
Only the fool would take trouble to verify that his sentence was composed of ten a's, three b's,
four c's, four d's, forty-six e's, sixteen f's, four g's, thirteen h's, fifteen i's, two k's, nine
l's, four m's, twenty-five n's, twenty-four o's, five p's, sixteen r's, forty-one s's, thirty-seven
t's, ten u's, eight v's, eight w's, four x's, eleven y's, twenty-seven commas, twenty-three
apostrophes, seven hyphens and, last but not least, a single !

Filtering out whitespace:

Claimed character counts:
!(1) '(23) ,(27) -(7) a(10) b(3) c(4) d(4) e(46) f(16) g(4) h(13) i(15) k(2) l(9) m(4) n(25) o(24)
p(5) r(16) s(41) t(37) u(10) v(8) w(8) x(4) y(11)

Actual:
!(1) '(23) ,(27) -(7) a(10) b(3) c(4) d(4) e(46) f(16) g(4) h(13) i(15) k(2) l(9) m(4) n(25) o(24)
p(5) r(16) s(41) t(37) u(10) v(8) w(8) x(4) y(11)

Autogram? True
====================================================================================================
This pangram contains four as, one b, two cs, one d, thirty es, six fs, five gs, seven hs, eleven
is, one j, one k, two ls, two ms, eighteen ns, fifteen os, two ps, one q, five rs, twenty-seven ss,
eighteen ts, two us, seven vs, eight ws, two xs, three ys, & one z.

Filtering out non-letters:

Claimed character counts:
a(4) b(1) c(2) d(1) e(30) f(6) g(5) h(7) i(11) j(1) k(1) l(2) m(2) n(18) o(15) p(2) q(1) r(5) s(27)
t(18) u(2) v(7) w(8) x(2) y(3) z(1)

Actual:
a(4) b(1) c(2) d(1) e(30) f(6) g(5) h(7) i(11) j(1) k(1) l(2) m(2) n(18) o(15) p(2) q(1) r(5) s(27)
t(18) u(2) v(7) w(8) x(2) y(3) z(1)

Autogram? True
====================================================================================================
This sentence contains one hundred and ninety-seven letters: four a's, one b, three c's, five d's,
thirty-four e's, seven f's, one g, six h's, twelve i's, three l's, twenty-six n's, ten o's, ten r's,
twenty-nine s's, nineteen t's, six u's, seven v's, four w's, four x's, five y's, and one z.

Filtering out non-letters:

Claimed character counts:
a(4) b(1) c(3) d(5) e(34) f(7) g(1) h(6) i(12) l(3) n(26) o(10) r(10) s(29) t(19) u(6) v(7) w(4)
x(4) y(5) z(1)

Actual:
a(4) b(1) c(3) d(5) e(34) f(7) g(1) h(6) i(12) l(3) n(26) o(10) r(10) s(29) t(19) u(6) v(7) w(4)
x(4) y(5) z(1)

Autogram? True
====================================================================================================
Thirteen e's, five f's, two g's, five h's, eight i's, two l's, three n's, six o's, six r's, twenty
s's, twelve t's, three u's, four v's, six w's, four x's, two y's.

Filtering out non-letters:

Claimed character counts:
e(13) f(5) g(2) h(5) i(8) l(2) n(3) o(6) r(6) s(20) t(12) u(3) v(4) w(6) x(4) y(2)

Actual:
e(13) f(5) g(2) h(5) i(8) l(2) n(3) o(6) r(6) s(20) t(12) u(3) v(4) w(6) x(4) y(2)

Autogram? True
====================================================================================================
Fifteen e's, seven f's, four g's, six h's, eight i's, four n's, five o's, six r's, eighteen s's,
eight t's, four u's, three v's, two w's, three x's.

Filtering out whitespace:

Claimed character counts:
e(15) f(7) g(4) h(6) i(8) n(4) o(5) r(6) s(18) t(8) u(4) v(3) w(2) x(3)

Actual:
'(14) ,(13) e(15) f(7) g(4) h(6) i(8) n(4) o(5) r(6) s(18) t(8) u(4) v(3) w(2) x(3)

Autogram? False
====================================================================================================
Sixteen e's, five f's, three g's, six h's, nine i's, five n's, four o's, six r's, eighteen s's,
eight t's, three u's, three v's, two w's, four z's.

Filtering out non-letters:

Claimed character counts:
e(16) f(5) g(3) h(6) i(9) n(5) o(4) r(6) s(18) t(8) u(3) v(3) w(2) z(4)

Actual:
e(16) f(5) g(3) h(6) i(9) n(5) o(4) r(6) s(18) t(8) u(3) v(3) w(2) x(3) z(1)

Autogram? False
====================================================================================================
```
