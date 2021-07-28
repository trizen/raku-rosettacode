[1]: https://rosettacode.org/wiki/Change_e_letters_to_i_in_words

# [Change e letters to i in words][1]

```perl
my %ei = 'unixdict.txt'.IO.words.grep({ .chars > 5 and /<[ie]>/ }).map: { $_ => .subst('e', 'i', :g) };
put %ei.grep( *.key.contains: 'e' ).grep({ %ei{.value}:exists }).sort.batch(4)».gist».fmt('%-22s').join: "\n";
```

#### Output:
```
analyses => analysis   atlantes => atlantis   bellow => billow       breton => briton      
clench => clinch       convect => convict     crises => crisis       diagnoses => diagnosis
enfant => infant       enquiry => inquiry     frances => francis     galatea => galatia    
harden => hardin       heckman => hickman     inequity => iniquity   inflect => inflict    
jacobean => jacobian   marten => martin       module => moduli       pegging => pigging    
psychoses => psychosis rabbet => rabbit       sterling => stirling   synopses => synopsis  
vector => victor       welles => willis
```
