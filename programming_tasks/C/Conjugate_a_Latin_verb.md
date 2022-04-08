[1]: https://rosettacode.org/wiki/Conjugate_a_Latin_verb

# [Conjugate a Latin verb][1]

```perl
for <amāre dare> -> $infinitive {
    say "\nPresent active indicative conjugation of infinitive $infinitive.";
    my $verb = ($infinitive ~~ /^ (\w+) ['a'|'ā'] 're' $/)[0];
    say $verb ?? (conjugate $verb) !! "Sorry, don't know how to conjugate $infinitive"
}
 
sub conjugate ($verb) { ($verb X~ <ō ās at āmus ātis ant>).join: "\n" }
```

#### Output:
```
Present active indicative conjugation of infinitive amāre.
amō
amās
amat
amāmus
amātis
amant

Present active indicative conjugation of infinitive dare.
dō
dās
dat
dāmus
dātis
dant
```
