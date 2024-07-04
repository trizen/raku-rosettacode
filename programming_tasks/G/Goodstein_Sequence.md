[1]: https://rosettacode.org/wiki/Goodstein_Sequence

# [Goodstein Sequence][1]

```perl
# 20240219 Raku programming solution

sub bump($n is copy, $b) {
   loop ( my ($res, $i) = 0, 0; $n.Bool or return $res; $i++ ) {
      if my $d = ($n % $b).Int { $res += $d * (($b+1) ** bump($i,$b)).round }
      $n = ($n / $b).floor
   }
}

sub goodstein($n, $maxterms = 10) {
   my @res = $n;
   while @res.elems < $maxterms && @res[*-1] != 0 {
      @res.push(bump(@res[*-1], (@res.elems + 1)) - 1)
   }
   return @res
}

say "Goodstein(n) sequence (first 10) for values of n from 0 through 7:";
for 0..7 -> $i { say "Goodstein of $i: ", goodstein($i) }

say();
my $max = 16;
say "The Nth term of Goodstein(N) sequence counting from 0, for values of N from 0 through $max :";
for 0..$max -> $i { say "Term $i of Goodstein($i): {goodstein($i, $i+1)[*-1]}" }
```


You may [Attempt This Online!](https://ato.pxeger.com/run?1=XZK_btswEMaBjnqKr4YaUJas2ktTWElRdCm6eMpWdPAfyiIqkQ5FtjUMPUmXDM0LZczT9I5SXTkaCIj33Xe_O97vP3b93T88PHpXzt4_v3pq_QYb3xxErKFabM3hmCHeJDhFAGpjDhBojhCxlS1FVIJbzDPMC8Q6_2RMDWNhpfNWgzV0r9IUgwF9quT8eEd5XOQNu-dftMMp6JHecnAKIeJNukgwnQ5AKiNlklvj9Q7d4EYOvc_b4FMSoeVQF3VRxM3sjdm1TipNIuJt1r-ctE1LWYv5AEU4H7kyFdYFX_ysVC3DXS5rSeKbUeLVVYh8nc4W3_Camj93FhIOvq1EAD6rMoiRV4pFkmBGZ89JxzAuFgXs9RGTz2dunaCV917qrYQolW1dQC9pzj_WtSdwU0KjtKYhGFfRgPYVrpeTImLNPM-vMftAz0ATvrTmxFgtMcnGY6In7SFEUkT8VNQ6j-td0ZPdVRIrV4HHwQ7_SVcj0i09k1N6P3BlL3hXL3lDlRFz-L_AvuN69HtRkmiXOI3peSlpb8Lkuwm6frWHDf-36X8B)
