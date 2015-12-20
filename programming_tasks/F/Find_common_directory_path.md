[1]: http://rosettacode.org/wiki/Find_common_directory_path

# [Find common directory path][1]

```perl
my $sep = '/';
my @dirs = </home/user1/tmp/coverage/test
            /home/user1/tmp/covert/operator
            /home/user1/tmp/coven/members>;
 
my @comps = @dirs.map: { [ .comb(/ $sep [ <!before $sep> . ]* /) ] }; 
 
my $prefix = '';
 
while all(@comps[*]»[0]) eq @comps[0][0] {
    $prefix ~= @comps[0][0] // last;
    @comps».shift;
}
 
say "The longest common path is $prefix";
 
```


Output:


#### Output:
```
The longest common path is /home/user1/tmp
```


If you'd prefer a pure FP solution without side effects, you can use this:

```perl
my $sep := '/';
my @dirs := </home/user1/tmp/coverage/test
             /home/user1/tmp/covert/operator
             /home/user1/tmp/coven/members>;
 
my @comps := @dirs.map: { [ .comb(/ $sep [ <!before $sep> . ]* /) ] };
 
say "The longest common path is ",
    gather for 0..* -> $column {
        last unless all(@comps[*]»[$column]) eq @comps[0][$column];
        take @comps[0][$column] // last;
    }
```


Or here's another factoring, that focuses on building the result with cumulative sequences and getting the solution with \`first\`:

```perl
my $sep = '/';
my @dirs = </home/user1/tmp/coverage/test
            /home/user1/tmp/covert/operator
            /home/user1/tmp/coven/members>;
 
sub is_common_prefix { so $^prefix eq all(@dirs).substr(0, $prefix.chars) }
 
say ([\~] @dirs.comb(/ $sep [ <!before $sep> . ]* /)).reverse.first: &is_common_prefix
```