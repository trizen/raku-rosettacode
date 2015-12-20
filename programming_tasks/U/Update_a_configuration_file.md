[1]: http://rosettacode.org/wiki/Update_a_configuration_file

# [Update a configuration file][1]

Implemented as a command-line script which can make arbitrary in-place updates to such config files.



Assuming that the script is saved as `conf-update` and the config file as `test.cfg`, the four changes required by the task description could be performed with the command:


#### Output:
```
conf-update --/needspeeling --seedsremoved --numberofbananas=1024 --numberofstrawberries=62000 test.cfg
```


The script:

```perl
#!/usr/bin/env perl6
 
my $tmpfile = tmpfile;
 
sub MAIN ($file, *%changes) {
    %changes.=map({; .key.uc => .value });
    my %seen;
 
    my $out = open $tmpfile, :w;
 
    for $file.IO.lines {
        when /:s ^ ('#' .* | '') $/ {
            say $out: ~$0;
        }
        when /:s ^ (';'+)? [(\w+) (\w+)?]? $/ {
            next if !$1 or %seen{$1.uc}++;
            my $new = %changes{$1.uc}:delete;
            say $out: format-line $1, |( !defined($new)  ?? ($2, !$0)  !!
                                         $new ~~ Bool    ?? ($2, $new) !! ($new, True) );
        }
        default {
            note "Malformed line: $_\nAborting.";
            exit 1;
        }
    }
 
    say $out: format-line .key, |(.value ~~ Bool ?? (Nil, .value) !! (.value, True))
        for %changes;
 
    run 'mv', $tmpfile, $file; # work-around for NYI `move $tmpfile, $file;`
}
 
END { unlink $tmpfile if $tmpfile.IO.e }
 
 
sub format-line ($key, $value, $enabled) {
    ("; " if !$enabled) ~ $key.uc ~ (" $value" if defined $value);
}
 
sub tmpfile {
    $*SPEC.catfile: $*SPEC.tmpdir, ("a".."z").roll(20).join
}
```