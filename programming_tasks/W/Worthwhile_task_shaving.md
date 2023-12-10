[1]: https://rosettacode.org/wiki/Worthwhile_task_shaving

# [Worthwhile task shaving][1]

```perl
# 20220207 Raku programming solution 

my \shaved  = [1, 5, 30, 60, 300, 1800, 3600, 21600, 86400]; # time shaved off in seconds
my \columns = [ "1 SECOND", "5 SECONDS", "30 SECONDS", "1 MINUTE", "5 MINUTES",
                "30 MINUTES", "1 HOUR", "6 HOURS", "1 DAY" ];
my \diy     = 365.25;
my \minute  = 60;
my \hour    = minute * 60;
my \day     = hour * 24;
my \week    = day * 7;
my \month   = day * diy / 12;
my \year    = day * diy;
my \freq    = [50 * diy, 5 * diy, diy, diy/7, 12, 1]; # frequency per year
my \mult    = 5; # multiplier for table

sub fmtTime (\t, \interval) { printf "%-12s ", t.floor~" "~interval~(t == 1 ?? "" !! "S") }

say ' ' x 34~"HOW OFTEN YOU DO THE TASK";
printf("%-12s | %-12s %-12s %-12s %-12s %-12s %-12s\n", 
   ["SHAVED OFF", "50/DAY", "5/DAY", "DAILY", "WEEKLY", "MONTHLY", "YEARLY"]);
say '-' x 93;

for ^9 -> \y {
   printf "%-12s | ", columns[y];
   for ^6 -> \x {
      given my \t = freq[x] * shaved[y] * mult {
         when t < minute  { fmtTime t,        "SECOND" } 
         when t < hour    { fmtTime t/minute, "MINUTE" } 
         when t < day     { fmtTime t/hour,   "HOUR"   } 
         when t < 14*day  { fmtTime t/day,    "DAY"    } 
         when t < 9*week  { fmtTime t/week,   "WEEK"   }
         when t < year    { fmtTime t/month,  "MONTH"  }
         default          { print   '   N/A       '    }
      }
   }
   print "\n"
}
```

#### Output:
```
                                  HOW OFTEN YOU DO THE TASK
SHAVED OFF   | 50/DAY       5/DAY        DAILY        WEEKLY       MONTHLY      YEARLY
---------------------------------------------------------------------------------------------
1 SECOND     | 1 DAYS       2 HOURS      30 MINUTES   4 MINUTES    1 MINUTE     5 SECONDS
5 SECONDS    | 5 DAYS       12 HOURS     2 HOURS      21 MINUTES   5 MINUTES    25 SECONDS
30 SECONDS   | 4 WEEKS      3 DAYS       15 HOURS     2 HOURS      30 MINUTES   2 MINUTES
1 MINUTE     | 2 MONTHS     6 DAYS       1 DAYS       4 HOURS      1 HOUR       5 MINUTES
5 MINUTES    | 10 MONTHS    4 WEEKS      6 DAYS       21 HOURS     5 HOURS      25 MINUTES
30 MINUTES   |    N/A       6 MONTHS     5 WEEKS      5 DAYS       1 DAYS       2 HOURS
1 HOUR       |    N/A          N/A       2 MONTHS     10 DAYS      2 DAYS       5 HOURS
6 HOURS      |    N/A          N/A          N/A       2 MONTHS     2 WEEKS      1 DAYS
1 DAY        |    N/A          N/A          N/A       8 MONTHS     8 WEEKS      5 DAYS
```
