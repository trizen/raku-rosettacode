[1]: https://rosettacode.org/wiki/Write_entire_file

# [Write entire file][1]

```raku
spurt 'path/to/file', $file-data;
```


or

```raku
'path/to/file'.IO.spurt: $file-data;
```