[1]: http://rosettacode.org/wiki/FTP

# [FTP][1]

```perl6
use Net::FTP;
 
my $host = 'kernel.org';
my $user = 'anonymous';
my $password = '';
 
my $ftp = Net::FTP.new( host => $host, :passive );
 
$ftp.login( user => $user, pass => $password );
 
$ftp.cwd( 'pub/linux/kernel' );
 
say $_<name> for $ftp.ls;
 
$ftp.get( 'README', :binary );
```

#### Output:
```
COPYING
CREDITS
Historic
README
SillySounds
crypto
next
people
ports
projects
sha256sums.asc
testing
uemacs
v1.0
v1.1
v1.2
v1.3
v2.0
v2.1
v2.2
v2.3
v2.4
v2.5
v2.6
｢v3.0｣
v3.x
v4.x
```