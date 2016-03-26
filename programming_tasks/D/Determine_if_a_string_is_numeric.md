[1]: http://rosettacode.org/wiki/Determine_if_a_string_is_numeric

# [Determine if a string is numeric][1]

```perl
sub is-number( $term --> Bool ) {
    ?($term ~~ /\d/) and +$term ~~ Numeric;
}
 
printf "%10s %s\n", "<$_>", is-number( $_ ) for flat
<1 1.2 1.2.3 -6 1/2 12e B17 1.3e+12 1.3e12 -2.6e-3 zero
0x 0xA10 0b1001 0o16 0o18 2+5i>, '1 1 1', '', ' ';
```

#### Output:
```
       <1> True
     <1.2> True
   <1.2.3> False
      <-6> True
     <1/2> True
     <12e> False
     <B17> False
 <1.3e+12> True
  <1.3e12> True
 <-2.6e-3> True
    <zero> False
      <0x> False
   <0xA10> True
  <0b1001> True
    <0o16> True
    <0o18> False
    <2+5i> True
   <1 1 1> False
        <> False
       < > False
```