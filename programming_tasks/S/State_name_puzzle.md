[1]: https://rosettacode.org/wiki/State_name_puzzle

# [State name puzzle][1]

```perl
my @states = <
    Alabama Alaska Arizona Arkansas California Colorado Connecticut Delaware
    Florida Georgia Hawaii Idaho Illinois Indiana Iowa Kansas Kentucky
    Louisiana Maine Maryland Massachusetts Michigan Minnesota Mississippi
    Missouri Montana Nebraska Nevada New_Hampshire New_Jersey New_Mexico
    New_York North_Carolina North_Dakota Ohio Oklahoma Oregon Pennsylvania
    Rhode_Island South_Carolina South_Dakota Tennessee Texas Utah Vermont
    Virginia Washington West_Virginia Wisconsin Wyoming
>;
 
say "50 states:";
.say for anastates @states;
 
say "\n54 states:";
.say for sort anastates @states, < New_Kory Wen_Kory York_New Kory_New New_Kory >;
 
sub anastates (*@states) {
    my @s = @states.unique».subst('_', ' ');
 
    my @pairs = gather for ^@s -> $i {
	for $i ^..^ @s -> $j {
	    take [ @s[$i], @s[$j] ];
	}
    }
 
    my $equivs = hash @pairs.classify: *.lc.comb.sort.join;
 
    gather for $equivs.values -> @c {
	for ^@c -> $i {
	    for $i ^..^ @c -> $j {
		my $set = set @c[$i].list, @c[$j].list;
		take @c[$i].list.join(', ') ~ ' = ' ~ @c[$j].list.join(', ') if $set == 4;
	    }
	}
    }
}
```


Output:


#### Output:
```
50 states:
North Carolina, South Dakota = North Dakota, South Carolina

54 states:
New Kory, Kory New = Wen Kory, York New
New Kory, Wen Kory = York New, Kory New
New Kory, York New = Wen Kory, Kory New
New York, Kory New = New Kory, Wen Kory
New York, Kory New = New Kory, York New
New York, Kory New = Wen Kory, York New
New York, New Kory = Wen Kory, Kory New
New York, New Kory = Wen Kory, York New
New York, New Kory = York New, Kory New
New York, Wen Kory = New Kory, Kory New
New York, Wen Kory = New Kory, York New
New York, Wen Kory = York New, Kory New
New York, York New = New Kory, Kory New
New York, York New = New Kory, Wen Kory
New York, York New = Wen Kory, Kory New
North Carolina, South Dakota = North Dakota, South Carolina
```