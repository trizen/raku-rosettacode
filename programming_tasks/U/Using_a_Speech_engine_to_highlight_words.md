[1]: https://rosettacode.org/wiki/Using_a_Speech_engine_to_highlight_words

# [Using a Speech engine to highlight words][1]

```perl
# 20200826 Raku programming solution
 
my \s = "Actions speak louder than words.";
my \prev = $ = "";
my \prevLen = $ = 0;
my \bs = $ = "";
 
for s.split(' ', :skip-empty) {
   run "/usr/bin/espeak", $_ or die;
   bs = "\b" x prevLen if prevLen > 0;
   printf "%s%s%s ", bs, prev, $_.uc;
   prev = "$_ ";
   prevLen = $_.chars + 1
}
 
printf "%s%s\n", "\b" x prevLen, prev;
```
