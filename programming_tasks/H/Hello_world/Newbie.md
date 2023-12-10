[1]: https://rosettacode.org/wiki/Hello_world/Newbie

# [Hello world/Newbie][1]





Raku is a language spec and test suite, not an implementation. Any implementation which implements the spec and passes the test suite can call itself Raku. Philosophically, there is no official "Raku distribution". Practically, at this point (early 2020), Rakudo Raku is the only realistic option. There are others, but they are either not currently being developed or far behind Rakudo in functionality.



There are several ways to get and install Rakudo Raku depending on your operating system and the balance between your technical familiarity and tolerance of outdated code. Rakudo Raku is still in heavy development and the code base still has features being added, refined and optimized on a daily basis; a few weeks can make a big difference.



For ease of installation, there are MSI files for Windows, Homebrew packages for OSX and pre-built Linux packages for apt and yum based packaging systems. They will get you a working compiler most easily, but may be weeks or months behind a blead source build.



All of these options are detailed in the [Downloads section of the Rakuko.org website](https://rakudo.org/downloads).



If you do run into problems installing Rakudo, (or any Raku compiler, or have any Raku questions,) the #raku IRC channel on freenode.net nearly always has people willing to help out.



Once you have your compiler installed, open a terminal window and type:


```
 raku -e'say "Hello World!"'
 
```


or, under Windows cmd.exe,


```
 raku -e"say 'Hello World!'"
```


and press enter.



Alternately, type


```
 echo say 'Hello World!' > hw.raku
```


then type


```
 raku hw.raku
```


to execute it.



Note that the file name may have any extension or none at all.
