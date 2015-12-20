[1]: http://rosettacode.org/wiki/Native_shebang

# [Native shebang][1]

Perl 6 is not installed by default on most systems and does not have a default install directory, so the path will vary by system.



*File: echo.p6*

```perl6
#!/path/to/perl6
put @*ARGS;
```


*Usage:*


#### Output:
```
./echo.p6 Hello, world!
```

#### Output:
```
Hello, world!
```