[1]: https://rosettacode.org/wiki/Safe_mode

# [Safe mode][1]





*Mostly a cut-n-paste from the [Untrusted environment](https://rosettacode.org/wiki/Untrusted_environment#Raku) task.*



Raku doesn't really provide a high security mode for untrusted environments. By default, Raku is sort of a walled garden. It is difficult to access memory directly, especially in locations not controlled by the Raku interpreter, so unauthorized memory access is unlikely to be a threat with default Raku commands and capabilities.



It is possible (and quite easy) to run Raku with a restricted setting which will disable many IO commands that can be used to access or modify things outside of the Raku interpreter. However, a determined bad actor could theoretically work around the restrictions, especially if the nativecall interface is available. The nativecall interface allows directly calling in to and executing code from C libraries so anything possible in C is now possible in Raku. This is great for all of the power it provides, but along with that comes the responsibility and inherent security risk. The same issue arises with unrestricted loading of modules. If modules can be loaded, especially from arbitrary locations, then any and all restriction imposed by the setting can be worked around.



The restricted setting is modifiable, but by default places restrictions on or completely disables the following things:



Really, if you want to lock down a Raku instance so it is "safe" for unauthenticated, untrusted, general access, you are better off running it in some kind of locked down virtual machine or sandbox managed by the operating system rather than trying to build an ad hoc "safe" environment.
