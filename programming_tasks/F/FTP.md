[1]: https://rosettacode.org/wiki/FTP

# [FTP][1]



```perl
use Net::FTP;

my $host = 'speedtest.tele2.net';
my $user = 'anonymous';
my $password = '';

my $ftp = Net::FTP.new( host => $host, :passive );

$ftp.login( user => $user, pass => $password );

$ftp.cwd( 'upload' );

$ftp.cwd( '/' );

say $_<name> for $ftp.ls;

$ftp.get( '1KB.zip', :binary );
```

#### Output:
```
1000GB.zip
100GB.zip
100KB.zip
100MB.zip
10GB.zip
10MB.zip
1GB.zip
1KB.zip
1MB.zip
200MB.zip
20MB.zip
2MB.zip
3MB.zip
500MB.zip
50MB.zip
512KB.zip
5MB.zip
upload
```
