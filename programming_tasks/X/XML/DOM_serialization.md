[1]: https://rosettacode.org/wiki/XML/DOM_serialization

# [XML/DOM serialization][1]



```perl
use XML;
use XML::Writer;

say my $document = XML::Document.new(
    XML::Writer.serialize( :root[ :element['Some text here', ], ] )
);
```
```xml
<?xml version="1.0"?><root><element>Some text here</element></root>
```
