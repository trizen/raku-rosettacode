[1]: http://rosettacode.org/wiki/Substitution_Cipher

# [Substitution Cipher][1]

Feed it an action (encode, decode) and a file name at the command line and it will spit out the (en|de)coded file to STDOUT. Redirect into a file to save it. If no parameters are passed in, does the demo encode/decode.

```perl6
my $chr = (' ' .. '}').join('');
my $key = $chr.comb.pick(*).join('');
 
# Be very boring and use the same key every time to fit task reqs.
$key = q☃3#}^",dLs*>tPMcZR!fmC rEKhlw1v4AOgj7Q]YI+|pDB82a&XFV9yzuH<WT%N;iS.0e:`G\n['6@_{bk)=-5qx(/?$JoU☃;
 
sub MAIN ($action = 'encode', $file = '') {
 
    die 'Only options are encode or decode.' unless $action ~~ any 'encode'|'decode';
 
    my $text = qq:to/END/;
        Here we have to do is there will be a input/source file in which 
        we are going to Encrypt the file by replacing every upper/lower 
        case alphabets of the source file with another predetermined 
        upper/lower case alphabets or symbols and save it into another 
        output/encrypted file and then again convert that output/encrypted 
        file into original/decrypted file. This type of Encryption/Decryption
        scheme is often called a Substitution Cipher.
        END
 
    $text = $file.IO.slurp if $file;
 
    say "Key = $key\n";
 
    if $file {
        say &::($action)($text);
    } else {
        my $encoded;
        say "Encoded text: \n {$encoded = encode $text}";
        say "Decoded text: \n {decode $encoded}";
    }
}
 
sub encode ($text) { $text.trans($chr => $key) }
 
sub decode ($text) { $text.trans($key => $chr) }
```

#### Output:
```
Key = 3#}^",dLs*>tPMcZR!fmC rEKhlw1v4AOgj7Q]YI+|pDB82a&XFV9yzuH<WT%N;iS.0e:`G\n['6@_{bk)=-5qx(/?$JoU

Encoded text: 
 +`=`3(`3n.x`35b3:b3[-35n`=`3([@@30`3.3[{kq5Z-bq=e`3G[@`3[{3(n[en3
(`3.=`3\b[{\35b3]{e=?k535n`3G[@`30?3=`k@.e[{\3`x`=?3qkk`=Z@b(`=3
e.-`3.@kn.0`5-3bG35n`3-bq=e`3G[@`3([5n3.{b5n`=3k=`:`5`=_[{`:3
qkk`=Z@b(`=3e.-`3.@kn.0`5-3b=3-?_0b@-3.{:3-.x`3[53[{5b3.{b5n`=3
bq5kq5Z`{e=?k5`:3G[@`3.{:35n`{3.\.[{3eb{x`=535n.53bq5kq5Z`{e=?k5`:3
G[@`3[{5b3b=[\[{.@Z:`e=?k5`:3G[@`c39n[-35?k`3bG3]{e=?k5[b{ZQ`e=?k5[b{
-en`_`3[-3bG5`{3e.@@`:3.3Vq0-5[5q5[b{37[kn`=c

Decoded text: 
 Here we have to do is there will be a input/source file in which 
we are going to Encrypt the file by replacing every upper/lower 
case alphabets of the source file with another predetermined 
upper/lower case alphabets or symbols and save it into another 
output/encrypted file and then again convert that output/encrypted 
file into original/decrypted file. This type of Encryption/Decryption
scheme is often called a Substitution Cipher.
```