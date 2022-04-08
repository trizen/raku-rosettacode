[1]: https://rosettacode.org/wiki/Verhoeff_algorithm

# [Verhoeff algorithm][1]

Generate the tables rather than hard coding, They're not all that complex.

```perl
my @d = [^10] xx 5;
@d[$_][^5].=rotate($_), @d[$_][5..*].=rotate($_) for 1..4;
push @d: [@d[$_].reverse] for flat 1..4, 0;
 
my @i = 0,4,3,2,1,5,6,7,8,9;
 
my %h = flat (0,1,5,8,9,4,2,7,0).rotor(2 =>-1).map({.[0]=>.[1]}), 6=>3, 3=>6;
my @p = [^10],;
@p.push: [@p[*-1].map: {%h{$_}}] for ^7;
 
sub checksum (Int $int where * ≥ 0, :$verbose = True ) {
    my @digits = $int.comb;
    say "\nCheckdigit calculation for $int:";
    say " i  ni  p(i, ni)  c" if $verbose;
    my ($i, $p, $c) = 0 xx 3;
    say " $i   0      $p     $c" if $verbose;
    for @digits.reverse {
        ++$i;
        $p = @p[$i % 8][$_];
        $c = @d[$c; $p];
        say "{$i.fmt('%2d')}   $_      $p     $c" if $verbose;
    }
    say "Checkdigit: {@i[$c]}";
    +($int ~ @i[$c]);
}
 
sub validate (Int $int where * ≥ 0, :$verbose = True) {
    my @digits = $int.comb;
    say "\nValidation calculation for $int:";
    say " i  ni  p(i, ni)  c" if $verbose;
    my ($i, $p, $c) = 0 xx 3;
    for @digits.reverse {
        $p = @p[$i % 8][$_];
        $c = @d[$c; $p];
        say "{$i.fmt('%2d')}   $_      $p     $c" if $verbose;
        ++$i;
    }
    say "Checkdigit: {'in' if $c}correct";
}
 
## TESTING
 
for 236, 12345, 123456789012 -> $int {
    my $check = checksum $int, :verbose( $int.chars < 8 );
    validate $check, :verbose( $int.chars < 8 );
    validate +($check.chop ~ 9), :verbose( $int.chars < 8 );
}
```

#### Output:
```
Checkdigit calculation for 236:
 i  ni  p(i, ni)  c
 0   0      0     0
 1   6      3     3
 2   3      3     1
 3   2      1     2
Checkdigit: 3

Validation calculation for 2363:
 i  ni  p(i, ni)  c
 0   3      3     3
 1   6      3     1
 2   3      3     4
 3   2      1     0
Checkdigit: correct

Validation calculation for 2369:
 i  ni  p(i, ni)  c
 0   9      9     9
 1   6      3     6
 2   3      3     8
 3   2      1     7
Checkdigit: incorrect

Checkdigit calculation for 12345:
 i  ni  p(i, ni)  c
 0   0      0     0
 1   5      8     8
 2   4      7     1
 3   3      6     7
 4   2      5     2
 5   1      2     4
Checkdigit: 1

Validation calculation for 123451:
 i  ni  p(i, ni)  c
 0   1      1     1
 1   5      8     9
 2   4      7     2
 3   3      6     8
 4   2      5     3
 5   1      2     0
Checkdigit: correct

Validation calculation for 123459:
 i  ni  p(i, ni)  c
 0   9      9     9
 1   5      8     1
 2   4      7     8
 3   3      6     2
 4   2      5     7
 5   1      2     5
Checkdigit: incorrect

Checkdigit calculation for 123456789012:
Checkdigit: 0

Validation calculation for 1234567890120:
Checkdigit: correct

Validation calculation for 1234567890129:
Checkdigit: incorrect
```
