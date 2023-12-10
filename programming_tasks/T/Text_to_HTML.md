[1]: https://rosettacode.org/wiki/Text_to_HTML

# [Text to HTML][1]





The task specs are essentially non-existent. "Make a best guess at how to render mark-up free text"? Anything that could be trusted at all would either be extremely trivial or insanely complex. And it shows by the the task example writers staying away in droves. Five examples after seven years!?



Rather than waste time on that noise, I'll demonstrate POD6 to HTML conversion. POD6 is a simple, text-only mark-up used for Raku documentation. (It's Plain Old Documentation for Raku) It uses pretty simple textual markup and has multiple tools to convert the POD6 document in to many other formats, HTML among them.



It is **not** markup free, but it is actually usable in production.

```perl
use Pod::To::HTML;
use HTML::Escape;

my $pod6 = q:to/POD6/;
=begin pod

A very simple Pod6 document.

This is a very high-level, hand-wavey overview. There are I<lots> of other
options available.

=head1 Section headings

=head1 A top level heading

=head2 A second level heading

=head3 A third level heading

=head4 A fourth level heading

=head1 Text

Ordinary paragraphs do not require an explicit marker or delimiters.

Alternatively, there is also an explicit =para marker that can be used to
explicitly mark a paragraph.

=para
This is an ordinary paragraph.
Its text  will   be     squeezed     and
short lines filled.

=head1 Code

Enclose code in a =code block (or V<C< >> markup for short, inline samples)

=begin code
    my $name = 'Rakudo';
    say $name;
=end code

=head1 Lists

=head3 Unordered lists

=item  Grumpy
=item  Dopey
=item  Doc
=item  Happy
=item  Bashful
=item  Sneezy
=item  Sleepy

=head3 Multi-level lists

=item1  Animal
=item2  Vertebrate
=item2  Invertebrate

=item1  Phase
=item2  Solid
=item2  Liquid
=item2  Gas

=head1 Formatting codes

Formatting codes provide a way to add inline mark-up to a piece of text.

All Pod6 formatting codes consist of a single capital letter followed
immediately by a set of single or double angle brackets; Unicode double angle
brackets may be used.

Formatting codes may nest other formatting codes.

There are many formatting codes available, some of the more common ones:

=item1 V<B< >> Bold
=item1 V<I< >> Italic
=item1 V<U< >> Underline
=item1 V<C< >> Code
=item1 V<L< >> Hyperlink
=item1 V<V< >> Verbatim (Don't interpret anything inside as POD markup)

=head1 Tables

There is quite extensive markup to allow rendering tables.

A simple example:

=begin table :caption<Mystery Men>
        The Shoveller   Eddie Stevens     King Arthur's singing shovel
        Blue Raja       Geoffrey Smith    Master of cutlery
        Mr Furious      Roy Orson         Ticking time bomb of fury
        The Bowler      Carol Pinnsler    Haunted bowling ball
=end table

=end pod
POD6

# for display on Rosettacode
say escape-html render($pod6);

# normally
#say render($pod6);
```

#### Output:
```
<!doctype html>
<html lang="en">
    <head>
        <title></title>
        <meta charset="UTF-8" />
        <style>
        kbd { font-family: "Droid Sans Mono", "Luxi Mono", "Inconsolata", monospace }
        samp { font-family: "Terminus", "Courier", "Lucida Console", monospace }
        u { text-decoration: none }
        .nested {
            margin-left: 3em;
        }
        aside, u { opacity: 0.7 }
        a[id^="fn-"]:target { background: #ff0 }
        </style>
        <link rel="stylesheet" href="//design.raku.org/perl.css">

    </head>
    <body class="pod">
    <div id="___top"></div>

    <nav class="indexgroup">
<table id="TOC">
<caption><h2 id="TOC_Title">Table of Contents</h2></caption>
    <tr class="toc-level-1"><td class="toc-number">1</td><td class="toc-text"><a href="#Section_headings">Section headings</a></td></tr>
 <tr class="toc-level-1"><td class="toc-number">2</td><td class="toc-text"><a href="#A_top_level_heading">A top level heading</a></td></tr>
 <tr class="toc-level-2"><td class="toc-number">2.1</td><td class="toc-text"><a href="#A_second_level_heading">A second level heading</a></td></tr>
 <tr class="toc-level-3"><td class="toc-number">2.1.1</td><td class="toc-text"><a href="#A_third_level_heading">A third level heading</a></td></tr>
 <tr class="toc-level-4"><td class="toc-number">2.1.1.1</td><td class="toc-text"><a href="#A_fourth_level_heading">A fourth level heading</a></td></tr>
 <tr class="toc-level-1"><td class="toc-number">3</td><td class="toc-text"><a href="#Text">Text</a></td></tr>
    <tr class="toc-level-1"><td class="toc-number">4</td><td class="toc-text"><a href="#Code">Code</a></td></tr>
      <tr class="toc-level-1"><td class="toc-number">5</td><td class="toc-text"><a href="#Lists">Lists</a></td></tr>
 <tr class="toc-level-3"><td class="toc-number">5.0.1</td><td class="toc-text"><a href="#Unordered_lists">Unordered lists</a></td></tr>
        <tr class="toc-level-3"><td class="toc-number">5.0.2</td><td class="toc-text"><a href="#Multi-level_lists">Multi-level lists</a></td></tr>
        <tr class="toc-level-1"><td class="toc-number">6</td><td class="toc-text"><a href="#Formatting_codes">Formatting codes</a></td></tr>
           <tr class="toc-level-1"><td class="toc-number">7</td><td class="toc-text"><a href="#Tables">Tables</a></td></tr>
              
</table>
</nav>

    <div class="pod-body">
    <p>A very simple Pod6 document.</p>
<p>This is a very high-level, hand-wavey overview. There are <em>lots</em> of other options available.</p>
<h1 id="Section_headings"><a class="u" href="#___top" title="go to top of document">Section headings</a></h1>
<h1 id="A_top_level_heading"><a class="u" href="#___top" title="go to top of document">A top level heading</a></h1>
<h2 id="A_second_level_heading"><a class="u" href="#___top" title="go to top of document">A second level heading</a></h2>
<h3 id="A_third_level_heading"><a class="u" href="#___top" title="go to top of document">A third level heading</a></h3>
<h4 id="A_fourth_level_heading"><a class="u" href="#___top" title="go to top of document">A fourth level heading</a></h4>
<h1 id="Text"><a class="u" href="#___top" title="go to top of document">Text</a></h1>
<p>Ordinary paragraphs do not require an explicit marker or delimiters.</p>
<p>Alternatively, there is also an explicit =para marker that can be used to explicitly mark a paragraph.</p>
<p>This is an ordinary paragraph. Its text will be squeezed and short lines filled.</p>
<h1 id="Code"><a class="u" href="#___top" title="go to top of document">Code</a></h1>
<p>Enclose code in a =code block (or C&lt; &gt; markup for short, inline samples)</p>
<pre class="pod-block-code">    my $name = &#39;Rakudo&#39;;
    say $name;
</pre>
<h1 id="Lists"><a class="u" href="#___top" title="go to top of document">Lists</a></h1>
<h3 id="Unordered_lists"><a class="u" href="#___top" title="go to top of document">Unordered lists</a></h3>
<ul><li><p>Grumpy</p>
</li>
<li><p>Dopey</p>
</li>
<li><p>Doc</p>
</li>
<li><p>Happy</p>
</li>
<li><p>Bashful</p>
</li>
<li><p>Sneezy</p>
</li>
<li><p>Sleepy</p>
</li>
</ul>
<h3 id="Multi-level_lists"><a class="u" href="#___top" title="go to top of document">Multi-level lists</a></h3>
<ul><li><p>Animal</p>
</li>
<ul><li><p>Vertebrate</p>
</li>
<li><p>Invertebrate</p>
</li>
</ul>
<li><p>Phase</p>
</li>
<ul><li><p>Solid</p>
</li>
<li><p>Liquid</p>
</li>
<li><p>Gas</p>
</li>
</ul>
</ul>
<h1 id="Formatting_codes"><a class="u" href="#___top" title="go to top of document">Formatting codes</a></h1>
<p>Formatting codes provide a way to add inline mark-up to a piece of text.</p>
<p>All Pod6 formatting codes consist of a single capital letter followed immediately by a set of single or double angle brackets; Unicode double angle brackets may be used.</p>
<p>Formatting codes may nest other formatting codes.</p>
<p>There are many formatting codes available, some of the more common ones:</p>
<ul><li><p>B&lt; &gt; Bold</p>
</li>
<li><p>I&lt; &gt; Italic</p>
</li>
<li><p>U&lt; &gt; Underline</p>
</li>
<li><p>C&lt; &gt; Code</p>
</li>
<li><p>L&lt; &gt; Hyperlink</p>
</li>
<li><p>V&lt; &gt; Verbatim (Don&#39;t interpret anything inside as POD markup)</p>
</li>
</ul>
<h1 id="Tables"><a class="u" href="#___top" title="go to top of document">Tables</a></h1>
<p>There is quite extensive markup to allow rendering tables.</p>
<p>A simple example:</p>
<table class="pod-table">
<caption>Mystery Men</caption>
<tbody>
<tr> <td>The Shoveller</td> <td>Eddie Stevens</td> <td>King Arthur&#39;s singing shovel</td> </tr> <tr> <td>Blue Raja</td> <td>Geoffrey Smith</td> <td>Master of cutlery</td> </tr> <tr> <td>Mr Furious</td> <td>Roy Orson</td> <td>Ticking time bomb of fury</td> </tr> <tr> <td>The Bowler</td> <td>Carol Pinnsler</td> <td>Haunted bowling ball</td> </tr>
</tbody>
</table>
    </div>

    </body>
</html>
```
