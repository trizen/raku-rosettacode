[1]: http://rosettacode.org/wiki/Memory_allocation

# [Memory allocation][1]

Like Perl 5, Perl 6 is intended to run largely stackless, so all allocations are really on the heap, including activation records. Allocations are managed automatically. It is easy enough to allocate a memory buffer of a particular size however, if you really need it:

```perl
my $buffer = Buf.new(0 xx 1024);
```