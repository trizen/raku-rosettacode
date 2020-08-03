[1]: https://rosettacode.org/wiki/Window_creation

# [Window creation][1]

**Library** [GTK](https://github.com/perl6/gtk-simple)



Exit either by clicking the button or the close window control in the upper corner.

```raku
use GTK::Simple;
use GTK::Simple::App;
 
my GTK::Simple::App $app .= new(title => 'Simple GTK Window');
 
$app.size-request(250, 100);
 
$app.set-content(
    GTK::Simple::VBox.new(
        my $button = GTK::Simple::Button.new(label => 'Exit'),
    )
);
 
$app.border-width = 40;
 
$button.clicked.tap: { $app.exit }
 
$app.run;
```