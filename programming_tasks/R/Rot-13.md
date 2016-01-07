[1]: http://rosettacode.org/wiki/Rot-13

# [Rot-13][1]

As in Perl 5, there are `-n` and `-p` command-line options that ease the creation of a unix filter.



Here for instance with `-p`

```perl
.=trans: 'a..mn..z' => 'n..za..m', :ii
```