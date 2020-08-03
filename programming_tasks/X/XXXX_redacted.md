[1]: https://rosettacode.org/wiki/XXXX_redacted

# [XXXX redacted][1]

```perl
sub redact ( Str $str, Str $redact, :i(:$insensitive) = False, :p(:$partial) = False, :o(:$overkill) = False ) {
    my $rx =
        $insensitive ??
            $partial ??
           $overkill ?? rx/:i <?after ^ | <:Po> | \s > (<[\w<:!Po>-]>*? [\w*\']? $redact [\w*\'\w+]? \S*?) <?before $ | <:Po> | \s > / #'
                     !! rx/:i ($redact) /
                     !! rx/:i <?after ^ | [\s<:Po>] | \s > ($redact) <?before $ | <:Po> | \s > /
                     !!
            $partial ??
           $overkill ?? rx/ <?after ^ | <:Po> | \s > (<[\w<:!Po>-]>*? [\w*\']? $redact [\w*\'\w+]? \S*?) <?before $ | <:Po> | \s > / #'
                     !! rx/ ($redact) /
                     !! rx/ <?after ^ | [\s<:Po>] | \s > ($redact) <?before $ | <:Po> | \s > /
    ;
    $str.subst( $rx, {'X' x $0.chars}, :g )
}
 
my %*SUB-MAIN-OPTS = :named-anywhere;
 
# Operate on a given file with the given parameters
multi MAIN (
    Str $file,    #= File name with path
    Str $target,  #= Redact target text string
    :$i = False,  #= Case insensitive flag
    :$p = False,  #= Partial words flag
    :$o = False   #= Overkill flag
  ) { put $file.IO.slurp.&redact( $target, :i($i), :p($p), :o($o) ) }
 
# Operate on the internal strings / parameters
multi MAIN () {
 
# TESTING
 
    my $test = q:to/END/;
        Tom? Toms bottom tomato is in his stomach while playing the "Tom-tom" brand tom-toms. That's so tom.
        'Tis very tomish, don't you think?
        END
        #'
 
    for 'Tom', 'tom', 't' -> $redact {
        say "\nRedact '$redact':";
        for '[w|s|n]', $redact, {},
            '[w|i|n]', $redact, {:i},
            '[p|s|n]', $redact, {:p},
            '[p|i|n]', $redact, {:p, :i},
            '[p|s|o]', $redact, {:p, :o},
            '[p|i|o]', $redact, {:p, :i, :o}
        -> $option, $str, %opts { printf "%s %s\n", $option, $test.&redact($str, |%opts) }
    }
 
    my $emoji = '🧑 👨 🧔 👨‍👩‍👦';
    printf "%20s %s\n", '', $emoji;
    printf "%20s %s\n", "Redact '👨' [w]", $emoji.&redact('👨');
    printf "%20s %s\n", "Redact '👨‍👩‍👦' [w]", $emoji.&redact('👨‍👩‍👦');
 
    # Even more complicated Unicode
 
    $emoji = 'Argentina🧑🇦🇹  France👨🇫🇷  Germany🧔🇩🇪  Netherlands👨‍👩‍👦🇳🇱';
    printf "\n%20s %s\n", '', $emoji;
    printf "%20s %s\n", "Redact '👨' [p]", $emoji.&redact('👨', :p);
    printf "%20s %s\n", "Redact '👨‍👩‍👦' [p]", $emoji.&redact('👨‍👩‍👦', :p);
    printf "%20s %s\n", "Redact '👨' [p|o]", $emoji.&redact('👨', :p, :o);
    printf "%20s %s\n", "Redact '👨‍👩‍👦' [p|o]", $emoji.&redact('👨‍👩‍👦', :p, :o);
}
```
