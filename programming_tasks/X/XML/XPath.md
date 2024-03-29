[1]: https://rosettacode.org/wiki/XML/XPath

# [XML/XPath][1]



```perl
use XML::XPath;

my $XML = XML::XPath.new(xml => q:to/END/);
<inventory title="OmniCorp Store #45x10^3">
  <section name="health">
    <item upc="123456789" stock="12">
      <name>Invisibility Cream</name>
      <price>14.50</price>
      <description>Makes you invisible</description>
    </item>
    <item upc="445322344" stock="18">
      <name>Levitation Salve</name>
      <price>23.99</price>
      <description>Levitate yourself for up to 3 hours per application</description>
    </item>
  </section>
  <section name="food">
    <item upc="485672034" stock="653">
      <name>Blork and Freen Instameal</name>
      <price>4.95</price>
      <description>A tasty meal in a tablet; just add water</description>
    </item>
    <item upc="132957764" stock="44">
      <name>Grob winglets</name>
      <price>3.56</price>
      <description>Tender winglets of Grob. Just add water</description>
    </item>
  </section>
</inventory>
END

put "First item:\n", $XML.find('//item[1]')[0];

put "\nPrice elements:";
.contents.put for $XML.find('//price').List;

put "\nName elements:\n", $XML.find('//name')».contents.join: ', ';
```

#### Output:
```
First item:
<item upc="123456789" stock="12"> <name>Invisibility Cream</name>  <price>14.50</price>  <description>Makes you invisible</description>  </item>

Price elements:
14.50
23.99
4.95
3.56

Name elements:
Invisibility Cream, Levitation Salve, Blork and Freen Instameal, Grob winglets
```
