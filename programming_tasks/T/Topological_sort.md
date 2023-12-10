[1]: https://rosettacode.org/wiki/Topological_sort

# [Topological sort][1]



```perl
sub print_topo_sort ( %deps ) {
    my %ba;
    for %deps.kv -> $before, @afters {
        for @afters -> $after {
            %ba{$before}{$after} = 1 if $before ne $after;
            %ba{$after} //= {};
        }
    }

    while %ba.grep( not *.value )».key -> @afters {
        say ~@afters.sort;
        %ba{@afters}:delete;
        for %ba.values { .{@afters}:delete }
    }

    say %ba ?? "Cycle found! {%ba.keys.sort}" !! '---';
}

my %deps =
    des_system_lib => < std synopsys std_cell_lib des_system_lib dw02
                                                     dw01 ramlib ieee >,
    dw01           => < ieee dw01 dware gtech                         >,
    dw02           => < ieee dw02 dware                               >,
    dw03           => < std synopsys dware dw03 dw02 dw01 ieee gtech  >,
    dw04           => < dw04 ieee dw01 dware gtech                    >,
    dw05           => < dw05 ieee dware                               >,
    dw06           => < dw06 ieee dware                               >,
    dw07           => < ieee dware                                    >,
    dware          => < ieee dware                                    >,
    gtech          => < ieee gtech                                    >,
    ramlib         => < std ieee                                      >,
    std_cell_lib   => < ieee std_cell_lib                             >,
    synopsys       => <                                               >;

print_topo_sort(%deps);
%deps<dw01> = <ieee dw01 dware gtech dw04>; # Add unresolvable dependency
print_topo_sort(%deps);
```

#### Output:
```
ieee std synopsys
dware gtech ramlib std_cell_lib
dw01 dw02 dw05 dw06 dw07
des_system_lib dw03 dw04
---
ieee std synopsys
dware gtech ramlib std_cell_lib
dw02 dw05 dw06 dw07
Cycle found! des_system_lib dw01 dw03 dw04
```


Some differences from the Perl 5 version include use of
formal parameters; use of `»` as a "hyper" operator, that is, a parallelizable implicit loop; and use of normal lambda-like notation to bind loop parameters, so we can have multiple loop parameters bound on each iteration.  Also,
since `=>` is now a real pair composer rather than a synonym for comma, the data can be represented with real pair notation that points to quoted word lists delimited by angle brackets rather than `[qw(...)]`.
