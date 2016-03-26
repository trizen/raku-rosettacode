[1]: http://rosettacode.org/wiki/GUI_component_interaction

# [GUI component interaction][1]

```perl
use GTK::Simple;
 
my GTK::Simple::App $app .= new(title => 'GUI component interaction');
 
$app.set_content(
    my $box = GTK::Simple::VBox.new(
        my $value     = GTK::Simple::Entry.new(text => '0'),
        my $increment = GTK::Simple::Button.new(label => 'Increment'),
        my $random    = GTK::Simple::Button.new(label => 'Random'),
    )
);
 
$app.size_request(400, 100);
$app.border_width = 20;
$box.spacing = 10;
 
$value.changed.tap: {
    ($value.text ||= '0') ~~ s:g/<-[0..9]>//;
}
 
$increment.clicked.tap: {
    $value.text += 1;
}
 
$random.clicked.tap: {
    # Dirty hack to work around the fact that GTK::Simple doesn't provide
    # access to GTK message dialogs yet :P
    if run «zenity --question --text "Reset to random value?"» {
        $value.text = (^100).pick;
    }
}
 
$app.run;
```


W/out class and using grid layout

```python
 
import random, tkMessageBox
from Tkinter import *
window = Tk()
window.geometry("300x50+100+100")
options = { "padx":5, "pady":5}
s=StringVar()
s.set(1)
def increase():
    s.set(int(s.get())+1)
def rand():
    if tkMessageBox.askyesno("Confirmation", "Reset to random value ?"):
        s.set(random.randrange(0,5000))
def update(e):
    if not e.char.isdigit():
        tkMessageBox.showerror('Error', 'Invalid input !') 
        return "break"
e = Entry(text=s)
e.grid(column=0, row=0, **options)
e.bind('<Key>', update)
b1 = Button(text="Increase", command=increase, **options )
b1.grid(column=1, row=0, **options)
b2 = Button(text="Random", command=rand, **options)
b2.grid(column=2, row=0, **options)
mainloop()
```