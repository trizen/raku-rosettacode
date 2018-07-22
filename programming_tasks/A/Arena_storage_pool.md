[1]: https://rosettacode.org/wiki/Arena_storage_pool

# [Arena storage pool][1]

Perl 6 is a high level language where, to a first approximation, everything is an object. Perl 6 dynamically allocates memory as objects are created and does automatic garbage collection and freeing of memory as objects go out of scope. There is almost no high level control over how memory is managed, it is considered to be an implementation detail of the virtual machine on which it is running.



If you absolutely must take manual control over memory management you would need to use the foreign function interface to call into a language that provides the capability, but even that would only affect items in the scope of that function, not anything in the mainline process.



There is some ability to specify data types for various objects which allows for (but does not guarantee) more efficient memory layout, but again, it is considered to be an implementation detail, the use that the virtual machine makes of that information varies by implementation maturity and capabilities.