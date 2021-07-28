[1]: https://rosettacode.org/wiki/Longest_palindromic_substrings

# [Longest palindromic substrings][1]

This version regularizes (ignores) case and ignores non alphanumeric characters. It is only concerned with finding the *longest* palindromic substrings so does not exhaustively find *all possible* palindromes. If a palindromic substring is found to be part of a longer palindrome, it is not captured separately. Showing the longest 5 palindromic substring groups. Run it with no parameters to operate on the default; pass in a file name to run it against that instead.

```perl
my @chars = ( @*ARGS[0] ?? @*ARGS[0].IO.slurp !! q:to/BOB/ ) .lc.comb: /\w/;
    Lyrics to "Bob" copyright Weird Al Yankovic
    https://www.youtube.com/watch?v=JUQDzj6R3p4
 
    I, man, am regal - a German am I
    Never odd or even
    If I had a hi-fi
    Madam, I'm Adam
    Too hot to hoot
    No lemons, no melon
    Too bad I hid a boot
    Lisa Bonet ate no basil
    Warsaw was raw
    Was it a car or a cat I saw?
 
    Rise to vote, sir
    Do geese see God?
    "Do nine men interpret?" "Nine men," I nod
    Rats live on no evil star
    Won't lovers revolt now?
    Race fast, safe car
    Pa's a sap
    Ma is as selfless as I am
    May a moody baby doom a yam?
 
    Ah, Satan sees Natasha
    No devil lived on
    Lonely Tylenol
    Not a banana baton
    No "x" in "Nixon"
    O, stone, be not so
    O Geronimo, no minor ego
    "Naomi," I moan
    "A Toyota's a Toyota"
    A dog, a panic in a pagoda
 
    Oh no! Don Ho!
    Nurse, I spy gypsies - run!
    Senile felines
    Now I see bees I won
    UFO tofu
    We panic in a pew
    Oozy rat in a sanitary zoo
    God! A red nugget! A fat egg under a dog!
    Go hang a salami, I'm a lasagna hog!
    BOB
#"
 
my @cpfoa = flat
(1 ..^ @chars).race(:1000batch).map: -> \idx {
    my @s;
    for 1, 2 {
       my int ($rev, $fwd) = $_, 1;
       loop {
            quietly last if ($rev > idx) || (@chars[idx - $rev] ne @chars[idx + $fwd]);
            $rev = $rev + 1;
            $fwd = $fwd + 1;
        }
        @s.push: @chars[idx - $rev ^..^ idx + $fwd].join if $rev + $fwd > 2;
        last if @chars[idx - 1] ne @chars[idx];
    }
    next unless +@s;
    @s
}
 
"{.key} ({+.value})\t{.value.unique.sort}".put for @cpfoa.classify( *.chars ).sort( -*.key ).head(5);
```


Returns the length, (the count) and the list:


#### Output:
```
29 (2)  doninemeninterpretninemeninod godarednuggetafateggunderadog
26 (1)  gohangasalamiimalasagnahog
23 (1)  arwontloversrevoltnowra
21 (4)  imanamregalagermanami mayamoodybabydoomayam ootnolemonsnomelontoo oozyratinasanitaryzoo
20 (1)  ratsliveonnoevilstar
```


This isn't intensively optimised but isn't too shabby either. When run against the first million digits of pi: [1000000 digits of pi text file](https://github.com/thundergnat/rc/blob/master/resouces/pi.txt) (Pass in the file path/name at the command line) we get:


#### Output:
```
13 (1)  9475082805749
12 (1)  450197791054
11 (8)  04778787740 09577577590 21348884312 28112721182 41428782414 49612121694 53850405835 84995859948
10 (9)  0045445400 0136776310 1112552111 3517997153 5783993875 6282662826 7046006407 7264994627 8890770988
9 (98)  019161910 020141020 023181320 036646630 037101730 037585730 065363560 068363860 087191780 091747190 100353001 104848401 111262111 131838131 132161231 156393651 160929061 166717661 182232281 193131391 193505391 207060702 211878112 222737222 223404322 242424242 250171052 258232852 267919762 272636272 302474203 313989313 314151413 314424413 318272813 323212323 330626033 332525233 336474633 355575553 357979753 365949563 398989893 407959704 408616804 448767844 450909054 463202364 469797964 479797974 480363084 489696984 490797094 532121235 546000645 549161945 557040755 559555955 563040365 563828365 598292895 621969126 623707326 636414636 636888636 641949146 650272056 662292266 667252766 681565186 684777486 712383217 720565027 726868627 762727267 769646967 777474777 807161708 819686918 833303338 834363438 858838858 866292668 886181688 895505598 896848698 909565909 918888819 926676629 927202729 929373929 944525449 944848449 953252359 972464279 975595579 979202979 992868299
```


in right around 7 seconds on my system.
