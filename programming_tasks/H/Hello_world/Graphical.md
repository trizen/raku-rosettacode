[1]: https://rosettacode.org/wiki/Hello_world/Graphical

# [Hello world/Graphical][1]



```perl
use GTK::Simple;
use GTK::Simple::App;

my GTK::Simple::App $app .= new;
$app.border-width = 20;
$app.set-content( GTK::Simple::Label.new(text => "Goodbye, World!") );
$app.run;
```
