[1]: https://rosettacode.org/wiki/Long_year

# [Long year][1]





December 28 is always in the last week of the year. (By ISO8601)

```perl
sub is-long ($year) { Date.new("$year-12-28").week[1] == 53 }

# Testing
say   "Long years in the 20th century:\n", (1900..^2000).grep: &is-long;
say "\nLong years in the 21st century:\n", (2000..^2100).grep: &is-long;
say "\nLong years in the 22nd century:\n", (2100..^2200).grep: &is-long;
```

#### Output:
```
Long years in the 20th century:
(1903 1908 1914 1920 1925 1931 1936 1942 1948 1953 1959 1964 1970 1976 1981 1987 1992 1998)

Long years in the 21st century:
(2004 2009 2015 2020 2026 2032 2037 2043 2048 2054 2060 2065 2071 2076 2082 2088 2093 2099)

Long years in the 22nd century:
(2105 2111 2116 2122 2128 2133 2139 2144 2150 2156 2161 2167 2172 2178 2184 2189 2195)
```
