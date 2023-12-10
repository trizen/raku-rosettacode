[1]: https://rosettacode.org/wiki/Lucky_and_even_lucky_numbers

# [Lucky and even lucky numbers][1]



```perl
sub luck(\a,\b) {
    gather {
	my @taken = take a;
	my @rotor;
	my $i = b;

	loop {
	    loop (my $j = 0; $j < @rotor; $j++) {
		--@rotor[$j] or last;
	    }
	    if $j < @rotor {
		@rotor[$j] = @taken[$j+1];
	    }
	    else {
		push @taken, take $i;
		push @rotor, $i - @taken;
	    }
	    $i += 2;
	}
    }
}

constant @lucky = luck(1,3);
constant @evenlucky = luck(2,4);

subset Luck where m:i/^ 'even'? 'lucky' $/;

multi MAIN (Int $num where * > 0) {
    say @lucky[$num-1];
}

multi MAIN (Int $num where * > 0, ',', Luck $howlucky = 'lucky') {
    say @::(lc $howlucky)[$num-1];
}

multi MAIN (Int $first where * > 0, Int $last where * > 0, Luck $howlucky = 'lucky') {
    say @::(lc $howlucky)[$first-1 .. $last - 1];
}

multi MAIN (Int $min where * > 0, Int $neg-max where * < 0, Luck $howlucky = 'lucky') {
    say grep * >= $min, (@::(lc $howlucky) ...^ * > abs $neg-max);
}
```

#### Output:
```
$ ./lucky
Usage:
  ./lucky <num>
  ./lucky <num> , [<howlucky>] 
  ./lucky <first> <last> [<howlucky>] 
  ./lucky <min> <neg-max> [<howlucky>]
$ ./lucky 20 , lucky
79
$ ./lucky 20 , evenlucky
76
$ ./lucky 1 20
1 3 7 9 13 15 21 25 31 33 37 43 49 51 63 67 69 73 75 79
$ ./lucky 1 20 evenlucky
2 4 6 10 12 18 20 22 26 34 36 42 44 50 52 54 58 68 70 76
$ ./lucky 6000 -6100
6009 6019 6031 6049 6055 6061 6079 6093
$ ./lucky 6000 -6100 evenLucky
6018 6020 6022 6026 6036 6038 6050 6058 6074 6090 6092
$ ./lucky 10000
115591
$ ./lucky 10000 , EVENLUCKY
111842
```
