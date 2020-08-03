[1]: https://rosettacode.org/wiki/Mayan_calendar

# [Mayan calendar][1]

```perl
my @sacred = <Imix’ Ik’ Ak’bal K’an Chikchan Kimi Manik’ Lamat Muluk Ok
              Chuwen Eb Ben Hix Men K’ib’ Kaban Etz’nab’ Kawak Ajaw>;
 
my @civil = <Pop Wo’ Sip Sotz’ Sek Xul Yaxk’in Mol Ch’en Yax Sak’ Keh
             Mak K’ank’in Muwan’ Pax K’ayab Kumk’u Wayeb’>;
 
my %correlation = :GMT({
    :gregorian(Date.new('2012-12-21')),
    :round([3,19,263,8]),
    :long(1872000)
});
 
sub mayan-calendar-round ($date) { .&tzolkin, .&haab given $date }
 
sub offset ($date, $factor = 'GMT') { Date.new($date) - %correlation{$factor}<gregorian> }
 
sub haab ($date, $factor = 'GMT') {
    my $index = (%correlation{$factor}<round>[2] + offset $date) % 365;
    my ($day, $month);
    if $index > 360 {
        $day = $index - 360;
        $month = @civil[18];
        if $day == 5 {
            $day = 'Chum';
            $month = @civil[0];
        }
    } else {
        $day = $index % 20;
        $month = @civil[$index div 20];
        if $day == 0 {
            $day = 'Chum';
            $month = @civil[(1 + $index) div 20];
        }
    }
    $day, $month
}
 
sub tzolkin ($date, $factor = 'GMT') {
    my $offset = offset $date;
    1 + ($offset + %correlation{$factor}<round>[0]) % 13,
    @sacred[($offset + %correlation{$factor}<round>[1]) % 20]
}
 
sub lord ($date, $factor = 'GMT') {
    'G' ~ 1 + (%correlation{$factor}<round>[3] + offset $date) % 9
}
 
sub mayan-long-count ($date, $factor = 'GMT') {
    my $days = %correlation{$factor}<long> + offset $date;
    reverse $days.polymod(20,18,20,20);
}
 
# HEADER
say ' Gregorian   Tzolk’in         Haab’             Long           Lord of ';
say '   Date       # Name       Day Month            Count         the Night';
say '-----------------------------------------------------------------------';
 
# DATES
<
1963-11-21
2004-06-19
2012-12-18
2012-12-21
2019-01-19
2019-03-27
2020-02-29
2020-03-01
2071-05-16
>.map: -> $date {
    printf "%10s   %2s %-9s %4s %-10s     %-14s %6s\n", Date.new($date),
      flat mayan-calendar-round($date), mayan-long-count($date).join('.'), lord($date);
}
```

#### Output:
```
 Gregorian   Tzolk’in        Haab’              Long           Lord of 
   Date       # Name       Day Month            Count         the Night
-----------------------------------------------------------------------
1963-11-21    3 Eb        Chum Keh            12.17.10.3.12      G9
2004-06-19    4 Ben         16 Sotz’          12.19.11.6.13      G7
2012-12-18    1 Kaban     Chum K’ank’in       12.19.19.17.17     G6
2012-12-21    4 Ajaw         3 K’ank’in       13.0.0.0.0         G9
2019-01-19    1 Ajaw        13 Muwan’         13.0.6.3.0         G6
2019-03-27    3 Manik’    Chum Wayeb’         13.0.6.6.7         G1
2020-02-29    4 Kimi        14 K’ayab         13.0.7.5.6         G7
2020-03-01    5 Manik’      15 K’ayab         13.0.7.5.7         G8
2071-05-16    1 Ok          18 Sip            13.2.19.4.10       G9
```