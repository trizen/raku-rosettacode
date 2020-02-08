[1]: https://rosettacode.org/wiki/Recursive_descent_parser_generator

# [Recursive descent parser generator][1]

Instead of writing a full-blown generator, it is possible to solve the task partly by making use of the built-in optimizer and study the relevant AST output


#### Output:
```
perl6 --target=optimize -e 'no strict; say ($one + $two) * $three - $four * $five' | tail -11
                  - QAST::Op(callstatic &say) <sunk> :statement_id<2> say ($one + $two) * $three - $four * $five
                    - QAST::Op(callstatic &infix:<->) <wanted> -
                      - QAST::Op(callstatic &infix:<*>) <wanted> *
                        - QAST::Op(callstatic &infix:<+>) <wanted> :statement_id<3> +
                          - QAST::Var(local __lowered_lex_5) <wanted> $one
                          - QAST::Var(local __lowered_lex_4) <wanted> $two
                        - QAST::Var(local __lowered_lex_3) <wanted> $three
                      - QAST::Op(callstatic &infix:<*>) <wanted> *
                        - QAST::Var(local __lowered_lex_2) <wanted> $four
                        - QAST::Var(local __lowered_lex_1) <wanted> $five
            - QAST::WVal(Nil)
```


As you can see by examining the nested tree, the calculations are done as follows (expressed using a postfix notation)


#### Output:
```
one two + three * four five * -
```
