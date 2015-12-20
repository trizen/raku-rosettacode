[1]: http://rosettacode.org/wiki/Type_detection

# [Type detection][1]

Perl 6 is a dynamic language that has gradual, duck typing. It provides introspection methods through its comprehensive MOP (Meta Object Protocol) making it easy to do type detection, subroutine signatures and multi-dispatch. Perl 6 types have two general flavors: content types and container types. Different container types have varying restrictions on what sort of content they can contain and in return provide specialized methods to operate on those contents. Content types give the compiler hints on how to best handle the information, what storage requirements it may have, what operators will work with it, etc.



This is really a very broad and kind of hand-wavey overview of Perl 6 types. For much more indepth coverage see [Perl 6 Synopsis S02: Bits and Pieces: Built-In Data Types](http://design.perl6.org/S02.html#Built-In\_Data\_Types%7C)

```perl6
sub type ($t) { say $t.perl, "\tis type: ", $t.WHAT }
 
# some content types
.&type for 1, 2.0, 3e0, 4i, π, Inf, NaN, 'String';
 
# some primitive container types
.&type for $, [ ], @, { }, %, (5 .. 7), (8 ... 10), /0/, {;}, sub {}, ( );
 
# undefined things
.&type for Any, Nil;
 
# user defined types
class my-type { };
 
my my-type $object;
 
$object.&type;
```

#### Output:
```
1       is type: (Int)
2.0     is type: (Rat)
3e0     is type: (Num)
<0+4i>  is type: (Complex)
3.14159265358979e0      is type: (Num)
Inf     is type: (Num)
NaN     is type: (Num)
"String"        is type: (Str)
Any     is type: (Any)
$[]     is type: (Array)
$[]     is type: (Array)
{}      is type: (Hash)
{}      is type: (Hash)
5..7    is type: (Range)
(8, 9, 10).Seq  is type: (Seq)
/0/     is type: (Regex)
-> ;; $_? is raw { #`(Block|61385680) ... }     is type: (Block)
sub () { #`(Sub|62948936) ... } is type: (Sub)
$()     is type: (List)
Any     is type: (Any)
Nil     is type: Nil
my-type is type: (my-type)
```