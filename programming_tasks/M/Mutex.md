[1]: https://rosettacode.org/wiki/Mutex

# [Mutex][1]

```perl
my $lock = Lock.new;
 
$lock.protect: { your-ad-here() }
```


Locks are reentrant. You may explicitly lock and unlock them, but the syntax above guarantees the lock will be unlocked on scope exit, even if by thrown exception or other exotic control flow. That being said, direct use of locks is discouraged in Perl 6 in favor of promises, channels, and supplies, which offer better composable semantics.