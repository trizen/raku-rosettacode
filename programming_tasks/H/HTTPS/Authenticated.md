[1]: https://rosettacode.org/wiki/HTTPS/Authenticated

# [HTTPS/Authenticated][1]

Used here to connect to my local wireless router to a page that is password protected. Obviously not going to be generally publicly accessible but should be easily adaptable to other sites / devices.

```perl
use HTTP::UserAgent;
 
my $username = 'username'; # my username
my $password = 'password'; # my password
my $address  = 'http://192.168.1.1/Status_Router.asp'; # my local wireless router
 
my $ua = HTTP::UserAgent.new;
$ua.auth( $username, $password );
my $response = $ua.get: $address;
say $response.is-success ?? $response.content !! $response.status-line;
```