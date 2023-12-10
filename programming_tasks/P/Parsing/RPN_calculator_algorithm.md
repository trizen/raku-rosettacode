[1]: https://rosettacode.org/wiki/Parsing/RPN_calculator_algorithm

# [Parsing/RPN calculator algorithm][1]



```perl
my $proggie = '3 4 2 * 1 5 - 2 3 ^ ^ / +';

class RPN is Array {

   method binop(&op) { self.push: self.pop R[&op] self.pop }

   method run($p) {
       for $p.words {
           say "$_ ({self})";
           when /\d/ { self.push: $_ }
           when '+'  { self.binop: &[+] }
           when '-'  { self.binop: &[-] }
           when '*'  { self.binop: &[*] }
           when '/'  { self.binop: &[/] }
           when '^'  { self.binop: &[**] }
           default   { die "$_ is bogus" }
       }
       say self;
   }
}

RPN.new.run($proggie);
```

#### Output:
```
3 ()
4 (3)
2 (3 4)
* (3 4 2)
1 (3 8)
5 (3 8 1)
- (3 8 1 5)
2 (3 8 -4)
3 (3 8 -4 2)
^ (3 8 -4 2 3)
^ (3 8 -4 8)
/ (3 8 65536)
+ (3 0.0001220703125)
3.0001220703125
```
