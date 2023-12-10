[1]: https://rosettacode.org/wiki/Checkpoint_synchronization

# [Checkpoint synchronization][1]



```perl
my $TotalWorkers = 3;
my $BatchToRun = 3;
my @TimeTaken = (5..15); # in seconds

my $batch_progress = 0;
my @batch_lock = map { Semaphore.new(1) } , ^$TotalWorkers;
my $lock = Lock.new;

sub assembly_line ($ID) {
   my $wait;
   for ^$BatchToRun -> $j {
      $wait = @TimeTaken.roll;
      say "Worker ",$ID," at batch $j will work for ",$wait," seconds ..";
      sleep($wait);
      $lock.protect: {
         my $k = ++$batch_progress;
         print "Worker ",$ID," is done and update batch $j complete counter ";
         say "to $k of $TotalWorkers";
         if ($batch_progress == $TotalWorkers) {
            say ">>>>> batch $j completed.";
            $batch_progress = 0; # reset for next batch
            for @batch_lock { .release }; # and ready for next batch
         };
       };

       @batch_lock[$ID].acquire; # for next batch
   }
}

for ^$TotalWorkers -> $i {
   Thread.start(
      sub {
         @batch_lock[$i].acquire;
         assembly_line($i);
      }
   );
}
```

#### Output:
```
Worker 1 at batch 0 will work for 6 seconds ..
Worker 2 at batch 0 will work for 32 seconds ..
Worker 0 at batch 0 will work for 13 seconds ..
Worker 1 is done and update batch 0 complete counter to 1 of 3
Worker 0 is done and update batch 0 complete counter to 2 of 3
Worker 2 is done and update batch 0 complete counter to 3 of 3
>>>>> batch 0 completed.
Worker 2 at batch 1 will work for 27 seconds ..
Worker 0 at batch 1 will work for 18 seconds ..
Worker 1 at batch 1 will work for 13 seconds ..
Worker 1 is done and update batch 1 complete counter to 1 of 3
Worker 0 is done and update batch 1 complete counter to 2 of 3
Worker 2 is done and update batch 1 complete counter to 3 of 3
>>>>> batch 1 completed.
Worker 2 at batch 2 will work for 5 seconds ..
Worker 0 at batch 2 will work for 28 seconds ..
Worker 1 at batch 2 will work for 33 seconds ..
Worker 2 is done and update batch 2 complete counter to 1 of 3
Worker 0 is done and update batch 2 complete counter to 2 of 3
Worker 1 is done and update batch 2 complete counter to 3 of 3
>>>>> batch 2 completed.
```
