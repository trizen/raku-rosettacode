[1]: https://rosettacode.org/wiki/Names_to_numbers

# [Names to numbers][1]

```perl
my $DEBUG = False;
 
constant @M = ('ones', 'thousand',
    ((<m b tr quadr quint sext sept oct non>,
        (map { ('', <un duo tre quattuor quin sex septen octo novem>).flat X~ $_ },
	    <dec vigint trigint quadragint quinquagint sexagint septuagint octogint nonagint>),
	        'cent').flat X~ 'illion')).flat;
 
constant %M = @M Z=> (1, 1000, 1000000 ... *);
 
grammar Number-Names {
    token TOP { ^ [.*? <number>]* .* $ }
 
    method returns {
	say callframe(1).subname, ' -> ', self.ast if $DEBUG;
	self;
    }
 
    method trimspace {
	my $pos = self.pos;
	--$pos while $pos and self.orig.substr($pos-1,1) ~~ /\s/;
	self.cursor($pos);
    }
 
    token gotsome($oldself) {
	<?{ self.pos != $oldself.pos }>
    }
 
    token number { :i «
	:my $*SOFAR = 0;
	[
	| 'zero' { make 0 }
	| ('minus' \s+ | 'negative' \s+)?
	    <group>+ % [','\s+|'and'\s+|<?after \s>] <quantnoun>
	    { make [+]($<group>[]».ast) * ($0 ?? -1 !! 1) * $<quantnoun>.ast }
	]
	<.gotsome(self)>
	<.trimspace>
	<.returns>
    }
 
    token group { :i
	<?{ $*SOFAR %% 1000 }>
	<hundreds> \s* <tweens($*SOFAR + $<hundreds>.ast)> \s* <zillions>
	<.gotsome(self)>
	\s*[ $ || <?after \W> || <?before \W> || <?before < th st nd rd th > » > ]
	{
	    my $group = $<zillions>.ast * ($<hundreds>.ast + $<tweens>.ast);
	    return () if $*SOFAR and $group > $*SOFAR;
	    $*SOFAR += $group;
	    make $group;
	}
	<.returns>
    }
 
    token hundreds { :i
	[
	| <date> <!!{ !$*SOFAR }>     { make $<date>.ast }
	| <ones>  ['-'|\s*] 'hundred' { make $<ones>.ast * 100 }
	| <?{ not $*SOFAR }>
	  <tweeny>['-'|\s*] 'hundred' { make $<tweeny>.ast * 100 }
	| ''                          { make 0 }
	]
	<.returns>
    }
 
    token tweens($c = 0) { :i
	:my $*SOFAR = $c;
	\s*
	[
	| 'ten'                           { make 10 }
	| <teens> <!before \s*'hundred'>  { make $<teens>.ast }
	| <ones> \s+ 'and' \s+ <tens>     { make $<ones>.ast + $<tens>.ast }
	| <scores> \s+ 'and' \s+ <ones>   { make $<scores>.ast + $<ones>.ast }
	| <scores> \s+ 'and' \s+ <tweeny> { make $<scores>.ast + $<tweeny>.ast }
	| <tens>[['-'|\s*['and'\s*]?]<ones>]? { make $<tens>.ast + ($<ones> ?? $<ones>.ast !! 0) }
	| <ones>                          { make $<ones>.ast }
	| <scores>                        { make $<scores>.ast }
	| 'and'\s+ <?{ $c }> <tweens>     { make $<tweens>.ast }
	| (\d+[\.\d+]?) <?{ !$c }>        { make +$0 }
	| ''                              { make 0 }
	]
	\s*
	<.returns>
    }
 
    token tweeny {
	[
	| <ones>[\s+|\-]                  { make $<ones>.ast }
	| 'ten'[\s+|\-]                   { make 10 }
	| <teens>[\s+|\-]                 { make $<teens>.ast }
	| <tens>[[\s*|\-]<ones>]?[\s+|\-] { make $<tens>.ast + ($<ones> ?? $<ones>.ast !! 0) }
	]
    }
 
    token date { :i
	<tweeny>
	[
	| 'ten'                          { make $<tweeny>.ast * 100 + 10 }
	| <teens> <!before \s*'hundred'> { make $<tweeny>.ast * 100 + $<teens>.ast }
	| <tens>[['-'|\s*]<ones>]?       { make $<tweeny>.ast * 100 + $<tens>.ast + ($<ones> ?? $<ones>.ast !! 0) }
	| ['ought'|oh?][\s+|\-]<ones>    { make $<tweeny>.ast * 100 + $<ones>.ast }
	]
    }
 
    token scores { :i
	[
	| ['one'|'a']?['-'|\s*] 'score' { make 20 }
	| 'two'       ['-'|\s*] 'score' { make 40 }
	| 'three'     ['-'|\s*] 'score' { make 60 }
	| 'four'      ['-'|\s*] 'score' { make 80 }
	]
	<.returns>
    }
 
    token teens { :i
	[
	| 'eleven'    { make 11 }
	| 'twelve'    { make 12 }
	| 'twelf'     { make 12 }
	| 'thirteen'  { make 13 }
	| 'fourteen'  { make 14 }
	| 'fifteen'   { make 15 }
	| 'sixteen'   { make 16 }
	| 'seventeen' { make 17 }
	| 'eighteen'  { make 18 }
	| 'nineteen'  { make 19 }
	]
	<.returns>
    }
 
    token tens { :i
        [
        | 'twent'[y|ie]  { make 20 }
        | 'thirt'[y|ie]  { make 30 }
        | 'fort'[y|ie]   { make 40 }
        | 'fift'[y|ie]   { make 50 }
        | 'sixt'[y|ie]   { make 60 }
        | 'sevent'[y|ie] { make 70 }
        | 'eight'[y|ie]  { make 80 }
        | 'ninet'[y|ie]  { make 90 }
        ]
        <.returns>
    }
 
    token ones { :i
	[
	| 'one'              { make 1 }
	| 'fir'<?before st>  { make 1 }
	| 'two'              { make 2 }
	| 'seco'<?before nd> { make 2 }
	| 'three'            { make 3 }
	| 'thi'<?before rd>  { make 3 }
	| 'four'             { make 4 }
	| 'five'             { make 5 }
	| 'fif'<?before th>  { make 5 }
	| 'six'              { make 6 }
	| 'seven'            { make 7 }
	| 'eight'            { make 8 }
	| 'eigh'<?before th> { make 8 }
	| 'nine'             { make 9 }
	| 'nin'<?before th>  { make 9 }
	| <?{ !$*SOFAR }> ['a'\s+]? <?before 'hundred' | 'thousand' | <zillions> <?{ $<zillions>.Str ne '' }> >  { make 1 }
	]
	<.returns>
    }
 
    token zillions { :i
	[
	| (\w+) <?{ %M{lc ~$0} }> { make +%M{lc ~$0} }
	| <quantnoun> { make $<quantnoun>.ast }
	| '' { make 1 }
	]
	<.returns>
    }
 
    token quantnoun { :i
	\s*
	[
	| 'pair's? ' of'? { make 2 }
	| 'dozen'         { make 12 }
	| 'gross' ' of'?  { make 144 }
	| ''              { make 1 }
	]
	<.returns>
    }
}
 
sub commify($_) {
    /\d\d\d\d\d/
	?? .flip.subst(/\d\d\d<?before \d>/, -> $/ {$/~','},:g).flip
	!! $_;
}
 
sub MAIN ($file = 'numnames') {
    $DEBUG = True if $file eq '-';
    my $fh = $file eq '-' ?? $*IN !! open($file);
    for $fh.lines -> $line {
	say $line;
	my $parse = Number-Names.parse($line);
	if $parse and +$parse<number> {
	    print "\t==> ";
	    my $prev = 0;
	    for $parse<number>[*] -> $n {
		print substr($line, $prev, $n.from - $prev);
		print commify(~$n.ast);
		$prev = $n.to;
	    }
	    say substr($line, $prev);
	}
    }
}
```

#### Output:
```
Seventy-two dollars
        ==> 72 dollars
Seventy two dollars
        ==> 72 dollars
One Hundred and One Dalmatians
        ==> 101 Dalmatians
A Hundred and One Dalmatians
        ==> 101 Dalmatians
One Hundred One Dalmatians
        ==> 101 Dalmatians
Hundred and One Dalmatians
        ==> 101 Dalmatians
One Thousand and One Nights
        ==> 1001 Nights
Two Thousand and One: A Space Odyssey
        ==> 2001: A Space Odyssey
math one-oh-one
        ==> math 101
Charlemagne died in eight-fourteen.
        ==> Charlemagne died in 814.
ten sixty-six
        ==> 1066
Nineteen Ought Three
        ==> 1903
Nineteen Eighty-Four
        ==> 1984
Twenty Thirteen
        ==> 2013
four billion, two hundred ninety-four million, nine hundred sixty-seven thousand, two hundred ninety five
        ==> 4,294,967,295
Nine quadrillion, seven trillion, one hundred ninety-nine billion, two hundred fifty-four million, seven hundred forty thousand, nine hundred ninety two
        ==> 9,007,199,254,740,992
Nine Hundred Ninety-Nine
        ==> 999
One Thousand One Hundred Eleven
        ==> 1111
Eleven Hundred Eleven
        ==> 1111
Eight Thousand Eight Hundred Eighty-Eight
        ==> 8888
Eighty-Eight Hundred Eighty-Eight
        ==> 8888
Seven Million Seven Hundred Seventy-Seven Thousand Seven Hundred Seventy-Seven
        ==> 7,777,777
Ninety-Nine Trillion Nine Hundred Ninety-Nine Billion Nine Hundred Ninety-Nine Million Nine Hundred Ninety-Nine Thousand Nine Hundred Ninety-Nine
        ==> 99,999,999,999,999
ninety-nine
        ==> 99
three hundred
        ==> 300
three hundred and ten
        ==> 310
one thousand, five hundred and one
        ==> 1501
twelve thousand, six hundred and nine
        ==> 12,609
five hundred and twelve thousand, six hundred and nine
        ==> 512,609
forty-three million, one hundred and twelve thousand, six hundred and nine
        ==> 43,112,609
two billion, one hundred
        ==> 2,000,000,100
zero
        ==> 0
eight
        ==> 8
one hundred
        ==> 100
one hundred twenty three
        ==> 123
one thousand one
        ==> 1001
ninety nine thousand nine hundred ninety nine
        ==> 99,999
one hundred thousand
        ==> 100,000
nine billion one hundred twenty three million four hundred fifty six thousand seven hundred eighty nine
        ==> 9,123,456,789
one hundred eleven billion one hundred eleven
        ==> 111,000,000,111
sing a song of sixpence, a pocket full of rye, four and twenty blackbirds baked in a pie
        ==> sing a song of sixpence, a pocket full of rye, 24 blackbirds baked in a pie
Fourscore and seven years ago, our four fathers...
        ==> 87 years ago, our 4 fathers...
The Ninety and Nine
        ==> The 99
Seven Brides for Seven Brothers
        ==> 7 Brides for 7 Brothers
Cheaper by the Dozen
        ==> Cheaper by the 12
five dozen roses
        ==> 60 roses
a dozen dozen roses
        ==> 144 roses
two gross of eggs
        ==> 288 eggs
two hundred pairs of socks
        ==> 400 socks
The First Noël
        ==> The 1st Noël
second place
        ==> 2nd place
Forty-second Street
        ==> 42nd Street
the twenty-third Psalm
        ==> the 23rd Psalm
The Fourth Estate
        ==> The 4th Estate
The Fifth Essence
        ==> The 5th Essence
The sixth sick shiek's sixth sheep's sick.
        ==> The 6th sick shiek's 6th sheep's sick.
in seventh heaven
        ==> in 7th heaven
The Eighth Wonder of the World!
        ==> The 8th Wonder of the World!
Malher's Ninth Symphony
        ==> Malher's 9th Symphony
a tenth of all he had
        ==> a 10th of all he had
the eleventh hour
        ==> the 11th hour
Twelfth Night
        ==> 12th Night
Friday the Thirteenth
        ==> Friday the 13th
Twentieth-Century Fox
        ==> 20th-Century Fox
one novemseptuagintillion, one octoseptuagintillion, one septenseptuagintillion, one sexseptuagintillion, one quinseptuagintillion, one quattuorseptuagintillion, one treseptuagintillion, one duoseptuagintillion, one unseptuagintillion, one septuagintillion, one novemsexagintillion, one octosexagintillion, one septensexagintillion, one sexsexagintillion, one quinsexagintillion, one quattuorsexagintillion, one tresexagintillion, one duosexagintillion, one unsexagintillion, one sexagintillion, one novemquinquagintillion, one octoquinquagintillion, one septenquinquagintillion, one sexquinquagintillion, one quinquinquagintillion, one quattuorquinquagintillion, one trequinquagintillion, one duoquinquagintillion, one unquinquagintillion, one quinquagintillion, one novemquadragintillion, one octoquadragintillion, one septenquadragintillion, one sexquadragintillion, one quinquadragintillion, one quattuorquadragintillion, one trequadragintillion, one duoquadragintillion, one unquadragintillion, one quadragintillion, one novemtrigintillion, one octotrigintillion, one septentrigintillion, one sextrigintillion, one quintrigintillion, one quattuortrigintillion, one tretrigintillion, one duotrigintillion, one untrigintillion, one trigintillion, one novemvigintillion, one octovigintillion, one septenvigintillion, one sexvigintillion, one quinvigintillion, one quattuorvigintillion, one trevigintillion, one duovigintillion, one unvigintillion, one vigintillion, one novemdecillion, one octodecillion, one septendecillion, one sexdecillion, one quindecillion, one quattuordecillion, one tredecillion, one duodecillion, one undecillion, one decillion, one nonillion, one octillion, one septillion, one sextillion, one quintillion, one quadrillion, one trillion, one billion, one million, one thousand, one
        ==> 1,001,001,001,001,001,001,001,001,001,001,001,001,001,001,001,001,001,001,001,001,001,001,001,001,001,001,001,001,001,001,001,001,001,001,001,001,001,001,001,001,001,001,001,001,001,001,001,001,001,001,001,001,001,001,001,001,001,001,001,001,001,001,001,001,001,001,001,001,001,001,001,001,001,001,001,001,001,001,001,001
```