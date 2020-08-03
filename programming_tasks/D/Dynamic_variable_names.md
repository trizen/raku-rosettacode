[1]: https://rosettacode.org/wiki/Dynamic_variable_names

# [Dynamic variable names][1]

You can [interpolate strings as variable names](https://docs.perl6.org/language/packages#Interpolating_into_names):

```raku
our $our-var = 'The our var';
my  $my-var  = 'The my var';
 
my $name  = prompt 'Variable name: ';
my $value = $::('name'); # use the right sigil, etc
 
put qq/Var ($name) starts with value ｢$value｣/;
 
$::('name') = 137;
 
put qq/Var ($name) ends with value ｢{$::('name')}｣/;
 
```