[1]: http://rosettacode.org/wiki/Align_columns

# [Align columns][1]

```perl
###to be called with perl6 columnaligner.pl <orientation>(left, center , right )
###with left as default
my $fh = [open](http://perldoc.perl.org/functions/open.html)  "example.txt" , :r  or [die](http://perldoc.perl.org/functions/die.html) "Can't read text file!\n" ;
my @filelines = $fh.lines ;
[close](http://perldoc.perl.org/functions/close.html) $fh ;
my @maxcolwidths ; #array of the longest words per column
#########fill the array with values#####################
for @filelines -> $line {
   my @words = $line.[split](http://perldoc.perl.org/functions/split.html)( "\$" ) ;
   for 0..@words.elems - 1 -> $i {
      if @maxcolwidths[ $i ] {
	 if @words[ $i ].chars > @maxcolwidths[$i] {
	    @maxcolwidths[ $i ] = @words[ $i ].chars ;
	 }
      }
      else {
	 @maxcolwidths.[push](http://perldoc.perl.org/functions/push.html)( @words[ $i ].chars ) ;
      }
   }
}
my $justification = @*ARGS[ 0 ] || "left" ;
##print lines , $gap holds the number of spaces, 1 to be added 
##to allow for space preceding or following longest word
for @filelines -> $line {
   my @words = $line.[split](http://perldoc.perl.org/functions/split.html)( "\$" ) ;
   for 0 ..^ @words -> $i {
      my $gap =  @maxcolwidths[$i] - @words[$i].chars + 1 ;
      if $justification eq "left" {
	 [print](http://perldoc.perl.org/functions/print.html) @words[ $i ] ~ " " x $gap ;
      } elsif $justification eq "right" {
	 [print](http://perldoc.perl.org/functions/print.html)  " " x $gap ~ @words[$i] ;
      } elsif $justification eq "center" {
	 $gap = ( @maxcolwidths[ $i ] + 2 - @words[$i].chars ) div 2 ;
	 [print](http://perldoc.perl.org/functions/print.html) " " x $gap ~ @words[$i] ~ " " x $gap ;
      }
   }
   say ''; #for the newline
}
```


Or another way. To be called exactly as the first script.

```perl
my @lines = slurp("example.txt").lines;
my @widths;
 
for @lines { for .[split](http://perldoc.perl.org/functions/split.html)('$').kv { @widths[$^key] max= $^word.chars; } }
for @lines { say .[split](http://perldoc.perl.org/functions/split.html)('$').kv.[map](http://perldoc.perl.org/functions/map.html): { (align @widths[$^key], $^word) ~ " "; } }
 
sub align($column_width, $word, $aligment = @*ARGS[0]) {
        my $lr = $column_width - $word.chars;
        my $c  = $lr / 2;
        given ($aligment) {
                when "center" { " " x $c.ceiling ~ $word ~ " " x $c.floor }
                when "right"  { " " x $lr        ~ $word                  }
                default       {                    $word ~ " " x $lr      }
        }
}
```