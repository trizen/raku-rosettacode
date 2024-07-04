[1]: https://rosettacode.org/wiki/Singly-linked_list/Reversal

# [Singly-linked list/Reversal][1]

Extending code from the [Singly-linked_list/Element_definition#Raku](https://rosettacode.org/wiki/Singly-linked_list/Element_definition#Raku) Task

```perl
# 20240220 Raku programming solution

class Cell { has ($.value, $.next) is rw;

   method reverse {
      my ($list, $prev) = self, Nil;
      while $list.defined {
         my $next = $list.next;
         $list.next = $prev;
         ($list, $prev) = ($next, $list)
      }
      return $prev;
   }

   method gist {
      my $cell = self;
      return ( gather while $cell.defined {
         take $cell.value andthen $cell = $cell.next;
      } ).Str
   }
}

sub cons ($value, $next = Nil) { Cell.new(:$value, :$next) }

my $list = cons 10, (cons 20, (cons 30, (cons 40, Nil)));

say $list = $list.reverse;
say $list = $list.reverse;
```

#### Output:
```
40 30 20 10
10 20 30 40
```


You may [Attempt This Online!](https://ato.pxeger.com/run?1=fVI9T8MwEJUY-ytuyBBLISofA2rE1J0Fid0klyaqcZHtNKAqv4SlA_wpfg13tpMGUTHl4nvv2e_efXwaue2Ox6_O1Zd33xdPpZLWwhqVggM00kKa5HupOswgyTW-OQGtBdMXiwUAvKBrdhUY3KOxCAc-4-N3oqnWOiK9UlPAPVhUdQYPrSoiqG9aheBheYV1q7GaBIJGwvcRNWD4pzj1T4eM4FtmzT-3p14rCywRgUP8GnSd0TORYW5uQ4y5s6Tk4QRDxW-FFDbSNWhGb4w8583J7dj1swWpK-LpSTv05o4HEPmjM-F19D7bPUO505zPGE-cBY1YUHbroNCnqxGwSkJ-xGYbPAmCe5GrZQapr66n6maqbpc-OCEEpW7liRsyiOkX_7TCesUtG7ftBw)
