[1]: http://rosettacode.org/wiki/Read_a_file_character_by_character/UTF8

# [Read a file character by character/UTF8][1]

Perl 6 has a built in method .getc to get a single character from an open file handle. File handles default to UTF-8, so they will handle multi-byte characters correctly.



To read a single character at a time from the Standard Input terminal; $\*IN in Perl 6:

```perl
.say while defined $_ = $*IN.getc;
```


Or, from a file:

```perl
my $filename = 'whatever';
 
my $in = open( $filename, :r ) or die "$!\n";
 
print $_ while defined $_ = $in.getc;
```