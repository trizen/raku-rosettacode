[1]: https://rosettacode.org/wiki/Table_creation

# [Table creation][1]


In Raku, there is no 'database' type built in, so it is somewhat ambiguous when specifying 'create a database table'. Raku offers bindings to most common databases through its DBIish module but mostly abstracts away the differences between the underlying databases, which hides many of the finer distinctions of what may be stored where. The actual data types and options available are properties of the database used.



If on the other hand, we are meant to show built in collective types that may be used to hold tabular data, this may be of some use.



In general, a container type can hold objects of any data type, even instances of their own type; allowing 'multi-dimensional' (tabular) containers.



Raku offers two broad categories of collective container types; those that do the Positional role and those that do Associative. Positional objects are collective objects that access the individual storage slots using an integer index. Associative objects use some sort of other pointer (typically string) to access their storage slots.



The various Associative types mostly differ in their value handling. Hash, Map and QuantHash may have any type of object as their value. All the others have some specific, usually numeric, type as their value.


```
Positional - Object that supports looking up values by integer index
    Array     Sequence of itemized objects
    List      Immutable sequence of objects

Associative - Object that supports looking up values by key (typically string)
    Bag        Immutable collection of distinct objects with integer weights
    BagHash    Mutable collection of distinct objects with integer weights
    Hash       Mapping from strings to itemized values
    Map        Immutable mapping from strings to values
    Mix        Immutable collection of distinct objects with Real weights
    MixHash    Mutable collection of distinct objects with Real weights
    QuantHash  Collection of objects represented as hash keys
    Set        Immutable collection of distinct objects, no value except 'present'
    SetHash    Mutable collection of distinct objects, no value except 'present'
```


If you want a persistent instance of any of these types, you need to declare the name with some scope constraint, but there are no prerequisites to creating instances. Simply assigning values to them will call them into existence.
