[1]: https://rosettacode.org/wiki/Run_as_a_daemon_or_service

# [Run as a daemon or service][1]



```perl
# Reference:
# https://github.com/hipek8/p6-UNIX-Daemonize/

use v6;
use UNIX::Daemonize;
use File::Temp;

my ($output, $filehandle) = tempfile(:tempdir("/tmp"),:!unlink) or die;

say "Output now goes to ",$output;

daemonize();

loop {
   sleep(1);
   spurt $output, DateTime.now.Str~"\n", :append;
}
```

#### Output:
```
root@ubuntu:~# su - david
<p>david@ubuntu:~$ ./dumper.p6
Output now goes to /tmp/x2ovx9JG8b
david@ubuntu:~$ tail -f /tmp/x2ovx9JG8b
2018-12-11T20:20:01.510484+08:00
2018-12-11T20:20:02.513732+08:00
2018-12-11T20:20:03.517063+08:00
2018-12-11T20:20:04.520394+08:00
2018-12-11T20:20:05.524871+08:00
2018-12-11T20:20:06.528244+08:00
2018-12-11T20:20:07.531985+08:00
2018-12-11T20:20:08.537776+08:00
2018-12-11T20:20:09.541606+08:00
2018-12-11T20:20:10.545796+08:00
2018-12-11T20:20:11.549047+08:00
2018-12-11T20:20:12.552704+08:00
^C
david@ubuntu:~$ exit
logout
root@ubuntu:~# tail -f /tmp/x2ovx9JG8b
2018-12-11T20:20:28.623690+08:00
2018-12-11T20:20:29.626978+08:00
2018-12-11T20:20:30.634309+08:00
2018-12-11T20:20:31.637481+08:00
2018-12-11T20:20:32.640794+08:00
2018-12-11T20:20:33.643947+08:00
2018-12-11T20:20:34.647146+08:00
2018-12-11T20:20:35.651008+08:00
^C
root@ubuntu:~# su - david
david@ubuntu:~$ tail -f /tmp/x2ovx9JG8b
2018-12-11T20:20:51.711357+08:00
2018-12-11T20:20:52.715044+08:00
2018-12-11T20:20:53.718921+08:00
2018-12-11T20:20:54.722134+08:00
2018-12-11T20:20:55.725970+08:00
2018-12-11T20:20:56.729160+08:00
2018-12-11T20:20:57.732376+08:00
2018-12-11T20:20:58.735409+08:00
2018-12-11T20:20:59.738886+08:00
2018-12-11T20:21:00.743045+08:00
2018-12-11T20:21:01.748113+08:00
2018-12-11T20:21:02.753204+08:00
2018-12-11T20:21:03.756665+08:00
2018-12-11T20:21:04.759902+08:00
^C
david@ubuntu:~$ pkill -c moar
1
</p>
```
