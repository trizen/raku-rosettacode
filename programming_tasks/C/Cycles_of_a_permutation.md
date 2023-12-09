[1]: https://rosettacode.org/wiki/Cycles_of_a_permutation

# [Cycles of a permutation][1]

```perl
# one-line
sub infix:<➜> ($start, $end) {$start.fc.comb(/\w/).antipairs.hash{ $end.fc.comb(/\w/) } »+» 1}

# cycle
sub infix:<➰> ($start, $end) {
    my @one-line = flat ($end ➜ $start);
    my @index = flat 1..+@one-line;
    my ($index, @cycles) = 0;
    for @index {
        my $this = $_ - 1;
        while @one-line[$this] {
            my $next = @one-line[$this];
            @cycles[$index].push: @index[$this];
            @one-line[$this] = Nil;
            $this = $next - 1;
        }
        ++$index;
    }
    @cycles.grep: *.elems > 1
}

# order of a cycle
sub order (@cycles) { [lcm] @cycles }

# signature of a cycle
sub signature (@cycles) {
    (@cycles.elems %% 2 and all @@cycles».elems %% 2) ?? 1 !! -1
}

# apply a one-line transform
sub apply-o ($string, @oneline) { $string.comb[@oneline].join }

# apply a cyclical transform
sub apply-c ($string, @cycle) {
    my @string = flat '', $string.comb;
    @cycle.map: { @string[|$_].=rotate(-1) }
    @string.join
}

# Alf & Bettys letter arrangements
my %arrangment =
    :Mon<HANDYCOILSERUPT>,
    :Tue<SPOILUNDERYACHT>,
    :Wed<DRAINSTYLEPOUCH>,
    :Thu<DITCHSYRUPALONE>,
    :Fri<SOAPYTHIRDUNCLE>,
    :Sat<SHINEPARTYCLOUD>,
    :Sun<RADIOLUNCHTYPES>;

# some convenience variables 
my @days = <Sun Mon Tue Wed Thu Fri Sat Sun>;
my @o = @days.rotor(2 => -1).map: { (%arrangment{.[0]} ➜ %arrangment{.[1]}) »-» 1 }
my @c = @days.rotor(2 => -1).map: { (%arrangment{.[0]} ➰ %arrangment{.[1]}) }

my $today;

# The task
say qq:to/ALF&BETTY/;
On Thursdays Alf and Betty should rearrange
their letters using these cycles:  {gist %arrangment<Wed> ➰ %arrangment<Thu>}

So that {%arrangment<Wed>} becomes {%arrangment<Wed>.&apply-o: (%arrangment<Wed> ➜ %arrangment<Thu>) »-» 1}

or they could use the one-line notation:  {gist %arrangment<Wed> ➜ %arrangment<Thu>}


To revert to the Wednesday arrangement they
should use these cycles:  {gist %arrangment<Thu> ➰ %arrangment<Wed>}

or with the one-line notation:  {gist %arrangment<Thu> ➜ %arrangment<Wed>}

So that {%arrangment<Thu>} becomes {%arrangment<Thu>.&apply-o: (%arrangment<Thu> ➜ %arrangment<Wed>) »-» 1}


Starting with the Sunday arrangement and applying each of the daily
permutations consecutively, the arrangements will be:

      {$today = %arrangment<Sun>}

{join "\n", @days[1..*].map: { sprintf "%s:  %s", $_, $today = $today.&apply-o: @o[$++] } }


To go from Wednesday to Friday in a single step they should use these cycles:
{gist %arrangment<Wed> ➰ %arrangment<Fri>}

So that {%arrangment<Wed>} becomes {%arrangment<Fri>}


These are the signatures of the permutations:

  Mon Tue Wed Thu Fri Sat Sun
  {@c.map(&signature)».fmt("%2d").join: '  '}

These are the orders of the permutations:

  Mon Tue Wed Thu Fri Sat Sun
  {@c.map(&order)».fmt("%2d").join: '  '}

Applying the Friday cycle to a string 10 times:

   {$today = 'STOREDAILYPUNCH'}

{join "\n", (1..10).map: {sprintf "%2d %s", $_, $today = $today.&apply-c: @c[4]} }
ALF&BETTY

say 'and one last transform:';
say 'STOREDAILYPUNCH'.&apply-c: [[<1 6 12 2 3 4 13 15 9 11 5 14 8 10 7>],];
```

#### Output:
```
On Thursdays Alf and Betty should rearrange
their letters using these cycles:  ([2 8 7 3 11 10 15 5 14 4] [9 12 13])

So that DRAINSTYLEPOUCH becomes DITCHSYRUPALONE

or they could use the one-line notation:  (1 4 7 14 15 6 8 2 13 11 3 9 12 5 10)


To revert to the Wednesday arrangement they
should use these cycles:  ([2 4 14 5 15 10 11 3 7 8] [9 13 12])

or with the one-line notation:  (1 8 11 2 14 6 3 7 12 15 10 13 9 4 5)

So that DITCHSYRUPALONE becomes DRAINSTYLEPOUCH


Starting with the Sunday arrangement and applying each of the daily
permutations consecutively, the arrangements will be:

      RADIOLUNCHTYPES

Mon:  HANDYCOILSERUPT
Tue:  SPOILUNDERYACHT
Wed:  DRAINSTYLEPOUCH
Thu:  DITCHSYRUPALONE
Fri:  SOAPYTHIRDUNCLE
Sat:  SHINEPARTYCLOUD
Sun:  RADIOLUNCHTYPES


To go from Wednesday to Friday in a single step they should use these cycles:
([1 10 15 7 6] [2 9 14 13 11 4 8 5 12])

So that DRAINSTYLEPOUCH becomes SOAPYTHIRDUNCLE


These are the signatures of the permutations:

  Mon Tue Wed Thu Fri Sat Sun
  -1  -1   1   1  -1   1  -1

These are the orders of the permutations:

  Mon Tue Wed Thu Fri Sat Sun
  18  30  12  30  10  33  40

Applying the Friday cycle to a string 10 times:

   STOREDAILYPUNCH

 1 DNPYAOETISLCRUH
 2 ORLSEPANTDIUYCH
 3 PYIDALERNOTCSUH
 4 LSTOEIAYRPNUDCH
 5 IDNPATESYLRCOUH
 6 TORLENADSIYUPCH
 7 NPYIAREODTSCLUH
 8 RLSTEYAPONDUICH
 9 YIDNASELPROCTUH
10 STOREDAILYPUNCH

and one last transform:
AUTOPSYCHILDREN
```
