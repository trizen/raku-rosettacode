[1]: https://rosettacode.org/wiki/Parse_EBNF

# [Parse EBNF][1]





This parses the EBNF rule set using a Raku grammar, then if it parses as valid EBNF, constructs a grammar and parses the test strings with that. EBNF rule sets that are naively syntactically correct but missing rules will parse as valid but will give a runtime failure warning about missing methods.
It is implemented and exercised using the flavor of EBNF and test cases specified on the [test page](https://rosettacode.org/wiki/Parse_EBNF/Tests).

```perl
# A Raku grammar to parse EBNF
grammar EBNF {
    rule         TOP { ^ <title>? '{' [ <production> ]+ '}' <comment>? $ }
    rule  production { <name> '=' <expression> <[.;]> }
    rule  expression { <term> +% "|" }
    rule        term { <factor>+ }
    rule      factor { <group> | <repeat> | <optional> | <identifier> | <literal> }
    rule       group { '(' <expression> ')' }
    rule      repeat { '{' <expression> '}' }
    rule    optional { '[' <expression> ']' }
    token identifier { <-[\|\(\)\{\}\[\]\.\;\"\'\s]>+ } #"
    token    literal { ["'" <-[']>+ "'" | '"' <-["]>+ '"'] } #"
    token      title { <literal> }
    token    comment { <literal> }
    token       name { <identifier>  <?before \h* '='> }
}

class EBNF::Actions {
    method        TOP($/) { 
                            say "Syntax Tree:\n", $/; # Dump the syntax tree to STDOUT
                            make 'grammar ' ~
                              ($<title> ?? $<title>.subst(/\W/, '', :g) !! 'unnamed') ~
                              " \{\n rule TOP \{^[<" ~ $/<production>[0]<name> ~
                              ">]+\$\}\n " ~ $<production>>>.ast ~ "\}"
                          }
   method production($/) { 
                            make 'token ' ~ $<name> ~ ' {' ~
                              $<expression>.ast ~ "}\n"
                          }
   method expression($/) { make join '|', $<term>>>.ast }
   method       term($/) { make join '\h*', $<factor>>>.ast }
   method     factor($/) { 
                            make $<literal>  ?? $<literal> !!
                              $<group>    ?? '[' ~ $<group>.ast    ~ ']'  !!
                              $<repeat>   ?? '[' ~ $<repeat>.ast   ~ '\\s*]*' !!
                              $<optional> ?? '[' ~ $<optional>.ast ~ ']?' !!
                              '<' ~ $<identifier> ~ '>'
                          }
   method     repeat($/) { make $<expression>.ast }
   method   optional($/) { make $<expression>.ast }
   method      group($/) { make $<expression>.ast }
}

# An array of test cases
my @tests = (
    {
        ebnf => 
            q<"a" {
                a = "a1" ( "a2" | "a3" ) { "a4" } [ "a5" ] "a6" ;
            } "z">
        ,
        teststrings => [
            'a1a3a4a4a5a6',
            'a1 a2a6',
            'a1 a3 a4 a6',
            'a1 a4 a5 a6',
            'a1 a2 a4 a4 a5 a6',
            'a1 a2 a4 a5 a5 a6',
            'a1 a2 a4 a5 a6 a7',
            'your ad here' 
        ]
    },
    {
        ebnf =>
            q<{
                expr = term { plus term } .
                term = factor { times factor } .
                factor = number | '(' expr ')' .
 
                plus = "+" | "-" .
                times = "*" | "/" .
 
                number = digit { digit } .
                digit = "0" | "1" | "2" | "3" | "4" | "5" | "6" | "7" | "8" | "9" .
            }>
        ,
        teststrings => [
            '2',
            '2*3 + 4/23 - 7',
            '(3 + 4) * 6-2+(4*(4))',
            '-2',
            '3 +',
            '(4 + 3'
        ]
    },
    {
        ebnf => q<a = "1";>,
        teststrings => ['foobar']
    },
    {
        ebnf => q<{ a = "1" ;>,
        teststrings => ['foobar']
    },
    {
        ebnf => q<{ hello world = "1"; }>,
        teststrings => ['foobar']
    },
    {
        ebnf => q<{ foo = bar . }>,
        teststrings => ['foobar']
    }
);

# Test the parser.
my $i = 1;
for @tests -> $test {
    unless EBNF.parse($test<ebnf>) {
         say "Parsing EBNF grammar:\n";
         say "{$test<ebnf>.subst(/^^\h*/,'',:g)}\n";
         say "Invalid syntax. Can not be parsed.\n";
         say '*' x 79;
         next;
    }
    my $p = EBNF.parse($test<ebnf>, :actions(EBNF::Actions));
    my $grammar = $p.ast;
    $grammar ~~ m/^'grammar '(\w+)/;
    my $title = $0;
    my $fn = 'EBNFtest'~$i++;
    my $fh = open($fn, :w) orelse .die;
    $fh.say( "\{\n", $grammar );
    $fh.say( qq|say "Parsing EBNF grammar '$title':\\n";| );
    $fh.say( qq|say q<{$test<ebnf>.subst(/^^\h*/,'',:g)}>;| );
    $fh.say(  q|say "\nValid syntax.\n\nTesting:\n";| );
    $fh.say(  q|CATCH { default { say " - $_" } };| );
    my $len = max $test<teststrings>.flat>>.chars;
    for $test<teststrings>.flat -> $s {
        $fh.say( qq|printf "%{$len}s", '{$s}';| ~
                 qq|printf " - %s\\n", {$title}.parse('{$s}')| ~
                 qq| ?? 'valid.' !! 'NOT valid.';|
               );
    }
    $fh.say( qq| "\\n"} |);
    $fh.close;
    say qqx/raku $fn/;
    say '*' x 79, "\n";
    unlink $fn;
}
```


Output:


```
Syntax Tree:
｢"a" {
                a = "a1" ( "a2" | "a3" ) { "a4" } [ "a5" ] "a6" ;
            } "z"｣
 title => ｢"a"｣
  literal => ｢"a"｣
 production => ｢a = "a1" ( "a2" | "a3" ) { "a4" } [ "a5" ] "a6" ;
            ｣
  name => ｢a｣
   identifier => ｢a｣
  expression => ｢"a1" ( "a2" | "a3" ) { "a4" } [ "a5" ] "a6" ｣
   term => ｢"a1" ( "a2" | "a3" ) { "a4" } [ "a5" ] "a6" ｣
    factor => ｢"a1" ｣
     literal => ｢"a1"｣
    factor => ｢( "a2" | "a3" ) ｣
     group => ｢( "a2" | "a3" ) ｣
      expression => ｢"a2" | "a3" ｣
       term => ｢"a2" ｣
        factor => ｢"a2" ｣
         literal => ｢"a2"｣
       term => ｢ "a3" ｣
        factor => ｢"a3" ｣
         literal => ｢"a3"｣
    factor => ｢{ "a4" } ｣
     repeat => ｢{ "a4" } ｣
      expression => ｢"a4" ｣
       term => ｢"a4" ｣
        factor => ｢"a4" ｣
         literal => ｢"a4"｣
    factor => ｢[ "a5" ] ｣
     optional => ｢[ "a5" ] ｣
      expression => ｢"a5" ｣
       term => ｢"a5" ｣
        factor => ｢"a5" ｣
         literal => ｢"a5"｣
    factor => ｢"a6" ｣
     literal => ｢"a6"｣
 comment => ｢"z"｣
  literal => ｢"z"｣

Parsing EBNF grammar 'a':

"a" {
a = "a1" ( "a2" | "a3" ) { "a4" } [ "a5" ] "a6" ;
} "z"

Valid syntax.

Testing:

     a1a3a4a4a5a6 - valid.
          a1 a2a6 - valid.
      a1 a3 a4 a6 - valid.
      a1 a4 a5 a6 - NOT valid.
a1 a2 a4 a4 a5 a6 - valid.
a1 a2 a4 a5 a5 a6 - NOT valid.
a1 a2 a4 a5 a6 a7 - NOT valid.
     your ad here - NOT valid.

*******************************************************************************

Syntax Tree:
｢{
                expr = term { plus term } .
                term = factor { times factor } .
                factor = number | '(' expr ')' .

                plus = "+" | "-" .
                times = "*" | "/" .

                number = digit { digit } .
                digit = "0" | "1" | "2" | "3" | "4" | "5" | "6" | "7" | "8" | "9" .
            }｣
 production => ｢expr = term { plus term } .
                ｣
  name => ｢expr｣
   identifier => ｢expr｣
  expression => ｢term { plus term } ｣
   term => ｢term { plus term } ｣
    factor => ｢term ｣
     identifier => ｢term｣
    factor => ｢{ plus term } ｣
     repeat => ｢{ plus term } ｣
      expression => ｢plus term ｣
       term => ｢plus term ｣
        factor => ｢plus ｣
         identifier => ｢plus｣
        factor => ｢term ｣
         identifier => ｢term｣
 production => ｢term = factor { times factor } .
                ｣
  name => ｢term｣
   identifier => ｢term｣
  expression => ｢factor { times factor } ｣
   term => ｢factor { times factor } ｣
    factor => ｢factor ｣
     identifier => ｢factor｣
    factor => ｢{ times factor } ｣
     repeat => ｢{ times factor } ｣
      expression => ｢times factor ｣
       term => ｢times factor ｣
        factor => ｢times ｣
         identifier => ｢times｣
        factor => ｢factor ｣
         identifier => ｢factor｣
 production => ｢factor = number | '(' expr ')' .

                ｣
  name => ｢factor｣
   identifier => ｢factor｣
  expression => ｢number | '(' expr ')' ｣
   term => ｢number ｣
    factor => ｢number ｣
     identifier => ｢number｣
   term => ｢ '(' expr ')' ｣
    factor => ｢'(' ｣
     literal => ｢'('｣
    factor => ｢expr ｣
     identifier => ｢expr｣
    factor => ｢')' ｣
     literal => ｢')'｣
 production => ｢plus = "+" | "-" .
                ｣
  name => ｢plus｣
   identifier => ｢plus｣
  expression => ｢"+" | "-" ｣
   term => ｢"+" ｣
    factor => ｢"+" ｣
     literal => ｢"+"｣
   term => ｢ "-" ｣
    factor => ｢"-" ｣
     literal => ｢"-"｣
 production => ｢times = "*" | "/" .

                ｣
  name => ｢times｣
   identifier => ｢times｣
  expression => ｢"*" | "/" ｣
   term => ｢"*" ｣
    factor => ｢"*" ｣
     literal => ｢"*"｣
   term => ｢ "/" ｣
    factor => ｢"/" ｣
     literal => ｢"/"｣
 production => ｢number = digit { digit } .
                ｣
  name => ｢number｣
   identifier => ｢number｣
  expression => ｢digit { digit } ｣
   term => ｢digit { digit } ｣
    factor => ｢digit ｣
     identifier => ｢digit｣
    factor => ｢{ digit } ｣
     repeat => ｢{ digit } ｣
      expression => ｢digit ｣
       term => ｢digit ｣
        factor => ｢digit ｣
         identifier => ｢digit｣
 production => ｢digit = "0" | "1" | "2" | "3" | "4" | "5" | "6" | "7" | "8" | "9" .
            ｣
  name => ｢digit｣
   identifier => ｢digit｣
  expression => ｢"0" | "1" | "2" | "3" | "4" | "5" | "6" | "7" | "8" | "9" ｣
   term => ｢"0" ｣
    factor => ｢"0" ｣
     literal => ｢"0"｣
   term => ｢ "1" ｣
    factor => ｢"1" ｣
     literal => ｢"1"｣
   term => ｢ "2" ｣
    factor => ｢"2" ｣
     literal => ｢"2"｣
   term => ｢ "3" ｣
    factor => ｢"3" ｣
     literal => ｢"3"｣
   term => ｢ "4" ｣
    factor => ｢"4" ｣
     literal => ｢"4"｣
   term => ｢ "5" ｣
    factor => ｢"5" ｣
     literal => ｢"5"｣
   term => ｢ "6" ｣
    factor => ｢"6" ｣
     literal => ｢"6"｣
   term => ｢ "7" ｣
    factor => ｢"7" ｣
     literal => ｢"7"｣
   term => ｢ "8" ｣
    factor => ｢"8" ｣
     literal => ｢"8"｣
   term => ｢ "9" ｣
    factor => ｢"9" ｣
     literal => ｢"9"｣

Parsing EBNF grammar 'unnamed':

{
expr = term { plus term } .
term = factor { times factor } .
factor = number | '(' expr ')' .

plus = "+" | "-" .
times = "*" | "/" .

number = digit { digit } .
digit = "0" | "1" | "2" | "3" | "4" | "5" | "6" | "7" | "8" | "9" .
}

Valid syntax.

Testing:

                    2 - valid.
       2*3 + 4/23 - 7 - valid.
(3 + 4) * 6-2+(4*(4)) - valid.
                   -2 - NOT valid.
                  3 + - NOT valid.
               (4 + 3 - NOT valid.

*******************************************************************************

Parsing EBNF grammar:

a = "1";

Invalid syntax. Can not be parsed.

*******************************************************************************
Parsing EBNF grammar:

{ a = "1" ;

Invalid syntax. Can not be parsed.

*******************************************************************************
Parsing EBNF grammar:

{ hello world = "1"; }

Invalid syntax. Can not be parsed.

*******************************************************************************
Syntax Tree:
｢{ foo = bar . }｣
 production => ｢foo = bar . ｣
  name => ｢foo｣
   identifier => ｢foo｣
  expression => ｢bar ｣
   term => ｢bar ｣
    factor => ｢bar ｣
     identifier => ｢bar｣

Parsing EBNF grammar 'unnamed':

{ foo = bar . }

Valid syntax.

Testing:

foobar - No such method 'bar' for invocant of type 'unnamed'

*******************************************************************************
```
