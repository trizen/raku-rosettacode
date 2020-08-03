[1]: https://rosettacode.org/wiki/Ternary_logic

# [Ternary logic][1]

The precedence of each operator is specified as equivalent to an existing operator. We've taken the liberty of using a double arrow for implication, to avoid confusing it with `⊃`, (U+2283 SUPERSET OF).

```perl
# Implementation:
enum Trit <Foo Moo Too>;
 
sub prefix:<¬> (Trit $a) { Trit(1-($a-1)) }
 
sub infix:<∧> (Trit $a, Trit $b) is equiv(&infix:<*>) { $a min $b }
sub infix:<∨> (Trit $a, Trit $b) is equiv(&infix:<+>) { $a max $b }
 
sub infix:<⇒> (Trit $a, Trit $b) is equiv(&infix:<..>) { ¬$a max $b }
sub infix:<≡> (Trit $a, Trit $b) is equiv(&infix:<eq>) { Trit(1 + ($a-1) * ($b-1)) }
 
# Testing:
say '¬';
say "Too {¬Too}";
say "Moo {¬Moo}";
say "Foo {¬Foo}";
 
sub tbl (&op,$name) {
    say '';
    say "$name   Too Moo Foo";
    say "   ╔═══════════";
    say "Too║{op Too,Too} {op Too,Moo} {op Too,Foo}";
    say "Moo║{op Moo,Too} {op Moo,Moo} {op Moo,Foo}";
    say "Foo║{op Foo,Too} {op Foo,Moo} {op Foo,Foo}";
}
 
tbl(&infix:<∧>, '∧');
tbl(&infix:<∨>, '∨');
tbl(&infix:<⇒>, '⇒');
tbl(&infix:<≡>, '≡');
 
say '';
say 'Precedence tests should all print "Too":';
say ~(
    Foo ∧ Too ∨ Too ≡ Too,
    Foo ∧ (Too ∨ Too) ≡ Foo,
    Too ∨ Too ∧ Foo ≡ Too,
    (Too ∨ Too) ∧ Foo ≡ Foo,
 
    ¬Too ∧ Too ∨ Too ≡ Too,
    ¬Too ∧ (Too ∨ Too) ≡ ¬Too,
    Too ∨ Too ∧ ¬Too ≡ Too,
    (Too ∨ Too) ∧ ¬Too ≡ ¬Too,
 
    Foo ∧ Too ∨ Foo ⇒ Foo ≡ Too,
    Foo ∧ Too ∨ Too ⇒ Foo ≡ Foo,
);
```

#### Output:
```
¬
Too Foo
Moo Moo
Foo Too

∧   Too Moo Foo
   ╔═══════════
Too║Too Moo Foo
Moo║Moo Moo Foo
Foo║Foo Foo Foo

∨   Too Moo Foo
   ╔═══════════
Too║Too Too Too
Moo║Too Moo Moo
Foo║Too Moo Foo

⇒   Too Moo Foo
   ╔═══════════
Too║Too Moo Foo
Moo║Too Moo Moo
Foo║Too Too Too

≡   Too Moo Foo
   ╔═══════════
Too║Too Moo Foo
Moo║Moo Moo Moo
Foo║Foo Moo Too

Precedence tests should all print "Too":
Too Too Too Too Too Too Too Too Too Too
```