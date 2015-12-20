[1]: http://rosettacode.org/wiki/Balanced_brackets

# [Balanced brackets][1]

There's More Than One Way To Do It.

```perl6
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


Here's a more idiomatic solution using a hyperoperator to compare all the characters to a backslash (which is between the brackets in ASCII), a triangle reduction to return the running sum, a <tt>given</tt> to make that list the topic, and then a topicalized junction and a topicalized subscript to test the criteria for balance.

```perl6
sub balanced($s) {
    .none < 0 and .[*-1] == 0
        given [\+] '\\' «leg« $s.comb;
}
 
my $n = prompt "Number of bracket pairs: ";
my $s = <[ ]>.roll($n*2).join;
say "$s { balanced($s) ?? "is" !! "is not" } well-balanced"
```


Of course, a Perl 5 programmer might just remove as many inner balanced pairs as possible and then see what's left.

```perl6
sub balanced($_ is copy) {
    () while s:g/'[]'//;
    $_ eq '';
}
 
my $n = prompt "Number of bracket pairs: ";
my $s = <[ ]>.roll($n*2).join;
say "$s is", ' not' x not balanced($s), " well-balanced";
```
```perl6
grammar BalBrack { token TOP { '[' <TOP>* ']' } }
 
my $n = prompt "Number of bracket pairs: ";
my $s = ('[' xx $n, ']' xx $n).flat.pick(*).join;
say "$s { BalBrack.parse($s) ?? "is" !! "is not" } well-balanced";
```