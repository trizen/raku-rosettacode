[1]: https://rosettacode.org/wiki/NYSIIS

# [NYSIIS][1]

This implementation removes common name suffixes similar to the reference implementation, even though it is not specified in the task description or on the linked [NYSIIS](http://en.wikipedia.org/wiki/New_York_State_Identification_and_Intelligence_System) page. This algorithm isn't too friendly to certain French kings.&#160;:)

```perl
sub no_suffix ($name) {
    $name.uc.subst: /\h (<[JS]>R) | (<[IVX]>+) $/, '';
}
 
sub nysiis ($name is copy) {
    given $name .= uc {
        s:g/<-[A..Z]>//;
        s/^MAC/MCC/;
        s/^P<[FH]>/FF/;
        s/^SCH/SSS/;
        s/^KN/N/;
        s/<[IE]>E$  /Y/;
        s/<[DRN]>T$ /D/;
        s/<[RN]>D$  /D/;
        s:c(1):g/EV/AF/;
        s:c(1):g/<[AEIOU]>+/A/;
        s:c(1):g/Q/G/;
        s:c(1):g/Z/S/;
        s:c(1):g/M/N/;
        s:c(1):g/KN/N/;
        s:c(1):g/K/C/;
        s:c(1):g/SCH/S/;
        s:c(1):g/PF/F/;
        s:c(1):g/K/C/;
        s:c(1):g/H(<-[AEIOU]>)/$0/;
        s:g/(<-[AEIOU]>)H/$0/;
        s:g/(.)W/$0/;
        s/ AY$ /Y/;
        s/  S$ //;
        s/  A$ //;
        s:g/ (.)$0+ /$0/;
     }
     return $name;
}
 
 
for «
    knight      mitchell        "o'daniel"      "brown  sr"     "browne III"
    "browne IV" "O'Banion"      Mclaughlin      McCormack       Chapman
    Silva       McDonald        Lawson          Jacobs          Greene
    "O'Brien"   Morrison        Larson          Willis          Mackenzie
    Carr        Lawrence        Matthews        Richards        Bishop
    Franklin    McDaniel        Harper          Lynch           Watkins
    Carlson     Wheeler         "Louis XVI"
» {
    my $nysiis = nysiis no_suffix $_;
    if $nysiis.chars > 6 {
        $nysiis .= subst: rx/ <after .**6> .+ /, -> $/ { "[$/]" };
    }
    printf "%10s,  %s\n", $_, $nysiis;
}
```


Output:


#### Output:
```
    knight,  NAGT
  mitchell,  MATCAL
  o'daniel,  ODANAL
  brown sr,  BRAN
browne III,  BRAN
 browne IV,  BRAN
  O'Banion,  OBANAN
Mclaughlin,  MCLAGL[AN]
 McCormack,  MCARNA[C]
   Chapman,  CAPNAN
     Silva,  SALV
  McDonald,  MCDANA[LD]
    Lawson,  LASAN
    Jacobs,  JACAB
    Greene,  GRAN
   O'Brien,  OBRAN
  Morrison,  MARASA[N]
    Larson,  LARSAN
    Willis,  WAL
 Mackenzie,  MCANSY
      Carr,  CAR
  Lawrence,  LARANC
  Matthews,  MAT
  Richards,  RACARD
    Bishop,  BASAP
  Franklin,  FRANCL[AN]
  McDaniel,  MCDANA[L]
    Harper,  HARPAR
     Lynch,  LYNC
   Watkins,  WATCAN
   Carlson,  CARLSA[N]
   Wheeler,  WALAR
 Louis XVI,  L
```