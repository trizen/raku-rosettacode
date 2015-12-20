[1]: http://rosettacode.org/wiki/Test_integerness

# [Test integerness][1]

In Perl 6, classes that implement the Numeric role have a method called <tt>narrow</tt> which returns an object with the same value but with the most appropriate type. So we can just test the type of _that_ object.

```perl6
for pi, 1e5, 1+0i {
    say "$_ is{" NOT" if .narrow !~~ Int} an integer.";
}
```

#### Output:
```
3.14159265358979 is NOT an integer.
100000 is an integer.
1+0i is an integer.
```