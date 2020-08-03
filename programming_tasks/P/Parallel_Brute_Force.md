[1]: https://rosettacode.org/wiki/Parallel_Brute_Force

# [Parallel Brute Force][1]

This solution can be changed from parallel to serial by removing the `.race` method.

```raku
use Digest::SHA;
constant @alpha2 = [X~] <a   m p y z> xx 2;
constant @alpha3 = [X~] <e l m p x z> xx 3;
 
my %WANTED = set <
    3a7bd3e2360a3d29eea436fcfb7e44c735d117c42d1c1835420b6b9942dd4f1b
    74e1bb62f8dabb8125a58852b63bdf6eaef667cb56ac7f7cdba6d7305c50a22f
    1115dd800feaacefdf481f1f9070374a2a81e27880f187396db67958b207cbad
>;
 
sub find_it ( $first_two ) {
    return gather for $first_two «~« @alpha3 -> $password {
        my $digest_hex = sha256($password).list.fmt('%02x', '');
        take "$password => $digest_hex" if %WANTED{$digest_hex};
    }
}
 
.say for flat @alpha2.race.map: {.&find_it.cache};
```

#### Output:
```
apple => 3a7bd3e2360a3d29eea436fcfb7e44c735d117c42d1c1835420b6b9942dd4f1b
mmmmm => 74e1bb62f8dabb8125a58852b63bdf6eaef667cb56ac7f7cdba6d7305c50a22f
zyzzx => 1115dd800feaacefdf481f1f9070374a2a81e27880f187396db67958b207cbad
```


Testers can adjust the run speed by replacing the @alpha constants with any of the below:

```raku
 
# True to actual RC task, but slowest
constant @alpha2 = 'aa'  ..  'zz';
constant @alpha3 = 'aaa' .. 'zzz';
# Reduced alphabets for speed during development
constant @alpha2 = [X~] <a   m p y z> xx 2;
constant @alpha3 = [X~] <e l m p x z> xx 3;
# Alphabets reduced by position for even more speed
constant @alpha2 = [X~] <a m z>, <p m y>;
constant @alpha3 = [X~] <m p z>, <l m z>, <e m x>;
# Completely cheating
constant @alpha2 = <ap  mm  zy>;
constant @alpha3 = <ple mmm zzx>;
```