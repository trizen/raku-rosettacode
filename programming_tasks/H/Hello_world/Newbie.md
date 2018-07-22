[1]: https://rosettacode.org/wiki/Hello_world/Newbie

# [Hello world/Newbie][1]

Perl 6 is a language spec and test suite, not an implementation. Any implemention which implements the spec and passes the test suite can call itself perl 6. Philosophically, there is no official "Perl 6 distribution". Practically, at this point (late 2015), Rakudo perl 6 is the only realistic option. There are others, but they are either not currently being developed or far behind Rakudo in functionality.



There are several ways to get and install Rakudo perl 6 depending on your operating system and the balance between your technical familiarity and tolerance of outdated code. Rakudo perl 6 is still in heavy development and the code base still has features being added, refined and optimized on a daily basis; a few weeks can make a big difference.



For ease of installation, there are MSI files for Windows, Homebrew packages for OSX and pre-built Linux packages for apt and yum based packaging systems. They will get you a working compiler most easily, but may be weeks or months out of date.



At least until Rakudo makes it out of beta, it is probably best to build from source.



All of these options are detailed on the Rakuko.org website: [http://rakudo.org/how-to-get-rakudo/ ](http://rakudo.org/how-to-get-rakudo/%7C).



If you do run into problems installing Rakudo, (or any perl 6 compiler, or have any Perl6 questions,) the #perl6 IRC channel on freenode.net nearly always has people willing to help out.



Once you have your compiler installed, open a terminal window and type:


#### Output:
```
 perl6 -e'say "Hello World!"'
 
```


or, under Windows cmd.exe,


#### Output:
```
 perl6 -e"say 'Hello World!'"
```


and press enter.



Alternately, type


#### Output:
```
 echo say 'Hello World!' > hw.p6
```


then type


#### Output:
```
 perl6 hw.p6
```


to execute it.



Note that the file name may have any extension or none at all.