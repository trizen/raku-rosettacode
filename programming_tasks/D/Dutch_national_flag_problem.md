[1]: http://rosettacode.org/wiki/Dutch_national_flag_problem

# [Dutch national flag problem][1]

Here are five ways to do it, all one liners (apart from the test apparatus).

```text
enum NL <red white blue>;
my @colors;
 
sub how'bout (&this-way) {
    sub show {
        say @colors;
        say "Ordered: ", [<=] @colors;
    }
 
    @colors = NL.roll(20);
    show;
    this-way;
    show;
    say '';
}
 
say "Using functional sort";
how'bout { @colors = sort *.value, @colors }
 
say "Using in-place sort";
how'bout { @colors .= sort: *.value }
 
say "Using a Bag";
how'bout { @colors = red, white, blue Zxx bag(@colors».key)<red white blue> }
 
say "Using the classify method";
how'bout { @colors = (.list for %(@colors.classify: *.value){0,1,2}) }
 
say "Using multiple greps";
how'bout { @colors = (.grep(red), .grep(white), .grep(blue) given @colors) }
```

#### Output:
```
Using functional sort
red red white white red red red red red red red white red white red red red white white white
Ordered: False
red red red red red red red red red red red red red white white white white white white white
Ordered: True

Using in-place sort
red blue white red white blue white blue red white blue blue blue red white white red blue red blue
Ordered: False
red red red red red red white white white white white white blue blue blue blue blue blue blue blue
Ordered: True

Using a Bag
red blue blue blue white red white red white blue blue red red red red blue blue red white blue
Ordered: False
red red red red red red red red white white white white blue blue blue blue blue blue blue blue
Ordered: True

Using the classify method
blue red white blue blue white white red blue red red white red blue white white red blue red white
Ordered: False
red red red red red red red white white white white white white white blue blue blue blue blue blue
Ordered: True

Using multiple greps
red white blue white white red blue white red white red white white white white white red red blue red
Ordered: False
red red red red red red red white white white white white white white white white white blue blue blue
Ordered: True
```