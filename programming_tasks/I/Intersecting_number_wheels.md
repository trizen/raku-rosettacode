[1]: https://rosettacode.org/wiki/Intersecting_number_wheels

# [Intersecting number wheels][1]


A succinct Raku example using a few additional language features. Wheels are implemented as infinite repeating sequences, allowing a single iterator to keep track of the current position. This means the code contains no position tracking whatsoever.

```perl
 
#| advance rotates a named wheel $n by consuming an item from an infinite sequence. It is called
#| from within a gather block and so can use take in order to construct an infinite, lazy sequence
#| of result values
sub advance($g, $n) {
	given $g{$n}.pull-one {
		when /\d/ { take $_ }
		default   { samewith $g, $_ } # samewith re-calls this function with new parameters
	}
}
 
#| Input groups are a hash containing each wheel name as the key, and a list of values constructed
#| using <> to split on whitespace. They are transformed using xx * to repeat the list infinitely.
#| We then retrieve the underlying iterator in order for wheel position to be persistent. Each group
#| is then aggregated into a lazy output sequence using an infinite loop inside a gather block.
[
	{A => <1 2 3>},
	{A => <1 B 2>, B => <3 4>},
	{A => <1 D D>, D => <6 7 8>},
	{A => <1 B C>, B => <3 4>, C => <5 B>},
]
	#| %() converts a list of pairs produced by map into a hash. $^k and $^v are implicit variables.
	#| They are processed in alphabetical order and make the block arity 2, called with two vars.
	#| .kv gets the list of wheel names and wheel values from the input entry
	==> map({ %(.kv.map: { $^k => (|$^v xx *).iterator }) })
	#| gather constructs a lazy sequence, in which we infinitely loop advancing wheel A
	==> map({ gather { loop { advance $_, 'A' }} })
	#| state variables are only initialised once, and are kept between invocations.
	==> map({ state $i = 1; say "Group {$i++}, First 20 values: $_[^20]" })
 
```

#### Output:
```
Group 1, First 20 values: 1 2 3 1 2 3 1 2 3 1 2 3 1 2 3 1 2 3 1 2
Group 2, First 20 values: 1 3 2 1 4 2 1 3 2 1 4 2 1 3 2 1 4 2 1 3
Group 3, First 20 values: 1 6 7 1 8 6 1 7 8 1 6 7 1 8 6 1 7 8 1 6
Group 4, First 20 values: 1 3 5 1 4 3 1 4 5 1 3 4 1 3 5 1 4 3 1 4
```
