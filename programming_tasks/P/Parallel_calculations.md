[1]: http://rosettacode.org/wiki/Parallel_calculations

# [Parallel calculations][1]

Assuming that <tt>factors</tt> is defined exactly as in the prime decomposition task:

```perl
my @nums = 12757923, 12878611, 123456789, 15808973, 15780709, 197622519;
 
my @factories;
@factories[$_] := factors(@nums[$_]) for ^@nums;
my $gmf = ([max] @factories»[0] »=>« @nums).value;
 
```


The line with the <tt>for</tt> loop is just setting up a bunch of lazy lists, one for each number to be factored, but doesn't actually do any of the work of factoring.
Most of the parallelizing work is done by the hyperoperators that demand the first value from each of the factories' lists, then builds (again in parallel) the pairs associating each first value with its original value. The <tt>[max]</tt> reduction finds the pair with the largest key, from which we can easily extract the greatest minimum factor candidate, and then refactor it completely.



The [rakudo](http://rosettacode.org/wiki/Rakudo) system does not actually do hypers in parallel yet, but when it does, this can automatically parallelize. (Hypers do parallelize in [pugs](http://rosettacode.org/wiki/Pugs), but it doesn't do some of the other things we rely on here.) It will be up to each individual compiler to determine how many cores to use for any given hyperoperator; the construct merely promises the compiler that it can be parallelized, but does not require that it must be.



There is also some pipelining that can happen within the <tt>factors</tt> routine itself, which uses a <tt>gather</tt>/<tt>take</tt> construct, which the compiler may implement using either coroutines or threads as it sees fit.
Threading pipelines can make more sense on, say, a cell architecture.