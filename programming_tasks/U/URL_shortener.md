[1]: https://rosettacode.org/wiki/URL_shortener

# [URL shortener][1]

As there is no requirement to obfuscate the shortened urls, I elected to just use a simple base 36 incremental counter starting at "0" to "shorten" the urls.



Sets up a micro web service on the local computer at port 10000. Run the service, then access it with any web browser at address localhost:10000 . Any saved urls will be displayed with their shortened id. Enter a web address in the text field to assign a shortened id. Append that id to the web address to automatically be redirected to that url. The url of **this** page is entered as id 0, so the address: " localhost:10000/0 " will redirect to here, to this page.



The next saved address would be accessible at localhost:10000/1 . And so on.



Saves the shortened urls in a local json file called urls.json so saved urls will be available from session to session. No provisions to edit or delete a saved url. If you want to edit the saved urls, edit the urls.json file directly with some third party editor then restart the service.



There is **NO** security or authentication on this minimal app. Not recommended to run this as-is on a public facing server. It would not be too difficult to *add* appropriate security, but it isn't a requirement of this task. Very minimal error checking and recovery.

```raku
# Persistent URL storage
use JSON::Fast;
 
my $urlfile = './urls.json'.IO;
my %urls = ($urlfile.e and $urlfile.f and $urlfile.s) ??
  ( $urlfile.slurp.&from-json ) !!
  ( index => 1, url => { 0 => 'http://rosettacode.org/wiki/URL_shortener#Perl_6' } );
 
$urlfile.spurt(%urls.&to-json);
 
# Setup parameters
my $host = 'localhost';
my $port = 10000;
 
# Micro HTTP service
use Cro::HTTP::Router;
use Cro::HTTP::Server;
 
my $application = route {
    post -> 'add_url' {
        redirect :see-other, '/';
        request-body -> (:$url) {
            %urls<url>{ %urls<index>.base(36) } = $url;
            ++%urls<index>;
            $urlfile.spurt(%urls.&to-json);
        }
    }
 
    get -> {
        content 'text/html', qq:to/END/;
        <form action="http://$host:$port/add_url" method="post">
        URL to add:</br><input type="text" name="url"></br>
        <input type="submit" value="Submit"></form></br>
        Saved URLs:
        <div style="background-color:#eeeeee">
        { %urls<url>.sort( +(*.key.parse-base(36)) ).join: '</br>' }
        </div>
        END
    }
 
    get -> $short {
        if my $link = %urls<url>{$short} {
            redirect :permanent, $link
        }
        else {
            not-found
        }
    }
}
 
my Cro::Service $shorten = Cro::HTTP::Server.new:
    :$host, :$port, :$application;
 
$shorten.start;
 
react whenever signal(SIGINT) { $shorten.stop; exit; }
 
```
