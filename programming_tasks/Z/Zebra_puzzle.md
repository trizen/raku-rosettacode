[1]: https://rosettacode.org/wiki/Zebra_puzzle

# [Zebra puzzle][1]

A rule driven approach:

```perl
my Hash @houses = (1 .. 5).map: { %(:num($_)) }; # 1 there are five houses
 
my @facts = (
    { :nat<English>, :color<red> },      # 2 The English man lives in the red house.
    { :nat<Swede>, :pet<dog> },          # 3 The Swede has a dog.
    { :nat<Dane>, :drink<tea> },         # 4 The Dane drinks tea.
    { :color<green>, :Left-Of(:color<white>) }, # 5 the green house is immediately to the left of the white house
    { :drink<coffee>, :color<green> },   # 6 They drink coffee in the green house.
    { :smoke<Pall-Mall>, :pet<birds> },  # 7 The man who smokes Pall Mall has birds.
    { :color<yellow>, :smoke<Dunhill> }, # 8 In the yellow house they smoke Dunhill.
    { :num(3), :drink<milk> },           # 9 In the middle house they drink milk.
    { :num(1), :nat<Norwegian> },        # 10 The Norwegian lives in the first house.
    { :smoke<Blend>, :Next-To(:pet<cats>) }, # 11 The man who smokes Blend lives in the house next to the house with cats.
    { :pet<horse>, :Next-To(:smoke<Dunhill>) }, # 12 In a house next to the house where they have a horse, they smoke Dunhill.
    { :smoke<Blue-Master>, :drink<beer> }, # 13 The man who smokes Blue Master drinks beer.
    { :nat<German>, :smoke<Prince> },      # 14 The German smokes Prince.
    { :nat<Norwegian>, :Next-To(:color<blue>) }, # 15 The Norwegian lives next to the blue house.
    { :drink<water>, :Next-To(:smoke<Blend>) },  # 16 They drink water in a house next to the house where they smoke Blend.
    { :pet<zebra> }, # who owns this?
);
 
sub MAIN {
    for gather solve(@houses, @facts) {
        #-- output
        say .[0].pairs.sort.map(*.key.uc.fmt("%-9s")).join(' | ');
        say .pairs.sort.map(*.value.fmt("%-9s")).join(' | ')
            for .list;
        last; # stop after first solution
    }
}
 
#| found a solution that fits all the facts
multi sub solve(@houses, @facts [ ]) {
    take @houses;
}
 
#| process the next fact
multi sub solve(@houses, @facts is copy) {
    my $fact = @facts.shift;
    for gather consider(@houses, |$fact) {
        solve(@$_, @facts)
    }
}
 
#| find all possible solutions for pairs of houses with
#| %a attributes, left of a house  with %b attributes
multi sub consider(@houses, :Left-Of(%b)!, *%a) {
    my @scenarios;
    for @houses {
        my $idx = .<num> - 1;
        if $idx > 0 && plausible(@houses[$idx-1], %a) && plausible(@houses[$idx], %b) {
            my @scenario = @houses.clone;
            @scenario[$idx-1] = %( %(@houses[$idx-1]), %a );
            @scenario[$idx] = %( %(@houses[$idx]), %b );
            take @scenario;
        }
    }
}
 
#| find all possible pairs of houses with %a attributes, either side
#! of a house  with %b attributes
multi sub consider(@houses, :Next-To(%b)!, *%a ) {
    consider(@houses, |%a, :Left-Of(%b) );
    consider(@houses, |%b, :Left-Of(%a) );
}
 
#| find all possible houses that match the given attributes
multi sub consider(@houses, *%fact) {
    for @houses.grep({plausible($_, %fact)}) -> $house {
        my @scenario = @houses.clone;
        my $idx = $house<num> - 1;
        @scenario[$idx] = %( %$house, %fact );
        take @scenario;
    }
}
 
#| plausible if doesn't conflict with anything
sub plausible(%house, %atts) {
    all %atts.keys.map: { (%house{$_}:!exists) || %house{$_} eq %atts{$_} };
}
 
```

#### Output:
```
COLOR     | DRINK     | NAT       | NUM       | PET       | SMOKE    
yellow    | water     | Norwegian | 1         | cats      | Dunhill  
blue      | tea       | Dane      | 2         | horse     | Blend    
red       | milk      | English   | 3         | birds     | Pall-Mall
green     | coffee    | German    | 4         | zebra     | Prince   
white     | beer      | Swede     | 5         | dog       | Blue-Master
```


Note: Facts can be shuffled by changing line 3 to `my @facts = pick *, (`. It seems to reliably find solutions, although execution times will vary (from under 1 sec up to about 10sec).