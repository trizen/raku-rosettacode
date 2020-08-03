[1]: https://rosettacode.org/wiki/Parallel_calculations

# [Parallel calculations][1]

Takes the list of numbers and converts them to a `HyperSeq` that is stored in a variable and evaluated concurrently. `HyperSeq`s overload `map` and `grep` to convert and pick values in worker threads. The runtime will pick the number of OS-level threads and assign worker threads to them while avoiding stalling in any part of the program. A `HyperSeq` is lazy, so the computation of values will happen in chunks as they are requested.



The hyper (and race) method can take two parameters that will tweak how the parallelization occurs:&#160;:degree and&#160;:batch.&#160;:degree is the number of worker threads to allocate to the job. By default it is set to the number of physical cores available. If you have a hyper threading processor, and the tasks are not cpu bound, it may be useful to raise that number but it is a reasonable default.&#160;:batch is how many sub-tasks are parceled out at a time to each worker thread. Default is 64. For small numbers of cpu intensive tasks a lower number will likely be better, but too low may make the dispatch overhead cancel out the benefit of threading. Conversely, too high will over-burden some threads and starve others. Over long-running processes with many hundreds / thousands of sub-tasks, the scheduler will automatically adjust the batch size up or down to try to keep the pipeline filled.



On my system, under the load I was running, I found a batch size of 3 to be optimal for this task. May be different for different systems and different loads.



As a relative comparison, perform the same factoring task on the same set of 100 numbers as found in the [SequenceL](https://rosettacode.org/wiki/Parallel_calculations#SequenceL) example, using varying numbers of threads. The absolute speed numbers are not very significant, they will vary greatly between systems, this is more intended as a comparison of relative throughput. On a Core i7-4770 @ 3.40GHz with 4 cores and hyper-threading under Linux, there is a distinct pattern where more threads on physical cores give reliable increases in throughput. Adding hyperthreads may (and, in this case, does seem to) give some additional marginal benefit.



Using the `prime-factors` routine as defined in the [prime decomposition](https://rosettacode.org/wiki/Prime_decomposition#Raku) task.

```raku
my @nums = 64921987050997300559,  70251412046988563035,  71774104902986066597,
           83448083465633593921,  84209429893632345702,  87001033462961102237,
           87762379890959854011,  89538854889623608177,  98421229882942378967,
           259826672618677756753, 262872058330672763871, 267440136898665274575,
           278352769033314050117, 281398154745309057242, 292057004737291582187;

my @factories = @nums.hyper(:3batch).map: *.&prime-factors;
printf "%21d factors: %s\n", |$_ for @nums Z @factories;
my $gmf = {}.append(@factories»[0] »=>« @nums).max: +*.key;
say "\nGreatest minimum factor: ", $gmf.key;
say "from: { $gmf.value }\n";
say 'Run time: ', now - INIT now;
say '-' x 80;

# For amusements sake and for relative comparison, using the same 100
# numbers as in the SequenceL example, testing with different numbers of threads.

@nums = <625070029 413238785 815577134 738415913 400125878 967798656 830022841
   774153795 114250661 259366941 571026384 522503284 757673286 509866901 6303092
   516535622 177377611 520078930 996973832 148686385 33604768 384564659 95268916
   659700539 149740384 320999438 822361007 701572051 897604940 2091927 206462079
   290027015 307100080 904465970 689995756 203175746 802376955 220768968 433644101
   892007533 244830058 36338487 870509730 350043612 282189614 262732002 66723331
   908238109 635738243 335338769 461336039 225527523 256718333 277834108 430753136
   151142121 602303689 847642943 538451532 683561566 724473614 422235315 921779758
   766603317 364366380 60185500 333804616 988528614 933855820 168694202 219881490
   703969452 308390898 567869022 719881996 577182004 462330772 770409840 203075270
   666478446 351859802 660783778 503851023 789751915 224633442 347265052 782142901
   43731988 246754498 736887493 875621732 594506110 854991694 829661614 377470268
   984990763 275192380 39848200 892766084 76503760>».Int;

for 1..8 -> $degree {
    my $start = now;
    my \factories = @nums.hyper(:degree($degree), :3batch).map: *.&prime-factors;
    my $gmf = {}.append(factories»[0] »=>« @nums).max: +*.key;
    say "\nFactoring {+@nums} numbers, greatest minimum factor: {$gmf.key}";
    say "Using: $degree thread{ $degree > 1 ?? 's' !! ''}";
    my $end = now;
    say 'Run time: ', $end - $start, ' seconds.';
}

# Prime factoring routines from the Prime decomposition task
sub prime-factors ( Int $n where * > 0 ) {
    return $n if $n.is-prime;
    return [] if $n == 1;
    my $factor = find-factor( $n );
    sort flat prime-factors( $factor ), prime-factors( $n div $factor );
}

sub find-factor ( Int $n, $constant = 1 ) {
    return 2 unless $n +& 1;
    if (my $gcd = $n gcd 6541380665835015) > 1 {
        return $gcd if $gcd != $n
    }
    my $x      = 2;
    my $rho    = 1;
    my $factor = 1;
    while $factor == 1 {
        $rho *= 2;
        my $fixed = $x;
        for ^$rho {
            $x = ( $x * $x + $constant ) % $n;
            $factor = ( $x - $fixed ) gcd $n;
            last if 1 < $factor;
        }
    }
    $factor = find-factor( $n, $constant + 1 ) if $n == $factor;
    $factor;
}
```

#### Output:
```
 64921987050997300559 factors: 736717 88123373087627
 70251412046988563035 factors: 5 43 349 936248577956801
 71774104902986066597 factors: 736717 97424255043641
 83448083465633593921 factors: 736717 113270202079813
 84209429893632345702 factors: 2 3 3 3 41 107880821 352564733
 87001033462961102237 factors: 736717 118092881612561
 87762379890959854011 factors: 3 3 3 3 331 3273372119315201
 89538854889623608177 factors: 736717 121537652707381
 98421229882942378967 factors: 736717 133594351539251
259826672618677756753 factors: 7 37118096088382536679
262872058330672763871 factors: 3 47 1864340839224629531
267440136898665274575 factors: 3 5 5 71 50223499887073291
278352769033314050117 factors: 7 39764681290473435731
281398154745309057242 factors: 2 809 28571 46061 132155099
292057004737291582187 factors: 7 151 373 2339 111323 2844911

Greatest minimum factor: 736717
from: 64921987050997300559 71774104902986066597 83448083465633593921 87001033462961102237 89538854889623608177 98421229882942378967

Run time: 0.2968644
--------------------------------------------------------------------------------

Factoring 100 numbers, greatest minimum factor: 782142901
Using: 1 thread
Run time: 0.3438752 seconds.

Factoring 100 numbers, greatest minimum factor: 782142901
Using: 2 threads
Run time: 0.2035372 seconds.

Factoring 100 numbers, greatest minimum factor: 782142901
Using: 3 threads
Run time: 0.14177834 seconds.

Factoring 100 numbers, greatest minimum factor: 782142901
Using: 4 threads
Run time: 0.110738 seconds.

Factoring 100 numbers, greatest minimum factor: 782142901
Using: 5 threads
Run time: 0.10142434 seconds.

Factoring 100 numbers, greatest minimum factor: 782142901
Using: 6 threads
Run time: 0.10954304 seconds.

Factoring 100 numbers, greatest minimum factor: 782142901
Using: 7 threads
Run time: 0.097886 seconds.

Factoring 100 numbers, greatest minimum factor: 782142901
Using: 8 threads
Run time: 0.0927695 seconds.
```


Beside `HyperSeq` and its (allowed to be) out-of-order equivalent `RaceSeq`, [Rakudo](https://rosettacode.org/wiki/Rakudo) supports primitive threads, locks and highlevel promises. Using channels and supplies values can be move thread-safely from one thread to another. A react-block can be used as a central hub for message passing.



In [Perl 6](https://rosettacode.org/wiki/Perl_6) most errors are bottled up `Exceptions` inside `Failure` objects that remember where they are created and thrown when used. This is useful to pass errors from one thread to another without losing file and line number of the source file that caused the error.



In the future hyper operators, junctions and feeds will be candidates for autothreading.
