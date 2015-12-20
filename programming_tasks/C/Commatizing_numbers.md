[1]: http://rosettacode.org/wiki/Commatizing_numbers

# [Commatizing numbers][1]

```perl
commatize 'pi=3.14159265358979323846264338327950288419716939937510582097494459231', :at(6), :ins(' '), :by(5);
commatize 'The author has two Z$100000000000000 Zimbabwe notes (100 trillion).', :ins<.>;
commatize '-in Aus$+1411.8millions';
commatize '===US$0017440 millions=== (in 2000 dollars)';
commatize '123.e8000 is pretty big.';
commatize 'The land area of the earth is  57268900(29% of the surface)  square miles.';
commatize 'Ain\'t no numbers in this here words, nohow, no way, Jose.';
commatize 'James was never known as  0000000007';
commatize 'Arthur Eddington wrote: I believe there are 15747724136275002577605653961181555468044717914527116709366231425076185631031296 protons in the universe.';
commatize '   $-140000±100  millions.';
commatize '6/9/1946 was a good year for some.';
 
sub commatize($s, :$at = 0, :$ins = ',', :$by = 3) {
    say "Before: ", $s;
    say " After: ", $s.subst:
        :continue($at), :1st,
        / <[1..9]> <[0..9]>* /,
        *.flip.comb(/<{ ".**1..$by" }>/).join($ins).flip;
}
```

#### Output:
```
Before: pi=3.14159265358979323846264338327950288419716939937510582097494459231
 After: pi=3.14159 26535 89793 23846 26433 83279 50288 41971 69399 37510 58209 74944 59231
Before: The author has two Z$100000000000000 Zimbabwe notes (100 trillion).
 After: The author has two Z$100.000.000.000.000 Zimbabwe notes (100 trillion).
Before: -in Aus$+1411.8millions
 After: -in Aus$+1,411.8millions
Before: ===US$0017440 millions=== (in 2000 dollars)
 After: ===US$0017,440 millions=== (in 2000 dollars)
Before: 123.e8000 is pretty big.
 After: 123.e8000 is pretty big.
Before: The land area of the earth is  57268900(29% of the surface)  square miles.
 After: The land area of the earth is  57,268,900(29% of the surface)  square miles.
Before: Ain't no numbers in this here words, nohow, no way, Jose.
 After: Ain't no numbers in this here words, nohow, no way, Jose.
Before: James was never known as  0000000007
 After: James was never known as  0000000007
Before: Arthur Eddington wrote: I believe there are 15747724136275002577605653961181555468044717914527116709366231425076185631031296 protons in the universe.
 After: Arthur Eddington wrote: I believe there are 15,747,724,136,275,002,577,605,653,961,181,555,468,044,717,914,527,116,709,366,231,425,076,185,631,031,296 protons in the universe.
Before:    $-140000±100  millions.
 After:    $-140,000±100  millions.
Before: 6/9/1946 was a good year for some.
 After: 6/9/1946 was a good year for some.
```