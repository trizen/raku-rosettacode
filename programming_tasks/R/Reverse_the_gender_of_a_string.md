[1]: https://rosettacode.org/wiki/Reverse_the_gender_of_a_string

# [Reverse the gender of a string][1]

Mechanically, this task is trivial. Perl 6 provides several flexible and powerful methods to wrangle text. Linguistically, this task is impossible (and laughable). Mappings are non-regular and in many cases, non-deterministic without semantic analysis of the content and context, which is **WAY** beyond what anyone is going to invest in a Rosettacode task. Whatever.



For extremely limited circumstances such as this example, this should suffice. Notice case matching and replication. Handles contractions, but ignores embedded matching text.

```raku
say S:g:ii/«'she'»/he/ given "She was a soul stripper. She took my heart! They say she's going to put me on a shelf.";
```

#### Output:
```
He was a soul stripper. He took my heart! They say he's going to put me on a shelf.
```