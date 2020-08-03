[1]: https://rosettacode.org/wiki/Search_a_list_of_records

# [Search a list of records][1]

The built-in method `.first` fulfills the requirements of this task.

It takes any [smart-matcher](https://docs.perl6.org/language/operators#infix_~~) as a predicate. The `:k` adverb makes it return the key (i.e. numerical index) instead of the value of the element.

```raku
my @cities =
  { name => 'Lagos',                population => 21.0  },
  { name => 'Cairo',                population => 15.2  },
  { name => 'Kinshasa-Brazzaville', population => 11.3  },
  { name => 'Greater Johannesburg', population =>  7.55 },
  { name => 'Mogadishu',            population =>  5.85 },
  { name => 'Khartoum-Omdurman',    population =>  4.98 },
  { name => 'Dar Es Salaam',        population =>  4.7  },
  { name => 'Alexandria',           population =>  4.58 },
  { name => 'Abidjan',              population =>  4.4  },
  { name => 'Casablanca',           population =>  3.98 },
;
 
say @cities.first(*<name> eq 'Dar Es Salaam', :k);
say @cities.first(*<population> < 5).<name>;
say @cities.first(*<name>.match: /^A/).<population>;
```

#### Output:
```
6
Khartoum-Omdurman
4.58
```