[1]: https://rosettacode.org/wiki/Permutations_with_some_identical_elements

# [Permutations with some identical elements][1]

```raku
sub permutations-with-some-identical-elements ( @elements, @reps = () ) {
    with @elements { (@reps ?? flat $_ Zxx @reps !! flat .keys.map(*+1) Zxx .values).permutations».join.unique }
 }
 
for (<2 1>,), (<2 3 1>,), (<A B C>, <2 3 1>), (<🦋 ⚽ 🙄>, <2 2 1>) {
    put permutations-with-some-identical-elements |$_;
    say '';
}
```

#### Output:
```
112 121 211

112223 112232 112322 113222 121223 121232 121322 122123 122132 122213 122231 122312 122321 123122 123212 123221 131222 132122 132212 132221 211223 211232 211322 212123 212132 212213 212231 212312 212321 213122 213212 213221 221123 221132 221213 221231 221312 221321 222113 222131 222311 223112 223121 223211 231122 231212 231221 232112 232121 232211 311222 312122 312212 312221 321122 321212 321221 322112 322121 322211

AABBBC AABBCB AABCBB AACBBB ABABBC ABABCB ABACBB ABBABC ABBACB ABBBAC ABBBCA ABBCAB ABBCBA ABCABB ABCBAB ABCBBA ACABBB ACBABB ACBBAB ACBBBA BAABBC BAABCB BAACBB BABABC BABACB BABBAC BABBCA BABCAB BABCBA BACABB BACBAB BACBBA BBAABC BBAACB BBABAC BBABCA BBACAB BBACBA BBBAAC BBBACA BBBCAA BBCAAB BBCABA BBCBAA BCAABB BCABAB BCABBA BCBAAB BCBABA BCBBAA CAABBB CABABB CABBAB CABBBA CBAABB CBABAB CBABBA CBBAAB CBBABA CBBBAA

🦋🦋⚽⚽🙄 🦋🦋⚽🙄⚽ 🦋🦋🙄⚽⚽ 🦋⚽🦋⚽🙄 🦋⚽🦋🙄⚽ 🦋⚽⚽🦋🙄 🦋⚽⚽🙄🦋 🦋⚽🙄🦋⚽ 🦋⚽🙄⚽🦋 🦋🙄🦋⚽⚽ 🦋🙄⚽🦋⚽ 🦋🙄⚽⚽🦋 ⚽🦋🦋⚽🙄 ⚽🦋🦋🙄⚽ ⚽🦋⚽🦋🙄 ⚽🦋⚽🙄🦋 ⚽🦋🙄🦋⚽ ⚽🦋🙄⚽🦋 ⚽⚽🦋🦋🙄 ⚽⚽🦋🙄🦋 ⚽⚽🙄🦋🦋 ⚽🙄🦋🦋⚽ ⚽🙄🦋⚽🦋 ⚽🙄⚽🦋🦋 🙄🦋🦋⚽⚽ 🙄🦋⚽🦋⚽ 🙄🦋⚽⚽🦋 🙄⚽🦋🦋⚽ 🙄⚽🦋⚽🦋 🙄⚽⚽🦋🦋
```
