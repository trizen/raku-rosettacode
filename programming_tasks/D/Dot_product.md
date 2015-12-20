[1]: http://rosettacode.org/wiki/Dot_product

# [Dot product][1]

We use the square-bracket meta-operator to turn the infix operator `+` into a reducing list operator, and the guillemet meta-operator to vectorize the infix operator `*`. Length validation is automatic in this form.

```perl
say [+] (1, 3, -5) »*« (4, -2, -1);
```