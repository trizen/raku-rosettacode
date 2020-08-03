[1]: https://rosettacode.org/wiki/Active_Directory/Connect

# [Active Directory/Connect][1]

Using module LMDB - bindings to the openLDAP library. Requires an LDAP instance.

```raku
use LMDB;
 
my %DB := LMDB::DB.open(:path<some-dir>, %connection-parameters);
 
```


%DB may be accessed, read from and written to like a native hash.
