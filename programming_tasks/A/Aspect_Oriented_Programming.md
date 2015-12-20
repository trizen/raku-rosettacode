[1]: http://rosettacode.org/wiki/Aspect_Oriented_Programming

# [Aspect Oriented Programming][1]

All methods in Perl 6 are really just routines under the control of the meta-object protocol for purposes of dispatch, so there are several ways to subvert the system in order to deal with aspects. First, you could intercept the MOP calls, or you could install alternate metaclasses. More likely, since methods are really just routines in disguise, and since a routine can be wrapped (in place) in an outer routine via the `.wrap` method, you'd just wrap all the methods and other routines that share a cross-cutting concern.



Note that the optimizer is free to assume routines are immutable after CHECK time (the end of compilation), so while you can always wrap routines during compile time, you might need a "`use soft`" declaration or some such to enable wrapping at run time, that is, in order to keep considering the routine to be mutable. (Only code doing the Routine role is ever mutable in Perl 6—bare blocks and other lambdas are considered immutable from the start.) Note that keeping routines mutable is likely to pessimize spots where the optimizer might otherwise be able to inline the code.



Of course, you can also simply redefine Perl 6 on the fly to be any language you want in the current lexical scope, so you are really only limited by your imagination in how you wish to express these "aspects", whatever you might mean by that term…