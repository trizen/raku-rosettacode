[1]: https://rosettacode.org/wiki/Remove_vowels_from_a_string

# [Remove vowels from a string][1]

Not going to bother with 'y', it's too touchy feely (How many vowels are in the word my? why? any?) and subject to interpretation.



Otherwise, this should work for *most* Latinate languages.



Apropos of nothing; reminds me of one of my favorite Between the Lions performances: [Sloppy Pop - Sometimes Y](https://www.youtube.com/watch?v=EkKKb6C8NKs)



Spec is 'Remove vowels from a string'; nothing about what they should be replaced with. I chose to replace them with spaces.



Strings from [http://mylanguages.org/](http://mylanguages.org/). No affiliation, but it's a nice resource. (note: these are not all the same sentence but are all from the same paragraph. They frankly were picked based on their vowel load.)

```perl
my @vowels = (0x20 .. 0x2fff).map: { .chr if .chr.samemark('x') ~~ m:i/<[aæeiıoœu]>/ }

my $text = q:to/END/;
   Norwegian, Icelandic, German, Turkish, French, Spanish, English:
   Undervisningen skal være gratis, i det minste på de elementære og grunnleggende trinn.
   Skal hún veitt ókeypis, að minnsta kosti barnafræðsla og undirstöðummentu.
   Hochschulunterricht muß allen gleichermaßen entsprechend ihren Fähigkeiten offenstehen.
   Öğrenim hiç olmazsa ilk ve temel safhalarında parasızdır. İlk öğretim mecburidir.
   L'éducation doit être gratuite, au moins en ce qui concerne l'enseignement élémentaire et fondamental.
   La instrucción elemental será obligatoria. La instrucción técnica y profesional habrá de ser generalizada.
   Education shall be free, at least in the elementary and fundamental stages.
   END

put $text.subst(/@vowels/, ' ', :g);
```

#### Output:
```
N rw g  n,  c l nd c, G rm n, T rk sh, Fr nch, Sp n sh,  ngl sh:
 nd rv sn ng n sk l v r  gr t s,   d t m nst  p  d   l m nt r   g gr nnl gg nd  tr nn.
Sk l h n v  tt  k yp s,  ð m nnst  k st  b rn fr ðsl   g  nd rst ð mm nt .
H chsch l nt rr cht m ß  ll n gl  ch rm ß n  ntspr ch nd  hr n F h gk  t n  ff nst h n.
 ğr n m h ç  lm zs   lk v  t m l s fh l r nd  p r s zd r.  lk  ğr t m m cb r d r.
L' d c t  n d  t  tr  gr t  t ,    m  ns  n c  q   c nc rn  l' ns  gn m nt  l m nt  r   t f nd m nt l.
L   nstr cc  n  l m nt l s r   bl g t r  . L   nstr cc  n t cn c  y pr f s  n l h br  d  s r g n r l z d .
 d c t  n sh ll b  fr  ,  t l  st  n th   l m nt ry  nd f nd m nt l st g s.
```
