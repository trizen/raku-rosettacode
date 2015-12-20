[1]: http://rosettacode.org/wiki/I_before_E_except_after_C

# [I before E except after C][1]

This solution uses grammars and actions to parse the given file, the `Bag` for tallying up occurrences of each possible thing we're looking for ("ie", "ei", "cie", and "cei"), and junctions to determine the plausibility of a phrase from the subphrases. Note that a version of rakudo newer than the January 2014 compiler or Star releases is needed, as this code relies on a recent bugfix to the `make` function.

```perl
grammar CollectWords {
    token TOP {
        [^^ <word> $$ \n?]+
    }
 
    token word {
        [ <with_c> | <no_c> | \N ]+
    }
 
    token with_c {
        c <ie_part>
    }
 
    token no_c {
        <ie_part>
    }
 
    token ie_part {
        ie | ei | eie # a couple words in the list have "eie"
    }
}
 
class CollectWords::Actions {
    method TOP($/) {
        make $<word>».ast.Bag;
    }
 
    method word($/) {
        if $<with_c> + $<no_c> {
            make ($<with_c>».ast, $<no_c>».ast);
        } else {
            make ();
        }
    }
 
    method with_c($/) {
        make "c" X~ $<ie_part>.ast;
    }
 
    method no_c($/) {
        make "!c" X~ $<ie_part>.ast;
    }
 
    method ie_part($/) {
        if ~$/ eq 'eie' {
            make ('ei', 'ie');
        } else {
            make ~$/;
        }
    }
}
 
sub plausible($good, $bad, $msg) {
    if $good > 2*$bad {
        say "$msg: PLAUSIBLE ($good ✔ vs. $bad ✘)";
        return True;
    } else {
        say "$msg: NOT PLAUSIBLE ($good ✔ vs. $bad ✘)";
        return False;
    }
}
 
my $results = CollectWords.parsefile("unixdict.txt", :actions(CollectWords::Actions)).ast;
 
my $phrasetest = [&] plausible($results<!cie>, $results<!cei>, "I before E when not preceded by C"),
                     plausible($results<cei>, $results<cie>, "E before I when preceded by C");
 
say "I before E except after C: ", $phrasetest ?? "PLAUSIBLE" !! "NOT PLAUSIBLE";
```

#### Output:
```
I before E when not preceded by C: PLAUSIBLE (466 ✔ vs. 217 ✘)
E before I when preceded by C: NOT PLAUSIBLE (13 ✔ vs. 24 ✘)
I before E except after C: NOT PLAUSIBLE
```


Note that within the original text file, a tab character was erroneously replaced with a space. Thus, the following changes to the text file are needed before this solution will run:


#### Output:
```
--- orig_1_2_all_freq.txt       2014-02-01 14:36:53.124121018 -0800
+++ 1_2_all_freq.txt    2014-02-01 14:37:10.525552980 -0800
@@ -2488,7 +2488,7 @@
        other than      Prep    43
        visited Verb    43
        cross   NoC     43
-       lie Verb        43
+       lie     Verb    43
        grown   Verb    43
        crowd   NoC     43
        recognised      Verb    43
```


This solution requires just a few modifications to the grammar and actions from the non-stretch goal.

```perl
grammar CollectWords {
    token TOP {
        ^^ \t Word \t PoS \t Freq $$ \n
        [^^ <word> $$ \n?]+
    }
 
    token word {
        \t+
        [ <with_c> | <no_c> | \T ]+ \t+
        \T+ \t+ # PoS doesn't matter to us, so ignore it
        $<freq>=[<.digit>+] \h*
    }
 
    token with_c {
        c <ie_part>
    }
 
    token no_c {
        <ie_part>
    }
 
    token ie_part {
        ie | ei
    }
}
 
class CollectWords::Actions {
    method TOP($/) {
        make $<word>».ast».flat.Bag;
    }
 
    method word($/) {
        if $<with_c> + $<no_c> {
            make ($<with_c>».ast xx $<freq>, $<no_c>».ast xx $<freq>);
        } else {
            make ();
        }
    }
 
    method with_c($/) {
        make "c" ~ $<ie_part>;
    }
 
    method no_c($/) {
        make "!c" ~ $<ie_part>;
    }
}
 
sub plausible($good, $bad, $msg) {
    if $good > 2*$bad {
        say "$msg: PLAUSIBLE ($good ✔ vs. $bad ✘)";
        return True;
    } else {
        say "$msg: NOT PLAUSIBLE ($good ✔ vs. $bad ✘)";
        return False;
    }
}
 
# can't use .parsefile like before due to the non-Unicode £ in this file.
my $file = slurp("1_2_all_freq.txt", :enc<iso-8859-1>);
my $results = CollectWords.parse($file, :actions(CollectWords::Actions)).ast;
 
my $phrasetest = [&] plausible($results<!cie>, $results<!cei>, "I before E when not preceded by C"),
                     plausible($results<cei>, $results<cie>, "E before I when preceded by C");
 
say "I before E except after C: ", $phrasetest ?? "PLAUSIBLE" !! "NOT PLAUSIBLE";
```

#### Output:
```
I before E when not preceded by C: NOT PLAUSIBLE (8222 ✔ vs. 4826 ✘)
E before I when preceded by C: NOT PLAUSIBLE (327 ✔ vs. 994 ✘)
I before E except after C: NOT PLAUSIBLE
```