[1]: https://rosettacode.org/wiki/Spinning_rod_animation/Text

# [Spinning rod animation/Text][1]

Traditionally these are know as [throbbers](http://en.wikipedia.org/wiki/throbber) or progress indicators.



This implementation will accept an array of elements to use as its throbber frames, or as a scrolling marquee and optionally a delay before it returns the next element.

```perl
class throbber {
    has @.frames;
    has $.delay is rw = 0;
    has $!index = 0;
    has Bool $.marquee = False;
    method next {
        $!index = ($!index + 1) % +@.frames;
        sleep $.delay if $.delay;
        if $!marquee {
            ("\b" x @.frames) ~ @.frames.rotate($!index).join;
        }
        else {
            "\b" ~ @.frames[$!index];
        }
    }
}
 
my $rod = throbber.new( :frames(< | / - \ >), :delay(.25) );
print "\e[?25lLong running process...  ";
print $rod.next for ^20;
 
my $clock = throbber.new( :frames("🕐" .. "🕛") );
print "\b \nSomething else with a delay...  ";
until my $done {
    # do something in a loop;
    sleep 1/12;
    print $clock.next;
    $done = True if $++ >= 60;
}
 
my $scroll = throbber.new( :frames('PLEASE STAND BY...      '.comb), :delay(.1), :marquee );
print "\b \nEXPERIENCING TECHNICAL DIFFICULTIES: { $scroll.frames.join }";
print $scroll.next for ^95;
 
END { print "\e[?25h\n" } # clean up on exit
```