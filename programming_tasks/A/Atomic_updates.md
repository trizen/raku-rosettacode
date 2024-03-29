[1]: https://rosettacode.org/wiki/Atomic_updates

# [Atomic updates][1]



```perl
#| A collection of non-negative integers, with atomic operations.
class BucketStore {
  
  has $.elems is required;
  has @!buckets = ^1024 .pick xx $!elems;
  has $lock     = Lock.new;
 
  #| Returns an array with the contents of all buckets.
  method buckets {
    $lock.protect: { [@!buckets] }
  }
 
  #| Transfers $amount from bucket at index $from, to bucket at index $to.
  method transfer ($amount, :$from!, :$to!) {
    return if $from == $to;
 
    $lock.protect: {
      my $clamped = $amount min @!buckets[$from];
 
      @!buckets[$from] -= $clamped;
      @!buckets[$to]   += $clamped;
    }
  }
}

# Create bucket store
my $bucket-store = BucketStore.new: elems => 8;
my $initial-sum = $bucket-store.buckets.sum;

# Start a thread to equalize buckets
Thread.start: {
  loop {
    my @buckets = $bucket-store.buckets;
    
    # Pick 2 buckets, so that $to has not more than $from
    my ($to, $from) = @buckets.keys.pick(2).sort({ @buckets[$_] });
    
    # Transfer half of the difference, rounded down
    $bucket-store.transfer: ([-] @buckets[$from, $to]) div 2, :$from, :$to;
  }
}

# Start a thread to distribute values among buckets
Thread.start: {
  loop {
    my @buckets = $bucket-store.buckets;
    
    # Pick 2 buckets
    my ($to, $from) = @buckets.keys.pick(2);
 
    # Transfer a random portion
    $bucket-store.transfer: ^@buckets[$from] .pick, :$from, :$to;
  }
}

# Loop to display buckets
loop {
  sleep 1;
  
  my @buckets = $bucket-store.buckets;
  my $sum = @buckets.sum;
  
  say "{@buckets.fmt: '%4d'}, total $sum";
  
  if $sum != $initial-sum {
    note "ERROR: Total changed from $initial-sum to $sum";
    exit 1;
  }
}
```

#### Output:
```
  23   52  831  195 1407  809  813   20, total 4150
1172   83  336  306  751  468  615  419, total 4150
 734  103 1086   88  313  136 1252  438, total 4150
 512  323  544  165  200    3 2155  248, total 4150
...
```
