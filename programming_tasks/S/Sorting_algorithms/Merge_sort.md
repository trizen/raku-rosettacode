[1]: https://rosettacode.org/wiki/Sorting_algorithms/Merge_sort

# [Sorting algorithms/Merge sort][1]

```perl
#| Recursive, single-thread, mergesort implementation
sub mergesort ( @a ) {
	return @a if @a <= 1;

	# recursion step
	my $m = @a.elems div 2;
	my @l = samewith @a[  0 ..^ $m ];
	my @r = samewith @a[ $m ..^ @a ];

	# short cut - in case of no overlapping in left and right parts
	return flat @l, @r if @l[*-1] !after @r[0];
	return flat @r, @l if @r[*-1] !after @l[0];

	# merge step
	return flat gather {
		take @l[0] before @r[0]
				?? @l.shift
				!! @r.shift
		     while @l and @r;

		take @l, @r;
	}
}
```


Some intial testing

```perl
my @data = 6, 7, 2, 1, 8, 9, 5, 3, 4;
say 'input  = ' ~ @data;
say 'output = ' ~ @data.&merge_sort;
```

#### Output:
```
input  = 6 7 2 1 8 9 5 3 4
output = 1 2 3 4 5 6 7 8 9
```


### concurrent implementation



Let's implement it using parallel sorting.

```perl
#| Recursive, naive multi-thread, mergesort implementation
sub mergesort-parallel-naive ( @a ) {
	return @a if @a <= 1;

	my $m = @a.elems div 2;

	# recursion step launching new thread
    my @l = start { samewith @a[ 0  ..^ $m ] };
	
    # meanwhile recursively sort right side
    my @r =         samewith @a[ $m ..^ @a ]  ;

	# as we went parallel on left side, we need to await the result
	await @l[0] andthen @l = @l[0].result;

	# short cut - in case of no overlapping left and right parts
	return flat @l, @r if @l[*-1] !after @r[0];
	return flat @r, @l if @r[*-1] !after @l[0];

	# merge step
	return flat gather {
		take @l[0] before @r[0]
				?? @l.shift
				!! @r.shift
		     while @l and @r;

		take @l, @r;
	}
}
```


and tune the batch size required to launch a new thread.

```perl
#| Recursive, batch tuned multi-thread, mergesort implementation
sub mergesort-parallel ( @a, $batch = 2**9 ) {
	return @a if @a <= 1;

	my $m = @a.elems div 2;

	# recursion step
	my @l = $m >= $batch
			  ?? start { samewith @a[ 0 ..^ $m ], $batch }
			  !!         samewith @a[ 0 ..^ $m ], $batch ;

	# meanwhile recursively sort right side
	my @r = samewith @a[ $m ..^ @a ], $batch;

	# if we went parallel on left side, we need to await the result
	await @l[0] andthen @l = @l[0].result if @l[0] ~~ Promise;

	# short cut - in case of no overlapping left and right parts
	return flat @l, @r if @l[*-1] !after @r[0];
	return flat @r, @l if @r[*-1] !after @l[0];

	# merge step
	return flat gather {
		take @l[0] before @r[0]
				?? @l.shift
				!! @r.shift
		     while @l and @r;

		take @l, @r;
	}
}
```


### testing



Let's run some tests ...

```perl
say "x" x 10 ~ " Testing " ~ "x" x 10;
use Test;
my @functions-under-test = &mergesort, &mergesort-parallel-naive, &mergesort-parallel;
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
mergesort
ok 1 -   =>
ok 2 - a  =>  a
ok 3 - a a  =>  a a
ok 4 - b a 3  =>  3 a b
ok 5 - h b a c d f e g  =>  a b c d e f g h
ok 6 - a 🎮 3 z 4 🐧  =>  3 4 a z 🎮 🐧
mergesort-parallel-naive
ok 7 -   =>
ok 8 - a  =>  a
ok 9 - a a  =>  a a
ok 10 - b a 3  =>  3 a b
ok 11 - h b a c d f e g  =>  a b c d e f g h
ok 12 - a 🎮 3 z 4 🐧  =>  3 4 a z 🎮 🐧
mergesort-parallel
ok 13 -   =>
ok 14 - a  =>  a
ok 15 - a a  =>  a a
ok 16 - b a 3  =>  3 a b
ok 17 - h b a c d f e g  =>  a b c d e f g h
ok 18 - a 🎮 3 z 4 🐧  =>  3 4 a z 🎮 🐧
```


### benchmarking



and some Benchmarking.

```perl
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
	single-thread         => { mergesort(@unsorted) },
	parallel-naive        => { mergesort-parallel-naive(@unsorted) },
	parallel-tiny-batch   => { mergesort-parallel(@unsorted, $t-batch) },
	parallel-small-batch  => { mergesort-parallel(@unsorted, $s-batch) },
	parallel-medium-batch => { mergesort-parallel(@unsorted, $m-batch) },
	parallel-large-batch  => { mergesort-parallel(@unsorted, $l-batch) },
}, :statistics;

my @metrics = <mean median sd>;
my $msg-row = "%.4f\t" x @metrics.elems ~ '%s';

say @metrics.join("\t");
for %results.kv -> $name, %m {
	say sprintf($msg-row, %m{@metrics}, $name);
}
```

#### Output:
```
elements: 40960, runs: 5, cpu-cores: 4, large/medium/small/tiny-batch: 8192/2048/512/128
mean    median  sd
7.7683  8.0265  0.5724  parallel-naive
3.1354  3.1272  0.0602  parallel-tiny-batch
2.6932  2.6599  0.1831  parallel-medium-batch
2.8139  2.7832  0.0641  parallel-large-batch
3.0908  3.0593  0.0675  parallel-small-batch
5.9989  5.9450  0.1518  single-thread
```
