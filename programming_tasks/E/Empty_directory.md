[1]: http://rosettacode.org/wiki/Empty_directory

# [Empty directory][1]

```perl
sub dir-is-empty ($d) { not dir $d }
```


The <tt>dir</tt> function returns a lazy list of filenames, excluding "<tt>.</tt>" and "<tt>..</tt>" automatically. Any boolean context (in this case the <tt>not</tt> function) will do just enough work on the lazy list to determine whether there are any elements, so we don't have to count the directory entries, or even read them all into memory, if there are more than one buffer's worth.