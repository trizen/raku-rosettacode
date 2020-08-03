[1]: https://rosettacode.org/wiki/Canonicalize_CIDR

# [Canonicalize CIDR][1]

### String manipulation

```perl
#!/usr/bin/env raku
 
# canonicalize a CIDR block: make sure none of the host bits are set
if (!@*ARGS) {
   @*ARGS = $*IN.lines;
}
 
for @*ARGS -> $cidr {
 
  # dotted-decimal / bits in network part
  my ($dotted, $size) = $cidr.split('/');
 
  # get IP as binary string
  my $binary = $dotted.split('.').map(*.fmt("%08b")).join;
 
  # Replace the host part with all zeroes
  $binary.substr-rw($size) = 0 x (32 - $size);
 
  # Convert back to dotted-decimal
  my $canon = $binary.comb(8).map(*.join.parse-base(2)).join('.');
 
  # And output
  say "$canon/$size";
}
```

#### Output:
```
$ canonicalize_cidr.raku 87.70.141.1/22
87.70.140.0/22
```


### Bit mask and shift

```perl
# canonicalize a IP4 CIDR block
sub CIDR-IP4-canonicalize ($address) {
  constant @mask = 24, 16, 8, 0;
 
  # dotted-decimal / subnet size
  my ($dotted, $size) = |$address.split('/'), 32;
 
  # get IP as binary address
  my $binary = sum $dotted.comb(/\d+/) Z+< @mask;
 
  # mask off subnet
  $binary +&= (2 ** $size - 1) +< (32 - $size);
 
  # Return dotted-decimal notation
  (@mask.map($binary +> * +& 0xFF).join('.'), $size)
}
 
my @tests = <
  87.70.141.1/22
  36.18.154.103/12
  62.62.197.11/29
  67.137.119.181/4
  161.214.74.21/24
  184.232.176.184/18
  100.68.0.18/18
  10.4.30.77/30
  10.207.219.251/32
  10.207.219.251
  110.200.21/4
  10.11.12.13/8
  10.../8
>;
 
printf "CIDR: %18s  Routing prefix: %s/%s\n", $_, |.&CIDR-IP4-canonicalize
  for @*ARGS || @tests;
```

#### Output:
```
CIDR:     87.70.141.1/22  Routing prefix: 87.70.140.0/22
CIDR:   36.18.154.103/12  Routing prefix: 36.16.0.0/12
CIDR:    62.62.197.11/29  Routing prefix: 62.62.197.8/29
CIDR:   67.137.119.181/4  Routing prefix: 64.0.0.0/4
CIDR:   161.214.74.21/24  Routing prefix: 161.214.74.0/24
CIDR: 184.232.176.184/18  Routing prefix: 184.232.128.0/18
CIDR:     100.68.0.18/18  Routing prefix: 100.68.0.0/18
CIDR:      10.4.30.77/30  Routing prefix: 10.4.30.76/30
CIDR:  10.207.219.251/32  Routing prefix: 10.207.219.251/32
CIDR:     10.207.219.251  Routing prefix: 10.207.219.251/32
CIDR:       110.200.21/4  Routing prefix: 96.0.0.0/4
CIDR:      10.11.12.13/8  Routing prefix: 10.0.0.0/8
CIDR:            10.../8  Routing prefix: 10.0.0.0/8
```
