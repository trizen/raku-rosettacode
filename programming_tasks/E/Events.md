[1]: https://rosettacode.org/wiki/Events

# [Events][1]

```perl
note now, " program start";
my $event = Channel.new;
 
my $todo = start {
    note now, " task start";
    $event.receive;
    note now, " event reset by task";
}
 
note now, " program sleeping";
sleep 1;
note now, " program signaling event";
$event.send(0);
await $todo;
```

#### Output:
```
Instant:1403880984.089974 program start
Instant:1403880984.095400 program sleeping
Instant:1403880984.095491 task start
Instant:1403880985.099381 program signaling event
Instant:1403880985.109395 event reset by task
```


See also [Handle_a_signal#Raku](https://rosettacode.org/wiki/Handle_a_signal#Raku) for an example of using Supplies to do reactive programming based on events (Unix signals in this case).