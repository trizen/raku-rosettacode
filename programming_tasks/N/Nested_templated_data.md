[1]: https://rosettacode.org/wiki/Nested_templated_data

# [Nested templated data][1]

Explicitly not using strings, using one data structure to fill in another. Since it *isn't* a string, the output format removes the newlines from the template; line feed (white space in general) isn't particularly significant in Perl 6 data structures. It does preserve the nesting though. In the second example, payload "buckets" that don't exist result in an undefined value being inserted; by default: Any.

```perl
say join "\n  ", '##PAYLOADS:', |my @payloads = 'Payload#' X~ ^7;
 
for [
     (((1, 2),
       (3, 4, 1),
       5),),
 
     (((1, 2),
       (10, 4, 1),
       5),)
    ] {
    say "\n      Template: ", $_.perl;
    say "Data structure: { @payloads[|$_].perl }";
}
```

#### Output:
```
##PAYLOADS:
  Payload#0
  Payload#1
  Payload#2
  Payload#3
  Payload#4
  Payload#5
  Payload#6

      Template: $(((1, 2), (3, 4, 1), 5),)
Data structure: ((("Payload#1", "Payload#2"), ("Payload#3", "Payload#4", "Payload#1"), "Payload#5"),)

      Template: $(((1, 2), (10, 4, 1), 5),)
Data structure: ((("Payload#1", "Payload#2"), (Any, "Payload#4", "Payload#1"), "Payload#5"),)
```