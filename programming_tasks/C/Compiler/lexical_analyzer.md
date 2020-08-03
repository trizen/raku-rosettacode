[1]: https://rosettacode.org/wiki/Compiler/lexical_analyzer

# [Compiler/lexical analyzer][1]

This is more complicated than strictly necessary for this task. It is set up to be easily adapted to do syntax analysis.



(Note: there are several bogus comments added solely to help with syntax highlighting.)

```raku
grammar tiny_C {
    rule TOP { ^ <.whitespace>? <tokens> + % <.whitespace> <.whitespace> <eoi> }
 
    rule whitespace { [ <comment> + % <ws> | <ws> ] }
 
    token comment    { '/*' ~ '*/' .*? }
 
    token tokens {
        [
        | <operator>   { make $/<operator>.ast   }
        | <keyword>    { make $/<keyword>.ast    }
        | <symbol>     { make $/<symbol>.ast     }
        | <identifier> { make $/<identifier>.ast }
        | <integer>    { make $/<integer>.ast    }
        | <char>       { make $/<char>.ast       }
        | <string>     { make $/<string>.ast     }
        | <error>
        ]
    }
 
    proto token operator    {*}
    token operator:sym<*>   { '*'               { make 'Op_multiply'    } }
    token operator:sym</>   { '/'<!before '*'>  { make 'Op_divide'      } }
    token operator:sym<%>   { '%'               { make 'Op_mod'         } }
    token operator:sym<+>   { '+'               { make 'Op_add'         } }
    token operator:sym<->   { '-'               { make 'Op_subtract'    } }
    token operator:sym('<='){ '<='              { make 'Op_lessequal'   } }
    token operator:sym('<') { '<'               { make 'Op_less'        } }
    token operator:sym('>='){ '>='              { make 'Op_greaterequal'} }
    token operator:sym('>') { '>'               { make 'Op_greater'     } }
    token operator:sym<==>  { '=='              { make 'Op_equal'       } }
    token operator:sym<!=>  { '!='              { make 'Op_notequal'    } }
    token operator:sym<!>   { '!'               { make 'Op_not'         } }
    token operator:sym<=>   { '='               { make 'Op_assign'      } }
    token operator:sym<&&>  { '&&'              { make 'Op_and'         } }
    token operator:sym<||>  { '||'              { make 'Op_or'          } }
 
    proto token keyword      {*}
    token keyword:sym<if>    { 'if'    { make 'Keyword_if'    } }
    token keyword:sym<else>  { 'else'  { make 'Keyword_else'  } }
    token keyword:sym<putc>  { 'putc'  { make 'Keyword_putc'  } }
    token keyword:sym<while> { 'while' { make 'Keyword_while' } }
    token keyword:sym<print> { 'print' { make 'Keyword_print' } }
 
    proto token symbol  {*}
    token symbol:sym<(> { '(' { make 'LeftParen'  } }
    token symbol:sym<)> { ')' { make 'RightParen' } }
    token symbol:sym<{> { '{' { make 'LeftBrace'  } }
    token symbol:sym<}> { '}' { make 'RightBrace' } }
    token symbol:sym<;> { ';' { make 'Semicolon'   } }
    token symbol:sym<,> { ',' { make 'Comma'       } }
 
    token identifier { <[_A..Za..z]><[_A..Za..z0..9]>* { make 'Identifier ' ~ $/ } }
    token integer    { <[0..9]>+                       { make 'Integer '    ~ $/ } }
 
    token char {
        '\'' [<-[']> | '\n' | '\\\\'] '\''
        { make 'Char_Literal ' ~ $/.subst("\\n", "\n").substr(1, *-1).ord }
    }
 
    token string {
        '"' <-["\n]>* '"' #'
        {
            make 'String ' ~ $/;
            note 'Error: Unknown escape sequence.' and exit if (~$/ ~~ m:r/ <!after <[\\]>>[\\<-[n\\]>]<!before <[\\]>> /);
        }
    }
 
    token eoi { $ { make 'End_of_input' } }
 
    token error {
        | '\'''\''                   { note 'Error: Empty character constant.' and exit }
        | '\'' <-[']> ** {2..*} '\'' { note 'Error: Multi-character constant.' and exit }
        | '/*' <-[*]>* $             { note 'Error: End-of-file in comment.'   and exit }
        | '"' <-["]>* $              { note 'Error: End-of-file in string.'    and exit }
        | '"' <-["]>*? \n            { note 'Error: End of line in string.'    and exit } #'
    }
}
 
sub parse_it ( $c_code ) {
    my $l;
    my @pos = gather for $c_code.lines>>.chars.kv -> $line, $v {
        take [ $line + 1, $_ ] for 1 .. ($v+1); # v+1 for newline
        $l = $line+2;
    }
    @pos.push: [ $l, 1 ]; # capture eoi
 
    for flat $c_code<tokens>.list, $c_code<eoi> -> $m {
        say join "\t", @pos[$m.from].fmt('%3d'), $m.ast;
    }
}
 
my $tokenizer = tiny_C.parse(@*ARGS[0].IO.slurp);
parse_it( $tokenizer );
```

#### Output:
```
  5  16 Keyword_print
  5  40 Op_subtract
  6  16 Keyword_putc
  6  40 Op_less
  7  16 Keyword_if
  7  40 Op_greater
  8  16 Keyword_else
  8  40 Op_lessequal
  9  16 Keyword_while
  9  40 Op_greaterequal
 10  16 LeftBrace
 10  40 Op_equal
 11  16 RightBrace
 11  40 Op_notequal
 12  16 LeftParen
 12  40 Op_and
 13  16 RightParen
 13  40 Op_or
 14  16 Op_subtract
 14  40 Semicolon
 15  16 Op_not
 15  40 Comma
 16  16 Op_multiply
 16  40 Op_assign
 17  16 Op_divide
 17  40 Integer 42
 18  16 Op_mod
 18  40 String "String literal"
 19  16 Op_add
 19  40 Identifier variable_name
 20  26 Char_Literal 10
 21  26 Char_Literal 92
 22  26 Char_Literal 32
 23   1 End_of_input
```