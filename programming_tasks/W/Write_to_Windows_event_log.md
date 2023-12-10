[1]: https://rosettacode.org/wiki/Write_to_Windows_event_log

# [Write to Windows event log][1]





There is not yet (that I am aware of) a native interface to the Windows logging functions, but Raku can shell out and run a console command just as easily as most of these other languages. It *does* have a native interface to the syslog functions under POSIX environments though.



(Same caveats as the others, needs to be run as administrator or with elevated privileges under Windows.)

```perl
given $*DISTRO {
    when .is-win {
        my $cmd = "eventcreate /T INFORMATION /ID 123 /D \"Bla de bla bla bla\"";
        run($cmd);
    }
    default { # most POSIX environments
        use Log::Syslog::Native;
        my $logger = Log::Syslog::Native.new(facility => Log::Syslog::Native::User);
        $logger.info("[$*PROGRAM-NAME pid=$*PID user=$*USER] Just thought you might like to know.");
    }
}
```
