[1]: https://rosettacode.org/wiki/Use_another_language_to_call_a_function

# [Use another language to call a function][1]

This [SO answer](https://stackoverflow.com/a/50771971/3386748), by JJ Merelo, explained the difficulty on providing a simple solution.  So this attempt takes the same approach as PicoLisp by summoning the interpreter at run time.



query.p6

```raku
#!/usr/bin/env perl6
 
sub MAIN (Int :l(:len(:$length))) {
   my Str $String = "Here am I";
   $*OUT.print: $String if $String.codes ≤ $length
}
```


query.c

```c
#include<stdio.h>
#include<stddef.h>
#include<string.h>
 
int Query(char *Data, size_t *Length) {
   FILE *fp;
   char buf[64];
 
   sprintf(buf, "/home/user/query.p6 --len=%zu", *Length);
   if (!(fp = popen(buf, "r")))
      return 0;
   fgets(Data, *Length, fp);
   *Length = strlen(Data);
   return pclose(fp) >= 0 && *Length != 0;
}
```

#### Output:
```
gcc -Wall -o main main.c query.c
./main
Here am I
```
