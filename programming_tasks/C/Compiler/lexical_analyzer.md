[1]: http://rosettacode.org/wiki/Compiler/lexical_analyzer

# [Compiler/lexical analyzer][1]

This is more complicated than strictly necessary for this task. It is set up to be easily adapted to do syntax analysis.



(Note: there are several bogus comments added solely to help with syntax highlighting.)

```perl
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
    token operator:sym<*>   { '*'               { make 'OP_MULTIPLY'  } }
    token operator:sym</>   { '/'<!before '*'>  { make 'OP_DIVIDE'    } }
    token operator:sym<+>   { '+'               { make 'OP_ADD'       } }
    token operator:sym<->   { '-'               { make 'OP_SUBTRACT'  } }
    token operator:sym('<='){ '<='              { make 'OP_LESSEQUAL' } }
    token operator:sym('<') { '<'               { make 'OP_LESS'      } }
    token operator:sym('>') { '>'               { make 'OP_GREATER'   } }
    token operator:sym<!=>  { '!='              { make 'OP_NOTEQUAL'  } }
    token operator:sym<=>   { '='               { make 'OP_ASSIGN'    } }
    token operator:sym<&&>  { '&&'              { make 'OP_AND'       } }
 
    proto token keyword      {*}
    token keyword:sym<if>    { 'if'    { make 'KEYWORD_IF'    } }
    token keyword:sym<putc>  { 'putc'  { make 'KEYWORD_PUTC'  } }
    token keyword:sym<while> { 'while' { make 'KEYWORD_WHILE' } }
    token keyword:sym<print> { 'print' { make 'KEYWORD_PRINT' } }
 
    proto token symbol  {*}
    token symbol:sym<(> { '(' { make 'LEFT_PAREN'  } }
    token symbol:sym<)> { ')' { make 'RIGHT_PAREN' } }
    token symbol:sym<{> { '{' { make 'LEFT_BRACE'  } }
    token symbol:sym<}> { '}' { make 'RIGHT_BRACE' } }
    token symbol:sym<;> { ';' { make 'SEMICOLON'   } }
    token symbol:sym<,> { ',' { make 'COMMA'       } }
 
    token identifier { <[_A..Za..z]><[_A..Za..z0..9]>*    { make 'IDENTIFER ' ~ $/ } }
    token integer    { <[0..9]>+                          { make 'INTEGER   ' ~ $/ } }
 
    token char {
        '\'' [<-[']> | '\n' | '\\\\'] '\''
        { make 'CHAR_LITERAL ' ~ $/.subst("\\n", "\n").substr(1, *-1).ord }
    }
 
    token string {
        '"' <-["\n]>* '"' #'
        {
            make 'STRING ' ~ $/;
            note 'Error: Unknown escape sequence.' and exit if (~$/ ~~ m:r/ <!after <[\\]>>[\\<-[n\\]>]<!before <[\\]>> /);
        }
    }
 
    token eoi { $ { make 'END_OF_INPUT' } }
 
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
    my @pos = gather for $c_code.lines».chars.kv -> $line, $v {
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
  5  15 KEYWORD_PRINT
  5  41 OP_SUBTRACT
  6  15 KEYWORD_PUTC
  6  41 OP_LESS
  7  15 KEYWORD_IF
  7  41 OP_GREATER
  8  15 KEYWORD_WHILE
  8  41 OP_LESSEQUAL
  9  15 LEFT_BRACE
  9  41 OP_NOTEQUAL
 10  15 RIGHT_BRACE
 10  41 OP_AND
 11  15 LEFT_PAREN
 11  41 SEMICOLON
 12  15 RIGHT_PAREN
 12  41 COMMA
 13  15 OP_SUBTRACT
 13  41 OP_ASSIGN
 14  15 OP_MULTIPLY
 14  41 INTEGER   42
 15  15 OP_DIVIDE
 15  41 STRING "String literal"
 16  15 OP_ADD
 16  41 IDENTIFER variable_name
 17  26 CHAR_LITERAL 10
 18  26 CHAR_LITERAL 92
 19  26 CHAR_LITERAL 32
 20   1 END_OF_INPUT
```