[1]: http://rosettacode.org/wiki/Align_columns

# [Align columns][1]

```perl
###to be called with perl6 columnaligner.pl <orientation>(left, center , right )
###with left as default
my $fh = open  "example.txt" , :r  or die "Can't read text file!\n" ;
my @filelines = $fh.lines ;
close $fh ;
my @maxcolwidths ; #array of the longest words per column
#########fill the array with values#####################
for @filelines -> $line {
   my @words = $line.split( "\$" ) ;
   for 0..@words.elems - 1 -> $i {
      if @maxcolwidths[ $i ] {
	 if @words[ $i ].chars > @maxcolwidths[$i] {
	    @maxcolwidths[ $i ] = @words[ $i ].chars ;
	 }
      }
      else {
	 @maxcolwidths.push( @words[ $i ].chars ) ;
      }
   }
}
my $justification = @*ARGS[ 0 ] || "left" ;
##print lines , $gap holds the number of spaces, 1 to be added 
##to allow for space preceding or following longest word
for @filelines -> $line {
   my @words = $line.split( "\$" ) ;
   for 0 ..^ @words -> $i {
      my $gap =  @maxcolwidths[$i] - @words[$i].chars + 1 ;
      if $justification eq "left" {
	 print @words[ $i ] ~ " " x $gap ;
      } elsif $justification eq "right" {
	 print  " " x $gap ~ @words[$i] ;
      } elsif $justification eq "center" {
	 $gap = ( @maxcolwidths[ $i ] + 2 - @words[$i].chars ) div 2 ;
	 print " " x $gap ~ @words[$i] ~ " " x $gap ;
      }
   }
   say ''; #for the newline
}
```


Or another way. To be called exactly as the first script.

```perl
my @lines = slurp("example.txt").lines;
my @widths;
 
for @lines { for .split('$').kv { @widths[$^key] max= $^word.chars; } }
for @lines { say |.split('$').kv.map: { (align @widths[$^key], $^word) ~ " "; } }
 
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


Or a more functional version, called like `./align.p6 left input.txt`, which however only supports left and right alignment (not center):

```perl
sub MAIN ($alignment where 'left'|'right', $file) {
    my @lines := $file.IO.lines.map(*.split: '$').List;
    my @widths = roundrobin(|@lines).map(*».chars.max);
    my $align  = {left=>'-', right=>''}{$alignment};
    my $format = @widths.map({ "%{++$}\${$align}{$_}s" }).join(" ") ~ "\n";
    printf $format, |$_ for @lines;
}
```