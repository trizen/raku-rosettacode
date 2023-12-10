[1]: https://rosettacode.org/wiki/Readline_interface

# [Readline interface][1]





Raku has a built in REPL that can be initiated by running the Raku executable without any parameters. It is fairly basic on its own but here are bindings available in the Raku ecosystem for [GNU Readline](https://tiswww.case.edu/php/chet/readline/rltop.html) and/or [Linenoise](https://github.com/antirez/linenoise) either of which will automatically provide command history, tab completion and more advance command line editing capability if installed. They are not included in the Raku distribution directly. There are incompatible licensing requirements and providing hooks for third party tools allows for more customization options.



Linenoise is generally the preferred option unless you really want the emacs compatible command line editing key bindings. Readline is arguably more powerful but is somewhat fiddly to set up.



If you are not inclined to install Readline *or* Linenoise, the REPL also works fairly well with 3rd party tools like rlwrap.
