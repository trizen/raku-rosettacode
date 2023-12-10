[1]: https://rosettacode.org/wiki/Vidir

# [Vidir][1]

This is a fairly faithful recreation of the vidir utility with some extra "enhancements". It is set up to work seamlessly with Linux, OSX and most unix-like operating systems. Probably could be adapted for Windows too but won't work there as is.



Takes several positional and/or named command line parameters:





**Positionals:**





**Named:**





Will get a list of all file names from the specified director(y|ies), put them in a  temporary file and open the text file with the default (or specified) text editor.



Edit the file names or directory names in the text editor: add, remove, modify file names, then save the text file; all of the modifications made to the text file will be applied to the file system.



**To edit a file name:** Just edit the file name, extension, whatever. **Warning!** There is  nothing preventing you from using characters in the file name which will make it difficult  to open, modify or delete the file. You can screw yourself quite easily if you make an effort. With great power comes great responsibility. The foot cannon is primed and loaded.



If you add a forward solidus (directory separator: **/**) to a filename, it will treat anything between separators as a directory name and **WILL CREATE** non-existent directories.



**To move a file:** Edit the directory path. The file will be moved to the new path. Non-existing directories will be created.



**To delete a file:** Delete the entire line (file name and identifier) in the text file.



**To add a file:** Add a new line with a unique integer (need not be in any order), one or more tabs, then the new file path/name. May be a relative or absolute path.



Notice that all of the above operations will fail to apply if you lack sufficient permissions for the affected files or directories.

```perl
use Sort::Naturally;
use File::Temp;

my %*SUB-MAIN-OPTS = :named-anywhere;

unit sub MAIN (
    Str $path         = '.',    #= default $path
    Str $filter       = '',     #= default file filter
    Bool :r(:$recurse)= False,  #= recursion flag
    Bool :v(:$verbose)= False,  #= verbose mode
    Str :e(:$editor)  = $*DISTRO ~~ /'Darwin'/ ?? "open" !! "xdg-open"; #= default editor
);

my $dir = $path;

# fix up path if necessary
$dir ~= '/' unless $dir.substr(*-1) eq '/';

# check that path is reachable
die "Can not find directory $dir" unless $dir.IO.d;


my @files;

# get files from that path
getdir( $dir, $filter );

@files.= sort( &naturally );

# set up a temp file and file handle
my ($filename, $filehandle) = tempfile :suffix('.vidir');

# write the filenames to the tempfile
@files.kv.map: { $filehandle.printf("%s\t%s\n", $^k, $^v) };

# flush the buffer to make sure all of the filenames have been written
$filehandle.flush;

# editor command
my $command = "$editor $filename";

# start text editor; suppress STDERR, some editors complain about open files being deleted
shell("$command 2> /dev/null");

react {
    # watch for file changes
    whenever IO::Notification.watch-path($filename) {
        # allow a short interval for the file to finish writing
        sleep .1;

        # read in changed file
        my %changes = $filename.IO.lines.map( { my ($k, $v) = .split(/\t+/); "{$k.trim}" => $v} );

        # walk the filenames and make the desired changes
        for ^@files -> $k {
            if %changes{"$k"}:exists {
                # name has changed, rename the file on disk
                if (%changes{"$k"}) ne @files[$k] {
                    # check to see that the desired directory exists
                    checkdir %changes{"$k"};
                    # notify and do it
                    say "Renaming: {@files[$k]} to " ~ %changes{"$k"} if $verbose;
                    rename @files[$k], %changes{"$k"} orelse .die;
                }
                %changes{"$k"}:delete;
            }
            else {
                # name is gone, delete the file
                # notify and do it
                say "Deleting: {@files[$k]}" if $verbose;
                @files[$k].unlink orelse .die;
            }
        }
        for %changes.kv -> $k, $v {
            # a new name is added, add an empty file with that name
            # check to see that the desired directory exists
            checkdir $v;
            # notify and do it
            say "Adding: $v" if $verbose;
            shell("touch $v") orelse .die;
        }
        # clean up when done
        done;
        exit;
    };
    # watch for CTRL-C, cleanup and exit
    whenever signal(SIGINT) {
        print "\b\b";
        done;
        exit;
    }
}

# get the files in a specified directory matching the filter parameter
sub files ( $dir, $filter = '' ) {
    if $filter.chars {
        $dir.IO.dir.grep( *.f ).grep( *.basename.contains(/<$filter>/) );
    } else {
        $dir.IO.dir.grep( *.f );
    }
}

# get the files in the present directory and recurse if desired
sub getdir ($dir, $filter) {
    if $recurse {
        @files.append: files($dir, $filter);
        getdir( $_, $filter ) for $dir.IO.dir.grep( *.d );
    } else {
        @files = files($dir, $filter);
    }
}

# check for existence of a directory and create it if not found
sub checkdir ($dir) {
    unless $dir.IO.dirname.IO.e {
        # if not, create it
        my @path = $dir.IO.dirname.split('/');
        for 1 .. @path {
            my $thispath = @path[^$_].join('/');
            unless $thispath.IO.e {
                say "Creating new directory $thispath" if $verbose;
                mkdir($thispath);
            }
        }
    }
}
```
