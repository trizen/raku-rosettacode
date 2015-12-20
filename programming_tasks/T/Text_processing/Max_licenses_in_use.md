[1]: http://rosettacode.org/wiki/Text_processing/Max_licenses_in_use

# [Text processing/Max licenses in use][1]

Redirecting the mlijobs.txt file to STDIN:

```perl6
my %licenses;
 
%licenses<count max> = 0,0;
 
for $*IN.lines -> $line { 
    my ( $license, $date_time );
    ( *, $license, *, $date_time ) = split /\s+/, $line;
    if $license eq 'OUT' {
        %licenses<count>++;
        if %licenses<count> > %licenses<max> {
            %licenses<max>   = %licenses<count>;
            %licenses<times> = [$date_time];
        }
        elsif %licenses<count> == %licenses<max> {
            %licenses<times>.push($date_time);
        }
    }
    else {
        if %licenses<count> == %licenses<max> {
            %licenses<times>[*-1] ~= " through " ~ $date_time;
        }
        %licenses<count>--;
    }
};
 
my $plural = %licenses<times>.elems == 1 ?? '' !! 's';
 
say "Maximum concurrent licenses in use: {%licenses<max>}, in the time period{$plural}:";
say join ",\n", %licenses<times>.list;
```


Example output:


#### Output:
```
Maximum concurrent licenses in use: 99, in the time periods:
2008/10/03_08:39:34 through 2008/10/03_08:39:45,
2008/10/03_08:40:40 through 2008/10/03_08:40:47
```