[1]: https://rosettacode.org/wiki/Singly-linked_list/Element_insertion

# [Singly-linked list/Element insertion][1]

```raku
my $letters = 'A' => 'C' => Mu;
 
sub insert-after($list, $after, $new) {
    loop (my $l = $list; $l; $l = $l.value) {
        if $l.key eqv $after {
            $l.value = $new => $l.value;
            return;
        }
    }
    die "Element $after not found";
}
 
$letters.&insert-after('A', 'B');
```