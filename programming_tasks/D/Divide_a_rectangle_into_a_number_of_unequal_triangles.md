[1]: https://rosettacode.org/wiki/Divide_a_rectangle_into_a_number_of_unequal_triangles

# [Divide a rectangle into a number of unequal triangles][1]

The first triangle bisects the rectangle via the diagonal.  The rest of them all got one vertex at the origin and a side defined by ratios of numbers from a descending positive integer sequence.

```perl
# 20220123 Raku programming solution 

# Proof :
#
#    H-----------A---------B-------C-----D---E
#    |                                       |
#    |                                       |
#    |                                       |
#    O---------------------------------------L
#
#    ▲OEL is unique as its area is the sum of the rest.
#
#    and also in terms of area ▲OHA > ▲OAB > ... > ▲ODE
 

sub UnequalDivider (\L,\H,\N where N > 2) { 

   my \sum = $ = 0 ; my \part = $ = 0 ; my @sequence = (N^...1) ; 

   loop {                                               #   if  ▲OHA ~ ▲OEL    
      sum  = @sequence.sum;                             #   increase 1st term 
      @sequence[0]*L*L/sum == H*H ?? (@sequence[0] +=1) !! last 
   }

   (  [ (0,0), (L,H), (L,0) ],  ).Array.append: @sequence.map: -> \chunk {
      [ (0,0), (L*part/sum,H), (L*(part+=chunk)/sum,H) ] ;
   } 
}

.say for UnequalDivider(1000,500,5);
```

#### Output:
```
[(0 0) (1000 500) (1000 0)]
[(0 0) (0 500) (400 500)]
[(0 0) (400 500) (700 500)]
[(0 0) (700 500) (900 500)]
[(0 0) (900 500) (1000 500)]
```
