[1]: http://rosettacode.org/wiki/Set_consolidation

# [Set consolidation][1]

```perl6
multi consolidate() { () }
multi consolidate(Set \this is copy, *@those) {
    gather {
        for consolidate |@those -> \that {
            if this ∩ that { this = this ∪ that }
            else           { take that }
        }
        take this;
    }
}
 
enum Elems <A B C D E F G H I J K>;
say $_, "\n    ==> ", consolidate |$_
    for [set(A,B), set(C,D)],
        [set(A,B), set(B,D)],
        [set(A,B), set(C,D), set(D,B)],
        [set(H,I,K), set(A,B), set(C,D), set(D,B), set(F,G,H)];
```

#### Output:
```
set(A, B) set(C, D)
    ==> set(C, D) set(A, B)
set(A, B) set(B, D)
    ==> set(A, B, D)
set(A, B) set(C, D) set(D, B)
    ==> set(A, B, C, D)
set(H, I, K) set(A, B) set(C, D) set(D, B) set(F, G, H)
    ==> set(A, B, C, D) set(H, I, K, F, G)
```