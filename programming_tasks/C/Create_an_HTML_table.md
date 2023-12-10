[1]: https://rosettacode.org/wiki/Create_an_HTML_table

# [Create an HTML table][1]





The below example is kind of boring, and laughably simple. For more interesting/complicated examples see:



[Show_Ascii_table#Raku](https://rosettacode.org/wiki/Show_Ascii_table#Raku) - *(simple)*

[Mayan_numerals#Raku](https://rosettacode.org/wiki/Mayan_numerals#Raku) - *(heavy styling)*

[Rosetta_Code/Count_examples/Full_list](https://rosettacode.org/wiki/Rosetta_Code/Count_examples/Full_list) - *(multi-column sortable)*

[Rosetta_Code/List_authors_of_task_descriptions/Full_list](https://rosettacode.org/wiki/Rosetta_Code/List_authors_of_task_descriptions/Full_list) - *(complex nested tables)*



*Note: the above examples are outputting wikitable formatting, not HTML directly. It's pretty much a one-for-one shorthand notation though and the principles and process are the same.*





This is certainly not the only or best way to generate HTML tables using Raku; just an example of one possible method.

```perl
my @header =  <&nbsp; X Y Z>;
my $rows = 5;

sub tag ($tag, $string, $param?) { return "<$tag" ~ ($param ?? " $param" !! '') ~ ">$string" ~ "</$tag>" };

my $table = tag('tr', ( tag('th', $_) for @header));

for 1 .. $rows -> $row { 
    $table ~=  tag('tr', ( tag('td', $row, 'align="right"')
    ~ (tag('td', (^10000).pick, 'align="right"') for 1..^@header)));  
}

say tag('table', $table, 'cellspacing=4 style="text-align:right; border: 1px solid;"');
```
