[1]: https://rosettacode.org/wiki/Jordan-Pólya_numbers

# [Jordan-Pólya numbers][1]

**Partial translation of [Go](#Go)**

```perl
# 20230719 Raku programming solution

my \factorials = 1, | [\*] 1..18; # with 0!

sub JordanPolya (\limit) {
   my \ix = (factorials.keys.first: factorials[*] >= limit) // factorials.end; 
   my ($k, @res) = 2, |factorials[0..ix];

   while $k < @res.elems {
      my \rk = @res[$k];
      for 2 .. @res.elems -> \l {
         my \kl = $ = @res[l] * rk;
         last if kl > limit;
         loop {
            my \p = @res.keys.first: { @res[$_] >= kl } # performance
            if p < @res.elems and @res[p] != kl {
               @res.splice: p, 0, kl
            } elsif p == @res.elems { 
               @res.append: kl 
            }
            kl > limit/rk ?? ( last ) !! kl *= rk
         }
      }
      $k++
   }
   return @res[1..*]
}

my @result = JordanPolya 2**30 ; 
say "First 50 Jordan-Pólya numbers:";
say [~] $_>>.fmt('%5s') for @result[^50].rotor(10);
print "\nThe largest Jordan-Pólya number before 100 million: ";
say @result.first: * < 100_000_000, :end;
```


You may [Attempt This Online!](https://ato.pxeger.com/run?1=bZNNbptAFMcX3XGKv12qYGKPwZWlyNROV110lUV3NrFwPNQjhg8NoMRKnIt0k0V7gh6h6iV6mj5gqEHNSAjNvP_7vY958-27CqLy5eVHWYSTqz9vfsdHbMLgrkiVCGSOJdwxnrDe2D5cxtwrD29xL4oDnIFh5OUOn1O1D5KbVB4DWBspYlGM8GgAqFDigRDWGcgifsxZKFReLHA-XhN-tYT2nk47JsaTvQfNs8xojI-K5yPCziizDsJhTDz4nlFJ7w9CcpgRPtRqxiWP8yYrnZiKiFDZ1mZETo0hTBVmYKzrNFlhI_-5au9IkrfZEqQPGyryzhoZ5AVECNKtmqq6xjTNukDNzDSu16JHneO27g_hTtT_jCvKNA6SO96jUMCsX3GQ7BtA5mNQ-_fj0qrMFPmplt1m_phUY73NGKOL0avneAKXeR1vuey1GK_xWZBldI2LKoE-prc7d2tK13N9Datp5AiDQWW0l9Rl4z_n9m9Gl5dGu1e8KFXSFO9WZRgnw6A2VwelLKji7uDObPu9AxqzPDhi-KnqPeaOlkxufv2sREkZ77jKF0Ovlq2ffZjb1YqFcWFdvJvnF6N6gHSE9e3c8ZlKaTwt1xl5RqZEUmC4Sb4cONWlvnIK8loE7DhxOFzHQSykFGmygA6q4e142HTdJNs6zTfGon4tzWvWj7p93H8B)
