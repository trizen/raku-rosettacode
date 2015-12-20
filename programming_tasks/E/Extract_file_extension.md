[1]: http://rosettacode.org/wiki/Extract_file_extension

# [Extract file extension][1]

```perl
use v6 ;
 
sub extension ( Str $filename --> Str ) {
   my $extension = $filename.split(/\./).pop ;
   if ( $extension ) {
      if ( $extension ~~ / <[\/_]> / ) {
	 return "" ;
      }
      else {
	 return "." ~ $extension ;
      }
   }
   else {
      return "" ;
   }
}
 
.say for ("mywebsite.com/picture/image.png" , "http://mywebsite.com/picture/image.png" ,
          "myuniquefile.longextension" , "/path/to.my/file" , "file.odd_one" ).map( { extension $_ } ) ;
 
 
```

#### Output:
```
.png
.png
.longextension
```