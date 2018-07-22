[1]: https://rosettacode.org/wiki/Abbreviations,_easy

# [Abbreviations, easy][1]

Demonstrate that inputting an empty string returns an empty string in addition to the required test input.

```perl
<
Add ALTer  BAckup Bottom  CAppend Change SCHANGE  CInsert CLAst COMPress COpy
COUnt COVerlay CURsor DELete CDelete Down DUPlicate Xedit EXPand EXTract Find
NFind NFINDUp NFUp CFind FINdup FUp FOrward GET Help HEXType Input POWerinput
Join SPlit SPLTJOIN  LOAD  Locate CLocate  LOWercase UPPercase  LPrefix MACRO
MErge MODify MOve MSG Next Overlay PARSE PREServe PURge PUT PUTD  Query  QUIT
READ  RECover REFRESH RENum REPeat  Replace CReplace  RESet  RESTore  RGTLEFT
RIght LEft  SAVE  SET SHift SI  SORT  SOS  STAck STATus  TOP TRAnsfer Type Up
> ~~ m:g/ ((<.:Lu>+) <.:Ll>*) /;
 
my %abr = '' => '', |$/.map: {
    my $abbrv = $_[0].Str.fc;
    |map { $abbrv.substr( 0, $_ ) => $abbrv.uc },
    $_[0][0].Str.chars .. $abbrv.chars
};
 
sub abbr-easy ( $str ) { %abr{$str.fc} // '*error*' }
 
# Testing
for 'riG   rePEAT copies  put mo   rest    types   fup.    6       poweRin', '' -> $str {
    put ' Input: ', $str;
    put 'Output: ', join ' ', $str.words.map: *.&abbr-easy;
}
```

#### Output:
```
 Input: riG   rePEAT copies  put mo   rest    types   fup.    6       poweRin
Output: RIGHT REPEAT *error* PUT MOVE RESTORE *error* *error* *error* POWERINPUT
 Input: 
Output: 
```