[1]: http://rosettacode.org/wiki/Extract_file_extension

# [Extract file extension][1]

```perl
use v6 ;
 
sub extension ( Str $filename --> Str ) {
   my $extension = $filename.[split](http://perldoc.perl.org/functions/split.html)(/\./).[pop](http://perldoc.perl.org/functions/pop.html) ;
   if ( $extension ) {
      if ( $extension ~~ / <[\/_]> / ) {
	 [return](http://perldoc.perl.org/functions/return.html) "" ;
      }
      else {
	 [return](http://perldoc.perl.org/functions/return.html) "." ~ $extension ;
      }
   }
   else {
      [return](http://perldoc.perl.org/functions/return.html) "" ;
   }
}
 
.say for ("mywebsite.com/picture/image.png" , "http://mywebsite.com/picture/image.png" ,
          "myuniquefile.longextension" , "/path/to.my/file" , "file.odd_one" ).[map](http://perldoc.perl.org/functions/map.html)( { extension $_ } ) ;
 
 
```

#### Output:
```
.png
.png
.longextension
```