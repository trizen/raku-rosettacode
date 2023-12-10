[1]: https://rosettacode.org/wiki/Variable_size/Set

# [Variable size/Set][1]


In Raku, normal user-facing types (Int, Rat, Str, Array, Hash) are all auto-sizing, so there is no need to specify a minimum size for them.  (Floating point, known as "Num", defaults to a machine double.)  For storage declarations, native storage types (starting with a lowercase letter) may also be specified, in which case the required bit size is part of the type name: int16, uint8 (aka "byte"), num32 (a "float"), complex64 (made of two num64's), etc.  More generally, such types are created through an API supporting representational polymorphism, in this case, the NativeHOW representation, when provides methods to set the size of a type; the actual allocation calculation happens when such generic types are composed into a class instance representing the semantics of the effective type to the compiler and run-time system.  But mostly this is not something users will concern themselves with directly.



By spec, arrays may be declared with dimensions of fixed size, but as of this writing, such arrays not yet implemented.  An array of fixed size that returns elements of a native type will be stored compactly, and uses exactly the memory you'd think it should, (modulo alignment constraints between elements and any slop at the end due to your memory allocator).
