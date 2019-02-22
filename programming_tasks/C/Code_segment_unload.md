[1]: https://rosettacode.org/wiki/Code_segment_unload

# [Code segment unload][1]

In general, there is no specific mechanism to do a timely destruction of an object, be it code, data, variables, whatever. Perl 6 does automatic garbage collection on objects that are no longer being used. Unlike previous version of Perl, Perl 6 doesn't use reference counting to track when memory may be garbage collected as that tends to be very difficult to get correct and performant in multi-threaded applications. Rather it performs a "reachability analysis" to determine when objects can be evicted from memory and safely removed.



Perl 6 objects can provide a "DESTROY" method, but you cannot be sure when (if ever) it will be called. Perl 6 tends to not be very memory frugal. If there is lots of memory to be had, it will cheerfully use lots of it. It gets more aggressive about conserving if memory is tight, but usually will trade memory for performance. If and when garbage collection *has* run, the freed memory is then available to to the general OS pool for any process to claim.



There are some specific details that will allow some finer control of garbage collection in special circumstances, but the average Perl 6 programmer will not need to know about or deal with them.