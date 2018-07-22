[1]: https://rosettacode.org/wiki/Balanced_brackets

# [Balanced brackets][1]

There's More Than One Way To Do It.



### Depth counter

```perl
sub balanced($s) {
    my $l = 0;
    for $s.comb {
        when "]" {
            --$l;
            return False if $l < 0;
        }
        when "[" {
            ++$l;
        }
    }
    return $l == 0;
}
 
my $n = prompt "Number of brackets";
my $s = (<[ ]> xx $n).flat.pick(*).join;
say "$s {balanced($s) ?? "is" !! "is not"} well-balanced"
```


### FP oriented



Here's a more idiomatic solution using a hyperoperator to compare all the characters to a backslash (which is between the brackets in ASCII), a triangle reduction to return the running sum, a `given` to make that list the topic, and then a topicalized junction and a topicalized subscript to test the criteria for balance.

```perl
sub balanced($s) {
    .none < 0 and .[*-1] == 0
        given ([\+] '\\' «leg« $s.comb).cache;
}
 
my $n = prompt "Number of bracket pairs: ";
my $s = <[ ]>.roll($n*2).join;
say "$s { balanced($s) ?? "is" !! "is not" } well-balanced"
```


### String munging



Of course, a Perl 5 programmer might just remove as many inner balanced pairs as possible and then see what's left.

```perl
sub balanced($_ is copy) {
    Nil while s:g/'[]'//;
    $_ eq '';
}
 
my $n = prompt "Number of bracket pairs: ";
my $s = <[ ]>.roll($n*2).join;
say "$s is", ' not' x not balanced($s), " well-balanced";
```


### Parsing with a grammar

```perl
grammar BalBrack { token TOP { '[' <TOP>* ']' } }
 
my $n = prompt "Number of bracket pairs: ";
my $s = ('[' xx $n, ']' xx $n).flat.pick(*).join;
say "$s { BalBrack.parse($s) ?? "is" !! "is not" } well-balanced";
```