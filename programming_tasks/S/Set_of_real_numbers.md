[1]: http://rosettacode.org/wiki/Set_of_real_numbers

# [Set of real numbers][1]

```perl6
class Iv {
    has $.range handles <min max excludes_min excludes_max minmax ACCEPTS>;
    method empty {
	$.min after $.max or $.min === $.max && ($.excludes_min || $.excludes_max)
    }
    multi method Bool() { not self.empty };
    method length() { $.max - $.min }
    method gist() {
	($.excludes_min ?? '(' !! '[') ~
	$.min ~ ',' ~ $.max ~
	($.excludes_max ?? ')' !! ']');
    }
}
 
class IvSet {
    has Iv @.intervals;
 
    sub canon (@i) {
	my @new = consolidate(|@i).grep(*.so);
	@new.sort(*.range.min);
    }
 
    method new(@ranges) {
	my @iv = canon @ranges.map: { Iv.new(:range($_)) }
	self.bless(*, :intervals(@iv));
    }
 
    method complement {
	my @new;
	my @old = @!intervals;
	if not @old {
	    return iv -Inf..Inf;
	}
	my $pre;
	push @old, $(Inf^..Inf) unless @old[*-1].max === Inf;
	if @old[0].min === -Inf {
	    $pre = @old.shift;
	}
	else {
	    $pre = -Inf..^-Inf;
	}
	while @old {
	    my $old = @old.shift;
	    my $excludes_min = !$pre.excludes_max;
	    my $excludes_max = !$old.excludes_min;
	    push @new, $(Range.new($pre.max,$old.min,:$excludes_min,:$excludes_max));
	    $pre = $old;
	}
	IvSet.new(@new);
    }
 
    method ACCEPTS(IvSet:D $me: $candidate) {
	so $.intervals.any.ACCEPTS($candidate);
    }
    method empty { so $.intervals.all.empty }
    multi method Bool() { not self.empty };
 
    method length() { [+] $.intervals».length }
    method gist() { join ' ', $.intervals».gist }
}
 
sub iv(**@ranges) { IvSet.new(@ranges) }
 
multi infix:<∩> (Iv $a, Iv $b) {
    if $a.min ~~ $b or $a.max ~~ $b or $b.min ~~ $a or $b.max ~~ $a {
	my $min = $a.range.min max $b.range.min;
	my $max = $a.range.max min $b.range.max;
	my $excludes_min = not $min ~~ $a & $b;
	my $excludes_max = not $max ~~ $a & $b;
	Iv.new(:range(Range.new($min,$max,:$excludes_min, :$excludes_max)));
    }
}
multi infix:<∪> (Iv $a, Iv $b) {
    my $min = $a.range.min min $b.range.min;
    my $max = $a.range.max max $b.range.max;
    my $excludes_min = not $min ~~ $a | $b;
    my $excludes_max = not $max ~~ $a | $b;
    Iv.new(:range(Range.new($min,$max,:$excludes_min, :$excludes_max)));
}
 
multi infix:<∩> (IvSet $ars, IvSet $brs) {
    my @overlap;
    for $ars.intervals -> $a {
	for $brs.intervals -> $b {
	    if $a.min ~~ $b or $a.max ~~ $b or $b.min ~~ $a or $b.max ~~ $a {
		my $min = $a.range.min max $b.range.min;
		my $max = $a.range.max min $b.range.max;
		my $excludes_min = not $min ~~ $a & $b;
		my $excludes_max = not $max ~~ $a & $b;
		push @overlap, $(Range.new($min,$max,:$excludes_min, :$excludes_max));
	    }
	}
    }
    IvSet.new(@overlap)
}
 
multi infix:<∪> (IvSet $a, IvSet $b) {
    iv |$a.intervals».range, |$b.intervals».range;
}
 
multi consolidate() { () }
multi consolidate($this is copy, *@those) {
    gather {
        for consolidate |@those -> $that {
            if $this ∩ $that { $this ∪= $that }
            else             { take $that }
        }
        take $this;
    }
}
 
multi infix:<−> (IvSet $a, IvSet $b) { $a ∩ $b.complement }
 
multi prefix:<−> (IvSet $a) { $a.complement; }
 
constant ℝ = iv -Inf..Inf;
 
my $s1 = iv(0^..1) ∪ iv(0..^2);
my $s2 = iv(0..^2) ∩ iv(1^..2);
my $s3 = iv(0..^3) − iv(0^..^1);
my $s4 = iv(0..^3) − iv(0..1) ;
 
say "\t\t\t\t0\t1\t2";
say "(0, 1] ∪ [0, 2) -> $s1.gist()\t", 0 ~~ $s1,"\t", 1 ~~ $s1,"\t", 2 ~~ $s1;	
say "[0, 2) ∩ (1, 2] -> $s2.gist()\t", 0 ~~ $s2,"\t", 1 ~~ $s2,"\t", 2 ~~ $s2;
say "[0, 3) − (0, 1) -> $s3.gist()\t", 0 ~~ $s3,"\t", 1 ~~ $s3,"\t", 2 ~~ $s3;	
say "[0, 3) − [0, 1] -> $s4.gist()\t", 0 ~~ $s4,"\t", 1 ~~ $s4,"\t", 2 ~~ $s4;	
 
say '';
 
say "ℝ is not empty: ", !ℝ.empty;
say "[0,3] − ℝ is empty: ", not iv(0..3) − ℝ;
 
my $A = iv(0..10)
        ∩
	[∪] (0..10).map: { iv $_ - 1/6 .. $_ + 1/6 }
 
my $B = iv 0..sqrt(1/6),
	   |(1..99).map({ $(sqrt($_-1/6) .. sqrt($_ + 1/6)) }),
	   sqrt(100-1/6)..10;
 
say 'A − A is empty: ', not $A − $A;
 
say '';
 
my $C = $A − $B;
say "A − B =";
say "  ",.gist for $C.intervals;
say "Length A − B = ", $C.length;
```

#### Output:
```
                                0       1       2
(0, 1] ∪ [0, 2) -> [0,2)        True    True    False
[0, 2) ∩ (1, 2] -> (1,2)        False   False   False
[0, 3) − (0, 1) -> [0,0] [1,3)  True    True    True
[0, 3) − [0, 1] -> (1,3)        False   False   True

ℝ is not empty: True
[0,3] − ℝ is empty: True
A − A is empty: True

A − B =
  [0.833333,0.912870929175277)
  (1.08012344973464,1.166667]
  [1.833333,1.95789002074512)
  (2.04124145231932,2.166667]
  (2.85773803324704,2.97209241668783)
  (3.02765035409749,3.13581462037113)
  [3.833333,3.85140666943045)
  (3.89444048184931,3.97911212877111)
  (4.02077936060494,4.10284454169706)
  (4.14326763155202,4.166667]
  [4.833333,4.88193950529227)
  (4.91596040125088,4.98330546257535)
  (5.01663898109747,5.08265022732563)
  (5.11533641774094,5.166667]
  (5.84522597225006,5.90197706987526)
  (5.93014895821906,5.98609499868932)
  (6.01387285088957,6.06904715201104)
  (6.09644705272396,6.15088069574865)
  [6.833333,6.84348838921594)
  (6.8677992593455,6.91616464041548)
  (6.94022093788567,6.98808509774554)
  (7.01189465598754,7.05927286151579)
  (7.08284312029193,7.12974987873581)
  (7.15308791129165,7.166667]
  [7.833333,7.86341740805697)
  (7.88458411500991,7.92674796706274)
  (7.94774601171091,7.98957654280459)
  (8.01040989379861,8.05191488612077)
  (8.07258735887489,8.11377429642539)
  (8.13428956127495,8.166667]
  (8.8411914732499,8.87881373457813)
  (8.89756521002609,8.93495010245347)
  (8.95358401237553,8.99073597284078)
  (9.00925450115972,9.04617783007461)
  (9.06458309392477,9.10128196098403)
  (9.1195760135363,9.15605446321358)
  [9.833333,9.84039294608367)
  (9.85731538841416,9.89107341663853)
  (9.9079092984679,9.94149552800449)
  (9.9582461641931,9.99166319154791)
Length A − B = 2.07586484118467
```