[1]: https://rosettacode.org/wiki/Active_object

# [Active object][1]





There is some jitter in the timer, but it is typically accurate to within a few thousandths of a second.

```perl
class Integrator {
    has $.f is rw = sub ($t) { 0 };
    has $.now is rw;
    has $.value is rw = 0;
    has $.integrator is rw;

    method init() {
        self.value = &(self.f)(0);
        self.integrator = Thread.new(
            :code({
                loop {
                    my $t1 = now;
                    self.value += (&(self.f)(self.now) + &(self.f)($t1)) * ($t1 - self.now) / 2;
                    self.now = $t1;
                    sleep .001;
                }
            }),
            :app_lifetime(True)
        ).run
    }

    method Input (&f-of-t) {
        self.f = &f-of-t;
        self.now = now;
        self.init;
    }

    method Output { self.value }
}

my $a = Integrator.new;

$a.Input( sub ($t) { sin(2 * π * .5 * $t) } );

say "Initial value: ", $a.Output;

sleep 2;

say "After 2 seconds: ", $a.Output;

$a.Input( sub ($t) { 0 } );

sleep .5;

say "f(0): ", $a.Output;
```

#### Output:
```
Initial value: 0
After 2 seconds: -0.0005555887464620366
f(0): 0
```
