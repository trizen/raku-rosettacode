[1]: http://rosettacode.org/wiki/Dining_philosophers

# [Dining philosophers][1]

We use locking mutexes for the forks, and a lollipop to keep the last person who finished eating from getting hungry until the next person finishes eating, which prevents a cycle of dependency from forming.  The algorithm should scale to many philosophers, and no philosopher need be singled out to be left-handed, so it's fair in that sense.

```perl
class Fork {
    has $!lock = Lock.new;
    method grab($who, $which) {
	say "$who grabbing $which fork";
	$!lock.lock;
    }
    method drop($who, $which) {
	say "$who dropping $which fork";
	$!lock.unlock;
    }
}
 
class Lollipop {
    has $!channel = Channel.new;
    method mine($who) { $!channel.send($who) }
    method yours { $!channel.receive }
}
 
sub dally($sec) { sleep 0.01 + rand * $sec }
 
sub MAIN(*@names) {
    @names ||= <Aristotle Kant Spinoza Marx Russell>;
 
    my @lfork = Fork.new xx @names;
    my @rfork = @lfork.rotate;
 
    my $lollipop = Lollipop.new;
    start { $lollipop.yours; }
 
    my @philosophers = do for flat @names Z @lfork Z @rfork -> $n, $l, $r {
	start { 
	    sleep 1 + rand*4;
	    loop {
		$l.grab($n,'left');
		dally 1;  # give opportunity for deadlock
		$r.grab($n,'right');
		say "$n eating";
		dally 10;
		$l.drop($n,'left');
		$r.drop($n,'right');
 
		$lollipop.mine($n);
		sleep 1;  # lick at least once
		say "$n lost lollipop to $lollipop.yours(), now digesting";
 
		dally 20;
	    }
	}
    }
    sink await @philosophers;
}
```

#### Output:
```
Aristotle grabbing left fork
Aristotle grabbing right fork
Aristotle eating
Marx grabbing left fork
Marx grabbing right fork
Marx eating
Spinoza grabbing left fork
Aristotle dropping left fork
Aristotle dropping right fork
Russell grabbing left fork
Kant grabbing left fork
Kant grabbing right fork
Spinoza grabbing right fork
Marx dropping left fork
Marx dropping right fork
Aristotle lost lollipop to Marx, now digesting
Spinoza eating
Spinoza dropping left fork
Spinoza dropping right fork
Kant eating
Russell grabbing right fork
Russell eating
Marx lost lollipop to Spinoza, now digesting
Kant dropping left fork
Kant dropping right fork
Spinoza lost lollipop to Kant, now digesting
Russell dropping left fork
Russell dropping right fork
Kant lost lollipop to Russell, now digesting
Spinoza grabbing left fork
Spinoza grabbing right fork
Spinoza eating
Aristotle grabbing left fork
Aristotle grabbing right fork
Aristotle eating
Aristotle dropping left fork
Aristotle dropping right fork
Russell lost lollipop to Aristotle, now digesting
Spinoza dropping left fork
Spinoza dropping right fork
Aristotle lost lollipop to Spinoza, now digesting
Russell grabbing left fork
Marx grabbing left fork
Russell grabbing right fork
Russell eating
Marx grabbing right fork
Kant grabbing left fork
Kant grabbing right fork
Kant eating
Russell dropping left fork
Russell dropping right fork
Spinoza lost lollipop to Russell, now digesting
Marx eating
Spinoza grabbing left fork
Kant dropping left fork
Kant dropping right fork
Russell lost lollipop to Kant, now digesting
Spinoza grabbing right fork
^C
```