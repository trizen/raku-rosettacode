[1]: https://rosettacode.org/wiki/Call_an_object_method

# [Call an object method][1]





### Basic method calls

```perl
class Thing { 
  method regular-example() { say 'I haz a method' }

  multi method multi-example() { say 'No arguments given' }
  multi method multi-example(Str $foo) { say 'String given' }
  multi method multi-example(Int $foo) { say 'Integer given' }
};

# 'new' is actually a method, not a special keyword:
my $thing = Thing.new;

# No arguments: parentheses are optional
$thing.regular-example;
$thing.regular-example();
$thing.multi-example;
$thing.multi-example();

# Arguments: parentheses or colon required
$thing.multi-example("This is a string");
$thing.multi-example: "This is a string";
$thing.multi-example(42);
$thing.multi-example: 42;

# Indirect (reverse order) method call syntax: colon required
my $foo = new Thing: ;
multi-example $thing: 42;
```


### Meta-operators



The `.` operator can be decorated with meta-operators.

```perl
my @array = <a z c d y>;
@array .= sort;  # short for @array = @array.sort;

say @array».uc;  # uppercase all the strings: A C D Y Z
```


### Classless methods



A method that is not in a class can be called by using the `&` sigil explicitly.

```perl
my $object = "a string";  # Everything is an object.
my method example-method {
    return "This is { self }.";
}

say $object.&example-method;  # Outputs "This is a string."
```
