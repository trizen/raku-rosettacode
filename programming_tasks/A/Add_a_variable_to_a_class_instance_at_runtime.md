[1]: http://rosettacode.org/wiki/Add_a_variable_to_a_class_instance_at_runtime

# [Add a variable to a class instance at runtime][1]

You can add variables/methods to a class at runtime by composing in a role. The role only affects that instance, though it is inheritable. An object created from an existing object will inherit any roles composed in with values set to those at the time the role was created. If you want to keep changed values in the new object, clone it instead.

```perl6
class Bar { }             # an empty class
 
my $object = Bar.new;     # new instance
 
role a_role {             # role to add a variable: foo,
   has $.foo is rw = 2;   # with an initial value of 2
}
 
$object does a_role;      # compose in the role
 
say $object.foo;          # prints: 2
$object.foo = 5;          # change the variable
say $object.foo;          # prints: 5
 
my $ohno = Bar.new;       # new Bar object
#say $ohno.foo;           # runtime error, base Bar class doesn't have the variable foo
 
my $this = $object.new;   # instantiate a new Bar derived from $object
say $this.foo;            # prints: 2 - original role value
 
my $that = $object.clone; # instantiate a new Bar derived from $object copying any variables
say $that.foo;            # 5 - value from the cloned object
```


That's what's going on underneath, but often people just mix in an anonymous role directly using the <tt>but</tt> operator. Here we'll mix an attribute into a normal integer.

```perl6
my $lue = 42 but role { has $.answer = "Life, the Universe, and Everything" }
 
say $lue;          # 42
say $lue.answer;   # Life, the Universe, and Everything
```


On the other hand, mixins are frowned upon when it is possible to compose roles directly into classes (as with Smalltalk traits), so that you get method collision detection at compile time. If you want to change a class at run time, you can also use monkey patching:

```perl6
use MONKEY-TYPING;
augment class Int {
    method answer { "Life, the Universe, and Everything" }
}
say 42.answer;     # Life, the Universe, and Everything
```


This practice, though allowed, is considered to be Evil Action at a Distance.