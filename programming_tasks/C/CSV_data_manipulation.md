[1]: https://rosettacode.org/wiki/CSV_data_manipulation

# [CSV data manipulation][1]

On the face of it this task is pretty simple. Especially given the sample CSV file and the total lack of specification of *what* changes to make to the file. Something like this would suffice.

```perl
my $csvfile = './whatever.csv';
my $fh = open($csvfile, :r);
my @header = $fh.get.split(',');
my @csv = map {[.split(',')]>>.Num}, $fh.lines;
close $fh;
 
my $out = open($csvfile, :w);
$out.say((@header,'SUM').join(','));
$out.say((@$_, [+] @$_).join(',')) for @csv;
close $out;
```


But if your CSV file is at all complex you are better off using a CSV parsing module. (Complex meaning fields that contain commas, quotes, newlines, etc.)

```perl
use Text::CSV;
my $csvfile = './whatever.csv';
my @csv = Text::CSV.parse-file($csvfile);
# modify(@csv); # do whatever;
csv-write-file( @csv, :file($csvfile) );
```