[1]: https://rosettacode.org/wiki/Letter_frequency

# [Letter frequency][1]





In Raku, whenever you want to count things in a collection, the rule of thumb is to use the Bag structure.



*In response to some of the breathless exposition in the [Frink](#Frink) entry.*



"How many other languages in this page do all or any of this correctly?" Quite a few I suspect. Some even moreso than Frink.

```perl
.&ws.say for slurp.comb.Bag.sort: -*.value;

sub ws ($pair) { 
    $pair.key ~~ /\n/
    ?? ('NEW LINE' => $pair.value)
    !! $pair.key ~~ /\s/
    ?? ($pair.key.uniname => $pair.value)
    !! $pair
}
```

#### Output:
```
SPACE => 522095
e => 325692
t => 222916
a => 199790
o => 180974
h => 170210
n => 167006
i => 165201
s => 157585
r => 145118
d => 106987
l => 97131
NEW LINE => 67662
u => 67340
c => 62717
m => 56021
f => 53494
w => 53301
, => 48784
g => 46060
p => 39932
y => 37985
b => 34276
. => 30589
v => 24045
" => 14340
k => 14169
T => 12547
- => 11037
I => 10067
A => 7355
H => 6600
M => 6206
; => 5885
E => 4968
C => 4583
S => 4392
' => 3938
x => 3692
! => 3539
R => 3531
P => 3424
O => 3401
j => 3390
B => 3185
W => 3180
N => 3053
? => 2976
F => 2754
G => 2508
: => 2468
J => 2448
L => 2444
q => 2398
V => 2200
_ => 2070
z => 1847
D => 1756
é => 1326
Y => 1238
U => 895
1 => 716
8 => 412
X => 333
K => 321
è => 292
3 => 259
2 => 248
5 => 220
0 => 218
* => 181
4 => 181
) => 173
( => 173
6 => 167
É => 146
7 => 143
Q => 135
] => 122
[ => 122
9 => 117
æ => 106
= => 75
ê => 74
Z => 59
à => 59
â => 56
> => 50
< => 50
/ => 50
ç => 48
NO-BREAK SPACE => 45
î => 39
ü => 37
| => 36
ô => 34
# => 26
ù => 18
ï => 18
Æ => 10
û => 9
+ => 5
È => 5
ë => 5
À => 4
@ => 2
ñ => 2
Ç => 2
$ => 2
% => 1
& => 1
{ => 1
} => 1
½ => 1
```
