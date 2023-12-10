[1]: https://rosettacode.org/wiki/Yahoo!_search_interface

# [Yahoo! search interface][1]







YahooSearch.rakumod:

```perl
use Gumbo;
use LWP::Simple;
use XML::Text;

class YahooSearch {
  has $!dom;

  submethod BUILD (:$!dom) { }

  method new($term) {
    self.bless(
      dom => parse-html(
        LWP::Simple.get("http://search.yahoo.com/search?p={ $term }")
      )
    );
  }

  method next {
    $!dom = parse-html(
      LWP::Simple.get(
        $!dom.lookfor( TAG => 'a', class => 'next' ).head.attribs<href> 
      )
    );
    self;
  }

  method text ($node) {
    return ''         unless $node;
    return $node.text if     $node ~~ XML::Text;

    $node.nodes.map({ self.text($_).trim }).join(' ');
  }

  method results {
    state $n = 0;
    for $!dom.lookfor( TAG => 'h3', class => 'title') {
      given .lookfor( TAG => 'a' )[0] {
        next unless $_;                                               # No Link
        next if .attribs<href> ~~ / ^ 'https://r.search.yahoo.com' /; # Ad
        say "=== #{ ++$n } ===";
        say "Title: { .contents[0] ?? self.text( .contents[0] ) !! '' }";
        say "  URL: { .attribs<href> }";

        my $pt = .parent.parent.parent.elements( TAG => 'div' ).tail;
        say " Text: { self.text($pt) }";
      }
    }
    self;
  }

}

sub MAIN (Str $search-term) is export {
  YahooSearch.new($search-term).results.next.results;
}
```


And the invocation script is simply:



yahoo-search.raku


```
   use YahooSearch; 
```


So:


```
   raku yahoo-search.raku test 
```


Should give out something like the following:

```text
=== #1 ===
Title: 
  URL: https://www.speedtest.net/
 Text: At Ookla, we are committed to ensuring that individuals with disabilities can access all of the content at www.speedtest.net. We also strive to make all content in Speedtest apps accessible. If you are having trouble accessing www.speedtest.net or Speedtest apps, please email legal@ziffdavis.com for assistance. Please put "ADA Inquiry" in the ...
=== #2 ===
Title: Test | Definition of Test by Merriam-Webster
  URL: https://www.merriam-webster.com/dictionary/test
 Text: Test definition is - a means of testing: such as. How to use test in a sentence.
=== #3 ===
Title: - Video Results
  URL: https://video.search.yahoo.com/search/video?p=test
 Text: More Test videos
```


...and should go up to result #21!
