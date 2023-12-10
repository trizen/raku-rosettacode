[1]: https://rosettacode.org/wiki/Range_modifications

# [Range modifications][1]

Quite a bit of this is just transforming syntax back and forth. Raku already has Ranges and Sequences as first class objects, though the syntax is different.



Demonstrate the task required examples: adding and removing integers, as well as an example adding and removing ranges to / from the sequence, in both native Raku syntax and the "Stringy" syntax required by the task.



Demo with some extremely large values / ranges. Capable of working with infinite ranges by default.



Won't handle negative numbers as written, mostly due the need to work around the syntax requirements for output.

```perl
my @seq;

-> $op, $string { printf "%20s -> %s\n", $op, $string } for
  'Start',     to-string( @seq  = canonicalize "" ),
  'add 77',    to-string( @seq .= &add(77) ),
  'add 79',    to-string( @seq .= &add(79) ),
  'add 78',    to-string( @seq .= &add(78) ),
  'remove 77', to-string( @seq .= &remove(77) ),
  'remove 78', to-string( @seq .= &remove(78) ),
  'remove 79', to-string( @seq .= &remove(79) );

say '';
-> $op, $string { printf "%20s -> %s\n", $op, $string } for
  'Start',    to-string( @seq  = canonicalize "1-3,5-5" ),
  'add 1',    to-string( @seq .= &add(1) ),
  'remove 4', to-string( @seq .= &remove(4) ),
  'add 7',    to-string( @seq .= &add(7) ),
  'add 8',    to-string( @seq .= &add(8) ),
  'add 6',    to-string( @seq .= &add(6) ),
  'remove 7', to-string( @seq .= &remove(7) );

say '';
-> $op, $string { printf "%20s -> %s\n", $op, $string } for
  'Start',     to-string( @seq  = canonicalize "1-5,10-25,27-30" ),
  'add 26',    to-string( @seq .= &add(26) ),
  'add 9',     to-string( @seq .= &add(9) ),
  'add 7',     to-string( @seq .= &add(7) ),
  'remove 26', to-string( @seq .= &remove(26) ),
  'remove 9',  to-string( @seq .= &remove(9) ),
  'remove 7',  to-string( @seq .= &remove(7) );

say '';
-> $op, $string { printf "%30s -> %s\n", $op, $string } for
  'Start',                  to-string( @seq  = canonicalize "6-57,160-251,2700-7000000" ),
  'add "2502-2698"',        to-string( @seq .= &add("2502-2698") ),
  'add 41..69',             to-string( @seq .= &add(41..69) ),
  'remove 17..30',          to-string( @seq .= &remove(17..30) ),
  'remove 4391..6527',      to-string( @seq .= &remove("4391-6527") ),
  'add 2699',               to-string( @seq .= &add(2699) ),
  'add 76',                 to-string( @seq .= &add(76) ),
  'add 78',                 to-string( @seq .= &add(78) ),
  'remove "70-165"',        to-string( @seq .= &remove("70-165") ),
  'remove 16..31',          to-string( @seq .= &remove(16..31) ),
  'add 1.417e16 .. 3.2e21', to-string( @seq .= &add(1.417e16.Int .. 3.2e21.Int) ),
  'remove "4001-Inf"',      to-string( @seq .= &remove("4001-Inf") );


sub canonicalize (Str $ranges) { sort consolidate |sort parse-range $ranges }

sub parse-range (Str $_) { .comb(/\d+|'Inf'/).map: { +$^α .. +$^ω } }

sub to-string (@ranges) { qq|"{ @ranges».minmax».join('-').join(',') }"| }

multi add (@ranges, Int   $i) { samewith @ranges, $i .. $i }
multi add (@ranges, Str   $s) { samewith @ranges, |parse-range($s) }
multi add (@ranges, Range $r) { @ranges > 0 ?? (sort consolidate |sort |@ranges, $r) !! $r }

multi remove (@ranges, Int   $i) { samewith @ranges, $i .. $i }
multi remove (@ranges, Str   $s) { samewith @ranges, |parse-range($s) }
multi remove (@ranges, Range $r) {
    gather for |@ranges -> $this {
        if $r.min <= $this.min {
            if $r.max >= $this.min and $r.max < $this.max {
                take $r.max + 1 .. $this.max
            }
            elsif $r.max < $this.min {
                take $this
            }
        }
        else {
            if $r.max >= $this.max and $r.min <= $this.max {
                take $this.min .. $r.min - 1
            }
            elsif $r.max < $this.max and $r.min > $this.min {
                take $this.min .. $r.min - 1;
                take $r.max + 1 .. $this.max
            }
            else {
                take $this
            }
        }
    }
}

multi consolidate() { () }

multi consolidate($this is copy, **@those) {
    sub infix:<∪> (Range $a, Range $b) { Range.new($a.min,max($a.max,$b.max)) }

    sub infix:<∩> (Range $a, Range $b) { so $a.max >= $b.min }

    my @ranges = sort gather {
        for consolidate |@those -> $that {
            next unless $that;
            if $this ∩ $that { $this ∪= $that }
            else             { take $that }
        }
        take $this;
    }
    for reverse ^(@ranges - 1) {
        if @ranges[$_].max == @ranges[$_ + 1].min - 1 {
            @ranges[$_] = @ranges[$_].min .. @ranges[$_ + 1].max;
            @ranges[$_ + 1]:delete
        }
    }
    @ranges
}
```

#### Output:
```
               Start -> ""
              add 77 -> "77-77"
              add 79 -> "77-77,79-79"
              add 78 -> "77-79"
           remove 77 -> "78-79"
           remove 78 -> "79-79"
           remove 79 -> ""

               Start -> "1-3,5-5"
               add 1 -> "1-3,5-5"
            remove 4 -> "1-3,5-5"
               add 7 -> "1-3,5-5,7-7"
               add 8 -> "1-3,5-5,7-8"
               add 6 -> "1-3,5-8"
            remove 7 -> "1-3,5-6,8-8"

               Start -> "1-5,10-25,27-30"
              add 26 -> "1-5,10-30"
               add 9 -> "1-5,9-30"
               add 7 -> "1-5,7-7,9-30"
           remove 26 -> "1-5,7-7,9-25,27-30"
            remove 9 -> "1-5,7-7,10-25,27-30"
            remove 7 -> "1-5,10-25,27-30"

                         Start -> "6-57,160-251,2700-7000000"
               add "2502-2698" -> "6-57,160-251,2502-2698,2700-7000000"
                    add 41..69 -> "6-69,160-251,2502-2698,2700-7000000"
                 remove 17..30 -> "6-16,31-69,160-251,2502-2698,2700-7000000"
             remove 4391..6527 -> "6-16,31-69,160-251,2502-2698,2700-4390,6528-7000000"
                      add 2699 -> "6-16,31-69,160-251,2502-4390,6528-7000000"
                        add 76 -> "6-16,31-69,76-76,160-251,2502-4390,6528-7000000"
                        add 78 -> "6-16,31-69,76-76,78-78,160-251,2502-4390,6528-7000000"
               remove "70-165" -> "6-16,31-69,166-251,2502-4390,6528-7000000"
                 remove 16..31 -> "6-15,32-69,166-251,2502-4390,6528-7000000"
        add 1.417e16 .. 3.2e21 -> "6-15,32-69,166-251,2502-4390,6528-7000000,14170000000000000-3200000000000000000000"
             remove "4001-Inf" -> "6-15,32-69,166-251,2502-4000"
```
