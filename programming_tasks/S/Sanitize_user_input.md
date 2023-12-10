[1]: https://rosettacode.org/wiki/Sanitize_user_input

# [Sanitize user input][1]

It would be helpful if the task author would be a little more specific about what he is after. How user inputs must be "sanitized" entirely depends on how the data is going to be used.



For internal usage, in Raku, where you are simply storing data into an internal data structure, it is pretty much a non issue. Variables in Raku aren't executed without specific instructions to do so. Full stop.



Your name is a string of 2.6 million null bytes? Ok. Good luck typing that in.



You're called 'rm -rf /'? Wow. sucks to be you.



Now, it may be a good idea to check for a maximum permitted length; (2.6e6 null bytes) but Raku would handle it with no problem.



The problem mostly comes in when you need to interchange data with some 3rd party library / system; but every different system is going to have it's own quirks and caveats. There is no "one size fits all" solution.



In general, when it comes to sanitizing user input, the best way to go about it is: **don't**. It's a losing game.



Instead either <u>validate</u> input to make sure it follows a certain format, <u>whitelist</u> input so only a know few commands are permitted, or if those aren't possible, <u>use 3rd party tools</u> the 3rd party system provides to make arbitrary input "safe" to run. Which one of these is used depends on what system you need to interact with.



For the case given, (Bobby Tables), where you are presumably putting names into some 3rd party data storage (nominally a database of some kind), you would use bound parameters to automatically "make safe" any user input. See [the Raku entry under the Parametrized SQL statement task](https://rosettacode.org/wiki/Parametrized_SQL_statement#Raku).



Validating is making sure the the input matches some predetermined format, usually with some sort of regular expression. For names, you probably want to allow a fixed maximum (and minimum!) number of: any word or digit character, space and period characters and possibly some small selection of non-word characters. It is a careful balance between too restrictive and too permissive. You need to avoid falling into pre-conceived assumptions about:
[names](https://www.kalzumeus.com/2010/06/17/falsehoods-programmers-believe-about-names/), [time](https://infiniteundo.com/post/25326999628/falsehoods-programmers-believe-about-time), [gender](https://medium.com/gender-2-0/falsehoods-programmers-believe-about-gender-f9a3512b4c9c), [addresses](https://www.mjt.me.uk/posts/falsehoods-programmers-believe-about-addresses/), [phone numbers](https://github.com/google/libphonenumber/blob/master/FALSEHOODS.md)... the list goes on.



When passing a user command to the operating system, you probably want to use whitelisting. Only a very few commands from a predetermined list are allowed to be used.


```
   if $command ∈ <ls time cd df> then { execute $command }
```


or some such. What the whitelist contains and how to determine if the input matches is a whole article in itself.



Unfortunately, this is very vague and hand-wavey due to the vagueness of the task description. Really, any language could copy/paste 95% or better of the above, change the language name, and be done with it. But until the task description is made a little more focused, it will have to do.
