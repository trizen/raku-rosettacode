[1]: https://rosettacode.org/wiki/Priority_queue

# [Priority queue][1]


This is a rather simple implementation. It requires the priority to be a positive integer value, with lower values being higher priority. There isn't a hard limit on how many priority levels you can have, though more than a few dozen is probably not practical.



The tasks are stored internally as an array of FIFO buffers, so multiple tasks of the same priority level will be returned in the order they were stored.

```perl
class PriorityQueue {
    has @!tasks;

    method insert (Int $priority where * >= 0, $task) {
        @!tasks[$priority].push: $task;
    }

    method get { @!tasks.first(?*).shift }

    method is-empty { ?none @!tasks }
}

my $pq = PriorityQueue.new;
 
for (
    3, 'Clear drains',
    4, 'Feed cat',
    5, 'Make tea',
    9, 'Sleep',
    3, 'Check email',
    1, 'Solve RC tasks',
    9, 'Exercise',
    2, 'Do taxes'
) -> $priority, $task {
    $pq.insert( $priority, $task );
}
 
say $pq.get until $pq.is-empty;
```

#### Output:
```
Solve RC tasks
Do taxes
Clear drains
Check email
Feed cat
Make tea
Sleep
Exercise
```
