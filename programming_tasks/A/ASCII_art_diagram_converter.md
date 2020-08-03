[1]: https://rosettacode.org/wiki/ASCII_art_diagram_converter

# [ASCII art diagram converter][1]

```perl
grammar RFC1025 {
    rule  TOP {  <.line-separator> [<line> <.line-separator>]+ }
    rule  line-separator { <.ws> '+--'+ '+' }
    token line  { <.ws> '|' +%% <field>  }
    token field  { \s* <label> \s* }
    token label { \w+[\s+\w+]* }
}
 
sub bits ($item) { ($item.chars + 1) div 3 }
 
sub deconstruct ($bits, %struct) {
    map { $bits.substr(.<from>, .<bits>) }, @(%struct<fields>);
}
 
sub interpret ($header) {
    my $datagram = RFC1025.parse($header);
    my %struct;
    for $datagram.<line> -> $line {
        FIRST %struct<line-width> = $line.&bits;
        state $from = 0;
        %struct<fields>.push: %(:bits(.&bits), :ID(.<label>.Str), :from($from.clone), :to(($from+=.&bits)-1))
          for $line<field>;
    }
    %struct
}
 
use experimental :pack;
 
my $diagram = q:to/END/;
 
    +--+--+--+--+--+--+--+--+--+--+--+--+--+--+--+--+
    |                      ID                       |
    +--+--+--+--+--+--+--+--+--+--+--+--+--+--+--+--+
    |QR|   Opcode  |AA|TC|RD|RA|   Z    |   RCODE   |
    +--+--+--+--+--+--+--+--+--+--+--+--+--+--+--+--+
    |                    QDCOUNT                    |
    +--+--+--+--+--+--+--+--+--+--+--+--+--+--+--+--+
    |                    ANCOUNT                    |
    +--+--+--+--+--+--+--+--+--+--+--+--+--+--+--+--+
    |                    NSCOUNT                    |
    +--+--+--+--+--+--+--+--+--+--+--+--+--+--+--+--+
    |                    ARCOUNT                    |
    +--+--+--+--+--+--+--+--+--+--+--+--+--+--+--+--+
 
END
 
my %structure = interpret($diagram);
 
say 'Line width: ', %structure<line-width>, ' bits';
printf("Name: %7s, bit count: %2d, bit %2d to bit %2d\n", .<ID>, .<bits>, .<from>, .<to>) for @(%structure<fields>);
say "\nGenerate a random 12 byte \"header\"";
say my $buf = Buf.new((^0xFF .roll) xx 12);
say "\nShow it converted to a bit string";
say my $bitstr = $buf.unpack('C*')».fmt("%08b").join;
say "\nAnd unpack it";
printf("%7s, %02d bits: %s\n", %structure<fields>[$_]<ID>,  %structure<fields>[$_]<bits>,
  deconstruct($bitstr, %structure)[$_]) for ^@(%structure<fields>);
```

#### Output:
```
Line width: 16 bits
Name:      ID, bit count: 16, bit  0 to bit 15
Name:      QR, bit count:  1, bit 16 to bit 16
Name:  Opcode, bit count:  4, bit 17 to bit 20
Name:      AA, bit count:  1, bit 21 to bit 21
Name:      TC, bit count:  1, bit 22 to bit 22
Name:      RD, bit count:  1, bit 23 to bit 23
Name:      RA, bit count:  1, bit 24 to bit 24
Name:       Z, bit count:  3, bit 25 to bit 27
Name:   RCODE, bit count:  4, bit 28 to bit 31
Name: QDCOUNT, bit count: 16, bit 32 to bit 47
Name: ANCOUNT, bit count: 16, bit 48 to bit 63
Name: NSCOUNT, bit count: 16, bit 64 to bit 79
Name: ARCOUNT, bit count: 16, bit 80 to bit 95

Generate a random 12 byte "header"
Buf:0x<78 47 7b bf 54 96 e1 2e 1b f1 69 a4>

Show it converted to a bit string
011110000100011101111011101111110101010010010110111000010010111000011011111100010110100110100100

And unpack it
     ID, 16 bits: 0111100001000111
     QR, 01 bits: 0
 Opcode, 04 bits: 1111
     AA, 01 bits: 0
     TC, 01 bits: 1
     RD, 01 bits: 1
     RA, 01 bits: 1
      Z, 03 bits: 011
  RCODE, 04 bits: 1111
QDCOUNT, 16 bits: 0101010010010110
ANCOUNT, 16 bits: 1110000100101110
NSCOUNT, 16 bits: 0001101111110001
ARCOUNT, 16 bits: 0110100110100100
```