[1]: https://rosettacode.org/wiki/Implicit_type_conversion

# [Implicit type conversion][1]

Perl 6 was designed with a specific goal of maximizing the principle of DWIM (Do What I Mean) while simultaneously minimizing the principle of DDWIDM (Don't Do What I Don't Mean). Implicit type conversion is a natural and basic feature.



Variable names in Perl 6 are prepended with a sigil.
The most basic variable container type is a scalar, with the sigil dollar sign: $x.
A single scalar variable in list context will be converted to a list of one element regardless of the variables structure.
(A scalar variable may be bound to any object, including a collective object.
A scalar variable is always treated as a singular item, regardless of whether the object is essentially composite or unitary.
There is no implicit conversion from singular to plural; a plural object within a singular container must be explicitly decontainerized somehow.
Use of a subscript is considered sufficiently explicit though.)



The type of object contained in a scalar depends on how you assign it and how you use it.

```perl
my $x;
 
$x = 1234;      say $x.WHAT; # (Int) Integer
$x = 12.34;     say $x.WHAT; # (Rat) Rational
$x = 1234e-2;   say $x.WHAT; # (Num) Floating point Number
$x = 1234+i;    say $x.WHAT; # (Complex)
$x = '1234';    say $x.WHAT; # (Str) String
$x = (1,2,3,4); say $x.WHAT; # (List)
$x = [1,2,3,4]; say $x.WHAT; # (Array)
$x = 1 .. 4;    say $x.WHAT; # (Range)
$x = (1 => 2);  say $x.WHAT; # (Pair)
$x = {1 => 2};  say $x.WHAT; # (Hash)
$x = {1, 2};    say $x.WHAT; # (Block)
$x = sub {1};   say $x.WHAT; # (Sub) Code Reference
$x = True;      say $x.WHAT; # (Bool) Boolean
```




Objects may be converted between various types many times during an operation. Consider the following line of code.

```perl
say :16(([+] 1234.ords).sqrt.floor ~ "beef");
```


In English: Take the floor of the square root of the sum of the ordinals of the digits of the integer 1234, concatenate that number with the string 'beef', interpret the result as a hexadecimal number and print it.



Broken down step by step:

```perl
my $x = 1234;                                  say $x, ' ', $x.WHAT; # 1234 (Int)
$x = 1234.ords;                                say $x, ' ', $x.WHAT; # 49 50 51 52 (List)
$x = [+] 1234.ords;                            say $x, ' ', $x.WHAT; # 202 (Int)
$x = ([+] 1234.ords).sqrt;                     say $x, ' ', $x.WHAT; # 14.2126704035519 (Num)
$x = ([+] 1234.ords).sqrt.floor;               say $x, ' ', $x.WHAT; # 14 (Int)
$x = ([+] 1234.ords).sqrt.floor ~ "beef";      say $x, ' ', $x.WHAT; # 14beef (Str)
$x = :16(([+] 1234.ords).sqrt.floor ~ "beef"); say $x, ' ', $x.WHAT; # 1359599 (Int)
```




Some types are not implicitly converted.
For instance, you must explicitly request and cast to Complex numbers and FatRat numbers.
(A normal Rat number has a denominator that is limited to 64 bits, with underflow to floating point to prevent performance degradation; a FatRat, in contrast, has an unlimited denominator size, and can chew up all your memory if you're not careful.)

```perl
my $x;
$x = (-1).sqrt;           say $x, ' ', $x.WHAT; # NaN (Num)
$x = (-1).Complex.sqrt;   say $x, ' ', $x.WHAT; # 6.12323399573677e-17+1i (Complex)
 
$x = (22/7) * 2;          say $x, ' ', $x.WHAT; # 6.285714 (Rat)
$x /= 10**10;             say $x, ' ', $x.WHAT; # 0.000000000629 (Rat)
$x /= 10**10;             say $x, ' ', $x.WHAT; # 6.28571428571429e-20 (Num)
 
$x = (22/7).FatRat * 2;   say $x, ' ', $x.WHAT; # 6.285714 (FatRat)
$x /= 10**10;             say $x, ' ', $x.WHAT; # 0.000000000629 (FatRat)
$x /= 10**10;             say $x, ' ', $x.WHAT; # 0.0000000000000000000629 (FatRat)
 
```


User defined types will support implicit casting if the object has Bridge method that tells it how to do so, or if the operators in question supply multiple dispatch variants that allow for coercions.
Note that Perl 6 does not support implicit assignment coercion to typed variables.
However, different-sized storage types (int16, int32, int64, for example) are not considered different types, and such assignment merely enforces a constraint that will throw an exception if the size is exceeded. (The calculations on the right side of the assignment are done in an arbitrarily large type such as Int.)



Types may be explicitly cast by using a bridge method (.Int, .Rat, .Str, whatever) or by using a coercion operator:


#### Output:
```
    + or -      numify
    ~           stringify
    ? or !      boolify
    i (postfix) complexify
    $()         singularize
    @()         pluralize
    %()         hashify
```