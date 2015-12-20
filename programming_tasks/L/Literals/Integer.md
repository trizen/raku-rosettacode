[1]: http://rosettacode.org/wiki/Literals/Integer

# [Literals/Integer][1]

These all print 255.

```perl6
say 255;
say 0d255;
say 0xff;
say 0o377;
say 0b1111_1111;
 
say :10<255>;
say :16<ff>;
say :8<377>;
say :2<1111_1111>;
say :3<100110>;
say :4<3333>;
say :12<193>;
say :36<73>;
```


There is a specced form for bases above 36, but rakudo does not yet implement it.