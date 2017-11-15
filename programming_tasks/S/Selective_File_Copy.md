[1]: http://rosettacode.org/wiki/Selective_File_Copy

# [Selective File Copy][1]

I have no idea how PL/I or COBOL store records and little enthusiasm to research it. If the task author can't be bothered to spell out what the format should look like, then I have no compunction about just making something up out of whole cloth. In the absence of better guidance I am going to make a binary encoded data file format with fixed sized fields consisting of a mix of ISO-8859-1 encoded text and raw binary (hex) encoded integers.



This is WAY more complicated than it could be. Could achieve the same effect in one or two lines of code, but this explicitly shows some of the possible mechanics.



Since the sfc.dat file is binary encoded, I can't include it here easily as text so [here is a link to an online copy](https://github.com/thundergnat/rc/blob/master/sfc.dat) instead.

```perl
my @format = ( # arbitrary and made up record format
    'field a' => { offset => 0,  length => 5, type => 'Str' },
    'field b' => { offset => 5,  length => 5, type => 'Str' },
    'field c' => { offset => 10, length => 4, type => 'Int' },
    'field d' => { offset => 14, length => 1, type => 'Str' },
    'field e' => { offset => 15, length => 5, type => 'Str' }
);
 
my $record-length = @format[*]».value».<length>.sum;
 
my $in = './sfc.dat'.IO.open :r :bin;
 
say "Input data as read from $in:";
my @records;
@records.push: get-record($in, $record-length) until $in.eof;
.perl.say for @records;
 
# not going to bother to actually write out to a file, if you really want to,
# supply a file handle to a local file
say "\nOutput:";
my $outfile = $*OUT; # or some other filename, whatever.
 
for @records -> $r {
    $outfile.printf( "%-5s%s%08x%5s\n", flat $r.{'field a','field d','field c'}, 'xxxxx' );
}
 
sub get-record($fh, $bytes) {
    my $record = $fh.read($bytes);
    return ().Slip unless $record.elems == $bytes;
    my %r = @format.map: {
        .key => do given $_.value.<type> -> $type
        {
            when $type eq 'Str' { $record.subbuf($_.value.<offset>, $_.value.<length>).decode }
            when $type eq 'Int' { sum $record.subbuf($_.value.<offset>, $_.value.<length>) Z+< (24,16,8,0) }
            default             { $record.subbuf($_.value.<offset>, $_.value.<length>) } # Buf
        }
    }
}
```

#### Output:
```
Input data as read from ./sfc.dat:
${"field a" => "A    ", "field b" => "bbbbB", "field c" => 1, "field d" => "+", "field e" => "d2345"}
${"field a" => "AA   ", "field b" => "bbbBB", "field c" => 2, "field d" => "+", "field e" => "1d345"}
${"field a" => "AAA  ", "field b" => "bbBBB", "field c" => 3, "field d" => "+", "field e" => "12d45"}
${"field a" => "AAAA ", "field b" => "bBBBB", "field c" => 4, "field d" => "-", "field e" => "123d5"}
${"field a" => "AAAAA", "field b" => "BBBBB", "field c" => 3729368837, "field d" => "-", "field e" => "1234d"}

Output:
A    +00000001xxxxx
AA   +00000002xxxxx
AAA  +00000003xxxxx
AAAA -00000004xxxxx
AAAAA-de49a705xxxxx
```