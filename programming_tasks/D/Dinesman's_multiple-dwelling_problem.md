[1]: http://rosettacode.org/wiki/Dinesman's_multiple-dwelling_problem

# [Dinesman's multiple-dwelling problem][1]

```perl6
sub parse_and_solve ($text) {
    my %ids;
    my $expr = (grammar {
        rule TOP { <fact>+ { make join ' && ', $<fact>>>.made } }
 
        rule fact { <name> (not)? <position>
                    { make sprintf $<position>.made.fmt($0 ??  "!(%s)" !! "%s"),
                                   $<name>.made }
        }
        rule position {
            || on bottom             { make "\@f[%s] == 1"                            }
            || on top                { make "\@f[%s] == +\@f"                         }
            || lower than <name>     { make "\@f[%s] < \@f[{$<name>.made}]"           }
            || higher than <name>    { make "\@f[%s] > \@f[{$<name>.made}]"           }
            || directly below <name> { make "\@f[%s] == \@f[{$<name>.made}] - 1"      }
            || directly above <name> { make "\@f[%s] == \@f[{$<name>.made}] + 1"      }
            || adjacent to <name>    { make "\@f[%s] == \@f[{$<name>.made}] + (-1|1)" }
            || on <ordinal>          { make "\@f[%s] == {$<ordinal>.made}"            }
            || { note "Failed to parse line " ~ +$/.prematch.comb(/^^/); exit 1; }
        }
 
        token name    { :i <[a..z]>+              { make %ids{~$/} //= (state $)++ } }
        token ordinal { [1st | 2nd | 3rd | \d+th] { make +$/.match(/(\d+)/)[0]     } }
    }).parse($text).made;
 
    EVAL 'for [1..%ids.elems].permutations -> @f {
              say %ids.kv.map({ "$^a=@f[$^b]" }) if (' ~ $expr ~ ');
          }'
}
 
parse_and_solve Q:to/END/;
    Baker not on top
    Cooper not on bottom
    Fletcher not on top
    Fletcher not on bottom
    Miller higher than Cooper
    Smith not adjacent to Fletcher
    Fletcher not adjacent to Cooper
    END
```


Supports the same grammar for the problem statement, as the Perl solution.


#### Output:
```
Baker=3 Cooper=2 Fletcher=4 Miller=5 Smith=1
```
```perl6
# Contains only five floors. 5! = 120 permutations.
for (flat (1..5).permutations) -> $b, $c, $f, $m, $s {
    say "Baker=$b Cooper=$c Fletcher=$f Miller=$m Smith=$s"
        if  $b != 5         # Baker    !live  on top floor.
        and $c != 1         # Cooper   !live  on bottom floor.
        and $f != 1|5       # Fletcher !live  on top or the bottom floor.
        and $m  > $c        # Miller    lives on a higher floor than Cooper.
        and $s != $f-1|$f+1 # Smith    !live  adjacent to Fletcher
        and $f != $c-1|$c+1 # Fletcher !live  adjacent to Cooper
    ;
}
```


Adding more people and floors requires changing the list that's being used for the permutations, adding a variable for the new person, a piece of output in the string and finally to adjust all mentions of the "top" floor.
Adjusting to different rules requires changing the multi-line if statement in the loop.


#### Output:
```
Baker=3 Cooper=2 Fletcher=4 Miller=5 Smith=1
```