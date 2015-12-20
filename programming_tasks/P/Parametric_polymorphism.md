[1]: http://rosettacode.org/wiki/Parametric_polymorphism

# [Parametric polymorphism][1]

```perl6
role BinaryTree[::T] {
    has T $.value;
    has BinaryTree[T] $.left;
    has BinaryTree[T] $.right;
 
    method replace-all(T $value) {
        $!value = $value;
        $!left.replace-all($value) if $!left.defined;
        $!right.replace-all($value) if $!right.defined;
    }
}
 
class IntTree does BinaryTree[Int] { }
 
my IntTree $it .= new(value => 1,
                      left  => IntTree.new(value => 2),
                      right => IntTree.new(value => 3));
 
$it.replace-all(42);
say $it.perl;
```

#### Output:
```
IntTree.new(value => 42, left => IntTree.new(value => 42, left => BinaryTree[T], right => BinaryTree[T]), right => IntTree.new(value => 42, left => BinaryTree[T], right => BinaryTree[T]))
```