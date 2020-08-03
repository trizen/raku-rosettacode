[1]: https://rosettacode.org/wiki/Arrays

# [Arrays][1]

At its most basic, an array in Perl 6 is quite similar to an array in Perl 5.

```perl
my @arr;
 
push @arr, 1;
push @arr, 3;
 
@arr[0] = 2;
 
say @arr[0];
```


### Some further exposition:



In Perl 6, arrays have a very specific definition: "A collection of Scalar containers that do the Positional Role." Scalar container means it is mutable and may contain any object; an Integer, a Rational number, a String, another Array, whatever... literally any other object that can be instantiated in Perl 6. The Positional Role means that it uses integer indexing for access. The index must be a positive integer, an expression that evaluates to a positive integer, or something that can be coerced to a positive integer. Arrays are always indexed from 0. The starting index can not be changed.



Arrays are unconstrained by default. They may hold any number of any type of object up to available memory. They do not need to be pre-allocated. Simply assigning (or even referring in some cases) to an index slot is enough to autovivify the container and allocate enough memory to hold the assigned object. Memory will automatically be allocated and will grow and shrink as necessary to hold the values assigned.



Values may be pushed onto the end of an array, popped off of the end, shifted off of the front or unshifted onto the front, and spliced into or out of the interior.


#### Output:
```
   @array.push: 'value';
   my $value = @array.pop;
   @array.unshift: 'value';
   my $value = @array.shift;
   @array.splice(2,3, <some arbitrary string values>);
```


Arrays may be constrained to only accept a certain number of objects or only a certain type of object.


#### Output:
```
   my Int @array; # can only hold Integer objects. Assigning any other type will cause an exception.
   my @array[9];  # can only 10 objects (zero indexed). Trying to assign to an index greater than 9 with cause an exception. 
```


Arrays are constructed with square brackets, an explicit constructor, or by coercing some other object either explicitly using a coercer or implicitly by simply assigning to an array variable. These are all arrays:


#### Output:
```
   [1, 2, 3, 4]
   ['a', 'b', 'c', 'd']
   Array.new<this is an array of words>
   ('as', 'is', 'this').Array
   my @implicit = <yep, this too>
```


Array variables in Perl 6 are variables whose names bear the @ sigil, and are expected to contain some sort of list-like object. Of course, other variables may also contain these objects, but @-sigiled variables always do, and are expected to act the part. Array storage slots are accessed through postcircumfix square bracket notation. Unlike Perl 5, @-sigiled variables are invariant on access, whether you are accessing one slot, many slots, or all of the slots. The first slot in @array is @array[0] not $array[0]. @array and $array are two different unconnected variables.


#### Output:
```
   @array[1]      # a single value in the 2nd slot
   @array[*-1]    # a single value in the last slot
   @array[1..5]   # an array slice, 2nd through 6th slots
   @array[1,3,7]  # slice, 2nd, 4th and 8th slot
   @array[8,5,2]  # can be in any order
   @array[*]      # all the slots
   @array[]       # all the slots (zen slice)
   @array[^10]    # first 10 slots (upto 10 or 0..9)
   @array.head(5) # first 5 slots
   @array.tail(2) # last two
```


Multi-dimensioned arrays also use postcircumfix square brackets for access. If the array is not ragged, (every sub array is the same size) you may use semicolon subscripting.


#### Output:
```
   @array[1][1]   # 2nd item in the second slot
   @array[1;1]    # same thing, implies rectangular (non-ragged) arrays
```


There are several objects that have an Iterable Role and a PositionalBindFailover Role which makes them act similar to arrays and allows them to be used nearly interchangeably in read-only applications. (Perl 6 is big on duck typing. "If it looks like a duck and quacks like a duck and waddles like a duck, it's a duck.") These constructs are ordered and use integer indexing and are often used in similar circumstances as arrays, however, **they are immutable**. Values in slots can not be changed. They can not be pushed to, popped from or spliced. They can easily converted to arrays by simply assigning them to an array variable.



**List**: A fixed Iterable collection of immutable values. Lists are constructed similarly to arrays:


#### Output:
```
   (1, 2, 3, 4)
   ('a', 'b', 'c', 'd')
   List.new(<this is a list of words>)
   ('as', 'is', 'this').List
   my @not-a-list = (<oops, this isn't>)
   my @implicit := (<but this is>) # note the values are bound := not assigned =
```


**Range**: Iterable list of consecutive numbers or strings with a lower and an upper boundary. (That boundary may be infinite.) Reified on demand.


#### Output:
```
   2..20    # integers two through twenty
   1..Inf   # natural numbers
   'a'..'z' # lowercase latin letters
   '⁰'..'⁹'  # superscript digits
```


**Sequence**: Iterable list of objects with some method to determine the next (or previous) item in the list. Reified on demand. Will try to automatically deduce simple arithmetic or geometric sequences. Pass in a code object to calculate more complex sequences.


#### Output:
```
  0,2,4 ... 64   # even numbers up to 64
  1,2,4 ... 64   # geometric increase
  1,1, *+* ... * # infinite Fibonacci sequence
  1,1,{$^n2 + $^n1 + 1} ... * # infinite Leonardo numbers
```


Postcircumfix indexing works for any object that has a Positional (or PositionalBindFailover) role, it need not be in a @-sigiled variable, or indeed, in a variable at all.


#### Output:
```
   [2,4,6,8,10][1]                             # 4 - anonymous array
   <my dog has fleas>[*-2]                     # 'has' - anonymous list
   sub a {(^Inf).grep: *.is-prime}; a()[99];   # 541 - (100th prime) subroutine returning a sequence
   my $lol = ((1,2), (3,4), (5,6)); $lol[1];   # (3 4) - list of lists in a Scalar variable
```