[1]: https://rosettacode.org/wiki/Nautical_bell

# [Nautical bell][1]

Perl 6 uses [UTC](http://en.wikipedia.org/wiki/Coordinated_Universal_Time) (GMT) time internally and by default. This will display the current UTC time and on the half hour, display a graphical representation of the bell. If run in a terminal with the system bell enabled, will also chime the system alarm bell.

```raku
my @watch = <Middle Morning Forenoon Afternoon Dog First>;
my @ordinal = <One Two Three Four Five Six Seven Eight>;
 
my $thishour;
my $thisminute = '';
 
loop {
    my $utc = DateTime.new(time);
    if $utc.minute ~~ any(0,30) and $utc.minute != $thisminute {
        $thishour   = $utc.hour;
        $thisminute = $utc.minute;
        bell($thishour, $thisminute);
    }
    printf "%s%02d:%02d:%02d", "\r", $utc.hour, $utc.minute, $utc.second;
    sleep(1);
}
 
sub bell ($hour, $minute) {
 
    my $bells = (($hour % 4) * 2 + $minute div 30) || 8;
 
    printf "%s%02d:%02d %9s watch, %6s Bell%s Gone: \t", "\b" x 9, $hour, $minute,
      @watch[($hour div 4 - !?($minute + $hour % 4) + 6) % 6],
      @ordinal[$bells - 1], $bells == 1 ?? '' !! 's';
 
    chime($bells);
 
    sub chime ($count) {
	for 1..$count div 2 {
		print "\a♫ ";
		sleep .25;
		print "\a";
		sleep .75;
	}
	if $count % 2 {
	     print "\a♪";
	     sleep 1;
        }
        print "\n";
    }
}
```

#### Output:
```
00:00     First watch,  Eight Bells Gone:       ♫ ♫ ♫ ♫ 
00:30    Middle watch,    One Bell Gone:        ♪
01:00    Middle watch,    Two Bells Gone:       ♫ 
01:30    Middle watch,  Three Bells Gone:       ♫ ♪
02:00    Middle watch,   Four Bells Gone:       ♫ ♫ 
02:30    Middle watch,   Five Bells Gone:       ♫ ♫ ♪
03:00    Middle watch,    Six Bells Gone:       ♫ ♫ ♫ 
03:30    Middle watch,  Seven Bells Gone:       ♫ ♫ ♫ ♪
04:00    Middle watch,  Eight Bells Gone:       ♫ ♫ ♫ ♫ 
04:30   Morning watch,    One Bell Gone:        ♪
05:00   Morning watch,    Two Bells Gone:       ♫ 
05:30   Morning watch,  Three Bells Gone:       ♫ ♪
06:00   Morning watch,   Four Bells Gone:       ♫ ♫ 
06:30   Morning watch,   Five Bells Gone:       ♫ ♫ ♪
07:00   Morning watch,    Six Bells Gone:       ♫ ♫ ♫ 
07:30   Morning watch,  Seven Bells Gone:       ♫ ♫ ♫ ♪
08:00   Morning watch,  Eight Bells Gone:       ♫ ♫ ♫ ♫ 
08:30  Forenoon watch,    One Bell Gone:        ♪
09:00  Forenoon watch,    Two Bells Gone:       ♫ 
09:30  Forenoon watch,  Three Bells Gone:       ♫ ♪
10:00  Forenoon watch,   Four Bells Gone:       ♫ ♫ 
10:30  Forenoon watch,   Five Bells Gone:       ♫ ♫ ♪
11:00  Forenoon watch,    Six Bells Gone:       ♫ ♫ ♫ 
11:30  Forenoon watch,  Seven Bells Gone:       ♫ ♫ ♫ ♪
12:00  Forenoon watch,  Eight Bells Gone:       ♫ ♫ ♫ ♫ 
12:30 Afternoon watch,    One Bell Gone:        ♪
13:00 Afternoon watch,    Two Bells Gone:       ♫ 
13:30 Afternoon watch,  Three Bells Gone:       ♫ ♪
14:00 Afternoon watch,   Four Bells Gone:       ♫ ♫ 
14:30 Afternoon watch,   Five Bells Gone:       ♫ ♫ ♪
15:00 Afternoon watch,    Six Bells Gone:       ♫ ♫ ♫ 
15:30 Afternoon watch,  Seven Bells Gone:       ♫ ♫ ♫ ♪
16:00 Afternoon watch,  Eight Bells Gone:       ♫ ♫ ♫ ♫ 
16:30       Dog watch,    One Bell Gone:        ♪
17:00       Dog watch,    Two Bells Gone:       ♫ 
17:30       Dog watch,  Three Bells Gone:       ♫ ♪
18:00       Dog watch,   Four Bells Gone:       ♫ ♫ 
18:30       Dog watch,   Five Bells Gone:       ♫ ♫ ♪
19:00       Dog watch,    Six Bells Gone:       ♫ ♫ ♫ 
19:30       Dog watch,  Seven Bells Gone:       ♫ ♫ ♫ ♪
20:00       Dog watch,  Eight Bells Gone:       ♫ ♫ ♫ ♫ 
20:30     First watch,    One Bell Gone:        ♪
21:00     First watch,    Two Bells Gone:       ♫ 
21:30     First watch,  Three Bells Gone:       ♫ ♪
22:00     First watch,   Four Bells Gone:       ♫ ♫ 
22:30     First watch,   Five Bells Gone:       ♫ ♫ ♪
23:00     First watch,    Six Bells Gone:       ♫ ♫ ♫ 
23:30     First watch,  Seven Bells Gone:       ♫ ♫ ♫ ♪
```