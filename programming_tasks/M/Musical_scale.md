[1]: https://rosettacode.org/wiki/Musical_scale

# [Musical scale][1]

```raku
for 0,2,4,5,7,9,11,12 {
    shell "play -n -c1 synth 0.2 sin %{$_ - 9}"
}
```