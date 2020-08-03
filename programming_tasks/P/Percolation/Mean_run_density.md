[1]: https://rosettacode.org/wiki/Percolation/Mean_run_density

# [Percolation/Mean run density][1]

```raku
sub R($n, $p) { [+] ((rand < $p) xx $n).squish }
 
say 't= ', constant t = 100;
 
for .1, .3 ... .9 -> $p {
    say "p= $p, K(p)= {$p*(1-$p)}";
    for 10, 100, 1000 -> $n {
	printf "  R(%6d, p)= %f\n", $n, t R/ [+] R($n, $p)/$n xx t
    }
}
```

#### Output:
```
t= 100
p= 0.1, K(p)= 0.09
  R(    10, p)= 0.088000
  R(   100, p)= 0.085600
  R(  1000, p)= 0.089150
p= 0.3, K(p)= 0.21
  R(    10, p)= 0.211000
  R(   100, p)= 0.214600
  R(  1000, p)= 0.211160
p= 0.5, K(p)= 0.25
  R(    10, p)= 0.279000
  R(   100, p)= 0.249200
  R(  1000, p)= 0.250870
p= 0.7, K(p)= 0.21
  R(    10, p)= 0.258000
  R(   100, p)= 0.215400
  R(  1000, p)= 0.209560
p= 0.9, K(p)= 0.09
  R(    10, p)= 0.181000
  R(   100, p)= 0.094500
  R(  1000, p)= 0.091330
```