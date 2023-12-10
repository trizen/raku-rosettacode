[1]: https://rosettacode.org/wiki/Order_by_pair_comparisons

# [Order by pair comparisons][1]

Raku's sort (like most languages) can take a custom "comparator" routine.
Since the calls to the comparator are minimized, and the info that the user provides is analogous to the required return values of the comparator, we just need to embed the prompt directly in the comparator.

```perl
my $ask_count = 0;
sub by_asking ( $a, $b ) {
    $ask_count++;
    constant $fmt = '%2d. Is %-6s [ less than | greater than | equal to ] %-6s? ( < = > ) ';
    constant %o = '<' => Order::Less,
                  '=' => Order::Same,
                  '>' => Order::More;

    loop {
        my $input = prompt sprintf $fmt, $ask_count, $a, $b;
        return $_ with %o{ $input.trim };
        say "Invalid input '$input'";
    }
}

my @colors = <violet red green indigo blue yellow orange>;
my @sorted = @colors.sort: &by_asking;
say (:@sorted);

die if @sorted».substr(0,1).join ne 'roygbiv';
my $expected_ask_count = @colors.elems * log(@colors.elems);
warn "Too many questions? ({:$ask_count} > {:$expected_ask_count})" if $ask_count > $expected_ask_count;
```

#### Output:
```
 1. Is violet [ less than | greater than | equal to ] red   ? ( < = > ) >
 2. Is green  [ less than | greater than | equal to ] indigo? ( < = > ) <
 3. Is blue   [ less than | greater than | equal to ] yellow? ( < = > ) >
 4. Is red    [ less than | greater than | equal to ] green ? ( < = > ) <
 5. Is violet [ less than | greater than | equal to ] green ? ( < = > ) >
 6. Is violet [ less than | greater than | equal to ] indigo? ( < = > ) >
 7. Is yellow [ less than | greater than | equal to ] orange? ( < = > ) >
 8. Is red    [ less than | greater than | equal to ] orange? ( < = > ) <
 9. Is green  [ less than | greater than | equal to ] orange? ( < = > ) >
10. Is green  [ less than | greater than | equal to ] yellow? ( < = > ) >
11. Is green  [ less than | greater than | equal to ] blue  ? ( < = > ) <
12. Is indigo [ less than | greater than | equal to ] blue  ? ( < = > ) >
sorted => [red orange yellow green blue indigo violet]
```
