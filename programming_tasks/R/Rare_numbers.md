[1]: https://rosettacode.org/wiki/Rare_numbers

# [Rare numbers][1]

### Translation of Rust

```perl
# 20220315 Raku programming solution

sub rare (\target where ( target > 0 and target ~~ Int )) {

   my \digit          = $ = 2;
   my $count          = 0;
   my @numeric_digits = 0..9 Z, 0 xx *;
   my @diffs1         = 0,1,4,5,6;

   # all possible digits pairs to calculate potential diffs
   my @pairs     = 0..9 X 0..9; 
   my @all_diffs = -9..9;

   # lookup table for the first diff
   my @lookup_1 = [ [[2, 2], [8, 8]],    # Diff = 0
                    [[8, 7], [6, 5]],    # Diff = 1
                    [],
                    [],
                    [[4, 0],       ],    # Diff = 4
                    [[8, 3],       ],    # Diff = 5
                    [[6, 0], [8, 2]], ]; # Diff = 6

   # lookup table for all the remaining diffs
   given my %lookup_n { for @pairs -> \pair { $_{ [-] pair.values }.push: pair } }

   loop {
      my @powers = 10 <<**<< (0..digit-1); #  powers like 1, 10, 100, 1000....
 
      #  for n-r (aka L) the required terms, like 9/ 99 / 999 & 90 / 99999 & 9999 & 900 etc
      my @terms = (@powers.reverse Z- @powers).grep: * > 0 ;

      # create a cartesian product for all potential diff numbers
      # for the first use the very short one, for all other the complete 19 element
      my @diff_list = digit == 2 ?? @diffs1 !! [X] @diffs1, |(@all_diffs xx digit div 2 - 1);

      my @diff_list_iter = gather for @diff_list -> \k {
         # remove invalid first diff/second diff combinations 
         { take k andthen next } if k.elems == 1 ;    
         given (my (\a,\b) = k.values) {       
            when a == 0 && b != 0                                    { next } 
            when a == 1 && b ∉ [ -7, -5, -3, -1,  1, 3, 5, 7       ] { next }
            when a == 4 && b ∉ [ -8, -6, -4, -2,  0, 2, 4, 6, 8    ] { next }
            when a == 5 && b ∉ [ -3,  7                            ] { next }
            when a == 6 && b ∉ [ -9, -7, -5, -3, -1, 1, 3, 5, 7, 9 ] { next } 
            default { take k }        
         }      
      }

      for @diff_list_iter -> \diffs {
         # calculate difference of original n and its reverse (aka L = n-r)
         # which must be a perfect square
         if (my \L = [+] diffs <<*>> @terms) > 0 and { $_ == $_.Int }(L.sqrt) {
            # potential candiate, at least L is a perfect square
            # placeholder for the digits
            my \dig = @ = 0 xx digit;

            # generate a cartesian product for each identified diff using the lookup tables
            my @c_iter = digit == 2
               ?? @lookup_1[diffs[0]].map: { [ $_ ] }
               !! [X] @lookup_1[diffs[0]], |(1..(+diffs + (digit % 2 - 1))).map: -> \k {
                  k == diffs ?? @numeric_digits !! %lookup_n{diffs[k]} }

            # check each H (n+r) by using digit combination
            for @c_iter -> \elt {
               for elt.kv -> \i, \pair { dig[i,digit-1-i] = pair.values }
               # for numbers with odd # digits restore the middle digit
               # which has been overwritten at the end of the previous cycle
               dig[(digit - 1) div 2] = elt[+elt - 1][0] if digit % 2 == 1 ;

	       my \rev = ( my \num = [~] dig ).flip;

               if num > rev and { $_ == $_.Int }((num+rev).sqrt) {
                  printf "%d: %12d reverse %d\n", $count+1, num, rev;
		  exit if ++$count == target;
               } 
            }
         }
      }
      digit++
   }
}

my $N = 5;
say "The first $N rare numbers are,";
rare $N;
```

#### Output:
```
The first 5 rare numbers are,
1:           65 reverse 56
2:       621770 reverse 77126
3:    281089082 reverse 280980182
4:   2022652202 reverse 2022562202
5:   2042832002 reverse 2002382402
```


### Using NativeCall with Rust



This example will make use of a modified version of the 'advanced' routine from the Rust entry.

```bash
~> cargo new --lib Rare && cd $_
     Created library `Rare` package
```


Add to the stock manifest file, Cargo.toml, all required dependencies and build target,

```bash
~/Rare> tail -5 Cargo.toml
[dependencies]
itertools = "0.10.3"

[lib]
crate-type = ["cdylib"]
```


Now replace the src/lib.rs file with the following,



lib.rs

```rust
use itertools::Itertools;
use std::collections::HashMap;

fn isqrt(n: u64) -> u64 {
    let mut s = (n as f64).sqrt() as u64;
    s = (s + n / s) >> 1;
    if s * s > n {
        s - 1
    } else {
        s
    }
}

fn is_square(n: u64) -> bool {
    match n & 0xf {
        0 | 1 | 4 | 9 => {
            let t = isqrt(n);
            t * t == n
        }
        _ => false,
    }
}

#[no_mangle]
/// This algorithm uses an advanced search strategy based on Nigel Galloway's approach
pub extern "C" fn advanced64(target: u8) -> *mut u64 {
    // setup
    let digit = 2u8;
    let mut results = Vec::new();
    let mut counter = 0_u8;

    let numeric_digits = (0..=9).map(|x| [x, 0]).collect::<Vec<_>>();
    let diffs1: Vec<i8> = vec![0, 1, 4, 5, 6];

    // all possible digits pairs to calculate potential diffs
    let pairs = (0_i8..=9)
        .cartesian_product(0_i8..=9)
        .map(|x| [x.0, x.1])
        .collect::<Vec<_>>();
    let all_diffs = (-9i8..=9).collect::<Vec<_>>();

    // lookup table for the first diff
    let lookup_1 = vec![
        vec![[2, 2], [8, 8]], //Diff = 0
        vec![[8, 7], [6, 5]], //Diff = 1
        vec![],
        vec![],
        vec![[4, 0]],         // Diff = 4
        vec![[8, 3]],         // Diff = 5
        vec![[6, 0], [8, 2]], // Diff = 6
    ];

    // lookup table for all the remaining diffs
    let lookup_n: HashMap<i8, Vec<_>> = pairs.into_iter().into_group_map_by(|elt| elt[0] - elt[1]);

    let mut d = digit;

    while target > counter {
        // powers like 1, 10, 100, 1000....
        let powers = (0..d).map(|x| 10_u64.pow(x.into())).collect::<Vec<u64>>();

        // for n-r (aka L) the required terms, like 9/ 99 / 999 & 90 / 99999 & 9999 & 900 etc
        let terms = powers
            .iter()
            .zip(powers.iter().rev())
            .map(|(a, b)| b.checked_sub(*a).unwrap_or(0))
            .filter(|x| *x != 0)
            .collect::<Vec<u64>>();

        // create a cartesian product for all potential diff numbers
        // for the first use the very short one, for all other the complete 19 element
        let diff_list_iter = (0_u8..(d / 2))
            .map(|i| match i {
                0 => diffs1.iter(),
                _ => all_diffs.iter(),
            })
            .multi_cartesian_product()
            // remove invalid first diff/second diff combinations - custom iterator would be probably better
            .filter(|x| {
                if x.len() == 1 {
                    return true;
                }
                match (*x[0], *x[1]) {
                    (a, b) if (a == 0 && b != 0) => false,
                    (a, b) if (a == 1 && ![-7, -5, -3, -1, 1, 3, 5, 7].contains(&b)) => false,
                    (a, b) if (a == 4 && ![-8, -6, -4, -2, 0, 2, 4, 6, 8].contains(&b)) => false,
                    (a, b) if (a == 5 && ![7, -3].contains(&b)) => false,
                    (a, b) if (a == 6 && ![-9, -7, -5, -3, -1, 1, 3, 5, 7, 9].contains(&b)) => {
                        false
                    }
                    _ => true,
                }
            });

        'OUTER: for diffs in diff_list_iter {
            // calculate difference of original n and its reverse (aka L = n-r)
            // which must be a perfect square
            let l: i64 = diffs
                .iter()
                .zip(terms.iter())
                .map(|(diff, term)| **diff as i64 * *term as i64)
                .sum();

            if l > 0 && is_square(l.try_into().unwrap()) {
                // potential candiate, at least L is a perfect square

                // placeholder for the digits
                let mut dig: Vec<i8> = vec![0_i8; d.into()];

                // generate a cartesian product for each identified diff using the lookup tables
                let c_iter = (0..(diffs.len() + d as usize % 2))
                    .map(|i| match i {
                        0 => lookup_1[*diffs[0] as usize].iter(),
                        _ if i != diffs.len() => lookup_n.get(diffs[i]).unwrap().iter(),
                        _ => numeric_digits.iter(), // for the middle digits
                    })
                    .multi_cartesian_product();

                // check each H (n+r) by using digit combination
                c_iter.for_each(|elt| {
                    for (i, digit_pair) in elt.iter().enumerate() {
                        dig[i] = digit_pair[0];
                        dig[d as usize - 1 - i] = digit_pair[1]
                    }

                    // for numbers with odd # digits restore the middle digit
                    // which has been overwritten at the end of the previous cycle
                    if d % 2 == 1 {
                        dig[(d as usize - 1) / 2] = elt[elt.len() - 1][0];
                    }

                    let num = dig
                        .iter()
                        .rev()
                        .enumerate()
                        .fold(0_u64, |acc, (i, d)| acc + 10_u64.pow(i as u32) * *d as u64);

                    let reverse = dig
                        .iter()
                        .enumerate()
                        .fold(0_u64, |acc, (i, d)| acc + 10_u64.pow(i as u32) * *d as u64);

                    if num > reverse && is_square(num + reverse) {
                        counter += 1;
                        results.push(num);
                    }
                });
                if counter == target {
                    break 'OUTER;
                }
            }
        }
        d += 1
    }
    let ptr = results.as_mut_ptr();
    std::mem::forget(results); // circumvent the destructor

    ptr
}
```


The needful shared library will be available after the build command,

```bash
~/Rare> cargo build
~/Rare> file target/debug/libRare.so
target/debug/libRare.so: ELF 64-bit LSB shared object, x86-64, version 1 (SYSV), dynamically linked, BuildID[sha1]=4f904cce7f8e82130826bf46f93fe9fe944ab9d0, with debug_info, not stripped
```


Here is the main Raku program,

```perl
use NativeCall;

constant LIB = '/home/hkdtam/Rare/target/debug/libRare.so';

sub advanced64(uint8) returns Pointer[uint64] is native(LIB) {*}

my $N = 5;
say "The first $N rare numbers are,";

for (advanced64 $N)[^$N].kv -> \nth,\rare {
   printf "%d: %12d reverse %d\n", nth+1, { $_, $_.flip }(rare)
}
```


Output is the same.
