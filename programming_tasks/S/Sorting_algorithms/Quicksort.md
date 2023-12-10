[1]: https://rosettacode.org/wiki/Sorting_algorithms/Quicksort

# [Sorting algorithms/Quicksort][1]

```perl
#| Recursive, single-thread, random pivot, single-pass, quicksort implementation
multi quicksort(\a where a.elems < 2) { a }
multi quicksort(\a, \pivot = a.pick) {
	my %prt{Order} is default([]) = a.classify: * cmp pivot;
	|samewith(%prt{Less}), |%prt{Same}, |samewith(%prt{More})
}
```


### concurrent implementation



The partitions can be sorted in parallel.

```perl
#| Recursive, parallel, random pivot, single-pass, quicksort implementation
multi quicksort-parallel-naive(\a where a.elems < 2) { a }
multi quicksort-parallel-naive(\a, \pivot = a.pick) {
	my %prt{Order} is default([]) = a.classify: * cmp pivot;
	my Promise $less = start { samewith(%prt{Less}) }
	my $more = samewith(%prt{More});
	await $less andthen |$less.result, |%prt{Same}, |$more;
}
```


Let's tune the parallel execution by applying a minimum batch size in order to spawn a new thread.

```perl
#| Recursive, parallel, batch tuned, single-pass, quicksort implementation
sub quicksort-parallel(@a, $batch = 2**9) {
	return @a if @a.elems < 2;

	# separate unsorted input into Order Less, Same and More compared to a random $pivot
	my $pivot = @a.pick;
	my %prt{Order} is default([]) = @a.classify( * cmp $pivot );

	# decide if we sort the Less partition on a new thread
	my $less = %prt{Less}.elems >= $batch
			        ?? start { samewith(%prt{Less}, $batch) }
			        !!         samewith(%prt{Less}, $batch);

	# meanwhile use current thread for sorting the More partition
	my $more = samewith(%prt{More}, $batch);

	# if we went parallel, we need to await the result
	await $less andthen $less = $less.result if $less ~~ Promise;

	# concat all sorted partitions into a list and return
	|$less, |%prt{Same}, |$more;
}
```


### testing



Let's run some tests.

```perl
say "x" x 10 ~ " Testing " ~ "x" x 10;
use Test;
my @functions-under-test = &quicksort, &quicksort-parallel-naive, &quicksort-parallel;
my @testcases =
		() => (),
		<a>.List => <a>.List,
		<a a> => <a a>,
		("b", "a", 3) => (3, "a", "b"),
		<h b a c d f e g> => <a b c d e f g h>,
		<a 🎮 3 z 4 🐧> => <a 🎮 3 z 4 🐧>.sort
		;

plan @testcases.elems * @functions-under-test.elems;
for @functions-under-test -> &fun {
	say &fun.name;
	is-deeply &fun(.key), .value, .key ~ "  =>  " ~ .value for @testcases;
}
done-testing;
```

#### Output:
```
xxxxxxxxxx Testing xxxxxxxxxx
1..18
quicksort
ok 1 -   =>
ok 2 - a  =>  a
ok 3 - a a  =>  a a
ok 4 - b a 3  =>  3 a b
ok 5 - h b a c d f e g  =>  a b c d e f g h
ok 6 - a 🎮 3 z 4 🐧  =>  3 4 a z 🎮 🐧
quicksort-parallel-naive
ok 7 -   =>
ok 8 - a  =>  a
ok 9 - a a  =>  a a
ok 10 - b a 3  =>  3 a b
ok 11 - h b a c d f e g  =>  a b c d e f g h
ok 12 - a 🎮 3 z 4 🐧  =>  3 4 a z 🎮 🐧
quicksort-parallel
ok 13 -   =>
ok 14 - a  =>  a
ok 15 - a a  =>  a a
ok 16 - b a 3  =>  3 a b
ok 17 - h b a c d f e g  =>  a b c d e f g h
ok 18 - a 🎮 3 z 4 🐧  =>  3 4 a z 🎮 🐧
```


### benchmarking



and some benchmarking

```perl
say "x" x 11 ~ " Benchmarking " ~ "x" x 11;
use Benchmark;
my $runs = 5;
my $elems = 10 * Kernel.cpu-cores * 2**10;
my @unsorted of Str = ('a'..'z').roll(8).join xx $elems;
my UInt $l-batch = 2**13;
my UInt $m-batch = 2**11;
my UInt $s-batch = 2**9;
my UInt $t-batch = 2**7;

say "elements: $elems, runs: $runs, cpu-cores: {Kernel.cpu-cores}, large/medium/small/tiny-batch: $l-batch/$m-batch/$s-batch/$t-batch";

my %results = timethese $runs, {
	single-thread         => { quicksort(@unsorted) },
	parallel-naive        => { quicksort-parallel-naive(@unsorted) },
	parallel-tiny-batch   => { quicksort-parallel(@unsorted, $t-batch) },
	parallel-small-batch  => { quicksort-parallel(@unsorted, $s-batch) },
	parallel-medium-batch => { quicksort-parallel(@unsorted, $m-batch) },
	parallel-large-batch  => { quicksort-parallel(@unsorted, $l-batch) },
}, :statistics;

my @metrics = <mean median sd>;
my $msg-row = "%.4f\t" x @metrics.elems ~ '%s';

say @metrics.join("\t");
for %results.kv -> $name, %m {
	say sprintf($msg-row, %m{@metrics}, $name);
}
```

#### Output:
```
xxxxxxxxxxx Benchmarking xxxxxxxxxxx
elements: 40960, runs: 5, cpu-cores: 4, large/medium/small/tiny-batch: 8192/2048/512/128
mean    median  sd
2.9503  2.8907  0.2071  parallel-small-batch
3.2054  3.1727  0.2078  parallel-tiny-batch
5.6524  5.0980  1.2628  parallel-naive
3.4717  3.3353  0.3622  parallel-medium-batch
4.6275  4.7793  0.4930  parallel-large-batch
6.5401  6.2832  0.5585  single-thread
```
