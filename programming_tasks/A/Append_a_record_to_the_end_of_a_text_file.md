[1]: https://rosettacode.org/wiki/Append_a_record_to_the_end_of_a_text_file

# [Append a record to the end of a text file][1]





This is kind of silly as it takes a string, converts it to a record, and then instantly converts it back to a string to write out to a file. Most of the "record handling" code is just demonstrating a possible way to store records in memory. It really has nothing to do with appending a string to a file.

```perl
class record {
    has $.name;
    has $.password;
    has $.UID;
    has $.GID;
    has $.fullname;
    has $.office;
    has $.extension;
    has $.homephone;
    has $.email;
    has $.directory;
    has $.shell;

    method gecos { join ',', $.fullname, $.office, $.extension, $.homephone, $.email }

    method gist {
        join ':',
        $.name,
        $.password,
        $.UID,
        $.GID,
        self.gecos,
        $.directory,
        $.shell;
    }
};

my $fname = 'foo.fil';

given $fname.IO.open(:w) { .close }; # clear file

sub append ($file, $line){
    my $fh = $file.IO.open(:a) or fail "Unable to open $file";
    given $fh {
        # Get a lock on the file, waits until lock is active
        .lock;
        # seek to the end in case some other process wrote to
        # the file while we were waiting for the lock
        .seek(0, SeekType::SeekFromEnd);
        # write the record
        .say: $line;
        .close;
    }
}

sub str-to-record ($str) {
    my %rec = <name password UID GID fullname office extension
      homephone email directory shell> Z=> $str.split(/<[:,]>/);
    my $user = record.new(|%rec);
}

for
  'jsmith:x:1001:1000:Joe Smith,Room 1007,(234)555-8917,(234)555-0077,jsmith@rosettacode.org:/home/jsmith:/bin/bash',
  'jdoe:x:1002:1000:Jane Doe,Room 1004,(234)555-8914,(234)555-0044,jdoe@rosettacode.org:/home/jdoe:/bin/bash'
-> $line {
        my $thisuser = str-to-record $line;
       $fname.&append: $thisuser.gist;
}

put "Last line of $fname before append:";
put $fname.IO.lines.tail;

$fname.&append: str-to-record('xyz:x:1003:1000:X Yz,Room 1003,(234)555-8913,(234)555-0033,xyz@rosettacode.org:/home/xyz:/bin/bash').gist;

put "Last line of $fname after append:";
put $fname.IO.lines.tail;
```

#### Output:
```
Last line of foo.fil before append:
jdoe:x:1002:1000:Jane Doe,Room 1004,(234)555-8914,(234)555-0044,jdoe@rosettacode.org:/home/jdoe:/bin/bash
Last line of foo.fil after append:
xyz:x:1003:1000:X Yz,Room 1003,(234)555-8913,(234)555-0033,xyz@rosettacode.org:/home/xyz:/bin/bash
```







Note that advisory locks do not prevent some other program (if it doesn't use flock) from unexpectedly writing to the file.
