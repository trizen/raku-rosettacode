[1]: https://rosettacode.org/wiki/Native_shebang

# [Native shebang][1]





Raku is not installed by default on most systems and does not have a default install directory, so the path will vary by system.



**File: echo.p6**

```perl
#!/path/to/raku
put @*ARGS;
```


**Usage:**


```
./echo.p6 Hello, world!
```

```
Hello, world!
```
