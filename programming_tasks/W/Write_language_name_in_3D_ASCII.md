[1]: http://rosettacode.org/wiki/Write_language_name_in_3D_ASCII

# [Write language name in 3D ASCII][1]

Produces a good old-fashioned stereogram, meant to be looked at by focusing beyond the screen, not by crossing your eyes. If you can't get it to converge, try shrinking the font size till the images are closer together than your eyes. Also helps to be old enough to have progressive lenses, so you can look through the reading part of your glasses. Or borrow glasses from someone who's farsighted. `:-)`

```text
my $text = q:to/END/;
 
 @@@@@              @@
 @    @              @     @@@
 @    @              @    @@
 @    @  @@@   @ @@  @    @@
 @@@@@  @   @  @@  @ @    @@@@@
 @      @@@@@  @     @    @@  @@
 @      @      @     @    @@  @@
 @       @@@   @     @@    @@@@
 
END
 
say '' for ^5;
for $text.lines -> $_ is copy {
    my $/;
    my @chars = ｢-+ ., ;: '"｣.comb.pick(*) xx *;
    s:g [' '] = @chars.shift;
    print "                              $_  ";
    s:g [('@'+)(.)] = @chars.shift ~ $0;
    .say;
}
say '' for ^5;
```

#### Output:
```
                              ";,' :-+  . ,. ; -:+"',';-. : " +   ";,' :-+  . ,. ; -:+"',';-. : " + 
                              :@@@@@  ,'+".-; ;-+,@@"'  :.  ;. "  :'@@@@@ ,'+".-; ;-+,,@@'  :.  ;. "
                              "@-.  @ ';,+:- +:  ,;@'". +@@@, "-  " @.  .@';,+:- +:  ,;:@". +;@@@ "-
                              '@,- ;@ "+: .+ -',:" @; . @@; -" ,  ':@- ;'@"+: .+ -',:" .@ . +@@ -" ,
                              ;@ +, @"-@@@' :@.@@  @-,:.@@+ ;'":  ; @+, .@-'@@@ : @;@@  @,:.+@@ ;'":
                               @@@@@+-@':.@, @@" @;@ +":@@@@@',    ;@@@@@--@:. @ .@@ :@ @+": @@@@@, 
                               @;-+, .@@@@@ :@"'+' @"-,:@@; @@.    +@-+, .,@@@@@::@'+'  @-,: @@ "@@ 
                              :@ . "- @,;+'; @" .-'@ +:,@@ ;@@'+  :-@. "- ,@;+'; "@ .-' @+:, @@;:@@+
                              +@'"., ; @@@ -:@;'  "@@-+.:@@@@,    +:@"., ; "@@@-:;@'  ".@@+.: @@@@  
                               ;.:+"-,'  +:'; -,"  .+:"  ;-.,' .   ;.:+"-,'  +:'; -,"  .+:"  ;-.,' .
```