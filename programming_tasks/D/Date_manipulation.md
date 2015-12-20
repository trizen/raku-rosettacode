[1]: http://rosettacode.org/wiki/Date_manipulation

# [Date manipulation][1]

Perl 6 comes with a build-in DateTime type
to support most aspects of standard civic time calculation
that are not dependent on cultural idiosyncracies. 

Unfortunately, Perl 6 does not yet have a date parsing module
(mostly due to a reticence to inflict Western cultural imperialism on other cultures...
or maybe just due to laziness), but that just gives us another opportunity to demonstrate the built-in grammar support.

```perl
my @month = <January February March April May June July August September October November December>;
my %month = (@month Z=> ^12).flat, (@month».substr(0,3) Z=> ^12).flat, 'Sept' => 8;
 
grammar US-DateTime {
    rule TOP { <month> <day>','? <year>','? <time> <tz> }
 
    token month {
	(\w+)'.'?  { make %month{$0} // die "Bad month name: $0" }
    }
 
    token day { (\d ** 1..2) { make +$0 } }
 
    token year { (\d ** 1..4) { make +$0 } }
 
    token time {
	(\d ** 1..2) ':' (\d ** 2) \h* ( :i <[ap]> \.? m | '' )
	{
	    my $h = $0 % 12;
	    my $m = $1;
	    $h += 12 if $2 and $2.substr(0,1).lc eq 'p';
	    make $h * 60 + $m;
	}
    }
 
    token tz {  # quick and dirty for this task
        [
        |        EDT  { make -4 }
        | [ EST| CDT] { make -5 }
        | [ CST| MDT] { make -6 }
        | [ MST| PDT] { make -7 }
        | [ PST|AKDT] { make -8 }
        | [AKST|HADT] { make -9 }
        |  HAST
        ]
    }
}
 
$/ = US-DateTime.parse('March 7 2009 7:30pm EST') or die "Can't parse date";
 
my $year     = $<year>.ast;
my $month    = $<month>.ast;
my $day      = $<day>.ast;
my $hour     = $<time>.ast div 60;
my $minute   = $<time>.ast mod 60;
my $timezone = $<tz>.ast * 3600;
 
my $dt = DateTime.new(:$year, :$month, :$day, :$hour, :$minute, :$timezone).in-timezone(0);
 
$dt .= delta(12,hour);
 
say "12 hours later, GMT: $dt";
say "12 hours later, PST: $dt.in-timezone(-8 * 3600)";
```

#### Output:
```
12 hours later, GMT: 2009-02-08T12:30:00Z
12 hours later, PST: 2009-02-08T04:30:00-0800
```