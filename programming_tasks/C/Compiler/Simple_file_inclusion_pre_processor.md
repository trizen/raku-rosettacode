[1]: https://rosettacode.org/wiki/Compiler/Simple_file_inclusion_pre_processor

# [Compiler/Simple file inclusion pre processor][1]

A file pre-processor is very nearly pointless for Raku. Raku is nominally an interpreted language; in actuality, it does single pass, just-in-time compilation on the fly as it loads the file. For libraries and constants (or semi constant variables) there is a robust built-in library (module) include system for code that can be pre-compiled and used on demand. `use some-library;` to import any objects exported by the module.



That's great for code references and constants, but isn't really intended for text inclusion of external files. Since the text file can not be pre-compiled, there isn't much point to having such a system. (For code at least, there are many different templating system modules for text templates; primarily, though not exclusively used for on-the-fly generation of dynamic HTML / XML documents.) One of those could certainly be bent into doing something like what this task is asking for, but are probably both over- and under-powered for the job.



So in Raku, there isn't a readily available system to do this, there isn't much point, and it's probably a bad idea...





Ah well, that never stopped us before.





A Raku script to do source filtering / preprocessing: save it and call it 'include'

```perl
unit sub MAIN ($file-name);
my $file = slurp $file-name;
put $file.=subst(/[^^|['{{' \s*]] '#include' \s+ (\S+) \s* '}}'?/, {run(«$*EXECUTABLE-NAME $*PROGRAM-NAME $0», :out).out.slurp(:close).trim}, :g);
```


This will find: any line starting with '#include' followed by a (absolute or relative) path to a file, or #include ./path/to/file.name enclosed in double curly brackets anywhere in the file.



It will replace the #include notation by the contents of the file referenced, will follow nested #includes arbitrarily deeply, and echo the processed file to STDOUT.



Let's test it out. Here is a test script and a bunch of include files.



Top level named... whatever, let's call it 'preprocess.raku'

```perl
# Top level test script file for #include Rosettacode
# 'Compiler/Simple file inclusion pre processor' task
 
# some code
 
say .³ for ^10;
 
#include ./include1.file
 
```


include1.file


#### Output:
```
# included #include1 file >1>
# test to ensure it only tries to execute #include of the right format

# more code
say .³³ for ^10;
# and more comments

#include    ./include2.file    # comments ok at end of include line
# <1<
```


include2.file


#### Output:
```
# nested #include2.file >2>
say "Test for an nested include inside a line: {{ #include ./include3.file }}";
# pointless but why not? <2<
```


include3.file


#### Output:
```
>3> Yep, it works! <3<
```


Invoke at a command line:



`raku include preprocess.raku`


#### Output:
```
# Top level test script file for #include Rosettacode
# 'Compiler/Simple file inclusion pre processor' task

# some code

say .³ for ^10;

# included #include1 file >1>
# test to ensure it only tries to execute #include of the right format

# more code
say .³³ for ^10;
# and more comments

# nested #include2.file >2>
say "Test for an nested include inside a line: >3> Yep, it works! <3<";
# pointless but why not? <2<# comments ok at end of include line
# <1<
```


You can either redirect that into a file, or just pass it back into the compiler to execute it:



`raku include preprocess.raku | raku`


#### Output:
```
0
1
8
27
64
125
216
343
512
729
0
1
8589934592
5559060566555523
73786976294838206464
116415321826934814453125
47751966659678405306351616
7730993719707444524137094407
633825300114114700748351602688
30903154382632612361920641803529
Test for an nested include inside a line: >3> Yep, it works! <3<
```


Note that this is not very robust, (it's 3 lines of code, what do you expect?) but it satisfies the task requirements as far as I can tell.
