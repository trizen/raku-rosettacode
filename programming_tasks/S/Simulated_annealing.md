[1]: https://rosettacode.org/wiki/Simulated_annealing

# [Simulated annealing][1]

```raku
# simulation setup
my \cities = 100;  # number of cities
my \kmax   = 1e6;  # iterations to run
my \kT     = 1;    # initial 'temperature'
 
die 'City count must be a perfect square.' if cities.sqrt != cities.sqrt.Int;
 
# locations of (up to) 8 neighbors, with grid size derived from number of cities
my \gs = cities.sqrt;
my \neighbors = [1, -1, gs, -gs, gs-1, gs+1, -(gs+1), -(gs-1)];
 
# matrix of distances between cities
my \D = [;];
for 0 ..^ cities² -> \j {
    my (\ab, \cd)       = (j/cities, j%cities)».Int;
    my (\a, \b, \c, \d) = (ab/gs, ab%gs, cd/gs, cd%gs)».Int;
    D[ab;cd] = sqrt (a - c)² + (b - d)²
}
 
sub T(\k, \kmax, \kT)      { (1 - k/kmax) × kT }                                 # temperature function, decreases to 0
sub P(\ΔE, \k, \kmax, \kT) { exp( -ΔE / T(k, kmax, kT)) }                        # probability to move from s to s_next
sub Es(\path)              { sum (D[ path[$_]; path[$_+1] ] for 0 ..^ +path-1) } # energy at s, to be minimized
 
# variation of E, from state s to state s_next
sub delta-E(\s, \u, \v) {
    my (\a,   \b,  \c,  \d) = D[s[u-1];s[u]], D[s[u+1];s[u]], D[s[v-1];s[v]], D[s[v+1];s[v]];
    my (\na, \nb, \nc, \nd) = D[s[u-1];s[v]], D[s[u+1];s[v]], D[s[v-1];s[u]], D[s[v+1];s[u]];
    if    v == u+1 { return (na + nd) - (a + d) }
    elsif u == v+1 { return (nc + nb) - (c + b) }
    else           { return (na + nb + nc + nd) - (a + b + c + d) }
}
 
# E(s0), initial state
my \s = @ = flat 0, (1 ..^ cities).pick(*), 0;
my \E-min-global = my \E-min = $ = Es(s);
 
for 0 ..^ kmax -> \k {
    printf "k:%8u  T:%4.1f  Es: %3.1f\n" , k, T(k, kmax, kT), Es(s)
            if k % (kmax/10) == 0;
 
    # valid candidate cities (exist, adjacent)
    my \cv = neighbors[(^8).roll] + s[ my \u = 1 + (^(cities-1)).roll ];
    next if cv ≤ 0 or cv ≥ cities or D[s[u];cv] > sqrt(2);
 
    my \v  = s[cv];
    my \ΔE = delta-E(s, u, v);
    if ΔE < 0 or P(ΔE, k, kmax, kT) ≥ rand { # always move if negative
        (s[u], s[v]) = (s[v], s[u]);
        E-min += ΔE;
        E-min-global = E-min if E-min < E-min-global;
    }
}
 
say "\nE(s_final): " ~ E-min-global.fmt('%.1f');
say "Path:\n" ~ s».fmt('%2d').rotor(20,:partial).join: "\n";
```

#### Output:
```
k:       0  T: 1.0  Es: 522.0
k:  100000  T: 0.9  Es: 185.3
k:  200000  T: 0.8  Es: 166.1
k:  300000  T: 0.7  Es: 174.7
k:  400000  T: 0.6  Es: 146.9
k:  500000  T: 0.5  Es: 140.2
k:  600000  T: 0.4  Es: 127.5
k:  700000  T: 0.3  Es: 115.9
k:  800000  T: 0.2  Es: 111.9
k:  900000  T: 0.1  Es: 109.4

E(s_final): 109.4
Path:
 0 10 20 30 40 50 60 84 85 86 96 97 87 88 98 99 89 79 78 77
67 68 69 59 58 57 56 66 76 95 94 93 92 91 90 80 70 81 82 83
73 72 71 62 63 64 74 75 65 55 54 53 52 61 51 41 31 21 22 32
42 43 44 45 46 35 34 24 23 33 25 15 16 26 36 47 37 27 17 18
28 38 48 49 39 29 19  9  8  7  6  5  4 14 13 12 11  2  3  1
 0
```
