[1]: https://rosettacode.org/wiki/Iterators

# [Iterators][1]

Raku has iterators, but is rare for casual users to ever explicitly use them. Operators and functions that are designed to work on Iterable objects generally have the iteration semantics built in; so don't need an iterator to be explicitly called. It is far, **far** more common to use object slices to do the task example operations.



Rakus iterators are one direction only (not reversible), since they are designed to be able to work with infinite streams. It is difficult to reverse a stream that has no end. If the object / collection is finite, it may be reversed, but that is a separate operation from iteration.



Once an iteration has been reified, it is discarded unless explicitly cached. This allows effortless iteration through multi-gigabyte sized data objects and streams without filling up main memory.



The following example iterates though a hash of Positional Iterable objects and demonstrates both using explicit iterators, and object slice operations on each; then has a semi contrived example of where directly using iterators may be actually useful in Raku; collating unique ascending values from several infinite sequence generators.

```perl
my %positional-iterable-types =
    array    => [<Sunday Monday Tuesday Wednesday Thursday Friday Saturday>],
    list     => <Red Orange Yellow Green Blue Purple>,
    range    => 'Rako' .. 'Raky',
    sequence => (1.25, 1.5, * × * … * > 50),
;
 
sub first-fourth-fifth ($iterable) {
    my $iterator = $iterable.iterator;
    gather {
        take $iterator.pull-one;
        $iterator.skip-one xx 2;
        take $iterator.pull-one;
        take $iterator.pull-one;
    }
}
 
say "Note: here we are iterating over the %positional-iterable-types hash, but
the order we get elements out is not the same as the order they were inserted.
Hashes are not guaranteed to be in any specific order, in fact, they are
guaranteed to _not_ be in any specific order.";
 
for %positional-iterable-types.values {
    say "\nType " ~ .^name ~ ', contents: ' ~ $_ ~
    "\nUsing iterators    : first, fourth and fifth from start: " ~ first-fourth-fifth($_) ~ ', and from end: ' ~ first-fourth-fifth(.reverse) ~
    "\nUsing object slices: first, fourth and fifth from start: " ~ .[0, 3, 4] ~ ', and from end: ' ~ .[*-1, *-4, *-5] ~ "\n";
};
 
 
say "\nWhere iterators really shine; when you are collating the values from several infinite generators.";
my @i = (1, * × 2 … *).iterator, (1, * × 3 … *).iterator, (1, * × 5 … *).iterator;
my @v = @i[0].pull-one, @i[1].pull-one, @i[2].pull-one;
 
my @seq = lazy gather loop {
    take my $min := @v.min;
    for ^@v { @v[$_] = @i[$_].pull-one if @v[$_] == $min };
}
 
say @seq[^25];
```

#### Output:
```
Note: here we are iterating over the %positional-iterable-types hash, but
the order we get elements out is not the same as the order they were inserted.
Hashes are not guaranteed to be in any specific order, in fact, they are
guaranteed to _not_ be in any specific order.

Type Range, contents: Rako Rakp Rakq Rakr Raks Rakt Raku Rakv Rakw Rakx Raky
Using iterators    : first, fourth and fifth from start: Rako Rakr Raks, and from end: Raky Rakv Raku
Using object slices: first, fourth and fifth from start: Rako Rakr Raks, and from end: Raky Rakv Raku


Type Seq, contents: 1.25 1.5 1.875 2.8125 5.273438 14.831543 78.2132149
Using iterators    : first, fourth and fifth from start: 1.25 2.8125 5.273438, and from end: 78.2132149 2.8125 1.875
Using object slices: first, fourth and fifth from start: 1.25 2.8125 5.273438, and from end: 78.2132149 2.8125 1.875


Type Array, contents: Sunday Monday Tuesday Wednesday Thursday Friday Saturday
Using iterators    : first, fourth and fifth from start: Sunday Wednesday Thursday, and from end: Saturday Wednesday Tuesday
Using object slices: first, fourth and fifth from start: Sunday Wednesday Thursday, and from end: Saturday Wednesday Tuesday


Type List, contents: Red Orange Yellow Green Blue Purple
Using iterators    : first, fourth and fifth from start: Red Green Blue, and from end: Purple Yellow Orange
Using object slices: first, fourth and fifth from start: Red Green Blue, and from end: Purple Yellow Orange


Where iterators really shine; when you are collating the values from several infinite generators.
(1 2 3 4 5 8 9 16 25 27 32 64 81 125 128 243 256 512 625 729 1024 2048 2187 3125 4096)
```
