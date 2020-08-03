[1]: https://rosettacode.org/wiki/XML_Validation

# [XML Validation][1]

```perl
#!/usr/bin/env perl6
 
# 20191009 Perl 6 programming solution
 
# Reference:
# https://github.com/p6-xml/LibXML-p6
 
use v6.d;
use LibXML;
use LibXML::Schema;
 
my $good_xml         = '<a>5</a>';
my $bad_xml          = '<a>5<b>foobar</b></a>';
 
my $xsdschema = q:to<EOF>;
   <xsd:schema xmlns:xsd="http://www.w3.org/2001/XMLSchema">
      <xsd:element name="a" type="xsd:integer"/>
   </xsd:schema>
EOF
 
my LibXML $p .= new();
 
for ( $good_xml, $bad_xml ) {
   my $x = $p.parse: :string($_);
   try { LibXML::Schema.new( string => $xsdschema ).validate( $x ) }
   !$! ?? say "Valid." !! say $!.message() ;
}
```

#### Output:
```
Valid.
Schemas validity error : Element 'a': Element content is not allowed, because the type definition is simple.
```
