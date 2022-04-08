[1]: https://rosettacode.org/wiki/Day_of_the_week_of_Christmas_and_New_Year

# [Day of the week of Christmas and New Year][1]

```perl
my @d-o-w = < Sunday Monday Tuesday Wednesday Thursday Friday Saturday >;
 
.say for (flat 2020..2022, (1500 .. 2500).roll(7)).sort.map: {
     "In {$_}, New Years is on a { @d-o-w[Date.new($_,  1,  1).day-of-week % 7] }, " ~
     "and Christmas on a { @d-o-w[Date.new($_, 12, 25).day-of-week % 7] }."
}
```

#### Output:
```
In 1578, New Years is on a Sunday, and Christmas on a Monday.
In 1590, New Years is on a Monday, and Christmas on a Tuesday.
In 1642, New Years is on a Wednesday, and Christmas on a Thursday.
In 1957, New Years is on a Tuesday, and Christmas on a Wednesday.
In 2020, New Years is on a Wednesday, and Christmas on a Friday.
In 2021, New Years is on a Friday, and Christmas on a Saturday.
In 2022, New Years is on a Saturday, and Christmas on a Sunday.
In 2242, New Years is on a Saturday, and Christmas on a Sunday.
In 2245, New Years is on a Wednesday, and Christmas on a Thursday.
In 2393, New Years is on a Friday, and Christmas on a Saturday.
```
