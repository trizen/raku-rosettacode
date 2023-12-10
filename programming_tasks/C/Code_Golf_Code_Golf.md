[1]: https://rosettacode.org/wiki/Code_Golf:_Code_Golf

# [Code Golf: Code Golf][1]

Not very interesting, as it's pretty much just standard, non-obscure Raku. The output string is so short, there isn't any easy way to golf it shorter than just printing it directly. 17 bytes.

```perl
print <Code Golf>
```

#### Output:
```
Code Golf
```


Assuming we can't use the string literal in the source, the shortest I've come up with is:

```perl
print chrs 37,9,2,3,70,33,9,10,0 X+^102 # 39 chars, 39 bytes
```


[Try it online!](https://tio.run/##K0gtyjH7/7@gKDOvRCE5o6hYwdhcx1LHSMdYx9xAx9gYyDY00DFQiNCOMzQw@v8fAA)

```perl
print chrs -3,㊶,㉚,㉛,-㊳,1,㊶,㊳,㉜X+㉎ # 33 Chars, 49 bytes
```
```perl
print <Dpef!Hpmg>.ords».pred.chrs # 33 Chars, 34 bytes. Somewhat cheaty as it _does_ contain a string literal, but not the same literal as the output
```


Same output for each. Of course, to actually run any of that code you need the Raku compiler at 18.0Kb, the nqp vm interpreter at 17.9 Kb and the moar virtual machine at 17.9Kb. (Or the Java virtual machine, which is remarkably difficult to come up with a size for...)
