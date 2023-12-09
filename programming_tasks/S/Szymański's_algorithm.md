[1]: https://rosettacode.org/wiki/Szymański%27s_algorithm

# [Szymański&#039;s algorithm][1]

```perl
# 20230822 Raku programming solution

use OO::Monitors;

my \N = 10;

monitor Szymański {

   has @.tasks;
   my $critical = 0;

   method runSzymański($id) {
      @.tasks[$id] = 1; 
      ( my @others = @.tasks ).splice: $id,1;
      until @others.all ~~ 0|1|2 { $*THREAD.yield }
      @.tasks[$id] = 3; 
      if @others.any ~~ 1 {
         @.tasks[$id] = 2;
         until @others.any ~~ 4 { $*THREAD.yield }
      }
      @.tasks[$id] = 4;
      until @.tasks[^$id].all ~~ 0|1 { $*THREAD.yield }
      $critical = ((my $previous = $critical) + $id * 3) div 2;
      say "Thread $id changed the critical value from $previous to $critical";
      until @.tasks[$id^..*-1].all ~~ 0|1|4 { $*THREAD.yield }
      @.tasks[$id] = 0
   }
}

my $flag = Szymański.new: tasks => 0 xx N;
await Promise.allof( ^N .pick(*).map: { start { $flag.runSzymański: $_ } } );
```

#### Output:
```
Thread 6 changed the critical value from 0 to 9
Thread 1 changed the critical value from 9 to 6
Thread 5 changed the critical value from 6 to 10
Thread 4 changed the critical value from 10 to 11
Thread 7 changed the critical value from 11 to 16
Thread 9 changed the critical value from 16 to 21
Thread 8 changed the critical value from 21 to 22
Thread 3 changed the critical value from 22 to 15
Thread 2 changed the critical value from 15 to 10
Thread 0 changed the critical value from 10 to 5
```
