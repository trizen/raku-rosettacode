[1]: https://rosettacode.org/wiki/Search_in_paragraph%27s_text

# [Search in paragraph&#039;s text][1]

Generalized. Pipe in the file from the shell, user definable search term and entry point.

```perl
unit sub MAIN ( :$for, :$at = 0 );

put slurp.split( /\n\n+/ ).grep( { .contains: $for } )
         .map( { .substr: .index: $at } )
         .join: "\n----------------\n"
```

#### Output:
```
   raku search.raku --for='SystemError' --at='Traceback' < traceback.txt
```


*Matches expected except for not having a extraneous trailing paragraph separator.*


```
Traceback (most recent call last):
    vmodl.fault.SystemError: (vmodl.fault.SystemError) {
    dynamicType = ,
    dynamicProperty = (vmodl.DynamicProperty) [],
    msg = 'A general system error occurred: Unable to complete Sysinfo operation. Please see the VMkernel log file for more details.: Sysinfo error: Bad parameterSee VMkernel log for details.',
    faultCause = ,
    faultMessage = (vmodl.LocalizableMessage) [],
    reason = 'Unable to complete Sysinfo operation. Please see the VMkernel log file for more details.: Sysinfo error: Bad parameterSee VMkernel log for details.'
    }
----------------
Traceback (most recent call last):
[Tue Jan 21 16:16:19.252221 2020] [wsgi:error] [pid 6515:tid 3041002528] [remote 10.0.0.12:50757] SystemError: unable to access /home/dir
[Tue Jan 21 16:16:19.249067 2020] [wsgi:error] [pid 6515:tid 3041002528] [remote 10.0.0.12:50757] mod_wsgi (pid=6515): Failed to exec Python script file '/home/pi/RaspBerryPiAdhan/www/sysinfo.wsgi'.
[Tue Jan 21 16:16:19.249609 2020] [wsgi:error] [pid 6515:tid 3041002528] [remote 10.0.0.12:50757] mod_wsgi (pid=6515): Exception occurred processing WSGI script '/home/pi/RaspBerryPiAdhan/www/sysinfo.wsgi'.
----------------
Traceback (most recent call last): 11/01 18:24:57.728 ERROR| traceback:0013| File "/tmp/sysinfo/autoserv-0tMj3m/common_lib/log.py", line 70, in decorated_func 11/01 18:24:57.729 ERROR| traceback:0013| fn(*args, **dargs) 11/01 18:24:57.730 ERROR| traceback:0013| File "/tmp/sysinfo/autoserv-0tMj3m/bin/base_sysinfo.py", line 286, in log_after_each_test 11/01 18:24:57.731 ERROR| traceback:0013| old_packages = set(self._installed_packages) 11/01 18:24:57.731 ERROR| traceback:0013| SystemError: no such file or directory
----------------
Traceback (most recent call last):
    File "/usr/lib/vmware-vpx/vsan-health/pyMoVsan/VsanClusterPrototypeImpl.py", line 1492, in WaitForUpdateTask
    WaitForTask(task)
    File "/usr/lib/vmware-vpx/pyJack/pyVim/task.py", line 123, in WaitForTask
    raise task.info.error
    vmodl.fault.SystemError: (vmodl.fault.SystemError) {
    dynamicType = ,
    dynamicProperty = (vmodl.DynamicProperty) [],
    msg = 'A general system error occurred: Unable to complete Sysinfo operation. Please see the VMkernel log file for more details.: Sysinfo error: Bad parameterSee VMkernel log for details.',
    faultCause = ,
    faultMessage = (vmodl.LocalizableMessage) [],
    reason = 'Unable to complete Sysinfo operation. Please see the VMkernel log file for more details.: Sysinfo error: Bad parameterSee VMkernel log for details.'
    }
```
