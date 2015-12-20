[1]: http://rosettacode.org/wiki/Rosetta_Code/Find_bare_lang_tags

# [Rosetta Code/Find bare lang tags][1]

The only tricky thing here is the use of the <tt>ms</tt> form of match, short for <tt>m:sigspace</tt>. This causes whitespace in the regex to be considered "significant", that is, it matches optional whitespace at those positions, as if you'd put <tt>\s\*</tt> there. Of course, the regexes themselves are in Perl 6 syntax, which is quite different from Perl 5 regex syntax (and arguably much cleaner). Regex syntax is perhaps the area in which Perl 6 diverges most from Perl 5.

```perl6
my $lang = '(no language)';
my $total = 0;
my %blanks;
 
for lines() {
  when / '<lang>' / {
    %blanks{$lang}++;
    $total++;
  }
  when ms/ '==' '{{' 'header' '|' ( \S+? ) '}}' '==' / {
    $lang = $0.lc;
  }
}
 
say "$total bare language tag{ 's' if $total != 1 }\n";
say .value, ' in ', .key for %blanks.sort;
```

#### Output:
```
2 bare language tags

1 in (no language)
1 in perl
```