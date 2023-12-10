[1]: https://rosettacode.org/wiki/Starting_a_web_browser

# [Starting a web browser][1]





Uses the code from the [Separate the house number from the street name](https://rosettacode.org/wiki/Separate_the_house_number_from_the_street_name) task almost verbatim. Included here to make a complete, runnable example.

```perl
use File::Temp;

my $addresses = qq :to /END/;
    Plataanstraat 5
    Straat 12
    Straat 12 II
    Dr. J. Straat   12
    Dr. J. Straat 12 a
    Dr. J. Straat 12-14
    Laan 1940 – 1945 37
    Plein 1940 2
    1213-laan 11
    16 april 1944 Pad 1
    1e Kruisweg 36
    Laan 1940-’45 66
    Laan ’40-’45
    Langeloërduinen 3 46
    Marienwaerdt 2e Dreef 2
    Provincialeweg N205 1
    Rivium 2e Straat 59.
    Nieuwe gracht 20rd
    Nieuwe gracht 20rd 2
    Nieuwe gracht 20zw /2
    Nieuwe gracht 20zw/3
    Nieuwe gracht 20 zw/4
    Bahnhofstr. 4
    Wertstr. 10
    Lindenhof 1
    Nordesch 20
    Weilstr. 6
    Harthauer Weg 2
    Mainaustr. 49
    August-Horch-Str. 3
    Marktplatz 31
    Schmidener Weg 3
    Karl-Weysser-Str. 6
    END

my @row-color = '#d7fffe', '#9dbcd4';

# build the table
sub genTable () {
    my $table = '<table border="2"> <tr bgcolor="#02ccfe">' ~
    qq|<th>Address</th><th>Street</th><th>House Number</th>\n|;
    my $i = 0;
    for $addresses.lines -> $addr {
        $table ~= qq|<tr bgcolor="{@row-color[$i++ % 2]}"><td>{$addr}</td>|;
        $addr ~~ m[
            ( .*? )
            [
                \s+
                (
                | \d+ [ \- | \/ ] \d+
                | <!before 1940 | 1945> \d+ <[ a..z I . / \x20 ]>* \d*
                )
            ]?
            $
        ];
        quietly $table ~= qq|<td>{$0.Str}</td><td>{$1.Str||''}</td></tr>\n|;
    }
    $table ~ '</table>';
}

# generate the page content
sub content {
    qq :to /END/;
    <html>
    <head>
    <title>Rosetta Code - Start a Web Browser</title>
    <meta charset="UTF-8">
    </head>
    <body bgcolor="#d8dcd6">
    <p align="center">
    <font face="Arial, sans-serif" size="5">Split the house number from the street name</font>
    </p>
    <p align="center">
    { genTable }
    </p>
    </body>
    </html>
    END
}

# Use a temporary file name and file handle
my ($fn, $fh) = tempfile :suffix('.html');

# dump the content to the file
$fh.spurt: content;

# use appropriate command for Windows or X11
# other OSs/WMs may need different invocation
my $command = $*DISTRO.is-win ?? "start $fn" !! "xdg-open $fn";

# start the browser
shell $command;

# wait for a bit to give browser time to load before destroying temp file
sleep 5;
```
