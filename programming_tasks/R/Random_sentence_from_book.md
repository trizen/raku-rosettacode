[1]: https://rosettacode.org/wiki/Random_sentence_from_book

# [Random sentence from book][1]

Started out as translation of [Perl](#Perl), but diverged.

```perl
my $text = '36-0.txt'.IO.slurp.subst(/.+ '*** START OF THIS' .+? \n (.*?) 'End of the Project Gutenberg EBook' .*/, {$0} );

$text.=subst(/ <+punct-[.!?\’,]> /, ' ', :g);
$text.=subst(/ (\s) '’' (\s)  /, '', :g);
$text.=subst(/ (\w) '’' (\s)  /, {$0~$1}, :g);
$text.=subst(/ (\s) '’' (\w)  /, {$0~$1}, :g);

my (%one, %two);

for $text.comb(/[\w+(\’\w+)?]','?|<punct>/).rotor(3 => -2) {
    %two{.[0]}{.[1]}{.[2]}++;
    %one{.[0]}{.[1]}++;
}

sub weightedpick (%hash) { %hash.keys.map( { $_ xx %hash{$_} } ).pick }

sub sentence {
    my @sentence = <. ! ?>.roll;
    @sentence.push: weightedpick( %one{ @sentence[0] } );
    @sentence.push: weightedpick( %two{ @sentence[*-2] }{ @sentence[*-1] } // %('.' => 1) )[0]
      until @sentence[*-1] ∈ <. ! ?>;
    @sentence.=squish;
    shift @sentence;
    redo if @sentence < 7;
    @sentence.join(' ').tc.subst(/\s(<:punct>)/, {$0}, :g);
}

say sentence() ~ "\n" for ^10;
```

#### Output:
```
To the inhabitants calling itself the Committee of Public Supply seized the opportunity of slightly shifting my position, which had caused a silent mass of smoke rose slanting and barred the face.

Why was I after the Martian within the case, but that these monsters.

As if hell was built for rabbits!

Thenks and all that the Secret of Flying, was discovered.

Or did a Martian standing sentinel I suppose the time we drew near the railway officials connected the breakdown with the butt.

Flutter, flutter, went the bats, heeding it not been for the big table like end of it a great light was seen by the humblest things that God, in his jaws coming headlong towards me, and rapidly growing hotter.

Survivors there were no longer venturing into the side roads of the planet in view.

Just as they began playing at touch in and out into the west, but nothing to me the landscape, weird and vague and strange and incomprehensible that for the wet.

Just as they seem to remember talking, wanderingly, to myself for an accident, but the captain lay off the platforms, and my wife to their former stare, and his lower limbs lay limp and dead horses.

Entrails they had fought across to the post office a little one roomed squatter’s hut of wood, surrounded by a gradual development of brain and hands, the latter giving rise to the corner.
```
