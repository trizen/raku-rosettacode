[1]: http://rosettacode.org/wiki/Straddling_checkerboard

# [Straddling checkerboard][1]

The .trans method in Perl 6 improves on Perl 5's tr/// by allowing multi-character translation tables.



We build the full table during .new, which simplifies .encode and .decode.

```perl6
class Straddling_Checkerboard {
    has @!flat_board; # 10x3 stored as 30x1
    has $!plain2code; # Full translation table; invertable
    has @!table;      # Printable layout, like Wikipedia entry
 
    my $numeric_escape = '/';
    my $exclude = /<-[A..Z0..9.]>/; # Omit the escape character
 
    method display_table { say ~ .list for @!table };
 
    method decode ( Str $s --> Str ) {
        $s.trans($!plain2code.invert);
    }
 
    method encode ( Str $s, :$collapse? --> Str ) {
        my $replace = $collapse ?? '' !! '.';
        $s.uc.subst( $exclude, $replace, :g ).trans($!plain2code);
    }
 
    submethod BUILD ( :$alphabet, :$u where 0..9, :$v where 0..9 ) {
        die if $u == $v;
        die if $alphabet.comb.sort.join ne [~] './', 'A'..'Z';
 
        @!flat_board = $alphabet.uc.comb;
        @!flat_board.splice( $u min $v, 0, Any );
        @!flat_board.splice( $u max $v, 0, Any );
 
        @!table = [ ' ',             [ 0 ..  9]               ],
                  [ ' ', @!flat_board[ 0 ..  9].map: * // ' ' ],
                  [ $u,  @!flat_board[10 .. 19]               ],
                  [ $v,  @!flat_board[20 .. 29]               ];
 
        my @order = 0..9; # This may be passed as a param in the future
 
        my @nums = @order,
                   @order.map({ +"$u$_" }),
                   @order.map({ +"$v$_" });
 
        my %p2c = @!flat_board Z=> @nums;
        %p2c.delete(Any);
        %p2c{$_} = %p2c{$numeric_escape} ~ $_ for 0..9;
 
        $!plain2code = [%p2c.keys] => [%p2c.values];
    }
}
 
sub MAIN ( :$u = 3, :$v = 7, :$alphabet = 'HOLMESRTABCDFGIJKNPQUVWXYZ./' ) {
    my Straddling_Checkerboard $sc .= new: :$u, :$v, :$alphabet;
    $sc.display_table;
 
    for 0..1 -> $collapse {
        my $original = 'One night-it was on the twentieth of March, 1888-I was returning';
        my $en = $sc.encode($original, :$collapse);
        my $de = $sc.decode($en);
        say;
        say "Original: $original";
        say "Encoded:  $en";
        say "Decoded:  $de";
    }
}
```

#### Output:
```
  0 1 2 3 4 5 6 7 8 9
  H O L   M E S   R T
3 A B C D F G I J K N
7 P Q U V W X Y Z . /

Original: One night-it was on the twentieth of March, 1888-I was returning
Encoded:  13957839363509783697874306781397890578974539936590781347843083207878791798798798783678743067885972839363935
Decoded:  ONE.NIGHT.IT.WAS.ON.THE.TWENTIETH.OF.MARCH..1888.I.WAS.RETURNING

Original: One night-it was on the twentieth of March, 1888-I was returning
Encoded:  139539363509369743061399059745399365901344308320791798798798367430685972839363935
Decoded:  ONENIGHTITWASONTHETWENTIETHOFMARCH1888IWASRETURNING
```