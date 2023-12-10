[1]: https://rosettacode.org/wiki/Color_quantization

# [Color quantization][1]



```perl
use MagickWand;
use MagickWand::Enums;

my $frog = MagickWand.new;
$frog.read("./Quantum_frog.png");
$frog.quantize(16, RGBColorspace, 0, True, False);
$frog.write('./Quantum-frog-16-perl6.png');
```


See: [Quantum-frog-16-perl6.png](https://github.com/thundergnat/rc/blob/master/img/Quantum-frog-16-perl6.png) (offsite .png image)
