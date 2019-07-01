[1]: https://rosettacode.org/wiki/Checksumcolor

# [Checksumcolor][1]

To determine the colors, rather than breaking the md5sum into groups of 3 characters, (which leaves two lonely characters at the end), I elected to replicate the first 5 characters onto the end, then for each character, used it and the 5 characters following as a true-color index. I also added an option to output as HTML code for ease of pasting in here.

```perl
unit sub MAIN ($mode = 'ANSI');
 
if $*OUT.t or $mode eq 'HTML' { # if OUT is a terminal or if in HTML $module
 
    say '<div style="background-color:black; font-size:125%; font-family: Monaco, monospace;">'
      if $mode eq 'HTML';
 
    while my $line = get() {
        my $cs  = $line.words[0];
        my $css = $cs ~ $cs.substr(0,5);
        given $mode {
            when 'ANSI' {
                print "\e[48;5;232m";
                .print for $css.comb.rotor(6 => -5)».map({ ($^a, $^b).join })\
                .map( { sprintf "\e[38;2;%d;%d;%dm", |$_».parse-base(16) } ) Z~ $cs.comb;
                say "\e[0m  {$line.words[1..*]}";
            }
            when 'HTML' {
                print "$_\</span>" for $css.comb.rotor(6 => -5)\
                .map( { "<span style=\"color:#{.join};\">" } ) Z~ $cs.comb;
                say " <span style=\"color:#ffffff\"> {$line.words[1..*]}</span>";
                say '<br>';
            }
            default { say $line; }
        }
    }
 
    say '</div>' if $mode eq 'HTML';
} else { # just pass the unaltered line through
    .say while $_ = get();
}
 
```


Can't really show the ANSI output directly so show the HTML output. Essentially identical.


#### Output:
```
md5sum *.p6 | perl6 checksum-color.p6 HTML > checksum-color.html
```


yields:

<div style="background-color:black; color:white; width:600px; font-size:100%; font-family: Monaco, monospace;">

<span style="color:#f09a3f;">f</span><span style="color:#09a3fc;">0</span><span style="color:#9a3fc8;">9</span><span style="color:#a3fc85;">a</span><span style="color:#3fc855;">3</span><span style="color:#fc8551;">f</span><span style="color:#c8551d;">c</span><span style="color:#8551d8;">8</span><span style="color:#551d8a;">5</span><span style="color:#51d8a7;">5</span><span style="color:#1d8a70;">1</span><span style="color:#d8a703;">d</span><span style="color:#8a703d;">8</span><span style="color:#a703d6;">a</span><span style="color:#703d64;">7</span><span style="color:#03d64d;">0</span><span style="color:#3d64d8;">3</span><span style="color:#d64d8e;">d</span><span style="color:#64d8e9;">6</span><span style="color:#4d8e91;">4</span><span style="color:#d8e918;">d</span><span style="color:#8e918e;">8</span><span style="color:#e918ec;">e</span><span style="color:#918ece;">9</span><span style="color:#18ece2;">1</span><span style="color:#8ece23;">8</span><span style="color:#ece236;">e</span><span style="color:#ce236f;">c</span><span style="color:#e236f0;">e</span><span style="color:#236f09;">2</span><span style="color:#36f09a;">3</span><span style="color:#6f09a3;">6</span> <span style="color:#ffffff"> checksum-color (another copy).p6</span>


<span style="color:#f09a3f;">f</span><span style="color:#09a3fc;">0</span><span style="color:#9a3fc8;">9</span><span style="color:#a3fc85;">a</span><span style="color:#3fc855;">3</span><span style="color:#fc8551;">f</span><span style="color:#c8551d;">c</span><span style="color:#8551d8;">8</span><span style="color:#551d8a;">5</span><span style="color:#51d8a7;">5</span><span style="color:#1d8a70;">1</span><span style="color:#d8a703;">d</span><span style="color:#8a703d;">8</span><span style="color:#a703d6;">a</span><span style="color:#703d64;">7</span><span style="color:#03d64d;">0</span><span style="color:#3d64d8;">3</span><span style="color:#d64d8e;">d</span><span style="color:#64d8e9;">6</span><span style="color:#4d8e91;">4</span><span style="color:#d8e918;">d</span><span style="color:#8e918e;">8</span><span style="color:#e918ec;">e</span><span style="color:#918ece;">9</span><span style="color:#18ece2;">1</span><span style="color:#8ece23;">8</span><span style="color:#ece236;">e</span><span style="color:#ce236f;">c</span><span style="color:#e236f0;">e</span><span style="color:#236f09;">2</span><span style="color:#36f09a;">3</span><span style="color:#6f09a3;">6</span> <span style="color:#ffffff"> checksum-color (copy).p6</span>


<span style="color:#f09a3f;">f</span><span style="color:#09a3fc;">0</span><span style="color:#9a3fc8;">9</span><span style="color:#a3fc85;">a</span><span style="color:#3fc855;">3</span><span style="color:#fc8551;">f</span><span style="color:#c8551d;">c</span><span style="color:#8551d8;">8</span><span style="color:#551d8a;">5</span><span style="color:#51d8a7;">5</span><span style="color:#1d8a70;">1</span><span style="color:#d8a703;">d</span><span style="color:#8a703d;">8</span><span style="color:#a703d6;">a</span><span style="color:#703d64;">7</span><span style="color:#03d64d;">0</span><span style="color:#3d64d8;">3</span><span style="color:#d64d8e;">d</span><span style="color:#64d8e9;">6</span><span style="color:#4d8e91;">4</span><span style="color:#d8e918;">d</span><span style="color:#8e918e;">8</span><span style="color:#e918ec;">e</span><span style="color:#918ece;">9</span><span style="color:#18ece2;">1</span><span style="color:#8ece23;">8</span><span style="color:#ece236;">e</span><span style="color:#ce236f;">c</span><span style="color:#e236f0;">e</span><span style="color:#236f09;">2</span><span style="color:#36f09a;">3</span><span style="color:#6f09a3;">6</span> <span style="color:#ffffff"> checksum-color.p6</span>


<span style="color:#bbd8a9;">b</span><span style="color:#bd8a92;">b</span><span style="color:#d8a92c;">d</span><span style="color:#8a92c3;">8</span><span style="color:#a92c32;">a</span><span style="color:#92c326;">9</span><span style="color:#2c326c;">2</span><span style="color:#c326c8;">c</span><span style="color:#326c8a;">3</span><span style="color:#26c8a3;">2</span><span style="color:#6c8a35;">6</span><span style="color:#c8a35e;">c</span><span style="color:#8a35e8;">8</span><span style="color:#a35e80;">a</span><span style="color:#35e80d;">3</span><span style="color:#5e80d2;">5</span><span style="color:#e80d2d;">e</span><span style="color:#80d2d7;">8</span><span style="color:#0d2d71;">0</span><span style="color:#d2d71a;">d</span><span style="color:#2d71ab;">2</span><span style="color:#d71ab8;">d</span><span style="color:#71ab89;">7</span><span style="color:#1ab890;">1</span><span style="color:#ab8902;">a</span><span style="color:#b8902c;">b</span><span style="color:#8902cd;">8</span><span style="color:#902cdb;">9</span><span style="color:#02cdbb;">0</span><span style="color:#2cdbbd;">2</span><span style="color:#cdbbd8;">c</span><span style="color:#dbbd8a;">d</span> <span style="color:#ffffff"> something-completely-different.p6</span>

</div>
