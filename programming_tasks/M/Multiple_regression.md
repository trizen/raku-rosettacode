[1]: https://rosettacode.org/wiki/Multiple_regression

# [Multiple regression][1]

We're going to solve the example on the Wikipedia article using [Clifford](https://github.com/grondilu/clifford), a [geometric algebra](https://en.wikipedia.org/wiki/Geometric_algebra) module. Optimization for large vector space does not quite work yet, so it's going to take (a lof of) time and a fair amount of memory, but it should work.



Let's create four vectors containing our input data:



![image](https://rosettacode.org/mw/index.php?title=Special:MathShowImage&hash=6d51317055678716c670eef53bc4a0b9&mode=mathml)



Then what we're looking for are three scalars ![image](https://rosettacode.org/mw/index.php?title=Special:MathShowImage&hash=7b7f9dbfea05c83784f8b85149852f08&mode=mathml), ![image](https://rosettacode.org/mw/index.php?title=Special:MathShowImage&hash=b0603860fcffe94e5b8eec59ed813421&mode=mathml) and ![image](https://rosettacode.org/mw/index.php?title=Special:MathShowImage&hash=ae539dfcc999c28e25a0f3ae65c1de79&mode=mathml) such that:



![image](https://rosettacode.org/mw/index.php?title=Special:MathShowImage&hash=bddd5378dcc19922d4cc24ed135b3f3a&mode=mathml)



To get for instance ![image](https://rosettacode.org/mw/index.php?title=Special:MathShowImage&hash=7b7f9dbfea05c83784f8b85149852f08&mode=mathml) we can first make the ![image](https://rosettacode.org/mw/index.php?title=Special:MathShowImage&hash=b0603860fcffe94e5b8eec59ed813421&mode=mathml) and ![image](https://rosettacode.org/mw/index.php?title=Special:MathShowImage&hash=ae539dfcc999c28e25a0f3ae65c1de79&mode=mathml) terms disappear:



![image](https://rosettacode.org/mw/index.php?title=Special:MathShowImage&hash=77bff080723535c85621227af72dee59&mode=mathml)



Noting ![image](https://rosettacode.org/mw/index.php?title=Special:MathShowImage&hash=4d09086a4a272007c243ea764d5fdd0c&mode=mathml), we then get:



![image](https://rosettacode.org/mw/index.php?title=Special:MathShowImage&hash=490caa42b7f48dc598b1badff0f1a763&mode=mathml)



**Note:** a number of the formulae above are invisible to the majority of browsers, including Chrome, IE/Edge, Safari and Opera. They may (subject to the installation of necessary fronts) be visible to Firefox.






```raku
use Clifford;
my @height = <1.47 1.50 1.52 1.55 1.57 1.60 1.63 1.65 1.68 1.70 1.73 1.75 1.78 1.80 1.83>;
my @weight = <52.21 53.12 54.48 55.84 57.20 58.57 59.93 61.29 63.11 64.47 66.28 68.10 69.92 72.19 74.46>;
 
my $w = [+] @weight Z* @e;
 
my $h0 = [+] @e[^@weight];
my $h1 = [+] @height Z* @e;
my $h2 = [+] (@height X** 2) Z* @e;
 
my $I = $h0∧$h1∧$h2;
my $I2 = ($I·$I.reversion).Real;
 
say "α = ", ($w∧$h1∧$h2)·$I.reversion/$I2;
say "β = ", ($w∧$h2∧$h0)·$I.reversion/$I2;
say "γ = ", ($w∧$h0∧$h1)·$I.reversion/$I2;
```

#### Output:
```
α = 128.81280357844
β = -143.1620228648
γ = 61.960325442
```


This computation took over an hour with the april 2016 version of rakudo on MoarVM, running in a VirtualBox linux system guest hosted by a windows laptop with a i7 intel processor.