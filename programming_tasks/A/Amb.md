[1]: https://rosettacode.org/wiki/Amb

# [Amb][1]





### Using Junctions



Junctions are a construct that behave similarly to the wanted Amb operator. The only difference is, that they don't preserve the state that was True inside any control structure (like an if).



There is currently a trick, how you only get the "true" values from a Junction for any test: return from a subroutine. Because of DeMorgans Law, you'll have to switch and and or, since you want to return on falseness. Just look at 'all' in combination with the sub(){return unless test} as the amb operator.

```perl
#| an array of four words, that have more possible values. 
#| Normally we would want `any' to signify we want any of the values, but well negate later and thus we need `all'
my @a =
(all «the that a»),
(all «frog elephant thing»),
(all «walked treaded grows»),
(all «slowly quickly»);

sub test (Str $l, Str $r) {
    $l.ends-with($r.substr(0,1))
}

(sub ($w1, $w2, $w3, $w4){
  # return if the values are false
  return unless [and] test($w1, $w2), test($w2, $w3),test($w3, $w4);
  # say the results. If there is one more Container layer around them this doesn't work, this is why we need the arguments here.
  say "$w1 $w2 $w3 $w4"
})(|@a); # supply the array as argumetns
```


### Using the Regex Engine



By using a reduction metaoperator to calculate all possible combinations, we can Amb any number of sets with no arbitrary limits. A simple regex pattern can find out if a certain combination is correct or not.

```perl
# The Task #
my @firstSet  = «the that a»;
my @secondSet = «frog elephant thing»;
my @thirdSet  = «walked treaded grows»;
my @fourthSet = «slowly quickly»;

.say for doAmb [@firstSet, @secondSet, @thirdSet, @fourthSet];


sub doAmb( @lol ) {	# Takes out the correct sentences.
	my @sentences = map *.join(" "), [X] @lol;
	grep &isAmb, @sentences;
}

sub isAmb( $sentence ) {	# Checks `$sentence` for correctness.
	$sentence !~~ / (.) " " (.)	<!{$0 eq $1}> /	# <https://docs.raku.org/language/regexes#Regex_boolean_condition_check>
}
```
