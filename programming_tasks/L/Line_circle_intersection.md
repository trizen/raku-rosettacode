[1]: https://rosettacode.org/wiki/Line_circle_intersection

# [Line circle intersection][1]

Extend solution space to 3D. Reference: this [SO question and answers](https://stackoverflow.com/questions/1073336/)

```raku
#!/usr/bin/env perl6
 
sub LineCircularOBJintersection(@P1, @P2, @Centre, \Radius) {
   my @d = @P2 »-« @P1 ;           # d
   my @f = @P1 »-« @Centre ;       # c
 
   my \a = [+] @d»²;               # d dot d
   my \b = 2 * ([+] @f »*« @d);    # 2 * f dot d
   my \c = ([+] @f»²) - Radius²;   # f dot f - r²
   my \Δ =  b²-(4*a*c);            # discriminant
 
   if (Δ < 0) {
      return [];
   } else {
      my (\t1,\t2) = (-b - Δ.sqrt)/(2*a), (-b + Δ.sqrt)/(2*a);
      if 0 ≤ t1|t2 ≤ 1 {
         return @P1 »+« ( @P2 »-« @P1 ) »*» t1, @P1 »+« ( @P2 »-« @P1 ) »*» t2
      } else {
         return []
      }
   }
}
 
my \DATA = [
   [ <-10 11>, < 10 -9>, <3 -5>, 3 ],
   [ <-10 11>, <-11 12>, <3 -5>, 3 ],
   [ <  3 -2>, <  7 -2>, <3 -5>, 3 ],
   [ <  3 -2>, <  7 -2>, <0  0>, 4 ],
   [ <  0 -3>, <  0  6>, <0  0>, 4 ],
   [ <  6  3>, < 10  7>, <4  2>, 5 ],
   [ <  7  4>, < 11 18>, <4  2>, 5 ],
   [ <5  2 −2.26 >, <0.77 2 4>, <1 4 0>, 4 ]
];
 
for DATA {
   my @solution = LineCircularOBJintersection $_[0] , $_[1] , $_[2], $_[3];
   say "For data set: ", $_;
   say "Solution(s) is/are: ", @solution.Bool ?? @solution !! "None";
}
```

#### Output:
```
For data set: [(-10 11) (10 -9) (3 -5) 3]
Solution(s) is/are: [(3 -2) (6 -5)]
For data set: [(-10 11) (-11 12) (3 -5) 3]
Solution(s) is/are: None
For data set: [(3 -2) (7 -2) (3 -5) 3]
Solution(s) is/are: [(3 -2) (3 -2)]
For data set: [(3 -2) (7 -2) (0 0) 4]
Solution(s) is/are: [(-3.4641016151377544 -2) (3.4641016151377544 -2)]
For data set: [(0 -3) (0 6) (0 0) 4]
Solution(s) is/are: [(0 -4) (0 4)]
For data set: [(6 3) (10 7) (4 2) 5]
Solution(s) is/are: [(1 -2) (8 5)]
For data set: [(7 4) (11 18) (4 2) 5]
Solution(s) is/are: [(5.030680985703315 -2.892616550038399) (7.459885052032535 5.60959768211387)]
For data set: [(5 2 −2.26) (0.77 2 4) (1 4 0) 4]
Solution(s) is/are: [(4.2615520237084015 2 -1.1671668246843006) (1.13386504516801 2 3.461514141193441)]
```
