[1]: http://rosettacode.org/wiki/Rock-paper-scissors

# [Rock-paper-scissors][1]

This is slightly more complicated than it could be; it is a general case framework with input filtering. It weights the computers choices based on your previous choices. Type in at least the first two characters of your choice, or just hit enter to quit. Customize it by supplying your own <tt>%vs</tt> options/outcomes.



Here is standard Rock-Paper-Scissors.

```perl6
my %vs = (
    options => [<Rock Paper Scissors>],
    ro => {
        ro => [ 2, ''                        ],
        pa => [ 1, 'Paper covers Rock: '     ],
        sc => [ 0, 'Rock smashes Scissors: ' ]
    },
    pa => {
        ro => [ 0, 'Paper covers Rock: '    ],
        pa => [ 2, ''                       ],
        sc => [ 1, 'Scissors cut Paper: '   ]
    },
    sc => {
        ro => [ 1, 'Rock smashes Scissors: '],
        pa => [ 0, 'Scissors cut Paper: '   ],
        sc => [ 2, ''                       ]
    }
);
 
my %choices = %vs<options>.map({; $_.substr(0,2).lc => $_ });
my $keys    = %choices.keys.join('|');
my $prompt  = %vs<options>.map({$_.subst(/(\w\w)/,->$/{"[$0]"})}).join(' ')~"? ";
my %weight  = %choices.keys »=>» 1;
 
my @stats = 0,0,0;
my $round;
 
while my $player = (prompt "Round {++$round}: " ~ $prompt).lc {
    $player.=substr(0,2);
    say 'Invalid choice, try again.' and $round-- and next
      unless $player.chars == 2 and $player ~~ /<$keys>/;
    my $computer = %weight.keys.map( { $_ xx %weight{$_} } ).pick;
    %weight{$_.key}++ for %vs{$player}.grep( { $_.value[0] == 1 } );
    my $result = %vs{$player}{$computer}[0];
    @stats[$result]++;
    say "You chose %choices{$player},  Computer chose %choices{$computer}.";
    print %vs{$player}{$computer}[1];
    print ( 'You win!', 'You Lose!','Tie.' )[$result];
    say " - (W:{@stats[0]} L:{@stats[1]} T:{@stats[2]})\n",
};
```

#### Output:
```
Round 1: [Ro]ck [Pa]per [Sc]issors? ro
You chose Rock,  Computer chose Paper.
Paper covers Rock: You Lose! - (W:0 L:1 T:0)

Round 2: [Ro]ck [Pa]per [Sc]issors? pa
You chose Paper,  Computer chose Scissors.
Scissors cut Paper: You Lose! - (W:0 L:2 T:0)

Round 3: [Ro]ck [Pa]per [Sc]issors? pa
You chose Paper,  Computer chose Scissors.
Scissors cut Paper: You Lose! - (W:0 L:3 T:0)

Round 4: [Ro]ck [Pa]per [Sc]issors? ro
You chose Rock,  Computer chose Rock.
Tie. - (W:0 L:3 T:1)

Round 5: [Ro]ck [Pa]per [Sc]issors? sc
You chose Scissors,  Computer chose Scissors.
Tie. - (W:0 L:3 T:2)
...
```


Here is example output from the same code only with a different&#160;%vs data structure implementing [Rock-Paper-Scissors-Lizard-Spock](http://en.wikipedia.org/wiki/Rock-paper-scissors-lizard-Spock).

```perl6
my %vs = (
    options => [<Rock Paper Scissors Lizard Spock>],
    ro => {
        ro => [ 2, ''                            ],
        pa => [ 1, 'Paper covers Rock: '         ],
        sc => [ 0, 'Rock smashes Scissors: '     ],
        li => [ 0, 'Rock crushes Lizard: '       ],
        sp => [ 1, 'Spock vaporizes Rock: '      ]
    },
    pa => {
        ro => [ 0, 'Paper covers Rock: '         ],
        pa => [ 2, ''                            ],
        sc => [ 1, 'Scissors cut Paper: '        ],
        li => [ 1, 'Lizard eats Paper: '         ],
        sp => [ 0, 'Paper disproves Spock: '     ]
    },
    sc => {
        ro => [ 1, 'Rock smashes Scissors: '     ],
        pa => [ 0, 'Scissors cut Paper: '        ],
        sc => [ 2, ''                            ],
        li => [ 0, 'Scissors decapitate Lizard: '],
        sp => [ 1, 'Spock smashes Scissors: '    ]
    },
    li => {
        ro => [ 1, 'Rock crushes Lizard: '       ],
        pa => [ 0, 'Lizard eats Paper: '         ],
        sc => [ 1, 'Scissors decapitate Lizard: '],
        li => [ 2, ''                            ],
        sp => [ 0, 'Lizard poisons Spock: '      ]
    },
    sp => {
        ro => [ 0, 'Spock vaporizes Rock: '      ],
        pa => [ 1, 'Paper disproves Spock: '     ],
        sc => [ 0, 'Spock smashes Scissors: '    ],
        li => [ 1, 'Lizard poisons Spock: '      ],
        sp => [ 2, ''                            ]
    }
);
```

#### Output:
```
Round 1: [Ro]ck [Pa]per [Sc]issors [Li]zard [Sp]ock? li
You chose Lizard,  Computer chose Scissors.
Scissors decapitate Lizard: You Lose! - (W:0 L:1 T:0)

Round 2: [Ro]ck [Pa]per [Sc]issors [Li]zard [Sp]ock? sp
You chose Spock,  Computer chose Paper.
Paper disproves Spock: You Lose! - (W:0 L:2 T:0)

Round 3: [Ro]ck [Pa]per [Sc]issors [Li]zard [Sp]ock? ro
You chose Rock,  Computer chose Lizard.
Rock crushes Lizard: You Win! - (W:1 L:2 T:0)

Round 4: [Ro]ck [Pa]per [Sc]issors [Li]zard [Sp]ock? ro
You chose Rock,  Computer chose Scissors.
Rock smashes Scissors: You win! - (W:2 L:2 T:0)

Round 5: [Ro]ck [Pa]per [Sc]issors [Li]zard [Sp]ock? li
You chose Lizard,  Computer chose Paper.
Lizard eats Paper: You win! - (W:3 L:2 T:0)
...
```