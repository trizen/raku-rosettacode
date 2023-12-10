[1]: https://rosettacode.org/wiki/Rhonda_numbers

# [Rhonda numbers][1]

Find and show the first 15 so as to display the namesake Rhonda number 25662.

```perl
use Prime::Factor;

my @factor-sum;

@factor-sum[1000000] = 42; # Sink a large index to make access thread safe 

sub rhonda ($base) {
    (1..∞).hyper.map: { $_ if $base * (@factor-sum[$_] //= .&prime-factors.sum) == [×] .polymod($base xx *) }
}

for (flat 2..16, 17..36).grep: { !.&is-prime }  -> $b {
    put "\nFirst 15 Rhonda numbers to base $b:";
    my @rhonda = rhonda($b)[^15];
    my $ch = @rhonda[*-1].chars max @rhonda[*-1].base($b).chars;
    put "In base 10: " ~ @rhonda».fmt("%{$ch}s").join: ', ';
    put $b.fmt("In base %2d: ") ~ @rhonda».base($b)».fmt("%{$ch}s").join: ', ';
}
```

#### Output:
```
First 15 Rhonda numbers to base 4:
In base 10:      10206,      11935,      12150,      16031,      45030,      94185,     113022,     114415,     191149,     244713,     259753,     374782,     392121,     503773,     649902
In base  4:    2133132,    2322133,    2331312,    3322133,   22333212,  112333221,  123211332,  123323233,  232222231,  323233221,  333122221, 1123133332, 1133232321, 1322333131, 2132222232

First 15 Rhonda numbers to base 6:
In base 10:     855,    1029,    3813,    5577,    7040,    7304,   15104,   19136,   35350,   36992,   41031,   42009,   60368,   65536,   67821
In base  6:    3543,    4433,   25353,   41453,   52332,   53452,  153532,  224332,  431354,  443132,  513543,  522253, 1143252, 1223224, 1241553

First 15 Rhonda numbers to base 8:
In base 10:   1836,   6318,   6622,  10530,  14500,  14739,  17655,  18550,  25398,  25956,  30562,  39215,  39325,  50875,  51429
In base  8:   3454,  14256,  14736,  24442,  34244,  34623,  42367,  44166,  61466,  62544,  73542, 114457, 114635, 143273, 144345

First 15 Rhonda numbers to base 9:
In base 10:  15540,  21054,  25331,  44360,  44660,  44733,  47652,  50560,  54944,  76857,  77142,  83334,  83694,  96448,  97944
In base  9:  23276,  31783,  37665,  66758,  67232,  67323,  72326,  76317,  83328, 126376, 126733, 136273, 136723, 156264, 158316

First 15 Rhonda numbers to base 10:
In base 10:  1568,  2835,  4752,  5265,  5439,  5664,  5824,  5832,  8526, 12985, 15625, 15698, 19435, 25284, 25662
In base 10:  1568,  2835,  4752,  5265,  5439,  5664,  5824,  5832,  8526, 12985, 15625, 15698, 19435, 25284, 25662

First 15 Rhonda numbers to base 12:
In base 10:   560,   800,  3993,  4425,  4602,  4888,  7315,  8296,  9315, 11849, 12028, 13034, 14828, 15052, 16264
In base 12:   3A8,   568,  2389,  2689,  27B6,  29B4,  4297,  4974,  5483,  6A35,  6B64,  7662,  86B8,  8864,  94B4

First 15 Rhonda numbers to base 14:
In base 10:  11475,  18655,  20565,  29631,  31725,  45387,  58404,  58667,  59950,  63945,  67525,  68904,  91245,  99603, 125543
In base 14:   4279,   6B27,   76CD,   AB27,   B7C1,  1277D,  173DA,  17547,  17BC2,  19437,  1A873,  1B17A,  25377,  28427,  33A75

First 15 Rhonda numbers to base 15:
In base 10:  2392,  2472, 11468, 15873, 17424, 18126, 19152, 20079, 24388, 30758, 31150, 33004, 33550, 37925, 39483
In base 15:   A97,   AEC,  35E8,  4A83,  5269,  5586,  5A1C,  5E39,  735D,  91A8,  936A,  9BA4,  9E1A,  B385,  BA73

First 15 Rhonda numbers to base 16:
In base 10:  1000,  1134,  6776, 15912, 19624, 20043, 20355, 23946, 26296, 29070, 31906, 32292, 34236, 34521, 36465
In base 16:   3E8,   46E,  1A78,  3E28,  4CA8,  4E4B,  4F83,  5D8A,  66B8,  718E,  7CA2,  7E24,  85BC,  86D9,  8E71

First 15 Rhonda numbers to base 18:
In base 10:  1470,  3000,  8918, 17025, 19402, 20650, 21120, 22156, 26522, 36549, 38354, 43281, 46035, 48768, 54229
In base 18:   49C,   94C,  1998,  2G9F,  35FG,  39D4,  3B36,  3E6G,  49F8,  64E9,  6A6E,  77A9,  7G19,  8696,  956D

First 15 Rhonda numbers to base 20:
In base 10:  1815, 11050, 15295, 21165, 22165, 30702, 34510, 34645, 42292, 44165, 52059, 53416, 65945, 78430, 80712
In base 20:   4AF,  17CA,  1I4F,  2CI5,  2F85,  3GF2,  465A,  46C5,  55EC,  5A85,  6A2J,  6DAG,  84H5,  9G1A,  A1FC

First 15 Rhonda numbers to base 21:
In base 10:  1632,  5390,  8512, 12992, 15678, 25038, 29412, 34017, 39552, 48895, 49147, 61376, 85078, 89590, 91798
In base 21:   3EF,   C4E,   J67,  189E,  1EBC,  2EG6,  33EC,  3E2I,  45E9,  55I7,  5697,  6D3E,  93J7,  9E34,  9J37

First 15 Rhonda numbers to base 22:
In base 10:   2695,   4128,   7865,  28800,  31710,  37030,  71875,  74306, 117760, 117895, 121626, 126002, 131427, 175065, 192753
In base 22:    5CB,    8BE,    G5B,   2FB2,   2LB8,   3AB4,   6GB1,   6LBC,   B16G,   B1CJ,   B96A,   BI78,   C7BL,   G9FB,   I25B

First 15 Rhonda numbers to base 24:
In base 10:  2080,  2709,  3976,  5628,  5656,  7144,  8296,  9030, 10094, 17612, 20559, 24616, 26224, 29106, 31458
In base 24:   3EG,   4GL,   6LG,   9IC,   9JG,   C9G,   E9G,   FG6,   HCE,  16DK,  1BGF,  1IHG,  1LCG,  22CI,  26EI

First 15 Rhonda numbers to base 25:
In base 10:   6764,   9633,  13260,  22022,  53382,  57640,  66015,  69006,  97014, 140130, 142880, 144235, 159724, 162565, 165504
In base 25:    AKE,    FA8,    L5A,   1A5M,   3AA7,   3H5F,   45FF,   4AA6,   655E,   8O55,   93F5,   95JA,   A5DO,   AA2F,   AEK4

First 15 Rhonda numbers to base 26:
In base 10:   7788,   9322,   9374,  11160,  22165,  27885,  34905,  44785,  47385,  49257,  62517,  72709,  74217, 108745, 132302
In base 26:    BDE,    DKE,    DME,    GD6,   16KD,   1F6D,   1PGD,   2E6D,   2I2D,   2KMD,   3ECD,   43ED,   45KD,   64MD,   7DIE

First 15 Rhonda numbers to base 27:
In base 10:  4797, 11844, 12078, 13200, 14841, 17750, 24320, 26883, 27477, 46455, 52750, 58581, 61009, 61446, 61500
In base 27:   6FI,   G6I,   GF9,   I2O,   K9I,   O9B,  169K,  19NI,  1AII,  29JF,  2I9J,  2Q9I,  32IG,  337L,  339L

First 15 Rhonda numbers to base 28:
In base 10:  3094,  5808,  5832,  7462, 11160, 13671, 27270, 28194, 28638, 39375, 39550, 49500, 50862, 52338, 52938
In base 28:   3QE,   7BC,   7C8,   9EE,   E6G,   HC7,  16LQ,  17QQ,  18EM,  1M67,  1MCE,  273O,  28OE,  2AL6,  2BEI

First 15 Rhonda numbers to base 30:
In base 10:  3024,  3168,  5115,  5346,  5950,  6762,  7750,  7956,  8470,  9476,  9576,  9849, 10360, 11495, 13035
In base 30:   3AO,   3FI,   5KF,   5S6,   6IA,   7FC,   8IA,   8P6,   9CA,   AFQ,   AJ6,   AS9,   BFA,   CN5,   EEF

First 15 Rhonda numbers to base 32:
In base 10:  1944,  3600, 13520, 15876, 16732, 16849, 25410, 25752, 28951, 47472, 49610, 50968, 61596, 64904, 74005
In base 32:   1SO,   3GG,   D6G,   FG4,   GAS,   GEH,   OQ2,   P4O,   S8N,  1EBG,  1GEA,  1HOO,  1S4S,  1VC8,  288L

First 15 Rhonda numbers to base 33:
In base 10:    756,   7040,   7568,  13826,  24930,  30613,  59345,  63555,  64372, 131427, 227840, 264044, 313709, 336385, 344858
In base 33:     MU,    6FB,    6VB,    CMW,    MTF,    S3M,   1LGB,   1PBU,   1Q3M,   3LML,   6B78,   7BFB,   8O2B,   9BTG,   9JM8

First 15 Rhonda numbers to base 34:
In base 10:   5661,  14161,  15620,  16473,  22185,  37145, 125579, 134692, 135405, 138472, 140369, 177086, 250665, 255552, 295614
In base 34:    4UH,    C8H,    DHE,    E8H,    J6H,    W4H,   36LH,   3EHI,   3F4H,   3HQO,   3JEH,   4H6E,   6CSH,   6H28,   7HOI

First 15 Rhonda numbers to base 35:
In base 10:   8232,   9476,   9633,  18634,  30954,  41905,  52215,  52440,  56889,  61992,  62146,  66339,  98260, 102180, 103305
In base 35:    6P7,    7PQ,    7U8,    F7E,    P9E,    Y7A,   17LU,   17SA,   1BFE,   1FL7,   1FPL,   1J5E,   2A7F,   2DEF,   2EBK

First 15 Rhonda numbers to base 36:
In base 10:  1000,  4800,  5670,  8190, 10998, 12412, 13300, 15750, 16821, 23016, 51612, 52734, 67744, 70929, 75030
In base 36:    RS,   3PC,   4DI,   6BI,   8HI,   9KS,   A9G,   C5I,   CZ9,   HRC,  13TO,  14OU,  1G9S,  1IQ9,  1LW6
```
