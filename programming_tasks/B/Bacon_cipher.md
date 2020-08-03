[1]: https://rosettacode.org/wiki/Bacon_cipher

# [Bacon cipher][1]

Not truly a Bacon Cipher as it doesn't encode using font variations. But fits with the spirit if not the exact definition.

```raku
my $secret = q:to/END/;
    This task is to implement a program for encryption and decryption
    of plaintext using the simple alphabet of the Baconian cipher or
    some other kind of representation of this alphabet (make anything
    signify anything). This example will work with anything in the
    ASCII range... even code! $r%_-^&*(){}+~ #=`/\';*1234567890"'
    END
 
my $text = q:to/END/;
    Bah. It isn't really practical to use typeface changes to encode
    information, it is too easy to tell that there is something going
    on and will attract attention. Font changes with enough regularity
    to encode mesages relatively efficiently would need to happen so
    often it would be obvious that there was some kind of manipulation
    going on. Steganographic encryption where it is obvious that there
    has been some tampering with the carrier is not going to be very
    effective. Not that any of these implementations would hold up to
    serious scrutiny anyway. Anyway, here's a semi-bogus implementation
    that hides information in white space. The message is hidden in this
    paragraph of text. Yes, really. It requires a fairly modern file
    viewer to display (not display?) the hidden message, but that isn't
    too unlikely any more. It may be stretching things to call this a
    Bacon cipher, but I think it falls within the spirit of the task,
    if not the exact definition.
    END
#'
my @enc = "﻿", "​";
my %dec = @enc.pairs.invert;
 
sub encode ($c) { @enc[($c.ord).fmt("%07b").comb].join('') }
 
sub hide ($msg is copy, $text) { 
    $msg ~= @enc[0] x (0 - ($msg.chars % 8)).abs;
    my $head = $text.substr(0,$msg.chars div 8);
    my $tail = $text.substr($msg.chars div 8, *-1);
    ($head.comb «~» $msg.comb(/. ** 8/)).join('') ~ $tail;
}
 
sub reveal ($steg) {
    join '', map { :2(%dec{$_.comb}.join('')).chr }, 
    $steg.subst( /\w | <punct> | " " | "\n" /, '', :g).comb(/. ** 7/);
}
 
my $hidden = join '', map  { .&encode }, $secret.comb;
 
my $steganography = hide $hidden, $text;
 
say "Steganograpic message hidden in text:";
say $steganography;
 
say '*' x 70;
 
say "Hidden message revealed:";
say reveal $steganography;
```

#### Output:
```
Steganograpic message hidden in text:
B​﻿​﻿​﻿﻿​a​﻿​﻿﻿﻿​​h﻿​﻿﻿​​​​.﻿﻿​​﻿​﻿﻿ ﻿﻿﻿​​​﻿​I﻿﻿​​﻿﻿﻿﻿t​​​​﻿﻿​​ ​​﻿​﻿​​﻿i​﻿﻿﻿﻿﻿​​s﻿​﻿﻿​​​​n﻿﻿​​﻿​﻿﻿'﻿﻿﻿​​​﻿​t﻿﻿​​﻿​​​ ​﻿​﻿﻿﻿﻿﻿r​​﻿​﻿﻿​​e​﻿​​﻿​​​a​﻿﻿﻿﻿​​﻿l​​﻿﻿​​﻿﻿l​﻿​​​﻿​​y﻿​​​﻿﻿​﻿ ​​​﻿​​​﻿p​​​﻿​﻿﻿﻿r​﻿﻿﻿﻿﻿​​a﻿﻿﻿﻿​﻿​﻿c﻿﻿﻿﻿​​​﻿t﻿﻿﻿​​​﻿﻿i​﻿​​﻿​​​c​​​﻿﻿​​​a​​​﻿﻿​﻿​l​﻿﻿﻿﻿​​​ ﻿​​﻿​﻿​﻿t﻿﻿﻿﻿​​﻿﻿o​​﻿​​﻿​​ ​​​​​﻿﻿​u﻿﻿​﻿﻿﻿﻿﻿s​​﻿﻿​﻿​​e​﻿​​​﻿​​ ﻿﻿﻿​​​​​t﻿﻿​﻿​​​​y﻿﻿​​​​﻿﻿p﻿﻿​​​﻿​﻿e﻿​​﻿​﻿﻿​f​​﻿​​​​​a​﻿​​​﻿﻿​c﻿﻿﻿﻿﻿​​﻿e﻿﻿﻿​​​﻿​ ​​﻿​​﻿﻿​c﻿﻿﻿​﻿﻿﻿﻿h﻿​​﻿﻿​﻿﻿a​​﻿﻿​﻿​​n​﻿﻿﻿​​​​g​﻿﻿​﻿​​​e​﻿﻿​​​​﻿s﻿﻿﻿​​​﻿​ ﻿﻿​​﻿​﻿﻿t​​​﻿​​​​o​​﻿​​​﻿﻿ ﻿﻿​﻿​﻿​​e﻿​​​​​​﻿n﻿​​﻿﻿​﻿﻿c﻿﻿﻿​​​﻿﻿o﻿﻿​​﻿​​﻿d﻿​​﻿﻿﻿﻿​e​​﻿​﻿﻿​​
i​﻿​﻿﻿​​﻿n﻿​﻿​​​​​f﻿﻿﻿​​​﻿​o﻿﻿﻿​﻿﻿﻿﻿r﻿​​​﻿​﻿​m​​​﻿﻿​​​a​﻿​﻿﻿​​​t﻿​​​﻿​​﻿i﻿​​​﻿​﻿﻿o﻿﻿﻿​​​﻿​n﻿﻿​​﻿​﻿﻿,﻿​​﻿﻿​﻿​ ﻿​﻿﻿﻿﻿﻿​i​​﻿﻿​​​​t﻿​﻿﻿​​​﻿ ​​﻿​​​​﻿i﻿﻿﻿​​﻿​​s﻿﻿​​﻿﻿​﻿ ​﻿​﻿﻿﻿﻿﻿t​​﻿﻿﻿﻿​​o​﻿​​﻿﻿​​o​﻿﻿﻿﻿​​﻿ ​﻿﻿﻿​​﻿﻿e﻿﻿​​​﻿﻿﻿a​﻿​​﻿﻿​﻿s​​​​﻿​﻿﻿y﻿​﻿﻿﻿﻿﻿​ ​﻿​​​​​​t﻿﻿​​﻿﻿​﻿o﻿﻿﻿﻿​​​﻿ ​﻿﻿​​﻿​﻿t﻿﻿​​﻿﻿​﻿e​﻿​﻿﻿﻿﻿﻿l​﻿﻿﻿﻿​﻿​l​﻿﻿﻿﻿​​​ ﻿﻿﻿​​​​﻿t​​​​​​﻿​h​​﻿​​﻿​﻿a﻿​​​﻿﻿﻿﻿t​​​﻿​​​﻿ ﻿​﻿﻿﻿﻿﻿​t​﻿﻿﻿​​​​h﻿​﻿﻿​​​​e﻿﻿﻿﻿​​﻿​r﻿﻿﻿​​﻿﻿​e﻿​​​​﻿﻿​ ﻿﻿​﻿﻿﻿﻿﻿i​​﻿​​​​​s​​﻿﻿​﻿﻿﻿ ﻿​﻿​﻿​​​s﻿﻿​​​​﻿​o​​​​​﻿​​m﻿​​​﻿﻿​﻿e​﻿​﻿﻿﻿﻿﻿t​​﻿​​​​​h​​﻿​﻿﻿​​i﻿​﻿﻿﻿​​﻿n﻿​﻿​​​​﻿g﻿​﻿﻿​﻿﻿﻿ ﻿﻿​​﻿​﻿​g​​​﻿​﻿﻿​o​​﻿​​​﻿​i​﻿﻿​﻿﻿﻿​n﻿﻿﻿﻿﻿​​﻿g​​​​​​﻿﻿
o﻿﻿​​​﻿﻿​n﻿​​﻿﻿​﻿​ ​​​﻿﻿﻿﻿​a​​﻿﻿​﻿​​n﻿﻿​﻿​​​​d﻿﻿​​​​﻿﻿ ​﻿​​​﻿​​w​﻿​​​﻿​﻿i﻿​​﻿﻿﻿﻿​l​​​﻿​﻿﻿​l​﻿​﻿﻿​​​ ﻿​​​​​​﻿a​​​﻿﻿​﻿﻿t﻿﻿﻿​​﻿​​t​​​​﻿﻿​​r﻿﻿​﻿﻿﻿﻿﻿a​​​﻿​﻿﻿​c​﻿​﻿﻿﻿​​t﻿​﻿﻿​​​​ ﻿﻿​​﻿​﻿﻿a﻿﻿﻿​​﻿﻿﻿t﻿​​​﻿​​﻿t﻿​​​﻿﻿﻿﻿e​​﻿​﻿﻿﻿​n​﻿﻿﻿﻿​​​t﻿﻿﻿​﻿​​﻿i﻿​﻿​​​​﻿o​﻿﻿﻿​﻿﻿﻿n﻿﻿﻿​﻿​﻿﻿.﻿​​﻿​​﻿​ ​​﻿﻿﻿﻿​​F​﻿​﻿​​​​o﻿﻿​﻿​﻿​﻿n﻿﻿﻿﻿​​﻿﻿t﻿﻿​​​﻿​​ ​﻿​​​​﻿﻿c​​​​﻿​﻿﻿h​​﻿​﻿﻿﻿​a​﻿​﻿﻿​​​n﻿​​​﻿​​﻿g﻿​​​﻿﻿﻿​e﻿​﻿​​​﻿﻿s​​​​﻿​﻿﻿ ​​​﻿﻿​​​w​​﻿​​​﻿​i​﻿​﻿﻿​​​t﻿﻿​​﻿​​​h​﻿﻿​﻿​﻿﻿ ﻿﻿﻿​​﻿﻿﻿e﻿​​​﻿​​​n﻿​​​​﻿﻿​o​​​﻿​﻿﻿​u​﻿​﻿﻿﻿​​g﻿​﻿﻿​​​﻿h​​​﻿​​﻿﻿ ​​​﻿​﻿​﻿r﻿​﻿​﻿​​​e﻿﻿​﻿﻿﻿﻿﻿g​﻿​﻿​﻿﻿​u​﻿​﻿﻿﻿​​l﻿​﻿﻿​​​​a﻿﻿​​﻿​﻿﻿r﻿﻿﻿​​﻿﻿​i﻿​​​​​﻿﻿t﻿​​﻿﻿﻿﻿​y​​﻿​​﻿​​
t﻿​​﻿﻿​​﻿o﻿​﻿​﻿​﻿﻿ ﻿﻿﻿​​​﻿​e​​​​﻿​﻿﻿n​​​﻿​​﻿﻿c​​﻿​​﻿﻿﻿o​﻿﻿﻿﻿﻿​​d​﻿​​​​​﻿e​​​​​​​﻿ ﻿​﻿​​﻿​﻿m​​﻿​﻿﻿﻿﻿e﻿​​​﻿​​​s​​﻿​﻿﻿​​a​​﻿​﻿﻿​​g﻿​﻿﻿﻿﻿​﻿e﻿﻿﻿﻿​​﻿﻿s﻿﻿​​​﻿​​ ​﻿​​​​﻿﻿r​​​​﻿​﻿﻿e​​﻿​﻿﻿﻿​l​﻿​﻿﻿​​​a﻿​​​﻿​​﻿t﻿​​​﻿​﻿﻿i﻿﻿﻿​​﻿​﻿v﻿​​​﻿​​​e﻿﻿​﻿﻿﻿﻿﻿l​​​﻿​﻿﻿​y​﻿​﻿﻿﻿​​ ﻿﻿​﻿​﻿﻿﻿e​﻿​﻿​﻿﻿﻿f﻿﻿​​﻿​﻿﻿f​​​﻿﻿﻿﻿​i​​﻿﻿​﻿﻿​c​﻿﻿​﻿﻿​﻿i​﻿﻿﻿﻿﻿​​e​﻿﻿​﻿​​﻿n﻿﻿﻿​​​﻿​t​​﻿​​﻿﻿​l​​​​﻿﻿​﻿y​﻿​﻿​​​﻿ ﻿​﻿​​​﻿﻿w​﻿​​​﻿﻿​o﻿﻿﻿﻿﻿​​﻿u﻿​﻿​​​​﻿l​​﻿​​﻿﻿​d﻿​​​﻿​​​ ﻿﻿​﻿﻿﻿﻿﻿n​​﻿﻿﻿​​​e​﻿​​​​​​e﻿﻿​﻿﻿​​﻿d﻿​﻿​﻿​﻿﻿ ﻿﻿​﻿​﻿﻿﻿t﻿﻿﻿​﻿﻿​﻿o﻿​​​﻿﻿​﻿ ﻿​﻿﻿​﻿​​h﻿​​​​​﻿​a﻿​​﻿​​﻿​p​​​﻿﻿​﻿﻿p​​﻿﻿​﻿​﻿e​﻿﻿​﻿​﻿﻿n﻿﻿​﻿​﻿﻿​ ​​​​﻿​​​s​​​​﻿​﻿​o﻿​﻿​​​​​
o﻿﻿﻿﻿​﻿﻿﻿f​​﻿​​​​﻿t​​​﻿﻿﻿﻿﻿e﻿​﻿​​​​​n﻿​​​﻿﻿﻿​ ﻿﻿​​​﻿​​i​﻿​​﻿​﻿​t﻿​﻿﻿​​﻿﻿ ﻿​﻿​​﻿﻿​w﻿﻿​​﻿﻿​​o﻿​​﻿​﻿﻿﻿u​​﻿​﻿​﻿​l​﻿​​﻿﻿​​d﻿​​​﻿​​​ ﻿﻿﻿﻿​​​﻿b﻿​﻿​​﻿﻿﻿e﻿﻿​﻿﻿﻿​﻿ ﻿​﻿﻿​​​﻿o﻿﻿​﻿​﻿﻿﻿bvious that there was some kind of manipulation
going on. Steganographic encryption where it is obvious that there
has been some tampering with the carrier is not going to be very
effective. Not that any of these implementations would hold up to
serious scrutiny anyway. Anyway, here's a semi-bogus implementation
that hides information in white space. The message is hidden in this
paragraph of text. Yes, really. It requires a fairly modern file
viewer to display (not display?) the hidden message, but that isn't
too unlikely any more. It may be stretching things to call this a
Bacon cipher, but I think it falls within the spirit of the task,
if not the exact definition.
**********************************************************************
Hidden message revealed:
This task is to implement a program for encryption and decryption
of plaintext using the simple alphabet of the Baconian cipher or
some other kind of representation of this alphabet (make anything
signify anything). This example will work with anything in the
ASCII range... even code! $r%_-^&*(){}+~ #=`/\';*1234567890"'
```