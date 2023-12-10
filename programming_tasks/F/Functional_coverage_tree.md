[1]: https://rosettacode.org/wiki/Functional_coverage_tree

# [Functional coverage tree][1]



```perl
sub walktree ($data) {
    my (@parts, $cnt);

    while ($data ~~ m:nth(++$cnt)/$<head>=[(\s*)    \N+\n  ]      # split off one level as 'head' (or terminal 'leaf')
                                  $<body>=[[$0  \s+ \N+\n]*]/ ) { # next sub-level is 'body' (defined by extra depth of indentation)

        my ($head, $body) = ($<head>, $<body>);
        $head ~~ /'|' $<weight>=[\S*] \s* '|' $<coverage>=[\S*]/; # save values of weight and coverage (if any) for later

        my ($w, $wsum) = (0, 0);
        $head ~= .[0],
        $w    += .[1],
        $wsum += .[1] * .[2]
            for walktree $body;

        my $weight   = (~$<weight>   or 1).fmt('%-8s');
        my $coverage = $w == 0
                    ?? (~$<coverage> or 0).fmt('%-10s')
                    !! ($wsum/$w)         .fmt('%-10.2g');
        @parts.push: [$head.subst(/'|' \N+/, "|$weight|$coverage|"), $weight, $coverage ];
    }
    return @parts;
}

(say .[0] for walktree $_) given 

    q:to/END/
    NAME_HIERARCHY                  |WEIGHT  |COVERAGE  |
    cleaning                        |        |          |
        house1                      |40      |          |
            bedrooms                |        |0.25      |
            bathrooms               |        |          |
                bathroom1           |        |0.5       |
                bathroom2           |        |          |
                outside_lavatory    |        |1         |
            attic                   |        |0.75      |
            kitchen                 |        |0.1       |
            living_rooms            |        |          |
                lounge              |        |          |
                dining_room         |        |          |
                conservatory        |        |          |
                playroom            |        |1         |
            basement                |        |          |
            garage                  |        |          |
            garden                  |        |0.8       |
        house2                      |60      |          |
            upstairs                |        |          |
                bedrooms            |        |          |
                    suite_1         |        |          |
                    suite_2         |        |          |
                    bedroom_3       |        |          |
                    bedroom_4       |        |          |
                bathroom            |        |          |
                toilet              |        |          |
                attics              |        |0.6       |
            groundfloor             |        |          |
                kitchen             |        |          |
                living_rooms        |        |          |
                    lounge          |        |          |
                    dining_room     |        |          |
                    conservatory    |        |          |
                    playroom        |        |          |
                wet_room_&_toilet   |        |          |
                garage              |        |          |
                garden              |        |0.9       |
                hot_tub_suite       |        |1         |
            basement                |        |          |
                cellars             |        |1         |
                wine_cellar         |        |1         |
                cinema              |        |0.75      |
    END
```

#### Output:
```
NAME_HIERARCHY                  |WEIGHT  |COVERAGE  |

cleaning                        |1       |0.41      |
    house1                      |40      |0.33      |
        bedrooms                |1       |0.25      |
        bathrooms               |1       |0.5       |
            bathroom1           |1       |0.5       |
            bathroom2           |1       |0         |
            outside_lavatory    |1       |1         |
        attic                   |1       |0.75      |
        kitchen                 |1       |0.1       |
        living_rooms            |1       |0.25      |
            lounge              |1       |0         |
            dining_room         |1       |0         |
            conservatory        |1       |0         |
            playroom            |1       |1         |
        basement                |1       |0         |
        garage                  |1       |0         |
        garden                  |1       |0.8       |
    house2                      |60      |0.46      |
        upstairs                |1       |0.15      |
            bedrooms            |1       |0         |
                suite_1         |1       |0         |
                suite_2         |1       |0         |
                bedroom_3       |1       |0         |
                bedroom_4       |1       |0         |
            bathroom            |1       |0         |
            toilet              |1       |0         |
            attics              |1       |0.6       |
        groundfloor             |1       |0.32      |
            kitchen             |1       |0         |
            living_rooms        |1       |0         |
                lounge          |1       |0         |
                dining_room     |1       |0         |
                conservatory    |1       |0         |
                playroom        |1       |0         |
            wet_room_&_toilet   |1       |0         |
            garage              |1       |0         |
            garden              |1       |0.9       |
            hot_tub_suite       |1       |1         |
        basement                |1       |0.92      |
            cellars             |1       |1         |
            wine_cellar         |1       |1         |
            cinema              |1       |0.75      |
```
