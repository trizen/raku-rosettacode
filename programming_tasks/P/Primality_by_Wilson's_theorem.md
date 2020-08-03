[1]: https://rosettacode.org/wiki/Primality_by_Wilson%27s_theorem

# [Primality by Wilson's theorem][1]





Not a particularly recommended way to test for primality, especially for larger numbers. It *works*, but is slow and memory intensive.

```raku
sub postfix:<!> (Int $n) { (constant f = 1, |[\*] 1..*)[$n] }
 
sub is-wilson-prime (Int $p where * > 1) { (($p - 1)! + 1) %% $p }
 
# Pre initialize factorial routine (not thread safe)
9000!;
 
# Testing
put '   p  prime?';
printf("%4d  %s\n", $_, .&is-wilson-prime) for 2, 3, 9, 15, 29, 37, 47, 57, 67, 77, 87, 97, 237, 409, 659;
 
my $wilsons = (2,3,*+2…*).hyper.grep: *.&is-wilson-prime;
 
put "\nFirst 120 primes:";
put $wilsons[^120].rotor(20)».fmt('%3d').join: "\n";
 
put "\n1000th through 1015th primes:";
put $wilsons[999..1014];
```

#### Output:
```
   p  prime?
   2  True
   3  True
   9  False
  15  False
  29  True
  37  True
  47  True
  57  False
  67  True
  77  False
  87  False
  97  True
 237  False
 409  True
 659  True

First 120 primes:
  2   3   5   7  11  13  17  19  23  29  31  37  41  43  47  53  59  61  67  71
 73  79  83  89  97 101 103 107 109 113 127 131 137 139 149 151 157 163 167 173
179 181 191 193 197 199 211 223 227 229 233 239 241 251 257 263 269 271 277 281
283 293 307 311 313 317 331 337 347 349 353 359 367 373 379 383 389 397 401 409
419 421 431 433 439 443 449 457 461 463 467 479 487 491 499 503 509 521 523 541
547 557 563 569 571 577 587 593 599 601 607 613 617 619 631 641 643 647 653 659

1000th through 1015th primes:
7919 7927 7933 7937 7949 7951 7963 7993 8009 8011 8017 8039 8053 8059 8069 8081
```
